# Contributing

Thanks for stopping by. Bug reports, ideas, typo fixes and pull requests are all welcome, in roughly that order of how quickly they get a reply.

This action is looked after in spare time by one person with a day job and a coffee habit, so replies are best effort. The fastest way to get something changed is to make it easy to say yes to.

## Before you start

- Be kind. The [code of conduct](CODE_OF_CONDUCT.md) says how.
- Search the [issues](https://github.com/stphnwlsh/dotnet-outdated-action/issues) first. Someone may have beaten you to it.
- For anything bigger than a small fix, open an issue before you write the code, so we can agree on the approach before you spend an evening on it.
- Found a security problem? Don't open an issue. Follow the [security policy](SECURITY.md).

## How it's put together

There's not much to it, which is the point.

| Path | What it is |
| --- | --- |
| `action.yml` | The whole action: a composite action with two bash steps. One installs the tool, the other runs it. |
| `test/fixtures/OutdatedSample` | A project with old packages on purpose. The tests upgrade it, so please don't "fix" it. |
| `.github/workflows/test.yml` | Lints the workflows, then runs the action three times on Linux, Windows and macOS: filtered, everything, and again with nothing left to do. |
| `.github/workflows/example.yml` | The README example, run by hand against the fixture. |

Inputs reach the scripts through `env`, never by pasting `${{ inputs.* }}` into the script itself. That keeps a cheeky package name from turning into a shell command. Please keep it that way.

## Making a change

1. Fork the repository and branch from `main`. Name the branch `feature/`, `fix/` or `chore/` followed by a few words, like `fix/windows-path`.
1. Make the change, and update the README if an input, output or behaviour changed.
1. Push to your fork. The **Test** workflow runs there too, so you'll know it passes on all three operating systems before you open the pull request.
1. Open the pull request against `main`, fill in the template, and link the issue if there is one.

Every pull request is reviewed by the maintainer before it goes in. If a review takes a while, a polite nudge is fine.

## Releases

Releases follow [semantic versioning](https://semver.org). Each release gets a full tag, like `v3.0.0`, and the major tag, like `v3`, moves to point at the latest release in that major version. Workflows that use `@v3` pick up fixes without lifting a finger, and nobody's build changes behaviour under them without a new major version.

The maintainer releases from `main`:

```bash
git tag -a v3.0.1 -m "v3.0.1"
git tag -fa v3 -m "v3"
git push origin v3.0.1
git push origin v3 --force
```

Then publish a GitHub release from the full tag, with notes from [CHANGELOG.md](CHANGELOG.md), and tick **Publish this Action to the GitHub Marketplace**.

## Saying thanks

If the action saves you some time, you can [sponsor the work](https://github.com/sponsors/stphnwlsh). It goes on keeping things current, and the coffee.
