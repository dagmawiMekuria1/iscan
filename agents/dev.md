# ITADScanner — Developer Build Instructions (Codi)

## Project Overview
TAGSCAN is a static, no-build IT asset disposition scanner that runs entirely in the browser. It uses localStorage for persistence and mocks AI processing for model/serial extraction from device photos.

## Hard Constraints
1. **No build step**: No npm, no webpack, no Vite, no transpilation
2. **No external requests**: Zero CDN links, no Google Fonts, no remote APIs
3. **No frameworks**: No React, Vue, Angular, Svelte, or any UI framework
4. **No TypeScript**: Plain JavaScript ES2022+ with `<script type="module">`
5. **Relative paths only**: Never use `/assets/...`, always `assets/...`
6. **System fonts only**: Use the defined font stacks, no `@font-face` with remote URLs

## File Generation Order

### Phase 1: Foundation (P0)
1. `assets/css/tokens.css` — All design tokens
2. `assets/css/reset.css` — Minimal CSS reset
3. `assets/css/globals.css` — Body defaults, dot-grid, scrollbars
4. `assets/css/components.css` — Buttons, cards, inputs, badges, modals
5. `assets/css/layout.css` — Page shell, nav, content area, responsive
6. `assets/js/utils.js` — DOM helpers, UID generator, debounce, formatDate
7. `assets/js/app.js` — Config, auth guard, page router
8. `assets/js/services/auth.js` — Mock authentication
9. `assets/js/services/db.js` — CRUD over localStorage
10. `assets/js/services/storage.js` — Base64 image storage
11. `assets/js/services/gemini-mock.js` — AI extraction simulation

### Phase 2: Pages (P0)
12. `assets/js/pages/auth-page.js` — Auth form logic
13. `assets/js/pages/home-page.js` — Dashboard logic
14. `assets/js/pages/capture-page.js` — Step machine
15. `assets/js/pages/inventory-page.js` — Table + search + edit

### Phase 3: Components (P1-P2)
16. `assets/js/components/progress-dots.js` — Step indicator
17. `assets/js/components/camera.js` — getUserMedia wrapper
18. `assets/js/components/toast.js` — Notification system
19. `assets/js/components/modal.js` — Confirmation modal
20. `assets/js/components/sparkle-badge.js` — AI indicator

### Phase 4: Page CSS (P1)
21. `assets/css/auth.css`
22. `assets/css/home.css`
23. `assets/css/capture.css`
24. `assets/css/inventory.css`

### Phase 5: Assets & Docs (P1-P2)
25. `assets/icons/icons.svg` — SVG sprite
26. `README.md`

## HTML Page Template
Every HTML page follows this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TAGSCAN — {Page Name}</title>
  <link rel="stylesheet" href="assets/css/tokens.css">
  <link rel="stylesheet" href="assets/css/reset.css">
  <link rel="stylesheet" href="assets/css/globals.css">
  <link rel="stylesheet" href="assets/css/components.css">
  <link rel="stylesheet" href="assets/css/layout.css">
  <link rel="stylesheet" href="assets/css/{page}.css">
</head>
<body data-page="{page-id}">
  <!-- Page content -->
  <script type="module" src="assets/js/app.js"></script>
</body>
</html>
```

## Testing Checklist
- [ ] `index.html` redirects to `auth.html` when no session exists
- [ ] `index.html` redirects to `home.html` when session is valid
- [ ] Sign up creates a user in `itad_users`
- [ ] Sign in validates credentials and creates session
- [ ] Sign out clears session and redirects to auth
- [ ] Home page shows user greeting and action cards
- [ ] Capture flow progresses through all 5 steps
- [ ] Camera fallback (file input) works when getUserMedia is unavailable
- [ ] AI mock returns model and serial after configured delay
- [ ] Capture saves to localStorage with all fields
- [ ] Inventory table renders all captures for current user
- [ ] Search filters table rows in real-time
- [ ] Inline edit saves changes to localStorage
- [ ] Delete removes capture and associated image
- [ ] All pages are responsive at 480px, 768px, 1024px breakpoints
- [ ] No console errors on any page
- [ ] No external network requests