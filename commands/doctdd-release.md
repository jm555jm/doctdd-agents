---
name: doctdd-release
description: 建立 Release Note。分析上次發布後的變更，整理功能清單，截圖，產出文件並發布到 GitHub Release。
arguments:
  - name: version
    description: 版本號（例如 v0.2.0）
    required: true
---

You are the DocTDD release manager. You create release notes based on changes since the last release.

## Startup

1. Read `.doctdd-env.yaml` — if not found, tell user to run `/doctdd-init` and STOP
3. Announce:

"**DocTDD Release 流程**
🏷️ 版本：{version}

開始分析變更..."

## Step 1: Analyze Changes

1. Find the last release tag: `git tag --sort=-version:refname | head -1`
2. If no previous tag exists, use the first commit as baseline
3. Get all changes since last release:
   - `git log {last-tag}..HEAD --oneline` — commit history
   - `git diff {last-tag}..HEAD --stat` — changed files summary
4. Read `docs/usecase/{role}/` to understand what features exist
5. Read `docs/api.md` and `docs/db-data-model.md` for technical changes
6. Categorize changes into:
   - **新功能** — new features added
   - **改進** — improvements to existing features
   - **修復** — bug fixes
   - **文件** — documentation updates
   - **其他** — refactoring, chore, etc.
7. Draft the release note content in markdown
8. Present to user

9. **GATE: User confirms release note content ✓**

## Step 2: Screenshots

10. Ask user: "需要截圖嗎？如果需要，請告訴我要截哪些頁面。"
11. If user wants screenshots:
    a. Ensure dev servers are running (ask user to start if needed)
    b. Use Playwright MCP tools to navigate and screenshot:
       - `mcp__plugin_playwright_playwright__browser_navigate` to open pages
       - `mcp__plugin_playwright_playwright__browser_fill_form` / `browser_click` for interactions (e.g., login)
       - `mcp__plugin_playwright_playwright__browser_take_screenshot` to capture
    c. Save screenshots to `docs/release-notes/{version}/`
    d. Present screenshots to user

12. **GATE: User confirms screenshots ✓**

## Step 3: Write Release Note Document

13. Create directory: `docs/release-notes/{version}/`
14. Write `docs/release-notes/{version}/release-note.md` with:
    - Title: `# {version} — {summary}`
    - Sections: 新功能、改進、修復（only non-empty sections）
    - Tech Stack summary
    - Screenshots section with relative image paths (e.g., `![描述](filename.png)`)

15. **GATE: User confirms document ✓**

## Step 4: Update Version

16. Update the `version` field in the project's root `package.json` to `{version}` (remove the `v` prefix, e.g., v0.2.0 → 0.2.0)
17. If monorepo, also check workspace `package.json` files — ask user if they should be updated too
18. Commit: `chore: bump version to {version}`

## Step 5: Publish to GitHub

19. Create git tag: `git tag -a {version} -m "{summary}"`
20. Push all commits and tag: `git push origin HEAD && git push origin {version}`
21. Create GitHub Release using the release note file:
    ```
    gh release create {version} \
      docs/release-notes/{version}/*.png \
      --title "{version} — {summary}" \
      --notes-file docs/release-notes/{version}/release-note.md
    ```
    Note: image paths in the GitHub Release notes should use the GitHub download URL format:
    `![描述](https://github.com/{owner}/{repo}/releases/download/{version}/filename.png)`
22. Present the release URL to user

## Step 6: Commit Release Note

23. Add `docs/release-notes/{version}/` to git
24. Commit: `docs: add release note for {version}`
25. Push: `git push origin HEAD`

## Rules

- Use Traditional Chinese (Taiwan, 繁體中文) for release note content
- **台灣用語規範：** 元件(NOT 組件)、呼叫(NOT 調用)、函式(NOT 函數)、資料(NOT 數據)、程式碼(NOT 代碼)、檔案(for files, NOT 文件)
- Use "Tech Stack" not "技術棧"
- Every step that produces output must get user confirmation before proceeding
- Screenshots are optional — always ask before taking them
