## Using Shared Memory (`/dev/shm`)

{% if slide %}
**Concept:** A temporary filesystem (`tmpfs`) residing entirely in RAM.
* Extremely fast access times.
* Standard directory interface.
* Typically capped at 50% of total physical RAM.
{% endif %}

{% if page %}
`/dev/shm` (Shared Memory) is a temporary filesystem (`tmpfs`) in Linux that resides entirely in the node's Random Access Memory (RAM) rather than on physical disk storage. Accessing RAM is orders of magnitude faster than accessing global or local disk storage. It is typically mapped as a standard directory, making it easy to interact with using basic shell commands. On most systems, it is automatically capped at 50% of the total physical RAM of the node.
{% endif %}

### HPC Benefits

{% if slide %}
* **Apptainer Performance:** Pull and unpack container layers at RAM speeds.
* **High-Speed I/O:** Near-zero wait times for heavy read/write operations on temporary files.
{% endif %}

{% if page %}
Using `/dev/shm` can significantly accelerate job performance in HPC environments:

* **Apptainer Performance:** Setting `APPTAINER_CACHEDIR` to `/dev/shm` allows the container to pull and unpack layers at RAM speeds, significantly outperforming networked directories.
* **High-Speed I/O:** Performing heavy read/write operations on small temporary files (e.g., shuffling data, temporary logs, or IPC locks) in RAM reduces I/O wait times to near zero.
{% endif %}

### Critical Pitfalls

{% if slide %}
* ⚠️ **"RAM Theft":** Files count towards the Slurm `--mem` allocation (risk of OOM Kill).
* ⚠️ **Volatility:** Data is lost instantly upon node reboot.
* ⚠️ **"Ghost Files":** Files are not automatically cleaned up by Slurm after a job ends.
{% endif %}

{% if page %}
While powerful, `/dev/shm` introduces specific risks in an HPC context:

1. **The "RAM Theft" Problem:** Any file stored in `/dev/shm` counts directly toward the Slurm `--mem` allocation. For example, requesting 16GB of RAM and storing a 10GB dataset in `/dev/shm` leaves only 6GB for processing before Slurm triggers an Out of Memory (OOM) Kill.
2. **Volatility:** Data is not persistent and is lost instantly if the node reboots. It is strictly for temporary storage.
3. **The "Ghost Files" Issue:** Unlike standard temporary directories, `/dev/shm` is usually not automatically cleaned up by Slurm when a job terminates. Unremoved files remain in RAM, consuming resources and potentially impacting subsequent jobs on the node.
{% endif %}

### Proper Management & Apptainer Usage

{% if slide %}
**Best Practice:** Always use a Bash `trap` to ensure cleanup upon job completion or failure.

```bash
export JOB_SHM="/dev/shm/${USER}_${SLURM_JOB_ID}"
mkdir -p "$JOB_SHM"

cleanup_shm() { rm -rf "$JOB_SHM"; }
trap cleanup_shm EXIT SIGTERM SIGINT

```

{% endif %}

{% if page %}
To utilize `/dev/shm` effectively with Apptainer, environment variables must be configured to utilize a unique path, preventing collisions. Furthermore, to prevent "ghost files" from remaining after job completion, a Bash `trap` should be utilized. This ensures the directory is automatically deleted even if the job crashes or is cancelled by Slurm.

**Recommended Slurm Script Implementation:**

```bash
#!/bin/bash
#SBATCH --mem=32G
# ... other SBATCH params ...

# 1. Create a unique RAM-disk path
export JOB_SHM="/dev/shm/${USER}_${SLURM_JOB_ID}"
mkdir -p "$JOB_SHM"

# 2. Define the cleanup function
cleanup_shm() {
    echo "Cleanup: Removing temporary files from $JOB_SHM"
    rm -rf "$JOB_SHM"
}

# 3. Register the cleanup function to run on EXIT, SIGTERM, or SIGINT
trap cleanup_shm EXIT SIGTERM SIGINT

# 4. Configure and run Apptainer
export APPTAINER_CACHEDIR="$JOB_SHM/cache"
export APPTAINER_TMPDIR="$JOB_SHM/tmp"
mkdir -p "$APPTAINER_CACHEDIR" "$APPTAINER_TMPDIR"

# Bind mount /dev/shm to allow the container to use the high-speed RAM pool
apptainer run --bind /dev/shm --bind "$(pwd):/app" image.sif

```

{% endif %}

