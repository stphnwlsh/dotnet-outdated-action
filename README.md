# Dotnet Outdated Action

This action installs the `dotnet outdated` dotnet tool and then attempts to update any NuGet packages that are available.

## Inputs

## `location`

The location of the solution or project that needs to be checked for dependency updates. Default `""`.

For more information review the [Specifying the Path](https://github.com/dotnet-outdated/dotnet-outdated#specifying-the-path) documentation on the `dotnet outdated` GitHub

## `includes`

A comma separated list of inputs to use to limit packages being analysed to ones **including** those values. Default `""`.

**DO NOT INCLUDE SPACES** - this will break the step and cause your pipeline to fail.

The following example will only include packages with the word 'microsoft' in their name, regardless of casing.
```yml
with:
  includes: microsoft
```

This example will only include packages with the words 'microsoft' and 'automapper' in their name.
```yml
with:
  includes: microsoft,automapper
```

For more information review the [Includes and Excludes](https://github.com/dotnet-outdated/dotnet-outdated#including-and-excluding-packages) documentation on the `dotnet outdated` GitHub

## `excludes`

A comma separated list of inputs to use to limit packages being analysed to ones **excluding** those values. Default `""`.

**DO NOT INCLUDE SPACES** - this will break the step and cause your pipeline to fail.

The following example will exclude packages with the word 'microsoft' in their name, regardless of casing.
```yml
with:
  excludes: microsoft
```

This example will exclude packages with the words 'microsoft' and 'automapper' in their name.
```yml
with:
  excludes: microsoft,automapper
```

For more information review the [Includes and Excludes](https://github.com/dotnet-outdated/dotnet-outdated#including-and-excluding-packages) documentation on the `dotnet outdated` GitHub

## Outputs

## `updated`

Set to `true` if there were package updates and `false` if no package updates were required.

## Example usage

```
uses: stphnwlsh/dotnet-outdated-action@v2
with:
  location: Sample.sln
  includes: sample,nuget,packages
  excludes: nuget
```
