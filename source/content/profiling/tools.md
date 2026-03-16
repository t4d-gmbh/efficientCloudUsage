## Tools

{% if page %}
The identification of resource bottlenecks is achieved through the utilization of specialized monitoring and accounting tools.These tools allow for the empirical measurement of Central Processing Unit (CPU), memory, Input/Output (I/O), and network utilization across different storage and compute architectures.
{% endif %}

::::::{tab-set}
:::::{tab-item} Slurm
{% if page %}
Job accounting data is retrieved via the `sacct` and `sstat` utilities. This allows for real-time and retrospective analysis of resource consumption. While `sacct` queries the accounting database for historical execution metrics, `sstat` provides near real-time telemetry for active job steps.
{% endif %}

```bash
# Retrospective: List all past jobs for a specific user since the specified date
sacct -u <username> -S 2026-01-01 --format=JobID,State,NodeList,CPUTime,MaxRSS,MaxRSSNode

# Retrospective: Detailed resource accounting for a specific job ID
sacct -j <jobID> --format=JobID,JobName,State,AllocCPUS,CPUTimeRAW,TotalCPU,ReqMem,AveRSS,MaxRSS

# Real-time: View active resource utilization for a running job step
sstat -j <jobID.stepID> --format=JobID,AveCPU,AveRSS,MaxRSS,MaxDiskRead,MaxDiskWrite

# (Plugin) High-frequency data collection: Request continuous profiling during submission
sbatch --profile=all my_script.sh

```
:::::


:::::{tab-item} Unix
{% if page %}
General-purpose UNIX utilities provide high-frequency observation of the current system state.
{% endif %}

| Command | Function |
| --- | --- |
| `btop` | Interactive resource monitor (CPU, memory, disks, network). |
| `sar -u 1 3` | Reports CPU utilization at 1-second intervals. |
| `lshw -short` | Generates a brief hardware inventory. |
| `iostat -x 1` | Reports extended CPU and device input/output statistics at 1-second intervals. |
| `nvidia-smi` | Monitors NVIDIA GPU utilization, power draw, and memory allocation. |
| `nvtop` | Interactive GPU resource monitor (supports NVIDIA, AMD, and Intel GPUs). |
:::::
:::::{tab-item} I/O — `fio`
{% if page %}
The Flexible I/O Tester (`fio`) is utilized for benchmarking storage backends by simulating precise I/O workloads. Throughput (bandwidth) is typically evaluated using sequential operations with large block sizes, whereas latency (IOPS) is assessed using random operations with small block sizes. The `--direct=1` parameter is crucial as it bypasses the operating system's buffer cache, ensuring measurements reflect the underlying hardware capabilities.
{% endif %}

```{admonition} Intensive Throughput Warning
:class: warning
Execution of `fio` on shared filesystems (e.g., CephFS, NFS, Lustre) generates heavy synthetic load that may severely degrade performance for concurrent users. Coordination with system administrators is required before execution on shared infrastructure.

```

{% if page %}
The provided `clat` output string details the performance profile of an exceptionally fast storage operation.
To properly translate these values into an architectural understanding of latency, the base unit of measurement must first be contextualized.
The execution engine reports specific values in nanoseconds (`nsec`).
In standard storage benchmarking, latency is more commonly discussed in microseconds ($\mu$s) or milliseconds (ms).

Standard mechanical Hard Disk Drives (HDDs) operate in the millisecond range (e.g., 5,000 to 10,000 $\mu$s). Standard SATA Solid State Drives (SSDs) operate in the mid-microsecond range (e.g., 100 to 500 $\mu$s).

{% endif %}

::::{grid} 1 2 2 2
:gutter: 2

:::{grid-item}
```bash
# Throughput (Bandwidth): Sequential Read/Write (1MB blocks, queue depth 16)
fio --name=seq_throughput --directory=/var/tmp/test --rw=readwrite --bs=1M \
    --time_based --runtime=60 \
    --size=10G --numjobs=1 --iodepth=16 --direct=1

# Latency (IOPS): Random Read/Write (4KB blocks, queue depth 1, 4 threads)
fio --name=rand_latency --directory=/tmp/test --rw=randrw --bs=4k \
    --time_based --runtime=60 \
    --size=10G --numjobs=4 --iodepth=1 --direct=1

```
**Base Metrics (`clat`)**
* _Example:_ Standard Deviation (`stdev=412`) corresponds to **0.41 μs**.


:::
:::{grid-item}

```toml
# bench.fio
[global]
directory=${FIO_TEST_DIR}
size=10G
time_based
runtime=60
direct=1
iodepth=1
numjobs=1

[throughput]
rw=readwrite
bs=1M
numjobs=1
iodepth=16

[latency]
rw=randrw
bs=4k
numjobs=4
iodepth=1
```
```bash
FIO_TEST_DIR=/var/tmp/test fio bench.fio --section=latency
```
:::
::::

:::::

:::::{tab-item} `wrk`
{% if page %}
The [`wrk`](https://github.com/wg/wrk) utility is a modern HTTP benchmarking tool capable of generating significant load utilizing a single multi-core CPU. It is deployed to empirically measure the response latency and maximum throughput of RESTful endpoints, making it the standard tool for profiling object storage gateways (e.g., S3, Swift) where data is retrieved over HTTP/HTTPS rather than a POSIX filesystem.
{% endif %}

```bash
# Throughput and Latency: HTTP GET requests against an object storage endpoint
# Configuration: 12 threads, 100 concurrent connections, 30-second duration
wrk -t12 -c100 -d30s --latency "https://object-storage.example.com/bucket/test-file.bin"

```

:::::
::::::
