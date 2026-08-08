# Team CTF Brain

This Git repository is the shared source of truth for the team’s CTF work.

## At the start of every Codex session

1. Run `./brain sync` to pull the team’s latest notes.
2. Read `BRAIN.md`, then `notes/INDEX.md` and any notes relevant to the current target.
2. Confirm the active target before testing anything. Do not switch challenges without recording why.

## While working

- Record verified observations, exact endpoints, response fields, and reproducible steps.
- Use `./brain note "short title" "what was confirmed"` for a quick fact, or create a structured note from `notes/_template.md`.
- Mark hypotheses as `UNVERIFIED`; do not promote them to `BRAIN.md` until tested.
- Do not commit cookies, access tokens, API keys, VM credentials, SSH material, personal data, HAR exports, or large binary files.
- Keep request volume low and follow the CTF’s rules. No brute-force or destructive activity.
- Put ZIPs, screenshots, HAR files, and other large artifacts in the shared Google Drive folder, then add a link and short description in the relevant Markdown note.

## Before ending a session

1. Update the target's note and `notes/INDEX.md`.
2. Update `BRAIN.md` if the team-wide current state changed.
3. Run `./brain publish "brief summary of what changed"` to commit, sync, and push the notes.
