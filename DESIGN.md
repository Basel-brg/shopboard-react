---
name: ShopBoard (شوب بورد) - React Edition
description: The Akari Desk Atelier — warm Washi Ivory, Bamboo Tan ribs, Charcoal Ink, and Vermilion seal craft for pro desk equipment, declarative React components & 3D WebGL studio
colors:
  washi-ivory: '#f2eada'
  washi-cream: '#f6efe1'
  paper-ash: '#eae5d8'
  well-ash: '#e5dfd2'
  bamboo-tan: '#ccb693'
  bamboo-border: '#d4be9b'
  rib-shadow: '#c3b5a2'
  charcoal-ink: '#252522'
  charcoal-deep: '#12100f'
  muted-umeboshi: '#686358'
  vermilion-seal: '#c23828'
  bamboo-green: '#2e6f40'
typography:
  display:
    fontFamily: 'Readex Pro, IBM Plex Sans Arabic, Tsukimi Rounded, sans-serif'
    fontSize: '45px'
    fontWeight: 700
    lineHeight: 1.05
    letterSpacing: '-0.01em'
  headline:
    fontFamily: 'Readex Pro, IBM Plex Sans Arabic, sans-serif'
    fontSize: '24px'
    fontWeight: 700
    lineHeight: 1.2
  title:
    fontFamily: 'IBM Plex Sans Arabic, sans-serif'
    fontSize: '16px'
    fontWeight: 700
    lineHeight: 1.25
  body:
    fontFamily: 'IBM Plex Sans Arabic, sans-serif'
    fontSize: '15px'
    fontWeight: 400
    lineHeight: 1.5
  label:
    fontFamily: 'IBM Plex Sans Arabic, sans-serif'
    fontSize: '13px'
    fontWeight: 600
    lineHeight: 1.25
  mono:
    fontFamily: 'IBM Plex Mono, Doto, monospace'
    fontSize: '13px'
    fontWeight: 500
    lineHeight: 1.4
rounded:
  sm: '6px'
  md: '8px'
  lg: '10px'
  pill: '999px'
spacing:
  xs: '4px'
  sm: '8px'
  md: '16px'
  lg: '24px'
  xl: '40px'
components:
  button-primary:
    backgroundColor: '{colors.charcoal-ink}'
    textColor: '{colors.washi-cream}'
    rounded: '{rounded.pill}'
    padding: '0 24px'
    height: '46px'
  button-secondary:
    backgroundColor: '{colors.washi-cream}'
    textColor: '{colors.charcoal-ink}'
    rounded: '{rounded.pill}'
    padding: '0 20px'
    height: '41px'
  card-product:
    backgroundColor: '{colors.washi-cream}'
    textColor: '{colors.charcoal-deep}'
    rounded: '{rounded.md}'
    padding: '14px'
---

# دليل نظام التصميم والهوية البصرية | ShopBoard Design System

## 1. الفلسفة البصرية والمفهوم العام (Visual Philosophy)

**Creative North Star:** _The Akari Desk Atelier_ (`paper-folds-pleats-deployable-akari-light-sculpture`).

يقدم متجر **ShopBoard (شوب بورد)** أجهزة ومعدات مساحات العمل الاحترافية كأدوات معمارية وهندسية هادئة مستوحاة من رصانة فنون الورق الياباني (Washi) ودفء الخشب والنحاس، بعيداً عن أسلوب متاجر الألعاب التقليدية ذات الإضاءات الصاخبة:

- **الأرضية الأساسية:** ورق الواشي العاجي الدافئ (`#f2eada` / `#f6efe1`).
- **الهياكل والتقسيمات:** خطوط خشب الخيزران الهيكلية (`#ccb693` / `#d4be9b`).
- **النصوص والتحكم:** حبر الفحم العميق (`#252522` / `#12100f`).
- **أختام التمييز:** ختم شوب بورد القرمزي الصيني (`#c23828`).

---

## 2. لوحة الألوان ورموز التصميم (Color Palette & Design Tokens)

### أ. الطبقات السطحية والخلفيات (Surfaces & Grounds)

