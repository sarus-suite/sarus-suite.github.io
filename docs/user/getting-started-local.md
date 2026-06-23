# Getting started locally

Use the portable Sarus Suite bundle to try the CLI and EDF workflow on a Linux machine.

This path gives you a feel for `sarusctl`, EDF files, Podman execution, and the Parallax-aware storage configuration shipped in the bundle. It does not require Skybox, Slurm integration, or performance extensions.

## Download the bundle

Pick the release and architecture for your host:

```bash
VERSION=v0.0.4
ARCH=amd64   # or arm64

curl -LO "https://github.com/sarus-suite/sarus-suite/releases/download/${VERSION}/sarus-suite-${VERSION}-${ARCH}.tar.gz"
tar -xzf "sarus-suite-${VERSION}-${ARCH}.tar.gz"
```

## Enter the Sarus Suite shell

```bash
./sarus-suite/bin/sarus-suite-shell
```

The shell puts the bundled tools on `PATH`, creates private state and config directories, and points Podman and Parallax at the bundle configuration.

## Check the local runtime

```bash
sarus-suite-check
```

The check confirms that the bundled commands are visible, the generated configs exist, and Podman can initialize with the bundled runtime settings.

## Inspect an EDF

```bash
cat "$SARUS_SUITE_ROOT/examples/ubuntu.toml"
sarusctl render "$SARUS_SUITE_ROOT/examples/ubuntu.toml"
```

An EDF is a small declarative file that describes the container environment. The bundled examples keep the first run intentionally simple.

## Run from an EDF

```bash
sarusctl run "$SARUS_SUITE_ROOT/examples/ubuntu.toml" cat /etc/os-release
```

This runs the command inside the image described by the EDF using the bundled Podman runtime and Sarus Suite configuration.

## What you tried

- `sarus-suite-shell` activated the portable runtime bundle.
- `sarus-suite-check` validated the local host and bundled configuration.
- `sarusctl render` showed how the EDF is interpreted.
- `sarusctl run` mapped the EDF onto Podman execution.

For cluster-integrated operation, see the scheduler and installation material. Skybox and performance-extension hooks are separate cluster features.
