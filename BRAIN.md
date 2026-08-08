# Team CTF Brain

Last updated: 2026-08-08

## Current state

- **Primary next step:** use the 0x3fCube terminal’s `progress`, `hint`, and `inbox` commands to select the next unsolved challenge.
- **Rule of engagement:** work one target at a time; record evidence before changing course.

## 0x3fCube continuity — team-wide context

- DEF CON 33's archived terminal exposed `submit_base64`, badge registration, progress, hints, and message flows. Its messages directed players to QuestionableCorp and to ask for an employee badge.
- The live QuestionableCorp site says it was formerly Aperture Inc. Its current
  homepage-comment and privacy-page flags, plus the equivalent two values from
  the archived 2025 site, were all user-confirmed accepted on 2026-08-08. Exact
  values and evidence are in the private flag tracker.
- Current QuestionableCorp content links into `nullsociety.cc`, confirming a direct story bridge. See `notes/2026-08-08-0x3fcube-history.md` before testing.
- urlscan independently confirms the historical terminal response header
  `Flag: Jus7Wa1tingOnY0urRespons3`, but the wrapped candidate was rejected by
  the current terminal. Treat it as historical evidence, not a solve.

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
