# Signal Hi-Lo

Status: investigating

## Confirmed API surface

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/api/hilo/state` | Balance, reputation, current game, milestones, stats |
| POST | `/api/hilo/start` | Starts a game with `{ "bet": <valid amount> }` |
| POST | `/api/hilo/guess` | Submits `{ "guess": "higher" | "lower" }` |
| POST | `/api/hilo/cashout` | Ends a winning active game |
| GET | `/api/settings/bets` | Valid bets and current restrictions |

## Frontend observations

- API uses same-origin session cookies (`credentials: 'include'`). Do not save or share cookies.
- Response `nextCard` is the card displayed for the following turn.
- The frontend displays a flag only if a response contains `flags` or the legacy `flag` field.
- Source comments reference a “leaked deck” and demo mode, but no corresponding usable endpoint has been verified.

## Negative results / avoid repeating

- `/api/hilo/deck` returned 404.
- Unauthenticated or guessed routes are not evidence of a valid path.
- Race-style duplicate guesses were inconsistent and may end the active game; do not spam them.

## Next evidence needed

Find an actual game response or source reference that carries a flag/deck path, not a guessed endpoint.

## Full handoff

See [2026-08-07-signal-hilo-full-log.md](2026-08-07-signal-hilo-full-log.md) for the complete API contract, sanitized response examples, race-test results, and dead ends from today.
