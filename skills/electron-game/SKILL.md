---
name: electron-game
description: This skill should be used when implementing features for an Electron + Canvas 2D game application using IIFE modules, or when the /doctdd command dispatches frontend-expert or backend-expert agents during Phase 3. Provides IIFE module patterns, Electron IPC conventions, Canvas rendering guidance, and EventBus usage.
disable-model-invocation: true
version: 1.0.0
---

**Electron + Canvas 2D Game** technical knowledge for implementing game features.

## Tech Stack

- Application: Electron (desktop)
- Rendering: Canvas 2D (no WebGL, no framework)
- Module system: IIFE — no bundler, no import/export, `window.X` globals
- Communication: EventBus (pub/sub) for game events, Electron IPC for main ↔ renderer
- State: JSON-based persistence (Node.js side)

## Architecture — Two Processes

```
Main Process (main.js)          Renderer Process (renderer/)
├── Electron window 管理         ├── 遊戲渲染迴圈 (rAF)
├── 檔案 I/O、監控              ├── IIFE 模組群
├── 狀態管理 (Node.js)          ├── Canvas 2D 繪製
├── 存檔讀寫                     ├── EventBus 事件驅動
└── IPC handler                  └── window.X 全域存取
         ↕ preload.js (contextBridge)
```

**判斷你在哪一邊：**
- 改渲染邏輯、遊戲系統、UI → Renderer Process
- 改檔案讀寫、系統層、狀態持久化 → Main Process
- 需要跨程序通訊 → 透過 preload.js

## IIFE Module Pattern（MUST follow）

所有 renderer 模組必須使用此格式：

```javascript
window.ModuleName = (function() {
  var _privateVar = null;

  function _privateMethod() { }

  function publicMethod() { }

  return {
    publicMethod: publicMethod
  };
})();
```

- 私有變數/函式用 `_` 前綴
- 只 return 需要公開的 API
- 模組間透過 `window.X` 存取
- 保持與專案現有程式碼的 `var`/`let`/`const` 風格一致

## Script Loading Order

新模組必須加到 `index.html` 的正確位置。順序有依賴關係：

```
constants 模組（先載入，無依賴）
→ 基礎設施（EventBus 等）
→ 核心引擎
→ 遊戲系統
→ UI 模組
→ 主渲染入口（最後，負責初始化）
```

**新增模組時：** 讀 `index.html` 現有順序，確認依賴的模組已在前面載入。

## EventBus Convention

遊戲事件透過 EventBus 傳遞，解耦模組間通訊：

```javascript
EventBus.on('EVENT_NAME', function(data) { });
EventBus.emit('EVENT_NAME', data);
```

- 事件名用 UPPER_SNAKE_CASE
- 新增事件前，先 grep 現有事件名確認不衝突
- 查閱專案文件取得完整事件清單

## Electron IPC Pattern

Main ↔ Renderer 通訊透過 preload.js 橋接：

```javascript
// preload.js — 暴露 API
contextBridge.exposeInMainWorld('electronAPI', {
  methodName: (args) => ipcRenderer.invoke('channel-name', args)
});

// main.js — 處理呼叫
ipcMain.handle('channel-name', async (event, args) => { });

// renderer 側 — 呼叫
window.electronAPI.methodName(args);
```

新增 IPC 通道時，三個檔案都要改：`preload.js`（橋接）、`main.js`（handler）、renderer 模組（呼叫端）。

## Canvas Rendering Awareness

- 遊戲可能有多層座標系統（本地座標、世界座標、螢幕像素等）。**實作前先確認你操作的是哪一層，是否需要轉換。**
- 深度排序（draw order）對等軸測遊戲至關重要。新增可視元素時確認它在正確的繪製層級。
- 新增視覺元素時，優先查看專案是否有 PNG sprite 資源可用，再考慮程序化繪製。

## Constants Pattern

遊戲常數通常也是 IIFE 格式，放在獨立的 constants 目錄：

```javascript
window.ConstantName = (function() {
  var DATA = { };
  return { DATA: DATA };
})();
```

新增常數檔時，也要加到 `index.html`（在 renderer 模組之前）。

## Implementation Checklist

1. **讀 CLAUDE.md** — 專案特定的架構事實、陷阱、開發規則都在裡面
2. **讀相關文件** — 查閱專案 docs/ 取得座標系統、按鍵綁定、API 等資訊
3. **讀現有程式碼** — 保持與現有模組一致的風格和 pattern
4. **不隨意改架構** — 不能改變已定義的座標系統、模組職責、常數命名規則
5. **數字從 code 撈** — 不要憑記憶猜常數值，永遠讀 code 確認
