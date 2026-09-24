# renovate-config

Organization-wide [Renovate](https://docs.renovatebot.com/) policy for autobutler-org.

The policy is `default.json`. Every repository's `renovate.json` extends it by name, which
leaves room to add repository-specific rules underneath:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>autobutler-org/renovate-config"]
}
```

`org-inherited-config.json` is the safety net for a repository that has no `renovate.json`
yet. The Mend-hosted app reads that file automatically (the name is fixed on the hosted
app), and it does nothing but extend the same preset
([inherited config](https://docs.renovatebot.com/config-overview/#inherited-config)).
Edit `default.json`; leave the other file alone.

Renovate's runs, logs and job status for the org are on the
[Mend dashboard](https://developer.mend.io/github/autobutler-org).

## Policy

- `config:recommended`, which includes the Dependency Dashboard issue in each repository.
- Daily: updates open before 4am (UTC). A repository can override this per
  package, as iac does for its quark image.
- One grouped pull request per ecosystem for GitHub Actions, Terraform, npm, Go modules and
  Dart/Flutter (pub), so a busy day produces one PR and one CI run per ecosystem instead of
  one per dependency.
- Go updates run `go mod tidy`, as Dependabot did, so `go.sum` stays consistent.
- No TypeScript major updates: vue-tsc and typescript-eslint do not support TypeScript 7
  yet. Remove the rule once they do.

Keep `default.json` generic. Anything that applies to one repository belongs in that
repository's `renovate.json`.

## Changing it

The app only turns inherited config on after it sees a commit to
`org-inherited-config.json`. If a change does not seem to apply, check the logs on the
[Mend dashboard](https://developer.mend.io/github/autobutler-org).

Validate before pushing. The `Check` workflow runs the same thing on every pull request:

```bash
npx --yes --package renovate -- renovate-config-validator --strict default.json org-inherited-config.json
```
