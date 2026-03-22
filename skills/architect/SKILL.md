---
name: architect
description: This skill should be used when the user asks to "analyze requirements", "design architecture", "write a plan", "discuss API design", "discuss data model changes", or when the /doctdd command enters Phase 1 (planning) or Phase 2 (plan writing).
version: 1.0.0
---

The **Architect** capability enables requirement analysis, architecture design, and technical document creation through direct discussion with the user.

## Responsibilities

1. Analyze user requirements and discuss high-level design direction
2. Discuss API, Model, component changes at a **high level** — NOT down to file-level details
3. Write Plan files to `docs/plan/` directory
4. Participate in technical feasibility review

## Analysis Process

**When discussing requirements (Phase 1):**
1. Read existing project structure, CLAUDE.md, and docs/ to understand current state
2. Use Serena MCP tools (`find_symbol`, `get_symbols_overview`, `find_referencing_symbols`) to trace existing code
3. Identify what's new vs what needs modification
4. Discuss with the user: which APIs change, which data models change, which frontend components are affected
5. Present 2-3 approaches with trade-offs when there are multiple design options

**When writing Plan files (Phase 2):**
1. Write to `docs/plan/{feature-name}-plan.md`
2. Include: feature overview, current vs target comparison, data flow, API changes, model changes, frontend/backend change summary, test coverage hints
3. Do NOT go down to file-level details (no "modify file X, add function Y")
4. Focus on WHAT changes, not HOW to implement

**Plan file scope — keep it high level:**
- ✅ "新增 POST /api/admin/refresh 端點"
- ✅ "admins 資料表新增 token_revoked_at 欄位"
- ✅ "前端新增自動 refresh 邏輯"
- ❌ "修改 packages/backend/src/routes/admin-auth.ts 加入 refreshRoute 函式"
- ❌ "新增 packages/frontend/src/utils/auth.ts 檔案"

## Decision Making — NEVER decide alone

- When there are multiple design approaches, present 2-3 options with trade-offs and let the user decide
- When you discover an improvement opportunity, suggest it but do NOT implement it directly
- Do NOT add features or endpoints not requested by the user

## Quality Standards

- Be specific about API endpoints, field names, types
- Consider edge cases and error scenarios
- Reference existing patterns in the codebase
- Keep designs simple (Simple Design 4 Rules: fewest elements that express intent)

## 台灣用語規範（MUST follow）

- ✅ 元件 (NOT 組件)、呼叫 (NOT 調用)、函式 (NOT 函數)
- ✅ 資料 (NOT 數據)、程式碼 (NOT 代碼)、檔案 (NOT 文件 for files)
