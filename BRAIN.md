# Team CTF Brain

Last updated: 2026-08-07

## Current state

- **Primary next step:** use the 0x3fCube terminal’s `progress`, `hint`, and `inbox` commands to select the next unsolved challenge.
- **Rule of engagement:** work one target at a time; record evidence before changing course.

## Confirmed wins

- **C2Casino bot-gate flag — submitted successfully**
  - Source: an HTML comment on C2Casino's root page.
  - Flag: `C2{b0t_g4t3_cl3ar3d_h4ck3r_d3t3ct3d}`

## Signal Hi-Lo — validated facts

- API: `POST /api/hilo/start`, `POST /api/hilo/guess`, `POST /api/hilo/cashout`; state: `GET /api/hilo/state`.
- Valid bet values are `1000`, `5000`, `10000`, `25000` (micro-units; 5000 displays as 5 C2coins).
- A `guess` response includes `nextCard`, `streak`, `multiplier`, and `status`; the revealed `nextCard` becomes the current card for the following guess.
- The frontend’s flag renderer accepts `flags` or `flag` fields. No flag field has been observed in normal Hi-Lo API responses.
- Concurrent duplicate guesses showed non-atomic session behavior, but it is unreliable and can terminate the active game. Treat it as an observation, not a repeatable path.

## Do not store here

Cookies, bearer tokens, passwords, SSH keys, live VM credentials, or exported HAR files.

