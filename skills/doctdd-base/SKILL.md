---
name: doctdd-base
description: This skill should be used when any /doctdd command starts, to load shared environment configuration, rules, commit conventions, and Taiwan terminology standards.
version: 1.0.0
---

The **DocTDD Base** configuration provides shared environment loading, workflow rules, and commit conventions for all /doctdd commands.

## Phase 0: Load Environment

1. Read `.doctdd-env.yaml` from the project root directory
2. **If file does NOT exist:**
   - Tell the user: "找不到 `.doctdd-env.yaml`。請先執行 `/doctdd-init` 來設定專案環境。"
   - **STOP** — do not proceed
3. **If file exists:** parse and apply configuration

**Phase-skipping rules (automatic):**

| env 欄位 | 為 false 時跳過 |
|---|---|
| `stacks.e2e.enabled` | E2E 可行性、E2E 測試+紅燈、跑測試 |
| `stacks.backend.enabled` | Backend 可行性、Backend 實作 |
| `stacks.frontend.enabled` | Frontend 可行性、Frontend 實作 |
| `project.has_docs` | Smart Merge（但 Plan 檔案仍產出） |

**Skill loading:**
- Review skills: `Skill("doctdd-agents:{review_skill}")` for enabled stacks
- Tech skills: read `skills/{agent_skill}/SKILL.md`, include in agent dispatch prompt
- Test command: use `commands.test_e2e` from env

## Dispatching Agents with Tech Skills

When dispatching any implementation agent:
1. Read the tech skill content from `skills/{agent_skill}/SKILL.md`
2. Read the corresponding implementation notes from `docs/plan/implementation-notes/`
3. Include BOTH in the agent's dispatch prompt

## Rules

- **NEVER skip a GATE** — always wait for user confirmation
- **NEVER start implementation before all documents are confirmed AND feasibility reviewed**
- At each step, briefly announce what you're doing and why
- If an agent returns an error or unexpected result, report to the user and ask how to proceed
- Use Traditional Chinese (Taiwan, 繁體中文) for all communication

## 台灣用語規範

- ✅ 元件 (NOT 組件)、呼叫 (NOT 調用)、函式 (NOT 函數)
- ✅ 資料 (NOT 數據)、程式碼 (NOT 代碼)、檔案 (for files, NOT 文件)

## Commit Rules

1. **Commit order follows DocTDD:** `docs` → `test` → `feat`
2. **Logically different changes must be separate commits**
3. **Temporary/generated files must NOT be committed** — add them to `.gitignore`
4. **Frontend + backend for the same feature may be combined** into one commit
