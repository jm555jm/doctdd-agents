---
name: vue3-element-plus
description: This skill should be used when implementing frontend features with Vue 3 and Element Plus, or when the /doctdd command dispatches the frontend-expert agent during Phase 3. Provides Composition API, auto-import, and project structure knowledge.
disable-model-invocation: true
version: 1.0.0
---

**Vue 3 + Element Plus** technical knowledge for frontend implementation.

## Tech Stack

- Framework: Vue 3 with Composition API (`<script setup lang="ts">`)
- Build: Vite 8
- UI Library: Element Plus (auto-import via unplugin)
- Router: Vue Router 4
- Language: TypeScript (strict mode)

## Project Structure

```
packages/frontend/
├── src/
│   ├── App.vue
│   ├── main.ts
│   ├── router/
│   │   └── index.ts       # Vue Router setup
│   ├── views/             # Page components
│   ├── components/        # Reusable components
│   └── env.d.ts
├── vite.config.ts
└── package.json
```

## Code Style

- Use `<script setup lang="ts">` for all components
- No comments (except TODO)
- Descriptive variable and method names
- Element Plus components are auto-imported (no import statements needed)
- Use `fetch` for API calls (base URL: `/api`)
- Chinese text for UI labels and messages
- Form inputs must have `id`, and `<label>` must have matching `for` attribute (e.g. `<label for="username">帳號</label>` + `<el-input id="username" />`). This ensures a11y and enables Playwright `getByLabel` locators.

## Key Patterns

- Views are page-level components in `src/views/`
- Router configuration in `src/router/index.ts`
- API proxy configured in `vite.config.ts` to forward `/api` to backend

## Documentation Lookup

Before using Vue 3, Element Plus, or Vue Router APIs, check Context7 for up-to-date documentation:
- `mcp__context7__resolve-library-id` to find Vue 3 / Element Plus
- `mcp__context7__get-library-docs` to fetch API docs
