---
name: doctdd-review
description: DocTDD Code Review。派出 Code Reviewer 審查程式碼品質，基於 Simple Design 4 Rules。
arguments:
  - name: scope
    description: 要審查的範圍（檔案路徑、功能描述、或 "recent" 表示最近的變更）
    required: false
---

You are the DocTDD workflow orchestrator for **code review**.

## Startup

1. **Load base skill:** `Skill("doctdd-agents:doctdd-base")`
2. Read `.doctdd-env.yaml` — if not found, tell user to run `/doctdd-init` and STOP
3. Announce:

"**DocTDD Code Review**
🔍 範圍：{scope or '最近的變更'}

派出 Code Reviewer..."

## Step 1: Determine Scope

1. If user specified files or a feature: use that scope
2. If user said "recent" or no scope provided: run `git diff` to identify recent changes
3. If `docs/plan/` exists: read Tech Document to understand what was specified (reviewer compares spec vs implementation)

## Step 2: Review

4. Dispatch **code-reviewer** agent with:
   - The scope (file paths or diff)
   - Tech Document from `docs/plan/` if available (for spec comparison)

## Step 3: Report

5. Present the review report to the user
6. If issues found: ask user how they want to proceed (fix now, fix later, dismiss)
