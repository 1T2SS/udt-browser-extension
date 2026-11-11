# Universal Browser Extension & Userscript Template (`udt-browser-extension`)

> **AI-First Vibe Coding Preset** for Chrome, Firefox, Edge, and Safari **Manifest V3** extensions (**WXT Framework** / **Plasmo**) and modern TypeScript **Userscripts** (Violentmonkey / Tampermonkey).
> Includes built-in **Chrome DevTools MCP** integration and Shadow DOM isolation rules.

---

## Quick Start (30 Seconds)

### Option A: From GitHub
1. Click **"Use this template" → "Create a new repository"** on GitHub.
2. Clone your new repo and open it in any AI coding agent.
3. Describe the browser extension, sidepanel tool, overlay, or userscript you want to build!

### Option B: From Local UDT Hub
```powershell
.\scripts\new-project.ps1 -Preset browser-extension -Target "E:\Projects\MyExtension" -InitGit
```

---

## Key Features
- **Modern WXT Manifest V3 Architecture**: Uses **WXT** (`wxt.dev`) — the Vite-powered next-gen extension framework with instant HMR, automatic `manifest.json` generation, and cross-browser packaging for Chrome MV3 and Firefox.
- **Shadow DOM Content-Script Isolation**: Prevents host-page CSS from breaking your injected UI (and prevents your Tailwind styles from breaking the host website!).
- **Proactive Chrome DevTools MCP Integration**: Checks for **Chrome DevTools MCP** via [`.ai/MCP_AND_TOOLS.md`](./.ai/MCP_AND_TOOLS.md) so the AI can inspect content script injection, CSP errors, and extension service workers directly.
