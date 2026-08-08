# Signal Hi-Lo — full investigation log

**Date:** 2026-08-07  
**Status:** unsolved; no Hi-Lo flag value was received or submitted.  
**Scope:** C2Casino Signal Hi-Lo only. This log consolidates the browser, Network, and source-review work from today. It deliberately excludes cookies, HAR files, credentials, and user-specific balances.

## 1. Confirmed API contract

All requests were same-origin JSON requests made with the browser session. The frontend uses `credentials: 'include'`; do not copy, store, or share the session cookie.

| Method | Route | Request | Confirmed response/use |
| --- | --- | --- | --- |
| `GET` | `/api/hilo/state` | none | Account/game state, milestones, stats, mode/cap fields |
| `POST` | `/api/hilo/start` | `{ "bet": <amount> }` | Starts a game and returns the first card |
| `POST` | `/api/hilo/guess` | `{ "guess": "higher" }` or `{ "guess": "lower" }` | Resolves one turn and returns `nextCard` |
| `POST` | `/api/hilo/cashout` | no meaningful payload observed | Pays the current active-game multiplier and clears the game |
| `GET` | `/api/settings/bets` | none | Server-authoritative allowed bets and restrictions |

`/api/settings/bets` returned:

```json
{
  "validBets": [1000, 5000, 10000, 25000],
  "defaultBet": 5000,
  "maxBet": 25000,
  "tier": "restricted",
  "softBanned": false,
  "hardBanned": false
}
```

Amounts are micro-units: `5000` renders as **5 C2coins** in the game.

## 2. Response shapes actually observed

### Start

Observed fields:

```json
{
  "card": { "rank": "J", "suit": "♥", "value": 11 },
  "streak": 0,
  "bet": 5000,
  "env": "prod",
  "multiplier": 0,
  "newBalance": 2437200,
  "demoMode": false,
  "status": 0
}
```

The card value is authoritative for the next direction choice. There is no deck identifier, seed, nonce, or requested-deck echo in the response.

### Guess

Successful example:

```json
{
  "result": "correct",
  "nextCard": { "rank": "4", "suit": "♠", "value": 4 },
  "streak": 1,
  "multiplier": 1.1,
  "newBalance": 2437200,
  "status": 0
}
```

- `nextCard` is the card shown for the **following** guess.
- Ties lose (shown in the UI paytable).
- Normal responses observed only `status: 0`; none included `flag` or `flags`.

### State

Relevant shape:

```json
{
  "balance": 0,
  "reputation": 0,
  "activeGame": {
    "currentCard": { "rank": "8", "suit": "♥", "value": 8 },
    "streak": 2,
    "bet": 5000,
    "multiplier": 1.1
  },
  "streakMilestones": [],
  "stats": {},
  "demoMode": false,
  "capped": false,
  "winLimit": 50000000,
  "net": 0,
  "status": 0
}
```

`activeGame.currentCard` is the server-side source of truth after manual requests. If the UI and `/state` disagree, refresh the game iframe rather than sending more guesses.

### Cashout

Observed successful result:

```json
{
  "result": "cashout",
  "streak": 1,
  "multiplier": 1.1,
  "bet": 5000,
  "payout": 5500,
  "net": 500,
  "newBalance": 2428700,
  "status": 0
}
```

## 3. Frontend and source observations

- The game iframe source is `/games/signal-hilo/index.html`, with `game.js` and `/js/gameStatus.js` loaded.
- `gameStatus.js` only normalizes platform response status codes into toasts:
  - `0`: success
  - `1`: error
  - `2`: rate limited
  - `3`: suspicious activity
- The game frontend calls a flag renderer equivalent to:

```js
const earned = data.flags || (data.flag ? [{ code: data.flag, message: data.flagMessage }] : []);
```

  Therefore, flags must be delivered by a server response; changing browser UI state cannot create a valid flag.
- Comments/source text mention two possible award conditions:
  1. a **leaked deck** condition; and
  2. a **maximum-winnings / demo mode** condition.

  No response during normal game play delivered either corresponding flag.
- The visible milestone table was: `1 → 1.1x`, `5 → 1.3x`, `10 → 1.5x`, `15 → 1.8x`, `20 → 2x`, `25 → 2.8x`, `30 → 3.5x`, `40 → 7x`, `46+ → 10x`.

## 4. Tested negative results

| Test | Outcome | Conclusion |
| --- | --- | --- |
| `GET /api/hilo/deck` | `404` HTML page | Not a public deck endpoint |
| `OPTIONS /api/hilo/deck` | `404` | Same conclusion |
| Supplying a `deck` array alongside a valid `/start` request | Server began a game with an unrelated random card and did not echo/use the supplied deck | Extra body field is ignored; not client-selected deck control |
| Arbitrary guessed deck/debug/peek paths | `404` or no supporting evidence | Do not continue endpoint guessing/brute force |
| Browser storage inspection | Empty local/session storage; auth is cookie-backed | No client-side deck/seed recovered |

## 5. Duplicate-request race observation

Two concurrent `POST /api/hilo/guess` calls can sometimes both return `200` and each can report a valid-looking outcome, with different `nextCard` values. Example tests showed:

- on one high card, two identical `lower` requests both said `correct` but returned different next cards;
- on a low card, one concurrent `higher` request said `correct` while the concurrent `lower` request said `loss`;
- after duplicate guess activity, `/state` was required to learn which mutation actually remained;
- two concurrent cashouts yielded one successful cashout and one `400 { "error": "No active game.", "status": 1 }`.

**Conclusion:** there is nondeterministic/non-atomic session behavior under duplicate guesses, but it is not a reliable exploit or a safe way to reach a flag. It can end a round and reduce reputation. Do not repeat it without a challenge-specific reason and a low-risk test plan.

## 6. Hi-Lo outcome today

- No Hi-Lo flag string was observed in API responses, UI, Network records, or source snippets.
- The C2Casino bot-gate flag solved today is a **separate challenge**, not a Hi-Lo result:

```text
C2{b0t_g4t3_cl3ar3d_h4ck3r_d3t3ct3d}
```

- The probe ciphertext below is also **not** a Hi-Lo flag and its derived plaintext candidate was rejected:

```text
2d47171c1f0e0a071111012b2f140303022c29502d173e5a3b1d083e073a09330e1f3a161c300a1f311d0a11
```

## 7. Best next step

Do not gamble or enumerate endpoints further. The remaining intended path needs one of:

1. challenge-specific source/artifact that maps the `leaked deck` condition to a real route or input; or
2. a verified route/response that exposes a deck or flag field; or
3. a controlled way to reach the server-side win cap without random betting.

When new evidence arrives, record the exact route, method, body, status, and response fields here before attempting state-changing requests.
