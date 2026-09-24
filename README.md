# renovate-config

Organization-wide [Renovate](https://docs.renovatebot.com/) policy for autobutler-org.

The Mend-hosted Renovate app reads `org-inherited-config.json` from this repository and
merges it under every repository's own `renovate.json`
([inherited config](https://docs.renovatebot.com/config-overview/#inherited-config)).
A repository's config overrides anything set here.

Renovate's runs, logs and job status for the org are on the
[Mend dashboard](https://developer.mend.io/github/autobutler-org).

## Policy

- `config:recommended`, which includes the Dependency Dashboard issue in each repository.
- Weekly: updates open early on Monday morning (UTC). A repository can override this per
  package, as iac does for its quark image.
- One grouped pull request per ecosystem for GitHub Actions, Terraform and npm, so a busy
  week produces one PR and one CI run per ecosystem instead of one per dependency.

Keep this file generic. Anything that applies to one repository belongs in that
repository's `renovate.json`.

## Changing it

The app only turns inherited config on after it sees a commit to
`org-inherited-config.json`. If a change does not seem to apply, check the logs on the
[Mend dashboard](https://developer.mend.io/github/autobutler-org).

Validate before pushing:

```bash
npx --yes --package renovate -- renovate-config-validator --strict org-inherited-config.json
```
