# ShopBoard (React Edition) - Pro Desk Atelier & E-Commerce Web Application

ShopBoard (شوب بورد) is an editorial e-commerce platform and interactive 3D desk atelier built with **React 19**, **TypeScript**, and **Three.js** via **React Three Fiber**. Crafted around an _Akari Desk Atelier_ aesthetic—blending Washi Ivory, Bamboo Tan, Charcoal Ink, and Deep Emerald accents—the application offers a tactile shopping experience for curated workspace hardware, desktop accessories, and ergonomic equipment, complete with full **RTL Arabic localization**.

---

## Key Features

- **Curated Catalog & Deterministic Filtering**:
  - Live category filtering (Electronics, Ergonomics, Accessories, All).
  - Deterministic multi-mode sorters (Price: Low-to-High, High-to-Low, Default catalogue order).
  - Instant Quick-View dialog with keyboard escape navigation and high-resolution plate photography.
  - Live stock status badges and real-time inventory display.

- **Smart Shopping Cart & Checkout**:
  - Slide-over drawer with animated backdrop and accessible modal focus trapping.
  - Normalized state storage with delta quantity updates (`+1` / `-1`) to prevent race conditions.
  - Dynamic promotional coupon engine supporting percentage discounts (e.g., `SHOP20`, `WELCOME10`).
  - Automatic free-shipping threshold calculation with live progress indication.
  - Seamless checkout flow validating customer name, email, address, and city.

- **Interactive 3D WebGL Desk Studio (`src/desk-builder/`)**:
  - Parametric 3D desk customizer built with Three.js and React Three Fiber (`@react-three/fiber` & `@react-three/drei`).
  - Studio-grade three-point lighting, soft hemisphere ambient illumination, and real-time shadow casting.
  - Real-time desk dimension scaling and wood finishes (Natural Walnut, Scandinavian Oak, Dark Ash).
  - Interactive accessory placement: custom PC case with triple ARGB fans & tempered glass, curved ultra-wide gaming monitors, mechanical keyboards, precision mice, studio speakers, and desk mats.
  - 360-degree orbit navigation, raycasted object positioning, item rotation controls, and one-click "Add Full Desk Setup to Cart" bundling.

- **Orders Lifecycle Management (Mandatory 4-State UI)**:
  - Strict representation of all 4 asynchronous view states: **Loading** (shimmer skeletons), **Error** (actionable retry trigger), **Empty** (contextual empty state), and **Success** (interactive list).
  - Real-time metrics breakdown: total spent, completed deliveries, and active shipments.
  - Order details modal with line items, tax breakdowns, and status trackers (`delivered`, `shipped`, `processing`, `cancelled`).

- **Accessible Authentication & User Profiles**:
  - Controlled modal authentication dialog supporting seamless toggle between login and registration.
  - Inline input validation with accessible `aria-invalid` and `aria-describedby` announcements.
  - Persistent user session in `localStorage` with mock profile management and logout.

- **Editorial Design System & RTL Localization**:
  - Built natively for Right-to-Left Arabic layouts (`lang="ar" dir="rtl"`) using Readex Pro and IBM Plex Sans Arabic typography.
  - Pure CSS3 design tokens (`:root` variables) with zero runtime CSS-in-JS overhead and zero `!important`.

---

## Tech Stack

