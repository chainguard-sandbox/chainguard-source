# chainguard-source
Fetch the open source code related to Chainguard packages and images, as defined in the SBOMs.

Note: This shell script is a prototype. It will hopefully work for you but your mileage may vary.

# Example usage

Fetch sources by image name and tag:
```
$ chainguard-source --yes --image cgr.dev/chainguard/wolfi-base:latest
$ chainguard-source -y -i cgr.dev/chainguard/python:latest
$ chainguard-source -y -i cgr.dev/chainguard/jdk
```

Fetch sources by package name and version:
```
$ chainguard-source --yes --package hello-wolfi
$ chainguard-source -y -p hello-wolfi-2.12.1-r6
```

Fetch sources from local SBOM file:
```
chainguard-source -y --sbom /tmp/midnight-commander.sbom.spdx.json
chainguard-source -y -s /tmp/wordpress.latest.sbom.spdx.json
```

# Enterprise images

Chainguard images contain two types of APKs:

- **Public Wolfi packages** (`pkg:apk/wolfi/*`) — open source packages built from the [Wolfi](https://github.com/wolfi-dev) repository
- **Enterprise packages** (`pkg:apk/chainguard/*`) — private packages served from `apk.cgr.dev`

`chainguard-source` downloads source for both. When using `--image`, the script reads `/etc/apk/repositories` from the image to determine where each APK is served from, tries each repository in order, and falls back to `apk.cgr.dev` with authentication for packages not found in any public repo.

## Additional dependencies for enterprise images

- [`crane`](https://github.com/google/go-containerregistry/blob/main/cmd/crane/README.md) — reads the APK repository list from the image
- [`chainctl`](https://edu.chainguard.dev/chainguard/chainctl/) — generates a pull token for authenticating against `apk.cgr.dev`

## Usage

When using `--image` with a `cgr.dev/ORG/image` URL, the Chainguard org is auto-detected:
```
$ chainguard-source -y --image cgr.dev/example.com/redis:latest
```

For `--package` or `--sbom` mode, specify the org explicitly with `--org`:
```
$ chainguard-source -y --org example.com --sbom /tmp/image.sbom.spdx.json
```

Authentication uses `chainctl` — ensure you are logged in before running:
```
$ chainctl auth login
```