| اللون         | كود اللون | الاستخدام                                    |
| ------------- | --------- | -------------------------------------------- |
| `washi-ivory` | `#f2eada` | خلفية الصفحة الرئيسية، مساحة العمل الأولى    |
| `washi-cream` | `#f6efe1` | بطاقات المنتجات، خلفية النوافذ المنبثقة      |
| `paper-ash`   | `#eae5d8` | شريط الأتيليه الجانبي، شريط الفلاتر، التذييل |
| `well-ash`    | `#e5dfd2` | تجاويف صور المنتجات داخل البطاقات            |

### ب. خطوط البناء والتمييز (Structural Accents)

| اللون            | كود اللون | الاستخدام                                        |
| ---------------- | --------- | ------------------------------------------------ |
| `bamboo-tan`     | `#ccb693` | الخط الفاصل الرأسي، إطارات البطاقات              |
| `bamboo-border`  | `#d4be9b` | حدود الحقول، فواصل الأقسام                       |
| `charcoal-ink`   | `#252522` | النصوص الرئيسية، الأزرار الأساسية (Pill Buttons) |
| `charcoal-deep`  | `#12100f` | العناوين البارزة، نصوص الشاشات المركزة           |
| `vermilion-seal` | `#c23828` | الختم التراثي، شارات الخصم، تنبيهات الأخطاء      |
| `bamboo-green`   | `#2e6f40` | شريط التقدم للشحن المجاني، رسائل النجاح          |

---

## 3. الخطوط والطباعة (Typography & RTL Standards)

- **الاتجاه العام**: عربي كامل من اليمين إلى اليسار (`lang="ar" dir="rtl"`).
- **العناوين الرئيسية (`Readex Pro`, `IBM Plex Sans Arabic`)**:
  - وزن ثقيل `700`، ارتفاع سطر منضبط (`1.05 - 1.2`).
  - أرقام ثابتة العرض (`tabular-nums`) تضمن اصطفاف الأسعار والكميات.
- **النصوص وحقول الإدخال (`IBM Plex Sans Arabic`)**:
  - وضوح كامل في مقاسات `14px - 16px` مع وزن `400` و `600`.
- **الرموز التقنية والأكواد (`IBM Plex Mono`, `Doto`)**:
  - مخصصة لأكواد المنتجات (مثل `SB-KEY-01`)، أرقام التتبع، وأبعاد المكاتب ثلاثية الأبعاد.

---

## 4. شبكة التخطيط والتجاوب (Responsive Layout & Breakpoints)

- **منهجية التصميم**: الجوال أولاً (Mobile-First) بتوسع تدريجي يعتمد فقط على `min-width` بدون `!important`:
  - **Mobile (`< 640px`)**: تدفق عمودي أحادي، قائمة سفلية وملاحة قابلة للطي.
  - **Tablet (`min-width: 640px`)**: شبكة بطاقات ثنائية الأعمدة مع توسيع شريط البحث.
  - **Desktop (`min-width: 1024px`)**:
    - **شريط الأتيليه الجانبي (Atelier Rail)**: ثابت في الجهة اليمنى بعرض ~20% يحتوي على الملاحة والختم.
    - **منصة البداية (Hero Stage)**: تشغل 80% في نافذة `1376x768` مدمجة بصورة مكتب البلوط وشريط البطاقات الأربعة الأفقية.
    - **الكتالوج الكامل**: شبكة رباعية الأعمدة متوازنة مع لوحة تحكم كاملة.

---

## 5. قواعد العمق والأبعاد (Elevation & Depth Rules)

- **قاعدة الارتفاع الأحادي (Single Elevation Rule)**:
  - لا يجوز الجمع بين الحدود والظلال الساقطة على البطاقات.
  - يتم التعبير عن العمق والارتفاع حصراً عبر حدود خشب الخيزران الهيكلية `1px - 2px solid` وتدرج طبقات الورق الدافئة.

---

## 6. مكتبة المكونات التفاعلية (React Component Library)

### أ. منصة العرض وشريط الأتيليه (`ProductsHero.tsx`)

