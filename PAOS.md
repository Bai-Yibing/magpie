# PAOS fork notes

This fork carries PAOS-specific build and release automation for
[magpie](https://github.com/yetone/magpie). It does not change magpie's runtime
behaviour.

## Branches

- `main` — mirrors upstream `yetone/magpie`.
- `paos` — upstream + PAOS build/release automation only.

## Building terminal-only binaries (nogui)

PAOS runs `magpie serve`; the GUI is not used. Cross-compiles anywhere:

```bash
VERSION=$(git describe --tags --always)
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build -tags nogui -trimpath \
  -ldflags="-s -w -X main.version=$VERSION" -o dist/magpie-cli-windows-amd64.exe .
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -tags nogui -trimpath \
  -ldflags="-s -w -X main.version=$VERSION" -o dist/magpie-cli-linux-amd64 .
```

## Release tags

`paos-vX.Y.Z-pN` — upstream version `X.Y.Z` plus patch level `N`.
Tagging triggers `.github/workflows/paos-release.yml`, which builds both
binaries and attaches them to the GitHub release.

## Deployment

Deployment, configuration and health checks live outside this repository
(in a private control plane). This fork is only the build source.
