# doctdd-agents

Document-First Development (DocTDD) plugin for Claude Code.

自動化 DocTDD 開發流程：所有實作都必須先有文件，文件確認後才寫程式。

## 安裝

```bash
claude --plugin-dir ~/.claude/plugins/doctdd-agents
```

## 指令

| 指令 | 用途 |
|---|---|
| `/doctdd-init` | 初始化專案環境，產生 `.doctdd-env.yaml` |
| `/doctdd <任務>` | 執行完整 DocTDD 流程（Plan → 文件 → 可行性評估 → 實作 → 收尾） |
| `/doctdd-release <版本>` | 建立 Release Note（分析差異 → 截圖 → GitHub Release） |

## 快速開始

```bash
# 1. 啟動 Claude Code 並載入 plugin
claude --plugin-dir ~/.claude/plugins/doctdd-agents

# 2. 初始化專案環境（只需做一次）
/doctdd-init

# 3. 開始開發
/doctdd 學生管理功能
```

## 流程

```
Phase 1: 規劃（Architect Skill — 高層級討論）
  └─ GATE: 使用者確認方向 ✓

Phase 2: 文件（Document First）
  ├─ Plan 檔案 → GATE ✓
  ├─ Requirement Doc（AC 格式）→ GATE ✓
  └─ Tech Doc（API、DB）→ GATE ✓

Phase 2.5: 技術可行性評估
  ├─ E2E / Backend / Frontend 可行性 checklist
  ├─ Implementation Notes 產出
  └─ GATE: 使用者確認可行性 ✓

Phase 3: 實作
  ├─ E2E 測試（紅燈驗證）
  ├─ Backend + Frontend 平行實作
  ├─ 跑測試 → 全部通過
  ├─ Code Review → 修復 → 再審查
  └─ GATE: 使用者驗證功能 ✓

Phase 4: 收尾
  ├─ Commit Plan（保留 git 紀錄）
  ├─ Smart Merge 文件
  └─ Commit（docs → test → feat）
```

## 結構

```
doctdd-agents/
├── commands/
│   ├── doctdd.md           # 完整 DocTDD 流程
│   ├── doctdd-init.md      # 環境初始化
│   └── doctdd-release.md   # Release Note
├── agents/                 # 通用角色（不綁技術棧）
│   ├── architect.md        # 寫 Tech Doc
│   ├── backend-expert.md   # 後端實作
│   ├── frontend-expert.md  # 前端實作
│   ├── e2e-expert.md       # E2E 測試
│   ├── doc-manager.md      # 文件管理 + Smart Merge
│   └── code-reviewer.md    # Simple Design 4 Rules 審查
├── skills/                 # 技術知識（可切換）
│   ├── architect/          # 架構分析 + 可行性評估
│   ├── playwright/         # Playwright E2E 知識
│   ├── fastify-prisma/     # Fastify + Prisma 知識
│   └── vue3-element-plus/  # Vue 3 + Element Plus 知識
└── templates/
    └── doctdd-env.template.yaml
```

## 設計原則

- **Agent 通用化**：Agent 不知道專案用什麼框架，技術知識由 Skill 提供
- **Skill 注入**：協調者在派遣 Agent 時讀取對應的 tech skill 內容，貼進 Agent 的 prompt
- **Phase 自動推導**：`.doctdd-env.yaml` 只描述專案狀態，workflow phase 由 `/doctdd` 自動決定
- **GATE 機制**：每個需要使用者確認的節點都會暫停等待

## 環境設定檔

`.doctdd-env.yaml` 放在專案根目錄，由 `/doctdd-init` 自動產生：

```yaml
project:
  name: my-project
  has_docs: true
  has_claude_md: true
  monorepo: false

stacks:
  frontend:
    enabled: true
    agent_skill: vue3-element-plus
  backend:
    enabled: true
    agent_skill: fastify-prisma
  e2e:
    enabled: true
    agent_skill: playwright

commands:
  test_e2e: "npm run test:e2e"
```

## 跨專案使用

換專案時只需要：
1. 跑 `/doctdd-init` 產出新的 `.doctdd-env.yaml`
2. 如果技術棧不同（例如 React），建立對應的 skill（`skills/react/SKILL.md`）

## 使用者的職責

1. **Review 所有文件** — Plan、Requirement Doc、Tech Doc
2. **確認可行性評估** — 確保文件已通過技術可行性評估
3. **驗證功能** — 確認實作符合預期
4. **監督流程** — 確保流程不跳步
