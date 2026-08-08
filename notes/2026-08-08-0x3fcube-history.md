# 0x3fCube historical continuity

Status: investigated

## Goal

Determine whether the DEF CON 33 (2025) `?Cube` contest exposes mechanics or story elements that carry into the current event.

This is supporting OSINT only. The active target remains Signal Hi-Lo.

## Verified facts

- DEF CON 33 described `?Cube` as an Aperture Inc. contest spanning physical security, web exploitation, cryptography, and other puzzles. Point requirements applied after the first day.
- The archived 2025 terminal used account, badge-registration, flag-submission, progress, hints, and message APIs.
- Its client exposed both `submit` and `submit_base64 <base64-encoded string>` commands. `submit_base64` sent `{flag, base64: true}` to `POST /challenge/submit`.
- The archived message list told players to ask for an employee badge and welcomed them to `www.questionablecorp.cc`.
- The current QuestionableCorp site says it was formerly Aperture Inc., making it a direct story continuation rather than an unrelated CTF archive.
- QuestionableCorp's current home-page source contains the unverified candidate `cube{low_hanging_fruits!}` in an HTML comment.
- The current privacy page ends with Base32 text `MN2WEZL3MRXWG5LNMVXHI427MZXXEX3QOJUXMYLDPEQSCIL5`, which decodes to the unverified candidate `cube{documents_for_privacy!!!}`.
- The July 13, 2026 customer blog post contains a Null Society warning in an HTML comment and links six customer names to paths on `nullsociety.cc`.
- QuestionableCorp's `robots.txt` disallows `/portal` and `/files`; an unauthenticated GET of `/files` returns a 401 page.
- A DEF CON 32 (2024) archived custom 404 included an HTML-comment starter clue, `f: thanksforstoppingby`, and directed users to the terminal to submit flags. Hidden page-source clues are therefore a recurring pattern.
- urlscan's public 2023 and 2024 terminal captures both preserve the original
  response header `Flag: Jus7Wa1tingOnY0urRespons3`. The resulting candidate
  `cube{Jus7Wa1tingOnY0urRespons3}` was user-confirmed rejected by the current
  terminal on 2026-08-08, so it is historical evidence rather than a solve.
- The official DEF CON 32 contest listing contains Base32
  `H4QEG5LCMUQEAICEMVTGG33OEAZTEICSMVQWI6JAORXSAZLOM5QWOZJ7`, which decodes
  to `? Cube @ Defcon 32 Ready to engage?`. It is not flag-shaped.
- The DEF CON 31 homepage's hidden comment points to a TXT record at
  `twitter.0x3fcube.com`; its current value is only
  `https://twitter.com/0x3fcube`.
- The current DEF CON 34 contest description retains the Aperture Inc./`?Cube` storyline and says the contest covers physical security, web apps, communications systems, and cryptography.
- The public DEF CON 32 `? Cube` forum subforum contains four announcement topics from June 2024, all with zero replies; it does not expose a post-event flag list or write-up.
- The DEF CON 32 contest page links the 2024 event directly to `0x3fcube.com` and repeats the same Base32 promotional breadcrumb; the linked discussion thread contains no additional flag-shaped text.
- A fresh Cert Spotter inventory for the three known roots exposed only the already-known names: `api.0x3fcube.com`, `terminal.0x3fcube.com`, `hello.nullsociety.cc`, `portal.questionablecorp.cc`, and wildcard/root certificates. No new public hostname was found.
- A fresh public-source search for `0x3fcube`, `questionablecorp`, `nullsociety`, and the historical header string found no indexed repository or source hit. GitHub code search itself remains login-gated/rate-limited, so this is not proof that no private or unindexed mirror exists.

Neither `cube{...}` candidate above was submitted or confirmed accepted during this research.

## Reproduction / evidence

- DEF CON 33 contest listing: <https://defcon.outel.org/dcwp/dc33/activities/c-list/>
- Official site: <https://0x3fcube.com/>
- Archived DEF CON 33 terminal: <https://web.archive.org/web/20250809022638/https://terminal.0x3fcube.com/>
- Archived terminal client: <https://web.archive.org/web/20250809022638id_/https://terminal.0x3fcube.com/js/javascript.js>
- Archived DEF CON 32 custom 404: <https://web.archive.org/web/20240813213820id_/https://0x3fcube.com/will>
- Public 2023 terminal scan: <https://urlscan.io/result/e236bc27-1c78-482e-b716-544066d4bac9/>
- Public 2024 terminal scan: <https://urlscan.io/result/44341cbf-cbfc-4496-8074-7c8475f7c87a/>
- DEF CON 32 contest listing: <https://defcon.outel.org/dcwp/dc32/activities/c-list/>
- Current QuestionableCorp site: <https://questionablecorp.cc/>
- Privacy page: <https://questionablecorp.cc/privacy>
- Customer bridge post: <https://questionablecorp.cc/blog/our-customers-a-testament-to-excellence>
- Trust center: <https://questionablecorp.cc/trust-center>
- Current DEF CON 34 listing: <https://defcon.outel.org/dcwp/dc34/activities/c-list/>

Base32 reproduction:

```python
import base64

s = "MN2WEZL3MRXWG5LNMVXHI427MZXXEX3QOJUXMYLDPEQSCIL5"
print(base64.b32decode(s + "=" * ((8 - len(s) % 8) % 8)).decode())
```

## Hypotheses (unverified)

- Current challenge authors likely continue using hidden HTML comments, encoded text, corporate portal/document flows, badge identity, and cross-domain story pivots.
- The current terminal may retain a Base64 submission mode even if it is not shown in help.
- The six `nullsociety.cc/<customer-domain>` links may be an intended discovery map for current challenges.
- `cube{...}` and `C2{...}` may be accepted by different components or challenge generations; do not assume either candidate belongs to the current C2 scoreboard without submission evidence.

## Negative results

- Public search did not reveal a reliable challenge-name/flag list or write-up for DEF CON 33 `?Cube`.
- The public 0x3fCube Mastodon account currently reports zero posts.
- Internet Archive indexing did not return a useful QuestionableCorp URL inventory in this session.
- Wayback's terminal inventory contains only the root page and static CSS,
  JavaScript, and image assets; it contains no captured hint, message,
  progress, or challenge API response.
- urlscan response-body downloads require authentication, and searches for the
  historical JavaScript SHA-256 values found no public mirror.
- The six linked Null Society customer paths returned no useful body in a simple unauthenticated fetch.
- The legacy `twitter.0x3fcube.com` Wayback/API path was unavailable during this sweep (archive request blocked/rate-limited), and public search returned no indexed historical posts. Its known current TXT value remains only the public profile URL.

## Next smallest test

While authenticated to the authorized CTF terminal, run only the read-only `progress`, help, hints, and message/inbox commands and check whether `submit_base64` is still recognized. Separately, ask the team whether it wants either unverified `cube{...}` candidate submitted.
