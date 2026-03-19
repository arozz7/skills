---
description: Apply Electron framework standards — security hardening, IPC patterns, process separation, and context isolation. Use when working on Electron main/renderer process code.
---

# Electron Framework Standards

## Security (CRITICAL)
- **Context Isolation:** MUST be `true`.
- **Node Integration:** MUST be `false` in Renderers.
- **Sandboxing:** Enable `sandbox: true` for all windows.
- **IPC:** Use `ipcMain.handle` and `ipcRenderer.invoke` for communication. Validate all IPC payloads.

## Architecture
- **Process Separation:**
  - **Main Process:** Handles OS interactions, file system, and window management.
  - **Renderer Process:** Handles UI only. Logic should be minimal.
- **Preload Scripts:** Use `contextBridge` to expose specific, limited APIs to the renderer. Never expose the full `ipcRenderer` object.

## Development Loop
- Ensure graceful handling of window closures and app-quit events.
- Handle "crashes" and "unresponsive" events in the Main process.

## 🛑 What Not To Do
- Do not use `remote` module (it is deprecated and insecure).
- Do not execute arbitrary code sent from the Renderer in the Main process.
- Do not load remote content (websites) with `nodeIntegration` enabled.

## Code Organization & Size Limits
- **Soft Limit:** 300 lines.
- **Hard Limit:** 600 lines.
- **Action:** If a file exceeds the hard limit during editing, STOP and suggest a refactor using `/tz-skills:refactoring-protocol`.
- **Single Responsibility:** Each file should export one main component/class or a cohesive set of utilities.
