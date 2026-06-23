# Installation

Sarus Suite can be installed either from the packaged deployment flow or by assembling the system integration manually.

The manual path is presented first because it exposes the moving pieces an HPC administrator needs to reason about: distribution packages, Sarus-specific binaries, Podman configuration, Parallax storage, and scheduler integration. The packaged deployment flow then shows how those pieces can be delivered in a more reproducible way.

## Manual system installation

Advanced HPC administrators may prefer a manual site installation. In this model, the generic runtime dependencies come from the operating system or site package repositories, while the Sarus-specific components are installed from the Sarus Suite upstream projects.

This path is useful when a site already has a supported Podman stack, existing configuration management, or stricter control over system package provenance.

### 1. Install runtime dependencies

Install the generic container runtime dependencies with the system package manager. Package names vary by distribution, but the installed commands should cover:

- Podman and OCI runtime support: `podman`, `crun`, `conmon`
- Podman networking helpers: `pasta`, `netavark`, `aardvark-dns`
- FUSE and overlay helpers: `fuse-overlayfs`, `fusermount3`, `squashfuse_ll`
- Image tools: `mksquashfs`, `rsync`, `inotifywait`
- Rootless container support: `newuidmap`, `newgidmap`, and subordinate UID/GID ranges in `/etc/subuid` and `/etc/subgid`

The target hosts also need kernel support for unprivileged user namespaces and FUSE. On systems with AppArmor, the local policy must allow the user namespace behavior required by rootless containers.

### 2. Install Sarus-specific components

Install the Sarus binaries in a stable site prefix, for example `/opt/sarus-suite/bin`:

- `sarusctl` and `skybox`, built from [`sarus-suite/cluster-tooling`](https://github.com/sarus-suite/cluster-tooling)
- `parallax` and `parallax-mount-program`, built from [`sarus-suite/parallax`](https://github.com/sarus-suite/parallax)
- OCI hooks built from [`sarus-suite/performance-extensions`](https://github.com/sarus-suite/performance-extensions)

Sites that first want to validate only the CLI and EDF workflow can start with `sarusctl`, `parallax`, and `parallax-mount-program`, then add Skybox and performance-extension hooks as part of scheduler integration.

### 3. Render site configuration

Use the bundle configuration templates as a reference, replacing the `@@SARUS_SUITE_*@@` placeholders with the chosen system paths:

- `runtime/etc/containers/containers.conf`
- `runtime/etc/containers/storage.conf`
- `runtime/etc/containers/containers.conf.modules/hpc`
- `runtime/etc/parallax/parallax-mount.conf`
- `runtime/etc/sarus-suite/90-sarusctl.conf`

For a system installation, typical rendered paths include:

- A binary prefix such as `/opt/sarus-suite/bin`
- A shared Parallax image store on a filesystem visible to compute nodes
- A Podman graph root and run root appropriate for the site policy
- A temporary directory for Podman and Parallax mount operations
- A log directory for the Parallax mount program

Keep the rendered paths consistent across all configuration files:

- `storage.conf` must point Podman at the Parallax image store and the `parallax-mount-program`.
- `parallax-mount.conf` must point the mount program at the installed `fuse-overlayfs`, `squashfuse_ll`, and `inotifywait`.
- `90-sarusctl.conf` must point `sarusctl` at the installed `podman`, `parallax`, `crun`, mount program, temporary directory, and Parallax image store.
- `containers.conf` must select the intended runtime and helper binary directory.

### 4. Configure the HPC Podman module

The `hpc` Podman module is the site policy layer for Sarus container execution. It configures containers to use host IPC, network, PID, UTS, and cgroup namespaces, binds `/tmp`, uses local time, disables conmon cgroups, and selects `crun`.

Install the rendered module where Podman can load it as `hpc`, then make sure the Sarus configuration references:

```toml
podman_module = "hpc"
```

### 5. Validate the installation

After rendering configuration and installing binaries, validate the system in layers:

```bash
podman info
sarusctl images
sarusctl render /path/to/example.toml
sarusctl run /path/to/example.toml cat /etc/os-release
```

For cluster integration, continue with Skybox deployment and scheduler-side validation. The manual system installation should first prove that Podman, Parallax storage, and `sarusctl` agree on the same paths before enabling site-wide scheduler workflows.

## Packaged deployment

After the stack layout is understood, the [`deploy` component](https://github.com/sarus-suite/deploy) can simplify delivery.

!!! note "Current distribution support"
    The Deploy component currently supports openSUSE Leap 15.5.

Deploy removes the delivery friction for container tooling. It bundles the moving pieces of Sarus Suite (`sarusctl`, Parallax, Podman, conmon, crun, and dependencies) into one consistent suite that can be installed system-wide or unpacked in userspace.

Deploy standardizes the delivery of the stack and the process: one artifact, one process, same operation everywhere. Onboarding is simpler, operations are more predictable, and component versions are coordinated.

### What you get

- A coherent Sarus Suite: `sarusctl`, Parallax, Podman, conmon, crun, and dependencies, pinned to work together and wrapped as a single deliverable.
- Two delivery paths
    - Userspace. Unpack and configure. Useful for development and testing.
    - RPM repository. Install the meta-package.
- Sensible defaults for HPC. Bootstrap Sarus Suite with a productive initial configuration.

### What Deploy simplifies

- Version sprawl across multiple components. The whole toolchain is expressed as explicit dependencies, so upgrades are coordinated instead of managed individually.
- Move from snowflake deployments to reproducible installs. A single setup entrypoint handles the delivery, resolving what is needed.
- Reproducible builds. Components are built in a reproducible environment, so component behavior is consistent.
- Reduced configuration drift through pinned, packaged artifacts in a predictable release structure.

### Userspace quick start

As a standard user, you can deploy Sarus Suite in userspace. This downloads a userspace tarball for your OS and architecture, unpacks it to `./sarus-suite/`, and runs `sarusmgr configure`.

```bash
# Install
curl -sL https://github.com/sarus-suite/deploy/releases/latest/download/setup | sh
. sarus-suite/lib/sarus_env

# Test
sarusmgr test
```

## Supported distributions

- Manual installation depends on the site-provided packages and configuration for the target distribution.
- The Deploy component currently supports openSUSE Leap 15.5.
