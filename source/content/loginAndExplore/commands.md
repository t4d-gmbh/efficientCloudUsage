## Helpful Commands

{% if slide %}

::::{tab-set}
:::{tab-item} Slurm

```bash
# Check your queued/running jobs
squeue -u $USER

# Get detailed info about a specific job
sacct -j <jobid> --format=JobID,JobName,TotalCPU,ExitCode,Elapsed

# Request an interactive session on a compute node
srun --cpus-per-task=1 --mem=4G --time=00:20:00 --pty bash

# Submit a batch job
sbatch job.sh

# Cancel a job
scancel <jobid>

# View available partitions and their state
sinfo -s

# Check your account's fair-share and usage
sacctmgr show assoc user=$USER format=Account,User,Partition,GrpTRESMins
```

:::
:::{tab-item} Unix

```bash
# Filesystem overview (type, size, used, avail, mount)
df -hT -x tmpfs -x devtmpfs -x squashfs

# Show only network mounts (CephFS, NFS)
findmnt -J -t ceph,nfs,nfs4 -o TARGET,SOURCE,FSTYPE

# Disk usage of current directory
du -sh .

# Show current load average
cat /proc/loadavg

# CPU and memory overview
lscpu | head -20
free -h

# List loaded environment modules
module list

# Search for available modules
module avail 2>&1 | head -30

# Who else is on this login node?
w
```

:::
::::

{% else %}

When you first log in to the Science Cluster, a handful of commands helps you orient yourself quickly.

### Slurm Commands

| Command | Purpose |
|---|---|
| `squeue -u $USER` | List your queued and running jobs |
| `sacct -j <jobid> --format=JobID,JobName,State,ExitCode,Elapsed` | Detailed info about a finished job |
| `srun --cpus-per-task=1 --mem=4G --time=00:20:00 --pty bash` | Start an interactive session on a compute node |
| `sbatch job.sh` | Submit a batch job script |
| `scancel <jobid>` | Cancel a running or pending job |
| `sinfo -s` | View available partitions and their state |

### Unix / System Commands

| Command | Purpose |
|---|---|
| `df -hT -x tmpfs -x devtmpfs -x squashfs` | Filesystem overview (type, size, used, available) |
| `findmnt -J -t ceph,nfs,nfs4 -o TARGET,SOURCE,FSTYPE` | Show network mounts (CephFS, NFS) |
| `du -sh .` | Disk usage of the current directory |
| `cat /proc/loadavg` | Current system load average |
| `lscpu \| head -20` | CPU info |
| `free -h` | Memory overview |
| `module list` | List loaded environment modules |
| `module avail` | Search for available modules |
| `w` | List users on this login node |

{% endif %}
