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

Check the discrepancy between volatile temporal storage (typically `/tmp/`) and non-volatile temporal storage (typically `/var/tmp/`).

- Use `df` to check the type of temporal storage.
- Use `fio` to perform a read/write profiling.


:::{admonition} `fio` overwrites!
:class: warning

`fio` will attempt to overwrite the provided destination!
:::

{% if slide %}
:::::
:::::{tab-item}{% else %}###{% endif %} HPC Cluster (Shared Filesystem)

:::{admonition} Avoid performing benchmark tests
:class: warning
Benchmarking a shared filesystem is almost never a good idea!
You will be a **very noisy neighbour**!
:::

{% if slide %}
:::::
::::::
{% endif %}