| Domain                       | Technology                                                                                                                 | Purpose                                                               |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Framework**                | [React 19](https://react.dev/)                                                                                             | Declarative UI, Strict Mode, Concurrent Features                      |
| **Language**                 | [TypeScript 5.x / 7.x](https://www.typescriptlang.org/)                                                                    | Strict static typing, 0 `any` policy                                  |
| **Bundler & Dev Server**     | [Vite 8](https://vitejs.dev/)                                                                                              | High-speed ESM bundling and Hot Module Replacement (HMR)              |
| **3D WebGL Graphics**        | [Three.js](https://threejs.org/) + [React Three Fiber](https://r3f.docs.pmnd.rs/) + [Drei](https://github.com/pmndrs/drei) | Real-time 3D desk customizer and interactive scene controls           |
| **State Management**         | Native React Context API + Custom Hooks                                                                                    | Zero external dependencies (no Redux, Zustand, or MobX)               |
| **Styling**                  | Pure CSS3 + Design Tokens                                                                                                  | Custom `:root` properties, mobile-first responsive media queries      |
| **Unit & Integration Tests** | [Vitest](https://vitest.dev/) + [React Testing Library](https://testing-library.com/react)                                 | High-speed unit testing for pure logic and components                 |
| **End-to-End Tests**         | [Playwright](https://playwright.dev/)                                                                                      | Multi-browser headless E2E verification of critical user journeys     |
| **Linter & Formatter**       | [ESLint](https://eslint.org/) & [Prettier](https://prettier.io/)                                                           | Automated flat-config linting and formatting with Husky & lint-staged |

---

## Clean Architecture & Engineering Standards

The codebase adheres strictly to the **Modular Feature Architecture** pattern, enforcing a clean separation between domain logic, data fetching, application state, and presentation:

```text
src/modules/<feature-name>/
├── <feature>.types.ts         # Domain models, DTOs, prop interfaces, and state contracts
├── <feature>.api.ts           # Remote API requests routing through shared apiClient
├── <feature>.logic.ts         # Pure calculations, algorithms, filters (Zero React, zero hooks)
├── context/                   # Native React Context provider and custom hooks
│   ├── <Feature>Context.tsx
│   └── use<Feature>.ts
└── components/                # Smart & Dumb component split
    ├── <Feature>Container.tsx # Smart/Container: hooks, async lifecycles, orchestration
    ├── <Feature>List.tsx      # Presentational: receives props, emits callbacks
    ├── <Feature>Card.tsx      # Presentational: atomic display component
    └── <Feature>Skeleton.tsx  # Presentational: loading placeholder
```

### Architectural Principles:

1. **Pure Logic Isolation (`*.logic.ts`)**: Pricing formulas, tax rates, shipping thresholds, discount calculations, and sorting algorithms reside exclusively in pure functions without React or DOM dependencies.
2. **Centralized API Client (`src/shared/api/client.ts`)**: Raw `fetch()` calls and hardcoded endpoints are strictly forbidden. All HTTP communications pass through `apiClient` with unified headers, serialization, and typed `ApiException` error handling.
3. **Smart & Dumb Split**: Presentational components never subscribe to context or trigger direct API calls. They receive data strictly via `props` and notify parents via callbacks.
4. **Mandatory 4-State UI Lifecycle**: Every data-driven screen implements explicit handling for `loading`, `error`, `empty`, and `success`.
5. **No Direct DOM Manipulation**: The UI is entirely declarative. Direct manipulation of the DOM (`querySelector`, `innerHTML`, `getElementById`) is prohibited. React refs (`useRef`) are reserved exclusively for 3D canvas bridging and element focus.

---

## Project Structure

```text
shopboard-react/
├── public/                       # Static public assets
│   ├── assets/
│   │   ├── plates/               # High-resolution editorial photography plates
│   │   └── products/             # Catalog product images
│   ├── favicon.svg               # Application favicon
│   └── icons.svg                 # SVG sprite sheet
├── src/
│   ├── main.tsx                  # React entry point and global context providers
│   ├── App.tsx                   # Main application layout and modal orchestrator
│   ├── style.css                 # Design tokens, typography, grid, and responsive styling
│   ├── desk-builder/             # 3D Interactive WebGL Desk Atelier
│   │   ├── DeskStudioContainer.tsx
│   │   ├── studio-scene.ts       # Three.js scene, camera, lights, and floor setup
│   │   ├── studio-catalog.ts     # Procedural 3D asset geometries and materials
│   │   ├── studio-interaction.ts # Raycasting, dragging, and placement controls
│   │   ├── studio-cart.ts        # Setup pricing and shopping cart sync adapter
│   │   ├── types.ts              # 3D scene types and accessory definitions
│   │   └── components/
│   │       └── DeskStudioModal.tsx # Studio modal shell and 2D overlay controls
│   ├── modules/                  # Feature modules
│   │   ├── auth/                 # Authentication & profile management
│   │   │   ├── auth.types.ts
│   │   │   ├── context/
│   │   │   └── components/
│   │   ├── cart/                 # Cart, drawer, coupon engine, and checkout
│   │   │   ├── cart.types.ts
│   │   │   ├── cart.logic.ts     # Discount, shipping, subtotal pure calculations
│   │   │   ├── context/
│   │   │   └── components/
│   │   ├── orders/               # Order history, status tracker, 4-state UI
│   │   │   ├── orders.types.ts
│   │   │   ├── orders.api.ts
│   │   │   ├── orders.logic.ts   # Order metrics aggregation and status filtering
│   │   │   ├── context/
│   │   │   └── components/
│   │   └── products/             # Product catalog, filters, quick view
│   │       ├── products.types.ts
│   │       ├── products.api.ts
│   │       ├── products.logic.ts # Pure search, category filter, deterministic sorters
│   │       ├── context/
│   │       └── components/
│   └── shared/                   # Cross-cutting shared infrastructure
│       ├── api/
│       │   └── client.ts         # Centralized HTTP client and ApiException
│       ├── components/           # Reusable UI primitives (Button, Modal, Toast, Icons)
│       └── validation/           # Form validation rules and regex validators
├── tests/
│   ├── unit/                     # Vitest unit test suites (*.logic.test.ts)
│   │   ├── cart.logic.test.ts
│   │   ├── orders.logic.test.ts
│   │   └── products.logic.test.ts
│   └── e2e/                      # Playwright browser end-to-end test suites
│       ├── auth.spec.ts
│       ├── cart.spec.ts
│       ├── catalog.spec.ts
│       ├── desk-builder.spec.ts
│       └── orders.spec.ts
├── playwright.config.ts          # Playwright test runner configuration
├── vitest.config.ts              # Vitest test runner configuration
├── vite.config.ts                # Vite build configuration
├── tsconfig.json                 # TypeScript compiler project configuration
├── AGENTS.md                     # Single source of truth & rules for AI coding agents
├── CLAUDE.md                     # Guidelines & developer instructions
└── DESIGN.md                     # Visual token specification and design tokens
```

---

## Quickstart & CLI Commands

### Prerequisites

- **Node.js**: >= 18.x (v20+ recommended)
- **npm**: >= 9.x

### Installation

```bash
# Clone the repository and install dependencies
npm install
```

### Development

```bash
# Start local Vite development server with HMR
npm run dev
```

### Quality Assurance & Testing

```bash
# Run TypeScript typecheck without emitting files
npm run typecheck

# Run unit tests via Vitest
npm test

# Run Vitest in interactive watch mode
npm run test:watch

# Run Playwright End-to-End browser test suite
npm run test:e2e

# Run ESLint static code analysis
npm run lint

# Verify code formatting with Prettier
npm run format:check
```

### Production Build

```bash
# Typecheck and bundle application into dist/
npm run build

# Preview production build locally
npm run preview
```

---

## Accessibility & Localization Standards

- **Semantic HTML**: All interactive regions leverage native semantic tags (`<dialog>`, `<header>`, `<main>`, `<section>`, `<nav>`, `<button>`).
- **Input Accessibility**: Every form control has an explicit `<label htmlFor="...">` and communicates validation state via `aria-invalid` and `aria-describedby`.
- **Keyboard Navigation**: Dialogs and slide-overs support standard keyboard dismiss (`Escape`) and focus trapping.
- **RTL Support**: Built with CSS logical properties and directional awareness for optimal right-to-left Arabic rendering.
