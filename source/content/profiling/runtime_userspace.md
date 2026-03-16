### User-Space Polling Level (Concurrent Shell Processes)

{% if slide %}

* **Mechanism:** Concurrent monitoring utilities are executed via background loops.
* **Disadvantage:** Classified as a structural fallback. Reliance on the OS process scheduler introduces resource contention and temporal misalignment, reducing objective reproducibility.
* **Primary Tools:** `sysstat` (`pidstat`), `nvidia-smi` (or `rocm-smi`).
* **Principle:** The host operating system's pseudo-filesystem (`/proc`) and vendor-specific hardware drivers are queried iteratively by independent, user-space processes.
{% endif %}

{% if page %}
Concurrent monitoring utilities are executed via background loops. This approach is classified as a structural fallback. Reliance on the operating system's standard process scheduler introduces resource contention and temporal misalignment, reducing the objective reproducibility of the telemetry.

* **Primary Tools:** `sysstat` (`pidstat`), `nvidia-smi` (or `rocm-smi`).
* **Underlying Principle:** The host operating system's pseudo-filesystem (`/proc`) and vendor-specific hardware drivers are queried iteratively by independent, user-space processes.
{% endif %}

**Example (Concurrent Bash Polling):**

```bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --time=00:20:00
#SBATCH --output=job_%j.out

# Ensure propagation of termination signals to background telemetry processes
trap 'kill -TERM $APP_PID $HOST_PROF_PID $GPU_PROF_PID 2>/dev/null; wait $APP_PID' TERM INT

# 1. Execute primary workload in the background
./my_application &
APP_PID=$!

# 2. Execute host-level telemetry (CPU, Memory, I/O)
# -h: Horizontal formatting for automated parsing
# 5: Polling interval (seconds)
pidstat -p $APP_PID -u -r -d -h 5 > "host_telemetry_${SLURM_JOB_ID}.log" &
HOST_PROF_PID=$!

# 3. Execute hardware-level telemetry (GPU)
# --format=csv: Enforces structured data output
# -l 5: Polling interval (seconds)
nvidia-smi --query-gpu=timestamp,index,utilization.gpu,utilization.memory,memory.used,power.draw \
           --format=csv -l 5 > "gpu_telemetry_${SLURM_JOB_ID}.csv" &
GPU_PROF_PID=$!

# Suspend script execution until the primary workload terminates natively
wait $APP_PID

# Clean termination of isolated polling loops
kill -TERM $HOST_PROF_PID $GPU_PROF_PID 2>/dev/null

```
