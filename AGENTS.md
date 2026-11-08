# ShopBoard (React Edition) - AI Agent Rules & Engineering Standards

Single source of truth for any AI agent (Claude, Antigravity, Cursor, etc.) working on this project.
Read this file before making any changes. If you learn something durable about the project
(architecture decisions, conventions, gotchas, TODOs), document it here.

---

## 1. The Stack

- **Framework & Runtime**: React 19, Vite 8, TypeScript (strict mode, zero `any` policy).
- **State Management**: Native React Context API + Custom Hooks (Zero external state libraries like Redux or Zustand).
- **3D Graphics Engine**: Three.js via React Three Fiber (`@react-three/fiber`) & `@react-three/drei`.
- **Styling & Design System**: Pure CSS3 with `:root` CSS Design Tokens and CSS Modules (_Akari Desk Atelier_ aesthetic: Warm Stone, Washi Ivory, Deep Emerald, Bamboo Tan, and Charcoal Ink).
- **Testing**: Vitest + React Testing Library (Unit & Component Tests) & Playwright (Multi-browser E2E Tests).
- **Linter**: Oxlint (ultra-fast Rust linter).
- **Localization**: Full RTL Arabic support (`lang="ar" dir="rtl"`), styled with Readex Pro and IBM Plex Sans Arabic.

---

## 2. Folder Structure & Where a New Feature Goes

### Project Folder Structure

```text
shopboard-react/
├── public/                       # Static public assets (plates, products, icons.svg)
├── src/
│   ├── main.tsx                  # Application entry point & provider tree
│   ├── App.tsx                   # Layout root & modal orchestration
│   ├── style.css                 # Design tokens, typography, grid, responsive styles
│   ├── desk-builder/             # 3D Interactive WebGL Desk Atelier
│   │   ├── DeskStudioContainer.tsx # 3D Canvas wrapper & cart bridge
│   │   ├── studio-scene.ts       # Scene, camera, studio lighting, materials
│   │   ├── studio-catalog.ts     # Procedural 3D mesh definitions & accessories
│   │   ├── studio-interaction.ts # Raycasting, dragging, placement controls
│   │   ├── studio-cart.ts        # Pricing calculation & cart conversion
│   │   ├── types.ts              # 3D scene & accessory types
│   │   └── components/
│   │       └── DeskStudioModal.tsx # Full-screen 3D studio dialog
│   ├── modules/                  # Feature-sliced domains
│   │   ├── auth/                 # Authentication, user profile, credentials validation
│   │   ├── cart/                 # Slide-over cart, coupons, checkout calculation
│   │   ├── orders/               # Order history, status tracker, 4-state async view
│   │   └── products/             # Catalog browsing, filtering, search, quick view
│   └── shared/                   # Cross-cutting reusable infrastructure
│       ├── api/
│       │   └── client.ts         # Centralized HTTP/Fetch client (ApiException)
│       ├── components/           # Reusable UI primitives (Button, Modal, Toast, Icons)
│       └── validation/           # Shared form validators & regex patterns
└── tests/
    ├── unit/                     # Vitest unit test suites (*.logic.test.ts)
    └── e2e/                      # Playwright browser end-to-end test suites
```

### Where a New Feature Goes

When creating a **new feature** (e.g. `reviews`, `wishlist`, `discounts`), **always** create a dedicated folder under `src/modules/<feature-name>/` following this exact blueprint:

```text
src/modules/<feature-name>/
├── <feature>.types.ts         # 1. Type definitions: domain entities, DTOs, prop types, state shapes
├── <feature>.api.ts           # 2. Data / API: remote calls using shared apiClient
├── <feature>.logic.ts         # 3. Pure Logic: pure functions, calculations, algorithms (Zero React/Hooks)
├── context/                   # 4. State & Context: React Context Provider and Custom Hook
│   ├── <Feature>Context.tsx
│   └── use<Feature>.ts
└── components/                # 5. Smart / Dumb Component Split
    ├── <Feature>Container.tsx # Smart/Container: connects to context, handles async lifecycle
    ├── <Feature>List.tsx      # Presentational: pure props, renders lists, emits callbacks
    ├── <Feature>Card.tsx      # Presentational: atomic display card
    └── <Feature>Skeleton.tsx  # Presentational: loading placeholder
```

#### Step-by-Step Feature Implementation Workflow:

1. **Define Types (`<feature>.types.ts`)**: Declare domain models, API responses, form inputs, and state interfaces. Never use `any`.
2. **Implement Pure Logic (`<feature>.logic.ts`)**: Write business calculations, sorting, and transformations. **Zero React, zero hooks, zero DOM**.
3. **Write Unit Tests (`tests/unit/<feature>.logic.test.ts`)**: Test pure functions in isolation with Vitest before connecting to React.
4. **Implement API Service (`<feature>.api.ts`)**: Use `apiClient` from `src/shared/api/client.ts`. Never use raw `fetch()`.
5. **Create Context & Hook (`context/`)**: Create `<Feature>Context` and export a custom hook `use<Feature>()`. Keep state normalized.
6. **Build Components (`components/`)**:
   - Create Dumb/Presentational components driven entirely by props.
   - Create Smart/Container component implementing the **Mandatory 4-State UI pattern** (`loading`, `error`, `empty`, `success`).
7. **Integrate into App**: Mount the context provider in `src/main.tsx` (if global) or wrap the container in `src/App.tsx`.

