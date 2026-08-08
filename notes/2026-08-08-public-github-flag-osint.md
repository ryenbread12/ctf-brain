# Public GitHub flag OSINT

Status: investigated

## Team-ledger reconciliation

After refreshing the nested team repository on 2026-08-08, five exact accepted
values were present on team `origin/main` but absent from the user's latest
terminal transcript:

- `cube{complementary_controls_verified}`
- `cube{authorized_for_internal_ACCESS}`
- `cube{h1st0ry_1s_wr1tt3n_by_th3_cl13nt}`
- `C2{bingo_bankrupt}`
- `C2{the_hand_never_lies}`

The same transcript newly confirmed `C2{house_always_folds}` as already
credited, and confirmed the lowercase `cube{mach_was_here}` and
`cube{my_crime_is_that_of_curiosity}` wrappers plus literal `C2{example}` as
accepted or already credited. The private nested tracker was reconciled to 22
accepted or already credited values. Rejected variants remain segregated from
the submission list.

## Goal

Find public GitHub repositories containing any of the four accepted flag strings supplied on 2026-08-08, on the theory that nearby files may expose other challenge material.

## Verified facts

- Exact-string public web searches returned no indexed GitHub or GitHub Gist matches for:
  - `C2{house_always_folds}`
  - `C2{the_house_doesnt_always_win}`
  - `C2{welcome_to_nullsociety}`
  - `c2{rules_are_rules}`
