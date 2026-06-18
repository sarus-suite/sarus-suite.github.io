# Component index

A catalog of Sarus Suite components, their role in the stack, and upstream repositories. 

| Component                | Category             | Purpose                                                     | Repo                                                          | Releases |
|--------------------------|----------------------|-------------------------------------------------------------|---------------------------------------------------------------|----------|
| `sarusctl`               | Cluster tools        | EDF-driven CLI for launches; one command for scheduler-aware, policy-compliant containers | https://github.com/sarus-suite/sarusctl                       | https://github.com/sarus-suite/sarusctl/releases |
| `skybox`                 | Cluster tools        | Slurm SPANK integration so EDF jobs run with correct cgroups, env and node resources | https://github.com/sarus-suite/skybox                         | –        |
| `raster`                 | Cluster tools        | EDF parser library; single versioned implementation of the EDF spec                       | https://github.com/sarus-suite/raster                         | –        |
| `podman-driver`          | Cluster tools        | Rust crate to execute Podman and Parallax operations with EDF support                     | https://github.com/sarus-suite/podman-driver                  | –        |
| `parallax`               | Transparent storage  | Lightweight utility that exposes read-only SquashFS images on parallel filesystems (e.g. NFS) | https://github.com/sarus-suite/parallax                       | https://github.com/sarus-suite/parallax/releases |
| `parallax-mount-program` | Transparent storage  | Mount helper for Podman; presents squashed images as normal rootfs layers                 | https://github.com/sarus-suite/parallax-mount-program         | –        |
| `performance-extensions` | Performance          | CDI configuration, Podman config modules and hooks for device / performance tuning        | https://github.com/sarus-suite/performance-extensions         | –        |
| `deploy`                 | Dev experience       | Reproducible development-to-production environments for the suite                         | https://github.com/sarus-suite/deploy                         | –        |
| `containerfiles-ci`      | Dev experience       | Canonical CI build / test images with pinned dependencies                                  | https://github.com/sarus-suite/containerfiles-ci              | –        |

