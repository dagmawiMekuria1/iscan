# ITADScanner — Style Guide

## Language & Runtime
- **HTML5** semantic elements (`<main>`, `<section>`, `<nav>`, `<header>`, `<article>`)
- **CSS3** with custom properties (design tokens in `tokens.css`), `@layer` for cascade control
- **Vanilla JavaScript** ES2022+, loaded via `<script type="module">`
- **No build step**, no bundler, no npm, no external CDN resources

## File Organization
- Each HTML page is a standalone file in the project root.
- CSS is split by concern in `assets/css/` and loaded via `<link rel="stylesheet">` with relative paths.
- JS modules live in `assets/js/`, imported via relative `import` statements.
- All paths are **relative** (e.g., `assets/css/tokens.css`), never absolute (`/assets/…`).

## Naming Conventions

### Files & Folders
- **kebab-case** for all files: `capture-page.js`, `gemini-mock.js`, `progress-dots.js`
- CSS files named by scope: `tokens.css`, `reset.css`, `globals.css`, `components.css`, `layout.css`
- Page-specific CSS: `auth.css`, `home.css`, `capture.css`, `inventory.css`

### CSS
- Custom properties use `--category-descriptor` pattern: `--color-bg-primary`, `--space-4`, `--radius-md`
- Class names use **BEM-lite**: `.card`, `.card__title`, `.card--active`
- Utility classes are prefixed: `.u-visually-hidden`, `.u-flex`, `.u-gap-4`
- State classes: `.is-active`, `.is-disabled`, `.is-loading`, `.is-editing`

### JavaScript
- **camelCase** for variables, functions, and methods: `currentUser()`, `formatDate()`, `handleSubmit()`
- **PascalCase** for class constructors (if any): `ProgressDots`, `CameraController`
- **UPPER_SNAKE_CASE** for constants: `SESSION_TTL_MS`, `MAX_IMAGE_SIZE_BYTES`
- Module exports use named exports: `export function signIn()`, not default exports
- DOM query helpers: `$` for `querySelector`, `$$` for `querySelectorAll`

### IDs and Data Attributes
- HTML `id` attributes: **kebab-case**: `id="capture-form"`, `id="step-indicator"`
- Data attributes: `data-step="2"`, `data-capture-id="cap_xyz789"`, `data-ai-filled="true"`

## CSS Architecture

### Load Order (in every HTML file)
```html
<link rel="stylesheet" href="assets/css/tokens.css">
<link rel="stylesheet" href="assets/css/reset.css">
<link rel="stylesheet" href="assets/css/globals.css">
<link rel="stylesheet" href="assets/css/components.css">
<link rel="stylesheet" href="assets/css/layout.css">
<link rel="stylesheet" href="assets/css/{page}.css">
```

### Token Usage
- Never hard-code colors, spacing, or font sizes. Always reference tokens:
  - ✅ `color: var(--color-text-primary)`
  - ❌ `color: #e8e8ef`
- Exception: one-off values with a comment explaining why a token isn't appropriate.

### Responsive Design
- Mobile-first: base styles target phones, `@media (min-width: …)` adds complexity.
- Breakpoints: `480px` (sm), `768px` (md), `1024px` (lg), `1280px` (xl)
- Prefer CSS Grid and Flexbox over floats or absolute positioning.
- Use `container queries` where supported for component-level responsiveness.

## JavaScript Architecture

### Module Pattern
Every JS file is an ES module. Pages are initialized by `app.js` which detects the current page and dynamically imports the correct page module:

```js
// app.js detects page and calls init
const page = document.body.dataset.page;
if (page === 'capture') {
  const { init } = await import('./pages/capture-page.js');
  init();
}
```

### Service Layer
- `services/auth.js` — Authentication (mock): `signUp()`, `signIn()`, `signOut()`, `currentUser()`
- `services/db.js` — CRUD operations on localStorage: `createCapture()`, `getCaptures()`, `updateCapture()`, `deleteCapture()`
- `services/storage.js` — Image storage: `saveImage()`, `getImage()`, `deleteImage()`
- `services/gemini-mock.js` — AI simulation: `analyzeImage()`

### Error Handling
- Service functions return `{ success: boolean, data?: any, error?: string }`
- UI displays errors via the toast component
- Console errors are logged with `console.error()` for debugging

### Event Patterns
- Use `addEventListener` on specific elements, not global delegation (unless for dynamic content)
- Custom events via `dispatchEvent(new CustomEvent('name', { detail }))` for component communication
- Clean up event listeners when navigating away (if using SPA-like patterns)

## Accessibility
- All interactive elements must be keyboard-accessible
- Form inputs must have associated `<label>` elements
- Use `aria-` attributes for dynamic state: `aria-current="step"`, `aria-expanded`, `aria-live="polite"`
- Color contrast: ensure text meets WCAG AA against dark backgrounds
- Focus indicators: green glow ring on `:focus-visible`

## Performance
- No external requests — everything loads from local files
- Images stored as base64 in localStorage (capped at 5MB per image)
- Debounce search inputs (300ms)
- Use `requestAnimationFrame` for animations where JS is involved

## Security (Mock Context)
- Passwords are stored as `btoa()` encoded strings — this is intentionally insecure for a mock
- Session tokens are stored in `sessionStorage` — cleared on tab close
- All data access is filtered by `userId` — no cross-user data leakage in the mock

## Project Details
- **Name**: ITADScanner
- **Display Name**: TAGSCAN
- **Repo Name**: iscan