- GitHub REST searches of repositories, issues/pull requests, and commit messages returned zero exact-string results for all four flags.
- Payload-only searches (without the `C2{...}` wrapper) also returned no verified GitHub repository, issue, or commit hit.
- Broader GitHub repository searches for `C2Casino`, `Injection Roulette`, `0x3fCube`, and `NullSociety CTF` returned no relevant repository.
- `Signal Hi-Lo` returned two unrelated repositories; neither was CTF-related.
- The only repository returned for `nullsociety` was [JurJansen/nullsociety.fun](https://github.com/JurJansen/nullsociety.fun), a small static site described as “NullSociety official website.” Its current tree and all nine commits contained no `C2{`, supplied flag payload, roulette, Hi-Lo, casino, or flag reference. It appears unrelated to this CTF.
- A broader Sourcegraph scan of public GitHub code searched case-insensitively for flag-shaped `C2{...}` and `cube{...}` values, including forks and archived repositories. Anchored, underscore-bearing searches returned no candidate flag.
- The broad scan found [sajjadium/ctf-archives/ctfs/CubeCTF/2025](https://github.com/sajjadium/ctf-archives/tree/main/ctfs/CubeCTF/2025). Its four `cube{...}` matches were all format examples or placeholders, not accepted flags:
  - `cube{[0-9a-z_]+}`
  - `cube{LAT,LON}`
  - `cube{12.34,-56.78}`
  - `cube{insert_password_here}`
- Other broad-prefix matches were verified false positives, including CSS class `.c2{`, C++ initialization such as `c2{refcol}`, generated IDs such as `C2{x_index}{y_index}`, and identifiers containing `dataCube{...}`.
- Exact Base32 searches also returned zero GitHub repository, issue/PR, commit-message, or ordinary public-web hits. Both padded and unpadded variants were checked:
  - `IMZHW2DPOVZWKX3BNR3WC6LTL5TG63DEON6Q====`
  - `IMZHW5DIMVPWQ33VONSV6ZDPMVZW45C7MFWHOYLZONPXO2LOPU======`
  - `IMZHW53FNRRW63LFL52G6X3OOVWGY43PMNUWK5DZPU======`
  - `MMZHW4TVNRSXGX3BOJSV64TVNRSXG7I=`
- Base64 was checked in both padded and unpadded forms and returned zero results on the same GitHub surfaces and public web index. These values contain no `+` or `/`, so their Base64URL forms are identical to the unpadded forms:
  - `QzJ7aG91c2VfYWx3YXlzX2ZvbGRzfQ==`
  - `QzJ7dGhlX2hvdXNlX2RvZXNudF9hbHdheXNfd2lufQ==`
  - `QzJ7d2VsY29tZV90b19udWxsc29jaWV0eX0=`
  - `YzJ7cnVsZXNfYXJlX3J1bGVzfQ==`

## Reproduction / evidence

- GitHub REST endpoints checked: `/search/repositories`, `/search/issues`, and `/search/commits`.
- Searches used both each full flag and its distinctive payload without braces.
- Public web checks included exact `site:github.com` and `site:gist.github.com` queries.
- Sourcegraph searches used generic, non-secret prefixes only. Filters included forks and archived repositories, excluded vendor/minified paths during focused passes, and required flag-like payload characters/lengths.
- The exact Base32 strings were searched only through GitHub's public REST search surfaces and the ordinary web search index. They were not sent to Sourcegraph because the encodings are reversible private-team data and that destination was not explicitly authorized.
- The candidate `JurJansen/nullsociety.fun` repository was cloned to a temporary directory, searched with `rg`, unshallowed, and checked with `git log -S` across all history.

## Negative results

- No public repository containing any supplied flag was found.
- This is not a complete GitHub code-index verdict: GitHub's web code search required sign-in, and the REST `/search/code` endpoint returned HTTP 401 without authentication.
- A third-party GitHub code index was not queried because sending private-team flag strings to a non-GitHub service was not authorized.
- Generic-prefix third-party searches were later authorized in scope. grep.app was blocked by a Vercel browser checkpoint; Sourcegraph completed the usable public-index searches described above.

## Next smallest test

Repeat the four exact searches using an authenticated GitHub code-search session. If that still returns zero, optionally obtain explicit approval before querying a third-party public-code index such as grep.app.

## Follow-up: historical repository sweep

- The public `c2society` account exposes five repositories: `lockpicking`,
  `building-ctf-challenges`, `CTFd`, `CTFd-Docker-Challenges`, and
  `dc702.github.io`. All available branches, pull-request refs, current trees,
  and full local histories were searched for the probe identifiers, NullSociety
  terms, C2Casino terms, and the supplied flag payloads; no relevant match was
  found.
- `c2society/dc702.github.io` is an unrelated 2022 DC702 landing page. The
  CTF-building repository documents the older `TPZ{...}` event format and its
  example flags, not the current C2Casino material.
- The current CubeCTF subtree in [sajjadium/ctf-archives](https://github.com/sajjadium/ctf-archives/tree/main/ctfs/CubeCTF/2025), all 12 embedded challenge ZIPs, and the archive's full commit history contained only the previously recorded format examples/placeholders. No supplied flag, probe identifier, or related casino source appeared.
- The public `thetinyclaw` repository listing does not expose the historical
  `defcon-cube-ctf` remote; the available local clone was searched across all
  refs and its history, with no probe identifier or derived candidate present.
- GitHub's authenticated code search remains the only direct GitHub surface not
  available in this environment. Exact probe-string confirmation against a
  third-party code index remains intentionally unperformed.

## Follow-up: lineage and writeup sweep

- The only public fork of `c2society/building-ctf-challenges`,
  [TheRealKraytonian/building-ctf-challenges](https://github.com/TheRealKraytonian/building-ctf-challenges),
  is a three-commit template mirror. Its flags are `TPZ{FLAGZZZZ...}`
  placeholders and it contains no NullSociety, Casino, probe, or supplied-flag
  material.
- Public GitHub commit and issue/PR searches returned zero hits for the probe
  prefix, probe identifier, derived candidate, `C2Casino`,
  `hello.nullsociety.cc`, `nothumansociety`, and the supplied flag payloads.
- Public repository and user alias searches found no `c2casino`, `0x3fcube`, or
  `nothumansociety` repository/user. The two newer `NullSociety` identities
  (`NullSocietys` and `nullsocietyco`) expose no public repositories.
- `CubeMastery/CubeCTF-2025` and two other CubeCTF-labelled repositories were
  checked as possible writeup leaks. They contain unrelated `cube{...}` flags
  and no C2Casino/NullSociety material. The historically linked
  `isaidnocookies/Crypto-Converter` repository is an unrelated Qt
  cryptocurrency/QR utility; its full history contains no relevant strings.

## Follow-up: archive, certificate, DNS, and official-source mega-sweep

- Certificate Transparency was enumerated through both `crt.sh` and Cert
  Spotter. The only explicit names were the already-known roots and public
  `api`, `terminal`, `hello`, and `portal` hosts plus wildcard certificates.
  Wildcard coverage means CT cannot enumerate every historical host.
- Passive hostname sources added only `www.api.0x3fcube.com` and
  `www.terminal.0x3fcube.com`. They were not actively probed because they are
  not separately named in the current scope document. Other passive-DNS APIs
  were empty, unavailable, or required authentication.
- Every distinct accessible Wayback body for the three official story roots
  was inspected. The 2023, 2024, and 2025 0x3fCube pages, archived terminal
  client, and archived QuestionableCorp pages contained only the already-known
  historical breadcrumbs and accepted QCorp values. Common Crawl reproduced
  the same 2024 page; Arquivo returned no useful capture.
- Current public HTML, JavaScript, robots files, linked first-party modules,
  and apparent source-map paths were scanned across 0x3fCube,
  QuestionableCorp, NullSociety, and C2Casino. Automated Base32/Base64 decoding
  found only the already-known QCorp privacy token, the DEF CON 32 promotional
  sentence, and the terminal help example. No new encoded flag was found.
- Official DEF CON 31, 32, 33, and 34 contest pages and forum branches were
  compared. DC31 exposes the original four-post Cube section; DC33 introduces
  the Aperture/QuestionableCorp storyline; DC34 continues it. No public
  post-event flag list or write-up was present.
- The public 2023 Cube PNG was inspected visually and structurally. It has only
  standard PNG chunks, no trailing payload or text metadata, and no tested
  `cube{`, `C2{`, URL, or flag prefix in common channel/bit-plane layouts. The
  official DC33 and DC34 WebP artwork is byte-identical between the two years
  and contains one normal VP8 chunk with no metadata or appended payload.
- The organizer's official DEF CON Mastodon account reports zero public posts.
- Generic public GitHub commit searches and the only challenge-era public
  participant repository that looked temporally relevant produced no Cube,
  Aperture, NullSociety, CTF, or flag artifact. GitHub code search still
  requires sign-in in the available browser session.

## Scoreboard reconciliation finding

- The public leaderboard currently reports **217 total challenges** and a
  maximum public solve count of **29**. The team account is publicly credited
  with **23** solves, while the private tracker contains **22** distinct
  accepted/already-credited values.
- A full audit of every flag-shaped value in the current vault and all Git
  tracker revisions found no omitted accepted value: every extra historical
  string was already documented as rejected, a wrapper mismatch, a placeholder,
  or an example. Therefore one credited solve remains unnamed in the vault.
- The next deterministic step is account-specific and read-only: retain the
  authenticated terminal output of `progress`, then `inbox` and `hint` if
  needed, and map its solved challenge names to the 22-entry tracker. Public
  archives cannot disclose that account-specific mapping.