- شريط ملاحة رأسي أيقوني مع ختم التوثيق الصيني.
- 4 بطاقات مميزة مدمجة بالإطار المعماري للمتجر.

### ب. بطاقات المنتجات (`ProductCard.tsx`)

- نافذة عرض صورة منخفضة بتجويف `#e5dfd2`.
- زر المعاينة السريعة (`quick-view-trigger`) الذي يفتح تفاصيل المنتج دون مغادرة الصفحة.
- شارات التميز التحريرية ونسب الخصم الحسابية.
- زر إضافة سريع للسلة بحواف كبسولية (`pill`).

### ج. لوحة تحكم الحالات الأربعة (4-State UI Components)

- **`loading`**: هيكل وميضي متكامل (`<ProductsSkeleton />`).
- **`error`**: بطاقة تنبيهية واضحة بزر إجراء بديل "إعادة المحاولة (Try Again)".
- **`empty` (خادم فارغ)**: رسالة توضيحية لعدم وجود بيانات مع زر تحديث.
- **`empty` (بحث بدون نتائج)**: زر فوري لإعادة ضبط الفلاتر والبحث.
- **`success`**: شبكة المنتجات الكاملة.

### د. حقيبة التسوق المنبثقة (`CartDrawer.tsx`)

- شريط جانبي متحرك بسلاسة من اليسار لليمين.
- مؤشر تفاعلي يعرض المبلغ المتبقي لفتح الشحن المجاني مع تعبئة خضراء.
- تحكم فوري بالكميات بنمط الدلتا لمنع تكرار الإرسال.
- تطبيق أكواد الخصم (`SHOP20` و `WELCOME10`).
- نموذج فوري لبيانات التوصيل والطلب مع تحقق لحظي.

### هـ. استوديو المكاتب ثلاثي الأبعاد (`DeskStudioModal.tsx`)

- مسرح WebGL متكامل بإضاءة استوديو ثلاثية النقاط وظلال ناعمة (PCF Soft Shadows).
- سحب وإفلات العناصر أو إضافتها بضغطة زر.
- شريط تحكم سفلي في تدوير وحذف القطعة المحددة.
- تحويل التشكيلة ثلاثية الأبعاد بضغطة واحدة إلى سلة المشتريات.

### و. النوافذ المنبثقة وحوارات النظام (`Modal.tsx`)

- استخدام عنصر `<dialog>` القياسي مع دعم النقر خارج النافذة للإغلاق وزر `Escape`.
- شريط التنبيهات السريعة (`ToastContainer.tsx`) بإشعار خروج تلقائي ناعم.

---

## 7. إرشادات ما يجب فعله وما يُمنع (Do's and Don'ts)

### ✅ ما يجب فعله (Do's):

1. **استخدام العناصر الدلالية القياسية**: `<aside>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<figure>`, `<dialog>`.
2. **عزل المنطق النقي**: وضع أي حسابات أو فلاتر داخل ملفات `*.logic.ts` دون استيراد React أو التعديل المباشر على DOM.
3. **الالتزام بالتحقق المسبق**: إرفاق كل حقل إدخال بـ `<label>` صريح وسمات `aria-invalid` و `aria-describedby`.
4. **تحديث الكميات بالدلتا**: استخدام `+1` و `-1` دائماً لمنع مشاكل تزامن الحالة.

### ❌ ما يُمنع فعله (Don'ts):

1. **منع التعديل المباشر على DOM**: لا تستخدم `document.getElementById` أو `innerHTML` نهائياً؛ واجهات React تصريحية بالكامل.
2. **منع `!important` في CSS**: تنظيم التخصيص عبر التصنيفات النظيفة ورموز `:root`.
3. **منع دمج الظلال مع الإطارات**: الحفاظ على هدوء وبساطة ورشاقة أسلوب الأتيليه.
4. **منع إهمال تنظيف التأثيرات**: إعادة دوال التنظيف `return () => ...` دائماً في `useEffect` لمنع تسريب الذاكرة في Three.js أو المؤقتات.
