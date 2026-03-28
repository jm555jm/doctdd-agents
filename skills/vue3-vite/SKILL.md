---
name: vue3-vite
description: This skill should be used when implementing frontend features with Vue 3 and Vite, or when the /doctdd command dispatches the frontend-expert agent during Phase 3. Provides Composition API, composables, and project structure knowledge.
disable-model-invocation: true
version: 2.0.0
---

**Vue 3 + Vite** technical knowledge for frontend implementation.

## Tech Stack

- Framework: Vue 3 with Composition API (`<script setup lang="ts">`)
- Build: Vite
- Router: Vue Router 4
- Language: TypeScript (strict mode)

## Project Structure

```
packages/frontend/
├── src/
│   ├── App.vue
│   ├── main.ts
│   ├── router/
│   │   └── index.ts          # Vue Router setup
│   ├── views/                # Page components
│   ├── components/           # Reusable components
│   ├── composables/          # Shared composables (reactive state + functions)
│   ├── utils/                # Utility functions
│   ├── assets/               # CSS, images
│   └── env.d.ts
├── vite.config.ts
└── package.json
```

## Code Style

- Use `<script setup lang="ts">` for all components
- No comments (except TODO)
- Descriptive variable and method names
- Use native HTML elements for forms (input, select, textarea)
- Use `fetch` for API calls (base URL: `/api`)
- Chinese text for UI labels and messages
- Form inputs must have `id`, and `<label>` must have matching `for` attribute (e.g. `<label for="username">帳號</label>` + `<input id="username" />`). This ensures a11y and enables Playwright `getByLabel` locators.

## CSS Conventions

- Define design tokens as CSS variables in a shared CSS file
- All colors must use CSS variables, no hardcoded hex values
- Global shared classes: define in shared CSS file, import in `main.ts`
- Page-specific styles: use `<style scoped>`
- SVG data URIs are the only exception for hardcoded colors

## Key Patterns

- Views are page-level components in `src/views/`
- Router configuration in `src/router/index.ts`
- API proxy configured in `vite.config.ts` to forward `/api` to backend
- Composables for shared reactive state (singleton pattern: declare `ref` at module top level)
- Form validation: use reactive errors ref + `watch` to clear errors on input
- Create/Edit dual mode: single Vue component with `isEditMode` computed from route params
- Use Vue `<Teleport to="body">` for overlays (toast, modal) to avoid z-index issues

## Documentation Lookup

Before using Vue 3 or Vue Router APIs, check Context7 for up-to-date documentation:
- `mcp__context7__resolve-library-id` to find Vue 3
- `mcp__context7__get-library-docs` to fetch API docs
