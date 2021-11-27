# Dotnet Outdated Action

This action installs the `dotnet outdated` dotnet tool and then attempts to update any NuGet packages that are available.

## Inputs

## `location`

The location of the solution or project that needs dependency updates. Default `""`.

## Outputs

## `updated`

Set to `true` if there were package updates and `false` if no package updates were required.

## Example usage

```
uses: stphnwlsh/dotnet-outdated-action@v1
with:
  location: 'Sample.sln'
```
