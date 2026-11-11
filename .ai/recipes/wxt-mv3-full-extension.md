# Recipe: WXT Manifest V3 Browser Extension (Chrome, Firefox, Edge)

## When to Use
Use this recipe when building a modern Manifest V3 browser extension with popup, sidepanel, options page, background service worker, and Shadow-DOM-isolated content scripts.

## 1. Official Scaffold Commands
```powershell
bunx wxt@latest init .
bun install
bun add @webext-core/messaging zod lucide-react
```

## 2. Recommended Directory Layout (`entrypoints/`)
WXT automatically generates `manifest.json` from the files inside `entrypoints/`:
```text
├── entrypoints/
│   ├── background.ts         # MV3 Service Worker (top-level listeners + messaging)
│   ├── content/              # Content script injected into matching websites
│   │   ├── index.tsx         # defineContentScript({ cssInjectionMode: 'ui' }) + createShadowRootUi
│   │   └── style.css         # Scoped exclusively inside the Shadow DOM!
│   ├── popup/                # Toolbar popup UI
│   └── sidepanel/            # (Optional) Chrome Side Panel UI
├── components/               # Shared UI components
├── utils/
│   ├── messaging.ts          # @webext-core/messaging protocol map
│   └── storage.ts            # wxt/storage typed storage items
└── wxt.config.ts             # Permissions & manifest metadata
```

## 3. Verification & Packaging Commands
- **Typecheck & Generate WXT Types**: `bun run postinstall && bun x tsc --noEmit`
- **Production Build (Chrome MV3)**: `bun run build`
- **Production Build (Firefox)**: `bun run build:firefox`
- **Create Store-Ready `.zip`**: `bun run zip`
