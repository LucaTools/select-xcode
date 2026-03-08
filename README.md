# select-xcode

A GitHub Actions composite action that selects a specific Xcode version on macOS runners via `xcode-select`.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-select--xcode-blue?logo=github)](https://github.com/marketplace/actions/select-xcode)

This action is also published on the [GitHub Actions Marketplace](https://github.com/marketplace/actions/select-xcode).


## Version resolution order

The action resolves the Xcode version using the following priority:

1. **`xcode-version` input** — explicit version passed to the action
2. **`.xcode-version` file** — a file at the root of the repository containing the version string
3. **`XCODE_VERSION` environment variable** — a variable already set in the workflow environment

If none of the above provides a version, the action fails with a descriptive error message.

## Usage

### With an explicit version input

```yaml
- uses: LucaTools/select-xcode@main
  with:
    xcode-version: "26.2"
```

### With a `.xcode-version` file

Commit a `.xcode-version` file to the root of your repository:

```
26.2
```

Then use the action without any input:

```yaml
- uses: LucaTools/select-xcode@main
```

### With a pre-set environment variable

```yaml
env:
  XCODE_VERSION: "26.2"

steps:
  - uses: LucaTools/select-xcode@main
```

## Inputs

| Name            | Required | Default | Description                     |
|-----------------|----------|---------|---------------------------------|
| `xcode-version` | No       | `""`    | The version of Xcode to select. |

## Full workflow example

```yaml
name: CI

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4

      - uses: LucaTools/select-xcode@main
        with:
          xcode-version: "26.2"

      - name: Build
        run: xcodebuild build -scheme MyApp
```

## Requirements

- Must run on a macOS runner (e.g. `macos-latest`, `macos-26`).
- The requested Xcode version must be available at `/Applications/Xcode_<version>.app`.

## License

[MIT](LICENSE)
