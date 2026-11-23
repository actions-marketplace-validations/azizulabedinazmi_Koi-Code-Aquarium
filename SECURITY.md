# Security policy

## Supported versions

| Version | Supported |
| --- | --- |
| `v1` (latest `v1.x`) | ✅ |
| older tags | ❌ |

## Reporting a vulnerability

Please **don't** open a public issue for security problems.
Report them privately through [GitHub's private vulnerability reporting](../../security/advisories/new).
You'll get a reply within a few days.

## How the project handles tokens

- The Action uses the workflow's built-in `GITHUB_TOKEN` by default, which reaches only the repository running it, for that one job.
- The generator only reads **public** data: your public repositories' languages and git-koi's public activity snapshot.
- git-koi is downloaded from its official GitHub release, and its SHA-256 checksum is checked before it runs.
