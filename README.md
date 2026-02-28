# setup-anyzig

GitHub Action that installs [anyzig](https://github.com/marler8997/anyzig) as `zig` on `PATH`.

Unlike version-pinning actions, this does **not** install a fixed Zig release. `anyzig` resolves and downloads the compiler version from your project's `build.zig.zon` (`minimum_zig_version`), so your project config remains the source of truth.

## Usage

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: mattneel/setup-anyzig@v1
  - run: zig build test
```

## Inputs

| Input | Description | Required | Default |
| --- | --- | --- | --- |
| `version` | Anyzig release tag to install (e.g. `v2025_10_15`). `latest` fetches the latest release. | No | `latest` |
| `use-cache` | Cache the anyzig binary and its compiler cache between runs using `actions/cache` | No | `true` |
| `cache-key` | Extra cache key suffix for matrix disambiguation | No | `""` |

## `setup-anyzig` vs `mlugg/setup-zig`

- `mlugg/setup-zig` downloads a specific Zig version. Your workflow version and project version can drift if not kept in sync.
- `setup-anyzig` installs a universal wrapper (`anyzig`). Your project's `build.zig.zon` is the single source of truth for compiler version selection.

## Mach Support

`anyzig` also supports Mach projects and automatically handles `.mach_zig_version` in `build.zig.zon`.

## Supported Runners

- `ubuntu-latest` (`X64`, `ARM64`)
- `windows-latest` (`X64`, `ARM64`)
- `macos-latest` (`ARM64`)

`x86_64-macos` is not available from anyzig releases. Intel Mac runners (for example `macos-13`) are not supported.

## Local Testing with `act`

```bash
cd setup-anyzig
act -j test --matrix os:ubuntu-latest
```

`act` has limited macOS/Windows support, so `ubuntu-latest` is the primary local validation target.
