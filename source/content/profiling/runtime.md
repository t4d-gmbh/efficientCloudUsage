## Runtime

Resource profiling is categorized into three primary architectural tiers.
The selection of methodology determines the degree of measurement interference, temporal accuracy, and the overall reproducibility of the analytical pipeline.

---

### Infrastructure-Level Profiling (Workload Manager)

Data collection is executed passively by optimized daemons integrated directly into the compute node architecture.
This methodology operates independently of the application runtime and avoids the overhead of localized polling loops.
It is prioritized for generating highly reproducible, objective system-level telemetry.

* **Primary Tools:** SLURM `acct_gather_profile/hdf5` plugin, `sh5util`.
* **Underlying Principle:** System metrics (Energy, CPU, Memory, Network) are aggregated at the hardware/OS level by the scheduling daemon and written to standardized, open-source HDF5 binary archives.

#### Example (SLURM Batch Script)

```bash
#!/bin/bash
#SBATCH --job-name=infrastructure_profiling
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --time=00:30:00
#SBATCH --output=job_%j.out

# The --profile=all directive instructs the SLURM step launcher to activate node-level telemetry.
srun --profile=all ./my_application

# Post-execution consolidation of distributed binary archives into a single reproducible dataset.
# Note: Executable only if acct_gather.conf permissions permit user access.
sh5util -j $SLURM_JOB_ID

```

---

### Application-Level Profiling (Native Instrumentation)

Telemetry hooks are inserted directly into the application's runtime environment or memory allocator.
This methodology eliminates temporal misalignment by directly correlating hardware utilization with specific computational operations.
It is the optimal approach for granular hardware analysis, specifically for accelerator memory tracking.

* **Primary Tools:** PyTorch Profiler, Score-P, HPCToolkit.
* **Underlying Principle:** The application programming interface (API) is instrumented to intercept and record resource allocation events natively during execution.

#### Example (PyTorch GPU Memory Profiling)

```python
import torch
import torchvision.models as models
from torch.profiler import profile, record_function, ProfilerActivity

# Initialize workload and allocate to hardware accelerator
model = models.resnet18().cuda()
inputs = torch.randn(5, 3, 224, 224).cuda()

# Instantiate the open-source profiler with memory tracking enabled
with profile(
    activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA],
    profile_memory=True,
    record_shapes=True,
    with_stack=True
) as prof:
    # Context manager correlates hardware telemetry with a specific operational block
    with record_function("model_forward_pass"):
        model(inputs)

# Export standard summary table to standard output
print(prof.key_averages().table(sort_by="cuda_memory_usage", row_limit=10))

# Export comprehensive time-series trace to an open format for reproducible visualization (e.g., via chrome://tracing)
prof.export_chrome_trace("pytorch_memory_trace.json")

```

---

### User-Space Polling Level (Concurrent Shell Processes)

Concurrent monitoring utilities are executed via background loops.
This approach is classified as a structural fallback.
Reliance on the operating system's standard process scheduler introduces resource contention and temporal misalignment, reducing the objective reproducibility of the telemetry.

* **Primary Tools:** `sysstat` (`pidstat`), `nvidia-smi` (or `rocm-smi`).
* **Underlying Principle:** The host operating system's pseudo-filesystem (`/proc`) and vendor-specific hardware drivers are queried iteratively by independent, user-space processes.

#### Example (Concurrent Bash Polling)

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
