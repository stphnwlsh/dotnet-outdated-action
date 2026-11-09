# Dotnet Outdated Action

[![Test](https://github.com/stphnwlsh/dotnet-outdated-action/actions/workflows/test.yml/badge.svg)](https://github.com/stphnwlsh/dotnet-outdated-action/actions/workflows/test.yml)
[![Release](https://img.shields.io/github/v/release/stphnwlsh/dotnet-outdated-action?sort=semver)](https://github.com/stphnwlsh/dotnet-outdated-action/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A GitHub Action that upgrades every outdated NuGet package in your .NET solution in one go. It installs the excellent [dotnet outdated](https://github.com/dotnet-outdated/dotnet-outdated) tool, runs it with `--upgrade` and tells you whether anything changed, so the next step in your workflow can build it, test it and open one tidy pull request.

Think of it as the housemate who actually takes the bins out.

## Quick start

```yaml
- uses: stphnwlsh/dotnet-outdated-action@v3
  with:
    location: MySolution.sln
```

That's it. Your project files now point at the latest versions, and `steps.<id>.outputs.updated` says whether there was anything to do.

## The full loop

The action edits files and stops there. What happens next is up to you. The usual recipe is a weekly workflow that upgrades, builds, tests and opens a pull request when something changed:

```yaml
name: Update NuGet packages

on:
  schedule:
    - cron: '0 9 * * 1' # Mondays at 09:00 UTC, before the coffee wears off
  workflow_dispatch:

permissions:
  contents: write
  pull-requests: write

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - uses: actions/setup-dotnet@v5
        with:
          dotnet-version: '10.0.x'

      - name: Upgrade packages
        id: outdated
        uses: stphnwlsh/dotnet-outdated-action@v3
        with:
          location: MySolution.sln
          excludes: Microsoft.CodeAnalysis

      - name: Build and test
        if: steps.outdated.outputs.updated == 'true'
        run: dotnet test MySolution.sln

      - name: Open a pull request
        if: steps.outdated.outputs.updated == 'true'
        uses: peter-evans/create-pull-request@v7
        with:
          branch: chore/update-nuget-packages
          title: Update NuGet packages
          commit-message: Update NuGet packages
          body: Packages upgraded by dotnet outdated. The build and tests passed before this was opened.
```

Because the tests run before the pull request exists, a broken upgrade never makes it as far as your review queue.

## Inputs

| Input | Default | What it does |
| --- | --- | --- |
| `location` | `.` | The solution, project or folder to check. See [specifying the path](https://github.com/dotnet-outdated/dotnet-outdated#specifying-the-path). |
| `includes` | | Comma separated. Only packages whose names contain one of these values are upgraded, ignoring case. |
| `excludes` | | Comma separated. Packages whose names contain one of these values are left alone, ignoring case. |
| `version-lock` | `None` | `Major` stays inside the current major version, `Minor` inside the current minor. `None` takes whatever is newest. |
| `pre-release` | `Auto` | `Auto` only offers pre-releases to packages already on one, `Always` always does and `Never` never does. |
| `tool-version` | latest | Pin the version of `dotnet-outdated-tool`, if you like your surprises scheduled. |

`includes: microsoft, automapper` and `includes: microsoft,automapper` mean the same thing. Spaces used to break the step. They don't any more.

For the finer points of matching, see [including and excluding packages](https://github.com/dotnet-outdated/dotnet-outdated#including-and-excluding-packages).

## Outputs

| Output | What it means |
| --- | --- |
| `updated` | `true` when at least one package was upgraded, `false` when everything was already current. |

If dotnet outdated reports an error, the step fails rather than pretending all is well. The job summary shows what was upgraded either way.

## Why not just Dependabot?

Fair question. [Dependabot](https://docs.github.com/code-security/dependabot) is great, and if it does what you need, use it. This action is for when you want:

- **One pull request for everything**, upgraded and tested together, rather than a pull request per package.
- **Your schedule and your rules**, written in your own workflow, next to your own build and tests.
- **A green build before anyone looks at it**, because the tests run before the pull request is opened.
- **Your .NET SDK and your feeds**, since it runs `dotnet` on your runner and sees what your build sees.

Plenty of teams happily run both. They're friends, not rivals.

## Upgrading from v2

v3 is a tidy-up, but a couple of things behave differently:

- `location` is now honoured. In v2 it was quietly ignored and the tool always ran in the working directory, so if you set it to something that doesn't exist, v3 will now tell you.
- `location` is optional and defaults to `.`.
- Spaces in `includes` and `excludes` are now trimmed instead of breaking the step.
- The deprecated `set-output` command is gone, so no more warnings in your logs.
- New inputs: `version-lock`, `pre-release` and `tool-version`.

`@v2` still points where it always did, so nothing breaks until you choose to move.

## Contributing

Bug reports, ideas and pull requests are all welcome. [CONTRIBUTING.md](CONTRIBUTING.md) has how to run the tests and how releases work, and the [code of conduct](CODE_OF_CONDUCT.md) has how to be excellent to each other. Security problems go through the [security policy](SECURITY.md), not an issue.

## Thanks

The heavy lifting is all [dotnet outdated](https://github.com/dotnet-outdated/dotnet-outdated), by Jerrie Pelser and its contributors. This action just puts it on a timer and gives it a step output.

If it saves you some time, you can [sponsor the work](https://github.com/sponsors/stphnwlsh). It also funds [Lean Mean Clean Apps](https://leanmeancleanapps.github.io), and the coffee.

## Licence

[MIT](LICENSE), so do what you like with it.
