# RESEARCH_PLAYBOOK.md — Browser Extension & Userscript Research Protocol

> **Preset**: `browser-extension`
> **Rule**: Verify Manifest V3 lifecycle rules, host-page DOM selectors, and Content Security Policy (CSP) constraints before writing injection code.

---

## 1. Host Website DOM & CSP Inspection
When building a content script or userscript targeting a specific website:
1. Check if **Chrome DevTools MCP** or the browser subagent is available to inspect the live target page's DOM structure, data attributes, and network calls.
2. Prefer stable attributes (`data-testid`, `aria-label`, semantic roles, URL patterns) over fragile obfuscated CSS class names (`.css-1a2b3c`).
3. Handle Single-Page Application (SPA) client-side navigation! Websites like YouTube, GitHub, and X change URLs via `history.pushState` without full page reloads — use WXT's `ctx.addEventListener(window, 'wxt:locationchange', ...)` or a `MutationObserver` so your content script re-mounts cleanly on route changes.

## 2. Manifest V3 Service Worker Checklist
- Register all `chrome.runtime.onInstalled`, `chrome.runtime.onMessage`, and `chrome.alarms.onAlarm` listeners synchronously at the **top level** of `entrypoints/background.ts` (never inside an async callback).
- Use `chrome.alarms` instead of `setInterval` inside background service workers.
