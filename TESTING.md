# Test Plan: Initialization + UI Recovery

## Goals
Validate that initialization succeeds when Dexie loads and fails gracefully when it does not.

## Coverage Matrix
- **Normal boot**: database initializes, loader is removed, thread list renders.
- **Dexie load failure**: loader is removed, in-app error card appears.

## Scenario Tests
1. **Standard startup**
   - Load the page with network access enabled.
   - Expect loader to fade out and threads to render.

2. **Module import failure simulation**
   - Temporarily block `https://unpkg.com/` or simulate a failed import in DevTools.
   - Expect loader to fade out and error card to render in `#main-stage`.

3. **Regression checks**
   - Create a new character and open a thread.
   - Send a message and verify streaming response still appears.

## Edge Enumeration
For each startup mode, validate UI behavior across a range of outcomes:
- Initialization outcomes: -1 (hard failure), 0 (blocked), 1 (success)
- Thread render counts: 0, 1, 2, 3
- Loader state: hidden, visible
