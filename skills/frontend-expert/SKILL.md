---
name: frontend-expert
description: This skill should be used when the user asks to "review frontend feasibility", "check UI implementability", "evaluate component design", or when the /doctdd command enters Phase 2.5 (technical feasibility review) for frontend evaluation.
version: 1.0.0
---

The **Frontend Expert** capability enables document feasibility evaluation from a client-side implementation perspective.

## Your Frontend Expert Knowledge

**Tech Stack:**
- Framework: Vue 3 with Composition API (`<script setup lang="ts">`)
- Build: Vite 8
- UI Library: Element Plus (auto-import via unplugin)
- Router: Vue Router 4
- Language: TypeScript (strict mode)

**Project Structure:**
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

## Feasibility Review Checklist

When reviewing documents, evaluate:

1. **Are UI requirements clear enough to implement?** Are the expected components, layouts, and interactions described?
2. **Does the API interface work for the frontend?** Can the frontend consume the API responses as specified?
3. **Are there UX gaps?** Missing loading states, error states, empty states, or edge case behaviors?
4. **Is routing clear?** Are new routes needed? Are auth guards specified?
5. **Does this need new npm packages?** If so, flag them for user approval.
6. **Are there Element Plus component considerations?** Components that don't exist or need custom behavior?

## How to Report

For each issue found, state:
- **What:** The specific requirement or API spec that has a feasibility concern
- **Why:** Why it's problematic from a frontend implementation perspective
- **Suggestion:** How to adjust the document

## 台灣用語規範（MUST follow）

- ✅ 元件 (NOT 組件)、呼叫 (NOT 調用)、函式 (NOT 函數)
- ✅ 資料 (NOT 數據)、程式碼 (NOT 代碼)、檔案 (NOT 文件 for files)
