---
name: vue3-element-plus
description: Vue 3 + Vite + Element Plus technical knowledge — Composition API, auto-import, project structure. Loaded into frontend-expert agent during Phase 3.
version: 1.0.0
---

You now have **Vue 3 + Element Plus** technical knowledge for frontend implementation.

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

## Key Patterns

- Views are page-level components in `src/views/`
- Router configuration in `src/router/index.ts`
- API proxy configured in `vite.config.ts` to forward `/api` to backend

## Documentation Lookup

Before using Vue 3, Element Plus, or Vue Router APIs, check Context7 for up-to-date documentation:
- `mcp__context7__resolve-library-id` to find Vue 3 / Element Plus
- `mcp__context7__get-library-docs` to fetch API docs
