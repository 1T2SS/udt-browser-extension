# MCP_AND_TOOLS.md — Proactive Browser Extension Environment & MCP Setup (`browser-extension`)

> **AI Agent Instruction**: On Day-0 Bootstrap, check if `bun`/`node` and **Chrome DevTools MCP** are available. **If Chrome DevTools MCP or `bun` is missing, explain how it speeds up extension development and ask the user if you can install/configure it for them!** Record all verified tools in [`.ai/ENVIRONMENT.md`](./ENVIRONMENT.md).

---

## 1. Recommended MCP Servers & Skills to Check

| MCP Server / Skill | Purpose in `browser-extension` | How to Check & Offer Setup |
| :--- | :--- | :--- |
| **Chrome DevTools MCP** (`chrome-devtools`, `chrome-extensions`, `a11y-debugging`, `cookie-debugging`) | Inspects injected Shadow DOM content scripts, background service worker logs, CSP errors, and host-page DOM selectors live. | Check if `chrome-devtools` MCP is connected. If not, ask the user if they want help enabling Chrome DevTools MCP for live extension debugging! |
| **Fallow Static Analysis** (`fallow`) | Detects unused code and circular dependencies in TypeScript extension bundles. | Check `npx fallow --version`. |

---

## 2. Local Toolchain Doctor Checklist

Run these non-destructive checks and log the results in [`.ai/ENVIRONMENT.md`](./ENVIRONMENT.md):

```powershell
# 1. Check Bun, Node & pnpm
bun --version 2>$null
node --version 2>$null
pnpm --version 2>$null

# 2. Check Chrome / Edge Browser Installation
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\chrome.exe" -ErrorAction SilentlyContinue | Select-Object -ExpandProperty "(default)"
```

### Auto-Setup Commands (Offer to Run with User Permission if Missing)
- **Install Bun**: `winget install Oven-sh.Bun`

---

## 3. Save Verified Setup Info for Future Creations
Update [`.ai/ENVIRONMENT.md`](./ENVIRONMENT.md) (`Environment Check Status: VERIFIED`) and log any useful MV3 / Shadow DOM / CSP solutions in Section 5 of [`.ai/MEMORY.md`](./MEMORY.md).
