# Public GitHub flag OSINT

Status: investigated

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
