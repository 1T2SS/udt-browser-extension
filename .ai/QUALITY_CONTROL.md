# QUALITY_CONTROL.md — Master Quality Control Sub-Agent Protocol (`browser-extension`)

> **Preset**: `browser-extension`
> **QC Persona**: **Principal Browser Extension Architect + UI/UX Isolation Director** (`master-qc-director`)
> **Mandatory Rule**: Use **ONE** Master Quality Control Sub-Agent at the end of each development round. Never spawn multiple separate review sub-agents.
> **Pass Threshold**: Overall Average **`>= 8.5 / 10.0`** AND **every individual category `>= 8.5 / 10.0`**.

---

## 1. How to Invoke the Single Master QC Check

When a browser extension or userscript round is complete:
- **Antigravity**: Define/invoke the single `master-qc-director` sub-agent (`EnableWriteTools: true`, `EnableMCPTools: true`) to audit Manifest V3 compliance, Shadow DOM isolation, Chrome DevTools inspection, and build output, writing `.ai/QC_REPORT.md`.
- **Claude Code**: Invoke `.claude/agents/master-qc-director.md`.
- **Cursor / Windsurf / Copilot / Cline**: Execute a dedicated **Master QC Director Audit Pass** using the rubric below and write `.ai/QC_REPORT.md`.

---

## 2. The 5-Pillar Zero-Trust Deduction Rubric (Starts at `10.0` Per Category)

### Category 1: Injected UI, Popup Polish & Shadow DOM Isolation (`>= 8.5 / 10`)
- **`-3.0 pts` (Automatic Fail)**: Injecting un-scoped global CSS/Tailwind directly into the host website's `document.head` without **Shadow DOM isolation** (`createShadowRootUi`), breaking host-site styles!
- **`-1.5 pts`**: Cramped, unpolished popup/sidepanel UI or missing dark/light mode adaptation.

### Category 2: Extension Mechanics, SPA Navigation & Messaging Feel (`>= 8.5 / 10`)
- **`-2.0 pts`**: Content scripts that break on Single-Page Application (SPA) route changes (e.g., YouTube/GitHub client-side navigation) because they only run once on initial load.
- **`-1.5 pts`**: Fragile untyped `chrome.runtime.sendMessage` calls without error handling or typed messaging schemas.

### Category 3: Manifest V3 Architecture & Service Worker Persistence (`>= 8.5 / 10`)
- **`-4.0 pts` (Automatic Fail)**: Using deprecated `manifest_version: 2` or storing critical state in global background service worker variables (which get wiped when MV3 service workers go idle after 30s!) instead of `chrome.storage` (`wxt/storage`).
- **`-2.0 pts`**: Registering `chrome.runtime` / `chrome.alarms` listeners asynchronously instead of synchronously at the top level of the background script.

### Category 4: Feature Completeness & Host-Site Resilience (`>= 8.5 / 10`)
- **`-2.0 pts`**: Hardcoding brittle obfuscated CSS selectors (`.css-1x2y3z`) when semantic attributes or resilient selectors exist.
- **`-1.5 pts`**: Missing user toggle/settings persistence in popup/options.

### Category 5: Minimal Permission Security, CSP & Build Packaging (`>= 8.5 / 10`)
- **`-3.0 pts` (Automatic)**: `tsc --noEmit` or `bun run build` (`wxt build`) fails.
- **`-2.0 pts`**: Over-requesting `<all_urls>` or invasive permissions when `activeTab` or narrow `host_permissions` suffice, or violating MV3 Content Security Policy (`eval` / remote script execution).

---

## 3. Auto-Refinement Protocol (Up to 3 Rounds)
If any category scores `< 8.5 / 10`, `master-qc-director` sets `qc_status: REFINEMENT_REQUIRED` in [`.ai/QC_REPORT.md`](./QC_REPORT.md). The main agent must fix all listed deductions and re-run `master-qc-director` (up to 3 rounds max) before completing the round.
