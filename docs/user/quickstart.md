
# Quickstart

Follow these steps to use Sarus Suite on your HPC cluster.

1. **Load environment**

    ```bash
    module load sarus-suite podman
    ```

2. **Submit a simple job**

    === "SLURM"
        ```bash
        sbatch -N1 -t 10 --wrap='sarus-run podman run hello-world'
        ```
    === "PBS"
        ```bash
        qsub -l select=1:ncpus=4 -l walltime=00:10:00 -- sarus-run podman run hello-world
        ```

3. **Validate output** — You should see container output in the job logs.
