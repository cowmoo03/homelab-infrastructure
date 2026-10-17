# Public Documentation Rules

This repository is a sanitized technical portfolio. Maintain useful design details while keeping personal identities and operational access information private.

## Before Publishing

1. Use role labels such as `User1`, `User2`, `StorageUser`, and `Administrator` instead of personal names or actual account names.
2. Remove actual LAN/public/VPN addresses, tailnet hostnames, email addresses, MAC addresses, serial numbers, WWNs, and other identifying infrastructure values.
3. Never commit passwords, password hashes, tokens, private keys, certificates with identifying details, webhook URLs, environment secrets, or credential-bearing connection strings.
4. Keep raw configuration exports, terminal captures, inventory exports, backups, screenshots, and the private project handoff outside this repository. Publish only reviewed summaries or deliberately sanitized examples.
5. Review the complete staged diff, filenames, links, image metadata, and commit message. Ignore patterns are only a precaution: they do not protect already tracked files or detect secrets inside Markdown.
6. Label examples as examples. Distinguish completed work, latest reports, items awaiting live verification, and proposed changes. Add a review date to state summaries.
7. Use anonymous commit attribution where appropriate and GitHub's private/no-reply email setting. Repository ownership remains visible through the GitHub account.

## If Information Was Already Published

Remove it from current files promptly. Check earlier revisions and other repository surfaces. Rotate any exposed credential before relying on cleanup. A coordinated history rewrite may be needed; it changes commit IDs and requires existing clones to resynchronize. Old URLs, forks, caches, and third-party copies may persist even after rewriting a branch.

Do not put the sensitive value into a public issue, audit report, commit message, or example while describing its removal.
