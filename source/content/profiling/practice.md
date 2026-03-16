## 👷 Practical Part 👷

{% if page %}
The theoretical understanding of profiling tools is solidified through empirical testing across different hardware architectures. The following exercises isolate specific storage tiers to measure their distinct latency and throughput characteristics.

{% else %}
::::::{tab-set}
{% endif %}
{% if slide %}:::::{tab-item}{% else %}###{% endif %} Local vs. Cloud VM
### Local vs. Cloud VM

Evaluate the performance discrepancy between a locally attached block storage device and network-attached object storage from the perspective of an isolated virtual machine.

Use `fio` to evaluate the read/write performance discrepancy between a locally attached block storage device and network-attached block storage on a virtual machine.

Check the discreapancy between volatile temporal storage (typically `/tmp/`) and non-volatile temporal storage (typically `/var/tmp/`).

- Use `df` to check the type of temporal storage.
- Use `fio` to perform a read/write profiling.


:::{admonition} `fio` overwrites!
:class: warning

`fio` will attempt to overwrite the provided destination!
:::

{% if slide %}
:::::
:::::{tab-item}{% else %}###{% endif %} HPC Cluster (Shared Filesystem)

2. **Object Storage (`wrk`):** Upload a 1GB dummy file to the S3/Swift object storage bucket. Execute the `wrk` command against the object's public or pre-signed URL. Compare the HTTP response latency to the block storage disk latency.


Evaluate the performance characteristics of an enterprise distributed parallel filesystem compared to object storage.

1. **Shared Filesystem (`fio`):** Allocate an interactive compute node (`salloc`). Execute the sequential throughput `fio` test within the high-performance scratch directory (e.g., `/scratch` or `$SCRATCH`).
* *Note:* Do not execute the random I/O test with high thread counts on the shared parallel filesystem without prior authorization.


2. **Object Storage (`wrk`):** From the same compute node, execute the `wrk` test against the remote object storage bucket. Observe the bandwidth limitations imposed by the cluster's external network uplink compared to the internal InfiniBand/Ethernet fabric used by the shared filesystem.

{% if slide %}
:::::
::::::
{% endif %}

