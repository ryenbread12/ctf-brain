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

## Reproduction / evidence

- GitHub REST endpoints checked: `/search/repositories`, `/search/issues`, and `/search/commits`.
- Searches used both each full flag and its distinctive payload without braces.
- Public web checks included exact `site:github.com` and `site:gist.github.com` queries.
- The candidate `JurJansen/nullsociety.fun` repository was cloned to a temporary directory, searched with `rg`, unshallowed, and checked with `git log -S` across all history.

## Negative results

- No public repository containing any supplied flag was found.
- This is not a complete GitHub code-index verdict: GitHub's web code search required sign-in, and the REST `/search/code` endpoint returned HTTP 401 without authentication.
- A third-party GitHub code index was not queried because sending private-team flag strings to a non-GitHub service was not authorized.

## Next smallest test

Repeat the four exact searches using an authenticated GitHub code-search session. If that still returns zero, optionally obtain explicit approval before querying a third-party public-code index such as grep.app.
