# Installation

Installation can be done via the [`deploy` component](https://github.com/sarus-suite/deploy)

# Deploy

Removes the delivery friction for container tooling. It bundles the many moving pieces of sarus-suite (Sarusctl, Parallax, Podman, conmon, crun, and dependencies) into one consistent suite you can install or unpack system-wide or in userspace. Delivering standardized, working production and development environments with one command.

Deploy of sarus-suite standardizes the delivery of the stack and the process: one artifact, one process, same operation everywhere. Onboarding gets simplified. Operations are predictable. Accelerating development.

## What you get

- A coherent sarus-suite: Sarusctl, Parallax, Podman, conmon, crun, and dependencies, pinned to work together and wrapped as a single deliverable.
- Two delivery paths
    - Userspace. unpack + configure. Great for development and testing.
    - RPM repository. Install the meta-package.
- Get sensible defaults for HPC. Bootstrap sarus-suite with a productive intial configuration to get started.

## What Deploy simplifies

- Version sprawl of the multiple components into `one` suite. Expressing the whole toolchain as explicit dependencies, so upgrades are coordinated instead of managed individualy.
- Move from snowflake deployments to reproducible installs. A single setup entrypoint handles the delivery, resolving what is needed.
- Reproducible builds. We build components in a reproducible environment, so components are consistent.
- Prevents configuration drift by pinning and packaging. Artifacts are produced and bundled in a predictable structure ammenable for release.

## Quick start

### Userspace

As a standard user you can deploy it in USERSPACE.
This downloads a userspace tarball for your OS/arch, unpacks it to ./sarus-suite/, and runs sarusmgr configure.

```bash
    # Install
    curl -sL https://github.com/sarus-suite/deploy/releases/latest/download/setup | sh
    . sarus-suite/lib/sarus_env

    # Test
    sarusmgr test
```

## Supported distributions

- openSUSE Leap 15.5
