# Logic Map: White Screen Remediation

## Goal
Prevent the application from stalling behind the loading overlay by ensuring the database layer initializes reliably and errors are surfaced in the UI.

## Logic Chain
1. **Observation**: The UI remains blank because the loader overlay is only removed after successful initialization.
2. **Root Cause**: The dynamic import for Dexie points at a non-module build (`dist/dexie.js`). Module parsing fails, leaving `Dexie` undefined and `Database.init()` throwing.
3. **Correction**: Load the ESM build (`dist/dexie.mjs`) and resolve the constructor from the module export.
4. **Verification**:
   - If Dexie loads, the loader is dismissed and threads render as before.
   - If Dexie fails, the loader still dismisses and a visible error card is shown instead of a white screen.

## Rigor Checks
- **Deterministic failure handling**: If the constructor cannot be resolved, initialization throws an explicit error.
- **State transition**: Loader visibility now depends on both success and failure paths, preventing UI lock.
- **No mock removal**: Default seeded data remains intact; only initialization reliability is improved.
