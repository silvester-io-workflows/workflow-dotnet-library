Reusable workflow for packing and publishing NuGet packages from a .NET solution to GitHub Packages (or another registry).

## Usage

Add a workflow in the consuming repository that triggers when you want to publish and calls this reusable workflow. The reusable workflow always restores and tests; it only packs/publishes when the run is for a tag (e.g., `refs/tags/v1.2.3`):

```yaml
name: Publish NuGet Packages

on:
  push:
    branches: ["main"]
    tags: ["v*"]
  workflow_dispatch: {}

permissions:
  contents: read
  packages: write

jobs:
  publish:
    uses: silvester-io-workflows/workflow-dotnet-library/.github/workflows/publish-nuget.yaml@v1
    with:
      solution_path: ./MyLibrary.sln
      # package_source: https://nuget.pkg.github.com/<owner>/index.json  # optional override
      # runsettings_path: ./tests.ci.runsettings  # optional test settings
    secrets: inherit
```

### Inputs
- `solution_path` (required): Path to the solution to pack (e.g., `./src/MyLibrary.sln`).
- `configuration` (optional): Build configuration. Defaults to `Release`.
- `dotnet_version` (optional): .NET SDK version to install. Defaults to `10.0.x`.
- `package_source` (optional): NuGet source to push to. Defaults to the caller's GitHub Packages feed.
- `output_dir` (optional): Directory for packed artifacts. Defaults to `./artifacts/packages`.
- `restore_source` (optional): NuGet source to restore from for internal packages. Defaults to `https://nuget.pkg.github.com/silvester-io-libraries/index.json`.
- `runsettings_path` (optional): Path to a `.runsettings` file used during tests.
