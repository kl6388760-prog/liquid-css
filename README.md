# Liquid CSS

A premium, dependency-free CSS library for building frosted-glass interfaces on pure black. Monochrome, physically-plausible materials with the polish of visionOS / iOS 26.

![version](https://img.shields.io/badge/version-1.1.0-white)
![license](https://img.shields.io/badge/license-MIT-white)
![dependencies](https://img.shields.io/badge/dependencies-0-white)

---

## ✨ Features

- **Pure black canvas** — the material is calibrated for `#000`. White light on black glass.
- **Flat translucent fills** — no gradients used as surface material.
- **Strong backdrop blur** — `blur(28px) saturate(160%)` on every glass layer.
- **Crisp 1px glass lip** — a light line on the inner top edge, plus soft inner bottom shadow and inner ambient light.
- **Deep outer shadow** — separation from the black canvas.
- **ONE radial gradient only** — the optional cursor-follow specular highlight.
- **Zero dependencies** — no frameworks, no build tools, no external assets (Google Fonts optional).
- **Reduced-motion aware** — transforms and the specular are disabled under `prefers-reduced-motion: reduce`.
- **Native semantics** — real `<button>`, `<label>`, `<input type="checkbox">` under the hood.

---

## 📦 What's included

| Component | Class |
|---|---|
| Glass surface (utility) | `.lg-glass` |
| Buttons | `.lg-btn` + variants & sizes |
| Panels / cards | `.lg-panel` |
| Inputs & fields | `.lg-input`, `.lg-textarea`, `.lg-select`, `.lg-field` |
| Toggle / switch | `.lg-toggle` |
| Badge / tag | `.lg-badge` |
| Avatar | `.lg-avatar` |
| Progress bar | `.lg-progress` |
| Segmented control | `.lg-segmented` |
| Tooltip | `.lg-tooltip` |
| Divider | `.lg-divider` |
| List | `.lg-list` |
| Modal | `.lg-modal` |
| Specular highlight | `.lg-specular` |
| Layout helpers | `.lg-stack`, `.lg-row`, `.lg-grid` |
| Typography helpers | `.lg-eyebrow`, `.lg-heading`, `.lg-text` |

---

## 🚀 Install

### 1. Link the stylesheet

```html
<link rel="stylesheet" href="main.css">
```

### 2. (Optional) Load the typeface

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter+Tight:wght@400;500;600&display=swap" rel="stylesheet">
```

### 3. Put your UI on pure black

```html
<body class="lg-scope">
  <button class="lg-btn lg-btn--primary lg-btn--md">Continue</button>
</body>
```

> The `.lg-scope` class applies a scoped reset, sets `background: #000`, and uses the Inter Tight font stack. Liquid CSS never pollutes a page you're embedding it into.

---

## 🧪 Quick example

```html
<button class="lg-btn lg-btn--primary lg-btn--md lg-specular" data-specular>
  Continue
</button>

<div class="lg-panel lg-mt-5">
  <div class="lg-panel__header">
    <div>
      <h3 class="lg-panel__title">Appearance</h3>
      <p class="lg-panel__subtitle">Customize your interface</p>
    </div>
    <span class="lg-badge">New</span>
  </div>
  <div class="lg-panel__body">
    Panels inherit the same frosted material as buttons.
  </div>
  <div class="lg-panel__footer">
    <button class="lg-btn lg-btn--ghost lg-btn--sm">Cancel</button>
    <button class="lg-btn lg-btn--primary lg-btn--sm">Save</button>
  </div>
</div>
```

---

## 🪟 Modal

The modal is built on top of `.lg-panel--overlay`. Toggle it with the `.is-open` class.

```html
<button type="button" class="lg-btn lg-btn--primary lg-btn--md" data-open-modal="my-modal">
  Open modal
</button>

<div class="lg-modal" id="my-modal" role="dialog" aria-modal="true" aria-labelledby="my-modal-title">
  <div class="lg-modal__backdrop" data-close-modal></div>
  <div class="lg-modal__dialog lg-panel lg-panel--overlay">
    <div class="lg-panel__header">
      <div>
        <h3 class="lg-panel__title" id="my-modal-title">Glass dialog</h3>
        <p class="lg-panel__subtitle">Built with .lg-panel--overlay</p>
      </div>
      <button type="button" class="lg-modal__close" data-close-modal aria-label="Close">×</button>
    </div>
    <div class="lg-panel__body">
      This dialog uses the same frosted glass recipe as buttons and panels.
    </div>
    <div class="lg-panel__footer">
      <button type="button" class="lg-btn lg-btn--ghost lg-btn--sm" data-close-modal>Cancel</button>
      <button type="button" class="lg-btn lg-btn--primary lg-btn--sm">Confirm</button>
    </div>
  </div>
</div>

<script>
  document.querySelectorAll('[data-open-modal]').forEach(function (btn) {
    btn.addEventListener('click', function () {
      document.getElementById(btn.dataset.openModal).classList.add('is-open');
    });
  });
  document.querySelectorAll('[data-close-modal]').forEach(function (btn) {
    btn.addEventListener('click', function () {
      btn.closest('.lg-modal').classList.remove('is-open');
    });
  });
  document.addEventListener('keydown', function (e) {
    if (e.key === 'Escape') {
      document.querySelectorAll('.lg-modal.is-open').forEach(function (m) {
        m.classList.remove('is-open');
      });
    }
  });
</script>
```

---

## ✨ Specular highlight

The cursor-follow specular is the only radial gradient in the library. Add `.lg-specular` to any glass element and drive `--lg-x` / `--lg-y` from JavaScript.

```html
<button class="lg-btn lg-btn--md lg-specular">Continue</button>

<script>
  (function () {
    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
    document.querySelectorAll('.lg-specular').forEach(function (el) {
      el.addEventListener('mousemove', function (e) {
        var r = el.getBoundingClientRect();
        el.style.setProperty('--lg-x', ((e.clientX - r.left) / r.width) * 100 + '%');
        el.style.setProperty('--lg-y', ((e.clientY - r.top) / r.height) * 100 + '%');
      });
      el.addEventListener('mouseleave', function () {
        el.style.setProperty('--lg-x', '50%');
        el.style.setProperty('--lg-y', '50%');
      });
    });
  })();
</script>
```

---

## 🎨 Design tokens

Every material property is driven by CSS custom properties. Override them at `:root` or on any parent to re-theme a subtree.

### Surfaces

| Token | Default | Use |
|---|---|---|
| `--glass-fill-primary` | `rgba(255,255,255,0.035)` | Default button & panel fill |
| `--glass-fill-ghost` | `rgba(255,255,255,0.02)` | Ghost variant |
| `--glass-fill-raised` | `rgba(255,255,255,0.05)` | Hover / raised |
| `--glass-fill-sunken` | `rgba(255,255,255,0.015)` | Inputs, tracks |
| `--glass-fill-overlay` | `rgba(255,255,255,0.07)` | Modals, tooltips |

### Borders

| Token | Default |
|---|---|
| `--glass-border-primary` | `rgba(255,255,255,0.14)` |
| `--glass-border-ghost` | `rgba(255,255,255,0.08)` |
| `--glass-border-strong` | `rgba(255,255,255,0.22)` |

### Text

| Token | Default |
|---|---|
| `--text-primary` | `rgba(255,255,255,0.92)` |
| `--text-secondary` | `rgba(255,255,255,0.6)` |
| `--text-tertiary` | `rgba(255,255,255,0.4)` |
| `--text-disabled` | `rgba(255,255,255,0.28)` |

### Blur · radii · spacing · motion

| Token | Default |
|---|---|
| `--blur-sm` | `blur(16px) saturate(160%)` |
| `--blur-md` | `blur(28px) saturate(160%)` |
| `--blur-lg` | `blur(32px) saturate(160%)` |
| `--blur-xl` | `blur(40px) saturate(170%)` |
| `--radius-pill` | `100px` |
| `--radius-lg` | `22px` |
| `--radius-md` | `16px` |
| `--radius-sm` | `10px` |
| `--radius-xs` | `6px` |
| `--space-1 … --space-8` | `4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 px` |
| `--ease-glass` | `cubic-bezier(0.32, 0.72, 0, 1)` |
| `--dur-fast` | `180ms` |
| `--dur-base` | `280ms` |
| `--dur-slow` | `420ms` |
| `--font-sans` | `"Inter Tight", …system fallbacks` |

---

## ♿ Accessibility

- **Focus-visible** — buttons and inputs show a 1px white outline at 3px offset. Never removed.
- **Reduced motion** — under `prefers-reduced-motion: reduce`, transforms and the cursor specular are disabled; transitions collapse to ~0ms.
- **Native semantics** — use `<button type="button">`, `<label>` for inputs, and `<input type="checkbox">` behind the toggle.
- **Contrast** — body text uses `rgba(255,255,255,0.6)` on pure black (≈ 8.5:1), headings `0.92`. Keep disabled states out of critical flows.
- **Modals** — `role="dialog"`, `aria-modal="true"`, optional focus trap via JS and Esc to close.

---

## 🌐 Browser support

- Modern Chrome, Edge, Safari, Firefox — `backdrop-filter` ships everywhere.
- For older Safari use the `-webkit-backdrop-filter` fallback (already included).
- If `backdrop-filter` is unsupported, the translucent fill and inner shadows still read as glass — the material degrades gracefully.

---

## 📁 File structure

```
liquid-css/
├── main.css        ← the library (v1.1.0)
├── index.html      ← demo + documentation (EN / RU)
└── README.md       ← this file
```

---

## 📄 License

MIT — free for personal and commercial use.
