# Multi Library Template

This is my best effort at creating a template for solutions that contain multiple projects, especially for libraries.

## Tech Stack

| Purpose        | Technology                                                        |
| -------------- | ----------------------------------------------------------------- |
| Source control | [GitHub](https://github.com)                                      |
| CI/CD          | [AppVeyor](https://ci.appveyor.com/)                              |
| Documentation  | [XMLDoc2Markdown](https://charlesdevandiere.github.io/xmldoc2md/) |
| Test driver    | [NUnit](https://docs.nunit.org/)                                  |
| Test converge  | [coveralls](https://coveralls.io)                                 |

## Getting started

### Setup environment

1. Create a [GitHub](https://github.com) repository and clone it
2. Add the repository to [AppVeyor](https://ci.appveyor.com/)
3. Add the repository to [coveralls](https://coveralls.io)
4. Replace [variables](#variables) with your specific information
5. Confirm your code-style in the `.editorconfig`

Lastly replace this `README.md` with your own.

### Source projects

Add a project in `src/` with `dotnet new classlib -n "+++YOU.Project"` and define the missing nuget and assembly information. `AssemblyName` & `PackageId` can be inferred automatically.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFrameworks>netstandard2.0;netstandard2.1;net48;net50;net60</TargetFrameworks>
    <RootNamespace>+++YOU</RootNamespace>
  </PropertyGroup>

  <PropertyGroup Label="Assembly Info">
    <AssemblyName>+++YOU.Project</AssemblyName>
    <AssemblyTitle>+++YOU.Project</AssemblyTitle>
    <PackageId>+++YOU.Project</PackageId>
    <Title>+++YOU.Project</Title>
    <Description>This is about my project</Description>
    <PackageTags>csharp,library</PackageTags>
  </PropertyGroup>
</Project>
```

### Test projects

Add a test project in `tests/` with `dotnet new nunit -n "+++YOU.Project.Tests"`.

### Variables

There are a few variables denoted by the `+++` prefix. Search all project files for this prefix.

The following files contain variables `appveyor.yml`, `src/AssemblyInfo.cs` & `src/Directory.Build.props`

| Variable   | Explanation                                                 |
| ---------- | ----------------------------------------------------------- |
| +++PROJECT | The root name of your project. e.g. `Microsoft` or `System` |
| +++YOU     | The GitHub name of the owner of the project                 |
| +++VALUE   | Some specific value explained in the following comment      |

## Files

### `RELEASENOTES.md`

Contains the release notes. Realeises are usually separated by `---`. The file is automatically imported into the nuget package using the `Build.props`.

### `Build.props`

This template uses `Directory.Build.props` to reduce the redundancy in `.csproj` project files. There are distinct `props` for both `src/` and `tests/` where assembly & [nuget](nuget.org) information is defined, as well as common packages are included.

This allows new projects to be added using the [dotnet cli](https://docs.microsoft.com/en-us/dotnet/core/tools/) with minimal manual intervention.

To change the license to a file based change this line
```xml
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
```
to
```xml
    <PackageLicenseFile>../../LICENSE-MIT</PackageLicenseFile>
```

### Scripts

These scripts are usually used by the CI, but can be run locally as well.

- `tooling.ps1`: Installs required tools globally, requires the [choco](https://chocolatey.org/) package manager. Can be adopted to work with linux.
- `test.ps1`: Executes all test projects in `tests/`, collects code coverage, and uploads it to coveralls.
- `doc.ps1`: Generates markdown documentation from the `xml` documentation, generated by the build project, in the `doc/` directory. One subdirectory is created per project with the same name.

### `.gitignore`

The gitignore is created by [topal gitignore](https://www.toptal.com/developers/gitignore/api/visualstudio,visualstudiocode,rider) then modified to work with [VisualStudio](https://visualstudio.microsoft.com/), [JetBrains Rider](https://www.jetbrains.com/rider/) & [VSCode](https://code.visualstudio.com/).

### `.editorconfig`

The [EditorConfig](https://editorconfig.org) is a bit opinionated, so you might want to change some things. There is plugin support for all moderns IDEs and VIM.