
### Infrastructure-Level (Workload Manager)

{% if slide %}

* **Mechanism:** Passive data collection executed by optimized daemons integrated directly into the compute node architecture.
* **Advantage:** Operates independently of the application runtime, avoiding localized polling loop overhead. Prioritized for highly reproducible, objective system-level telemetry.
* **Primary Tools:** SLURM `acct_gather_profile/hdf5` plugin, `sh5util`.
* **Principle:** System metrics (Energy, CPU, Memory, Network) are aggregated at the hardware/OS level by the scheduling daemon and written to standardized, open-source HDF5 binary archives.
{% endif %}

{% if page %}
Data collection is executed passively by optimized daemons integrated directly into the compute node architecture. This methodology operates independently of the application runtime and avoids the overhead of localized polling loops. It is prioritized for generating highly reproducible, objective system-level telemetry.

* **Primary Tools:** SLURM `acct_gather_profile/hdf5` plugin, `sh5util`.
* **Underlying Principle:** System metrics (Energy, CPU, Memory, Network) are aggregated at the hardware/OS level by the scheduling daemon and written to standardized, open-source HDF5 binary archives.
{% endif %}

**Example (SLURM Batch Script):**

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

