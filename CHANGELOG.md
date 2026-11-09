# Changelog

All notable changes to this action. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow [semantic versioning](https://semver.org).

## [3.0.0] - 2026-11-09

### Changed

- `location` is now passed to dotnet outdated. In v2 it was accepted and then quietly ignored, so the tool always ran in the working directory.
- `location` is optional and defaults to `.`.
- Spaces around the commas in `includes` and `excludes` are trimmed instead of breaking the step.
- The `updated` output is written to `$GITHUB_OUTPUT` instead of the deprecated `set-output` command.
- Inputs reach the scripts through environment variables, so values can't be run as shell commands.
- The tool is updated if it's already installed, instead of failing the step.

### Added

- `version-lock`, `pre-release` and `tool-version` inputs.
- A job summary that shows what was upgraded.
- Tests on Linux, Windows and macOS, plus the community files that make it a proper open source project.

## [2.0.0] - 2021-11-30

### Added

- `includes` and `excludes` inputs, and the `updated` output.
- The step fails when dotnet outdated reports errors.

## [1.0.0] - 2021-11-27

### Added

- The first version: install dotnet outdated and upgrade everything.

[3.0.0]: https://github.com/stphnwlsh/dotnet-outdated-action/compare/v2...main
[2.0.0]: https://github.com/stphnwlsh/dotnet-outdated-action/compare/v1...v2
[1.0.0]: https://github.com/stphnwlsh/dotnet-outdated-action/releases/tag/v1
