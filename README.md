# rl-package GitHub Action

Packages a `.rl` source file into a self-contained binary for Linux, macOS, or Windows using the
[RL language](https://github.com/rl-lang/rl-lang), and uploads it as a workflow artifact.

## Usage

```yaml
steps:
  - uses: actions/checkout@v4

  - name: Package RL program
    uses: rl-lang/rl-package@v2
    with:
      file: 'main.rl'
      output: 'my-program'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `file` | Path to the `.rl` source file (relative to the calling repo root) | **Yes** | - |
| `output` | Output binary name, without extension | No | `program` |
| `version` | `rl-cli` version to install from crates.io (e.g. `0.2.1`). Omit for the latest published version. | No | `''` (latest) |

## How it works

1. Installs Rust via `dtolnay/rust-toolchain`.
2. Installs `rl-cli` with `cargo install` (cached per OS/version, so repeat runs are fast).
3. Runs `rl package <file> -o bin/<output>`.
4. Uploads the resulting binary as a workflow artifact named `<output>-<os>`.

Supports `ubuntu-latest`, `macos-latest`, and `windows-latest` runners.

## Examples

### Basic packaging
```yaml
- name: Package RL program
  uses: rl-lang/rl-package@v2
  with:
    file: 'main.rl'
```

### Custom output name
```yaml
- name: Package RL program
  uses: rl-lang/rl-package@v2
  with:
    file: 'src/main.rl'
    output: 'my-cli-tool'
```

### Pinned rl-cli version
```yaml
- name: Package RL program with a specific rl-cli version
  uses: rl-lang/rl-package@v2
  with:
    file: 'main.rl'
    version: '0.2.1'
```

### Build across all platforms
```yaml
name: Build binaries

on:
  push:
    tags: ['v*']

jobs:
  package:
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4

      - name: Package RL program
        uses: rl-lang/rl-package@v2
        with:
          file: 'main.rl'
          output: 'my-program'
```

## Versioning

| Reference | Description |
|-----------|-------------|
| `@main` | Latest development version |
| `@v2` | Latest v2.x release |
| `@v2.0.0` | Specific version |
| `@v1` | Legacy version — builds `rl-lang` from source (`dev` branch) on each run |

### Migrating from v1

`v2` installs the published `rl-cli` crate via `cargo install` (with caching) instead of cloning
and building `rl-lang`'s `dev` branch from source on every run. This is faster and reproducible,
but note:

- Builds are now pinned to a released `rl-cli` version by default, not a moving `dev` branch. Use
  the new `version` input if you need a specific release.
- macOS is now supported (previously the action silently did nothing on `macos-latest`).

## License

[MIT](LICENSE)
