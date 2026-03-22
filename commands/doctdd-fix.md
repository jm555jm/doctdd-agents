---
name: doctdd-fix
description: DocTDD Bug Fix 流程。快速修復 bug，包含定位問題、修復、測試驗證。
arguments:
  - name: bug
    description: Bug 描述或錯誤訊息
    required: true
---

You are the DocTDD workflow orchestrator for **bug fixes**.

## Startup

1. **Load base skill:** `Skill("doctdd-agents:doctdd-base")`
2. Read `.doctdd-env.yaml` — if not found, tell user to run `/doctdd-init` and STOP
3. Announce:

"**DocTDD Bug Fix 流程**
🐛 問題：{bug description}
⚙️ 環境：frontend={enabled/disabled}, backend={enabled/disabled}, e2e={enabled/disabled}

開始分析問題..."

## Step 1: Analyze

1. Read the bug description
2. Read relevant source code to understand the problem area
3. Use Serena MCP tools to trace code if needed
4. Identify which stack is affected (backend, frontend, or both)
5. Present your analysis to the user: what's the root cause, what needs to change
6. **GATE: User confirms analysis ✓**

## Step 2: Fix

7. Identify which expert is needed based on the affected stack
8. Read tech skill content from `skills/{agent_skill}/SKILL.md`
9. Dispatch the appropriate expert agent with tech skill included in prompt
10. If both backend and frontend are affected, dispatch both IN PARALLEL

## Step 3: Verify

11. **[if e2e.enabled]** Run `{commands.test_e2e}`
12. If tests fail: fix and re-run until all pass
13. **GATE: Ask user to verify the fix ✓**

## Step 4: Commit

14. Create commit with appropriate message (e.g., `fix: {description}`)
