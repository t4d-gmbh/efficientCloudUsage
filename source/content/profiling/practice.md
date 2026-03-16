## Performance Profiling: Practical Execution

{% if page %}
The theoretical understanding of profiling tools is solidified through empirical testing across different hardware architectures. The following exercises isolate specific storage tiers to measure their distinct latency and throughput characteristics.
{% endif %}

### Phase 1: Local / Cloud Virtual Machine

Evaluate the performance discrepancy between locally attached block storage and network-attached object storage from the perspective of an isolated virtual machine.

1. **Block Storage (`fio`):** Execute the random latency and sequential throughput `fio` commands detailed above within the `/tmp` directory of the VM. Record the IOPS and MB/s metrics.
2. **Object Storage (`wrk`):** Upload a 1GB dummy file to the S3/Swift object storage bucket. Execute the `wrk` command against the object's public or pre-signed URL. Compare the HTTP response latency to the block storage disk latency.

### Phase 2: HPC Cluster (Shared Filesystem)

Evaluate the performance characteristics of an enterprise distributed parallel filesystem compared to object storage.

1. **Shared Filesystem (`fio`):** Allocate an interactive compute node (`salloc`). Execute the sequential throughput `fio` test within the high-performance scratch directory (e.g., `/scratch` or `$SCRATCH`).
* *Note:* Do not execute the random I/O test with high thread counts on the shared parallel filesystem without prior authorization.


2. **Object Storage (`wrk`):** From the same compute node, execute the `wrk` test against the remote object storage bucket. Observe the bandwidth limitations imposed by the cluster's external network uplink compared to the internal InfiniBand/Ethernet fabric used by the shared filesystem.

