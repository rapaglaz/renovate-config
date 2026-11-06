# renovate-config

Shared Renovate presets for my repositories.

## Usage

The base preset is enough for most repositories. Add it in `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>rapaglaz/renovate-config"]
}
```

To get the Dependency Dashboard, add the `dashboard` preset. Technology presets come last:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>rapaglaz/renovate-config",
    "github>rapaglaz/renovate-config:dashboard",
    "github>rapaglaz/renovate-config:angular"
  ]
}
```

## Presets

| Preset       | Purpose                                                       |
| ------------ | ------------------------------------------------------------- |
| `default`    | Schedule, grouping and labels. No automerge and no dashboard. |
| `dashboard`  | Dashboard and approval of major and 0.x updates.              |
| `angular`    | One group for Angular packages.                               |
| `nx`         | One group for Nx packages, with a note about migrations.      |
| `playwright` | One group for the npm package and the Docker image.           |
| `react`      | One group for React packages.                                 |

## Rules

### Order in `extends`

Use this order: `default`, `dashboard`, then the technology presets. Renovate applies the last
matching package rule, so the groups must come after the base preset. Rules in the `renovate.json`
of a repository apply after all presets.

### Merging

Nothing merges automatically. I merge all PRs manually, also the security updates.

### Dependency Dashboard

The base preset disables the dashboard, so that it does not appear as an issue in the repository.
Major and 0.x updates then get their own PR with a label.

The `dashboard` preset enables the dashboard and requires approval of major and 0.x updates. Do not
add a rule with `dependencyDashboardApproval` to the base preset, because such a rule creates the
dashboard even when it is disabled.

### What belongs here

Add a preset only when at least two repositories use it. Rules for one repository stay in that
repository. This repository is public, so do not add host names, registries or other details of internal systems.

## Validation

```sh
npx --yes --package renovate -- renovate-config-validator --strict --no-global default.json dashboard.json angular.json nx.json playwright.json react.json renovate.json
```
