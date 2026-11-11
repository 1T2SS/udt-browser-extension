# Recipe: Modern TypeScript Userscript (`Vite` + `vite-plugin-monkey`)

## When to Use
Use this recipe when building a lightweight userscript for **Violentmonkey** or **Tampermonkey** with full TypeScript, npm package bundling, and instant browser hot-reload.

## 1. Official Scaffold Commands
```powershell
bun create monkey .
bun install
```

## 2. Configure Userscript Metadata (`vite.config.ts`)
Define your `@match` URLs and `@grant` permissions inside `vite.config.ts` via `monkey({ userscript: { ... } })`.

## 3. Verification Commands
- **Typecheck**: `bun x tsc --noEmit`
- **Build Single `.user.js` File**: `bun run build` (outputs `dist/*.user.js` ready for one-click install in Violentmonkey/Tampermonkey).
