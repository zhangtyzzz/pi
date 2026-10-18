# Fork automation

This repository follows `earendil-works/pi` without application-code changes.
The fork owns `.github/workflows/`; upstream workflow changes require manual
review instead of automatic installation.

- **Sync upstream:** daily at 23:33 Asia/Singapore (15:33 UTC), or manually from
  Actions on `main`. Merge upstream `main`, preserving the complete current
  workflow directory, including deleted workflows. Code conflicts stop the run
  before any push. Pushes never use force; a concurrent update fails safely.
- **CI:** keep upstream build, check, tests and MCP conformance. A successful bot
  sync explicitly dispatches CI because `GITHUB_TOKEN` pushes do not trigger
  another push workflow. Validation happens after synchronization; a failing CI
  reports the upstream regression without rolling back the mirrored commits.
- **Environments / pi-env daemons:** retain cross-platform environment tests and
  their reusable daemon build. They run on matching push/PR events or manually;
  bot sync does not independently dispatch this optional workflow.
- **npm audit:** retain production dependency and registry-signature checks;
  daily at 23:53 Asia/Singapore (15:53 UTC), or manually. New advisories can fail
  a scan even when the fork's code has not changed. Never hide failures or apply
  unreviewed `npm audit fix --force` changes just to get a green result.

Removed upstream-only workflows: contributor approval, PR/issue gates, issue
triage and analysis, closed-issue labels, model catalog production publication,
Nix catalog-pin/stable-branch maintenance, and the combined binary/npm/R2 release
pipeline. These depend on upstream staff, accounts, credentials, release policy
or infrastructure. Get official packages and binaries from upstream; this fork
does not republish them. Application source and packaging scripts remain intact.

No PAT, model key, PR approval permission or subscription is needed. The sync job
alone requests `contents: write` and `actions: write`; the repository's default
token permissions remain read-only. Historical failed runs remain in Actions.
GitHub schedules may be delayed and can be disabled after prolonged inactivity
in a public repository. Check the workflow state if scheduled runs stop.