---

## 3. Rules from Mistakes WE Saw the AI Make (In Our Own Words)

These rules are direct preventatives for recurring mistakes observed during AI code generation:

### Mistake 1: Mutating Arrays In-Place in Sorters & Filters

- **The Mistake**: The AI used `array.sort((a, b) => ...)` directly on catalog arrays or state arrays. In JavaScript, `.sort()` mutates the array in-place, which corrupted the underlying dataset, broke the "Default" unsorted order, and caused subtle, unpredictable re-render loops.
- **The Rule**: **Never mutate arrays in-place**. Always create a shallow copy first using `[...items].sort(...)` or `items.slice().sort(...)`. Furthermore, all sorting and filtering logic **must** live in `*.logic.ts` as pure, deterministic functions, and selecting "Default" must return the catalog in its exact original order.

### Mistake 2: Bypassing the Centralized API Client with Raw `fetch()`

- **The Mistake**: The AI wrote ad-hoc `fetch('/api/products')` or hardcoded URLs directly inside components and hooks. This bypassed authentication headers, failed to handle unified API base URLs, lost typed `ApiException` error parsing, and caused silent failures in production builds.
- **The Rule**: **Every HTTP request MUST route through `src/shared/api/client.ts`** (`apiClient.get()`, `apiClient.post()`, etc.). Never write raw `fetch()` or `axios` in components, hooks, or feature modules.

### Mistake 3: Masking Type Safety with `as any` or Blind Type Assertions

- **The Mistake**: When encountering TypeScript compiler errors with complex objects, event targets, or API payloads, the AI frequently resorted to `(item as any)` or unchecked casting `data as Product`. This introduced runtime `undefined is not a function` and missing property crashes that escaped the compiler.
- **The Rule**: **Zero `any` allowed without explicit written justification**. Use strict interfaces, discriminating unions, and runtime type guards (e.g. `if ('id' in obj)`) to validate unknown shapes before accessing properties.

### Mistake 4: Imperative DOM Manipulation Inside React Components

- **The Mistake**: The AI used `document.getElementById('modal')`, `document.querySelector`, or `.innerHTML` to open dialogs, change text, or update badge counters. This desynchronized React's Virtual DOM from the browser DOM, caused memory leaks, and broke keyboard accessibility focus rings.
- **The Rule**: **UI state must be 100% declarative**. Control modals, drawers, banners, and values strictly via React state (`isOpen`, `activeModal`) and props. React refs (`useRef`) are strictly reserved for imperative 3D Three.js canvas bridging or programmatic keyboard focus management (`element.focus()`).

### Mistake 5: Setting Absolute Cart Quantities Instead of Delta Increments

- **The Mistake**: The AI wrote cart quantity handlers using absolute values (`updateQuantity(id, newCount)`), reading the current quantity from state at the time of click. When users clicked "+1" multiple times quickly, stale closure state resulted in dropped increments.
- **The Rule**: **Cart mutations must always use delta semantics** (`updateQuantity(id, +1)` or `updateQuantity(id, -1)`). Compute derived cart totals (subtotals, discounts, taxes, shipping) dynamically using pure logic functions, never by storing duplicated totals in state.

---

## 4. Core Engineering Rules & Conventions

1. **Mandatory 4-State UI Lifecycle**: Every data-driven or async screen must explicitly handle all 4 states:
   - `loading`: Render visual skeleton/shimmer placeholders (`<*Skeleton />`).
   - `error`: Render an error banner with an actionable **"Try Again"** button.
   - `empty`: Distinguish between `no-items` (empty dataset) and `no-match` (active filters yield 0 results).
   - `success`: Render the populated list of components.
2. **Pure Logic Isolation**: All math, coupon calculations, shipping thresholds, and transforms belong in `*.logic.ts`. They must **NEVER** import React or depend on hooks/DOM.
3. **Semantic HTML & WCAG Accessibility**:
   - Use standard HTML5 elements (`<dialog>`, `<header>`, `<main>`, `<section>`, `<nav>`, `<button>`).
   - Every interactive control must have an accessible label (`<label htmlFor="...">` or `aria-label`).
   - Modals must support keyboard `Escape` to close and manage focus properly.
4. **Mobile-First CSS**:
   - Style mobile viewport first, scaling up with `min-width` breakpoints (`640px`, `768px`, `1024px`).
   - Never use `!important`. Manage CSS specificity using `:root` variables and class selectors.
5. **English Everywhere in Codebase**:
   - File names, code identifiers, comments, and commit messages must strictly be in English.
   - User-facing localized content is kept in Arabic for this project (`dir="rtl"`).
6. **Strict React Hook Dependencies & Cleanup**:
   - Always declare complete dependency arrays for `useEffect`, `useCallback`, and `useMemo`.
   - Any subscription, event listener, or Three.js animation frame initialized in `useEffect` must return an explicit cleanup function to prevent memory leaks.

---

## 5. Development CLI Commands

```bash
npm run dev          # Start local Vite dev server with HMR
npm run build        # Typecheck (tsc -b) and build production bundle into dist/
npm run typecheck    # Run TypeScript compiler check without emitting files
npm test             # Run Vitest unit & component test suite
npm run test:e2e     # Run Playwright browser E2E test suite
npm run lint         # Run Oxlint static code analysis
npm run preview      # Preview production build locally
```
