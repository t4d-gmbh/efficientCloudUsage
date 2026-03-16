## Connection Fabric

```{figure} ./../_static/fabric.png
:alt: Intra- and Inter-Node Architecture
:width: 100%
```

{% if slide %}
:::{admonition} Key takeaway
:class: info
_Inter‑host_ GPU-GPU:  
**High latency** (≈ 2-4µs) and **throughput ≈ 20 GB/s** for InfiniBand HDR.  
_Exception: NVSwitch (~0.1 µs) and hundreds of GB/s._

_Intra‑host GPU-GPU_:

- PCIe Gen 5 x16 limits to ≈ 63 GB/s per direction, latency ≈ 0.5 µs (≈ 1 µs on Gen 4).
- NVLink (2 – 12 links) provides ≥ 100 GB/s per link and latency ≈ 0.1 µs.
- Via‑CPU (GPU → host → GPU) adds roughly ×2 latency and reduces effective bandwidth to ≈ ½ of the raw PCIe rate.
:::
{% endif %}

{% if page %}

## GPU Communication Overview

Modern high‑performance systems move data between graphics processors in two distinct contexts: across separate physical machines (inter‑host) and within the same machine (intra‑host).
The choice of interconnect determines both the latency (the time a small message needs to travel) and the throughput (the amount of data that can be streamed continuously).
Below is a concise description of the typical characteristics of each path.

### Inter‑Host GPU ↔ GPU (InfiniBand)

When GPUs reside on different nodes, the data must travel through the host’s PCI‑Express interface, the CPU’s memory subsystem, and an InfiniBand network adapter before reaching the remote host.
A minimal‑size packet therefore experiences a one‑way latency of roughly 1 – 2 µs, which translates to a round‑trip latency of 2 – 4 µs.

A single InfiniBand HDR link operates at a raw line rate of 200 Gb/s, corresponding to a theoretical 25 GB/s per direction.
Because the payload is copied from GPU memory to host memory, handed to the NIC, transmitted across the fabric, and then copied back into the remote GPU, the practical sustained throughput settles around 20 GB/s.

An exception to this rule is the NVSwitch fabric, which resides inside a chassis and provides a silicon‑level, NVLink‑class connection between GPUs.
NVSwitch reduces the latency to roughly 0.1 µs and delivers aggregate bandwidth in the hundreds of GB/s, approaching a terabyte per second when many GPUs are interconnected.

### Intra‑Host GPU ↔ GPU

Inside a single server the communication path is much shorter, and the performance depends primarily on the PCI‑Express generation or on NVLink.

#### PCI‑Express

A GPU attached to a PCIe Gen 5 x16 slot can exchange data at up to approximately 63 GB/s in each direction.
The latency for a small message is about 0.5 – 0.8 µs.
The older Gen 4 generation offers roughly half the bandwidth (around 31 GB/s) and a latency in the range of 0.8 – 1.2 µs.

#### NVLink

NVLink provides a dedicated high‑speed link between GPUs.
The newest NVLink 3.0 version supplies at least 100 GB/s per link, while the previous NVLink 2.0 delivers about 50 GB/s.
Latency is extremely low, on the order of a tenth of a microsecond (typically 0.15 – 0.25 µs). Multiple links can be aggregated, further raising the total bandwidth available between a pair of GPUs.

#### GPU → CPU → GPU (host‑mediated copy)

If a transfer must pass through host memory (GPU to host, then host to the second GPU) the data traverses the PCIe bus twice.
This doubles the effective latency to roughly 1 – 1.5 µs on Gen 5 and cuts the usable bandwidth to about half of the raw PCIe rate, i.e., around 30 GB/s on Gen 5 and 15 GB/s on Gen 4.
The extra hop also introduces additional software overhead in the kernel and driver stacks.

### Summary

Intra‑node communication using NVLink or PCIe delivers sub‑microsecond latency and tens of gigabytes per second of bandwidth, making it ideal for tightly coupled GPU workloads.
Inter‑node communication over InfiniBand adds a few microseconds of latency and caps sustainable throughput near 20 GB/s per HDR link, unless an NVSwitch is employed to keep the traffic on‑chip, in which case latency drops dramatically and bandwidth rises to the hundreds‑of‑GB/s regime.
Understanding these limits helps architects choose the right topology for scaling GPU‑centric applications across single servers and large clusters.
{% endif %}
