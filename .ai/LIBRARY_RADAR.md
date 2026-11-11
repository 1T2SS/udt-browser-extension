# LIBRARY_RADAR.md — Browser Extension & Userscript Tech Stack Guide

> **Preset**: `browser-extension`
> **Rule for AI Agents**: Always build extensions using **Manifest V3 (MV3)** via **WXT** (or **vite-plugin-monkey** for userscripts). Never generate deprecated Manifest V2 (`manifest_version: 2`) Chrome extensions.

---

## 1. Architecture Decision Tree

| Extension / Script Goal | Recommended Primary Stack | Why It Excels | Matching Recipe |
| :--- | :--- | :--- | :--- |
| **Full Chrome / Firefox / Edge Extension (Popup, Sidepanel, Background Service Worker, Content Script Overlay)** | **WXT Framework** (`wxt`) + TypeScript + React / Svelte / Solid / Vanilla + Tailwind CSS | File-based `entrypoints/`, automatic MV3 `manifest.json`, Shadow DOM UI helper (`createShadowRootUi`), 1-command `.zip` packaging | [`.ai/recipes/wxt-mv3-full-extension.md`](./recipes/wxt-mv3-full-extension.md) |
| **Lightweight Site Enhancer / Automation Script (Violentmonkey / Tampermonkey)** | **Vite + `vite-plugin-monkey`** + TypeScript | Hot-reloads userscripts directly in your browser with full npm/TypeScript bundling | [`.ai/recipes/userscript-typescript-vite.md`](./recipes/userscript-typescript-vite.md) |

---

## 2. Approved Library Radar

- **Core Extension Framework**: **WXT** (`wxt`, `@wxt-dev/module-react` / `@wxt-dev/module-svelte` / `@wxt-dev/module-solid`, `@wxt-dev/storage`).
- **Type-Safe Messaging (Content Script ↔ Background Service Worker)**:
  - **`@webext-core/messaging`** or **`@webext-core/proxy-service`** (eliminates fragile untyped `chrome.runtime.sendMessage` boilerplate!).
- **Storage**:
  - **`wxt/storage`** or `chrome.storage.local` / `chrome.storage.sync` with Zod validation.
- **Userscript Bundling**:
  - **`vite-plugin-monkey`** (auto-generates `// ==UserScript==` headers, `@grant`, and `@match` rules).

---

## 3. Banned / Deprecated by Default (DO NOT USE)

| Banned Pattern / Tool | Why It Breaks Extensions | Use This Instead |
| :--- | :--- | :--- |
| **`manifest_version: 2` (Manifest V2)** | Blocked/removed in Chrome and Chromium browsers | Always use **Manifest V3 (`manifest_version: 3`)** |
| **Storing state in global variables inside the Background Service Worker** | MV3 Service Workers terminate after ~30 seconds of inactivity, wiping all in-memory globals! | Persist all background state in **`chrome.storage.local` / `chrome.storage.session`** (`wxt/storage`) |
| **Injecting remote `<script src="https://...">` or `eval()` in MV3** | Strictly blocked by Manifest V3 Content Security Policy (CSP) | Bundle all code at build time via WXT / Vite |
| **Injecting raw un-scoped Tailwind/CSS directly into `document.head` of host websites** | Breaks the host website's layout and leaks host CSS into your extension UI | Mount content-script UIs inside an isolated **Shadow DOM** (`createShadowRootUi` in WXT) |
| **Requesting `<all_urls>` or `tabs` permission when `activeTab` suffices** | Triggers slow Chrome Web Store manual security review | Request the minimum required permissions (`activeTab`, specific `host_permissions`) |
