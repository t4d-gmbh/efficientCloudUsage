## GPU-NVMe Latency Comparison

> **Note:** These figures represent typical ranges from various benchmarks and can vary significantly based on hardware generation, workload patterns, and specific implementations.

{% if page %}

### Latency Overview

{% endif %}

| Configuration | Typical One-Way Latency | Round-Trip Latency | Key Characteristics |
|---------------|------------------------|--------------------|---------------------|
| **Local NVMe (PCIe) - Standard** | 30–100 μs | 60–200 μs | CPU-mediated, kernel stack involved |
| **Local NVMe (PCIe) - GPU Direct Storage** | 20–60 μs | 40–120 μs | GPU↔SSD direct, CPU bypass |
| **InfiniBand - NVMe-oF/RDMA** | 60–170 μs | 120–340 μs | CPU bypass, lossless fabric, sub-10 μs network |
| **InfiniBand - TCP/IP (no RDMA)** | 200–500 μs | 400–1000 μs | CPU overhead, interrupt handling, context switches |
| **Ethernet - RoCE v2 (RDMA)** | 70–200 μs | 140–400 μs | Similar to IB RDMA, requires lossless Ethernet |
| **Ethernet - TCP/IP (no RDMA)** | 300–800 μs | 600–1600 μs | Highest overhead, best-effort network |

{% if page %}

### Key Observations

#### 1. Local vs. Remote
- **Local NVMe** is fastest because there's no network hop
- **GPU Direct Storage** provides the biggest local improvement (~30–50% reduction)
- **NAND flash physics** (50–150 μs) remains the dominant bottleneck regardless of transport

#### 2. RDMA Impact

| Transport | With RDMA | Without RDMA | Improvement |
|-----------|-----------|--------------|-------------|
| **InfiniBand** | 60–170 μs | 200–500 μs | ~3–4× faster |
| **Ethernet** | 70–200 μs | 300–800 μs | ~3–5× faster |

#### 3. RoCE Clarification

RoCE (RDMA over Converged Ethernet) comes in two versions:
- **RoCE v1**: Layer 2 only (same subnet)
- **RoCE v2**: Layer 3 routable (more common in practice)

Both require **lossless Ethernet** (Priority Flow Control) to work reliably, which adds complexity compared to standard TCP/IP.

### Practical Recommendations

| Use Case | Recommended Configuration |
|----------|---------------------------|
| **Single-node AI training** | GPU Direct Storage + Local NVMe |
| **Multi-node cluster (low latency)** | InfiniBand + NVMe-oF/RDMA |
| **Multi-node cluster (cost-sensitive)** | RoCE v2 + NVMe-oF/RDMA |
| **General purpose / mixed workloads** | Ethernet TCP/IP (simpler, adequate for many cases) |

### Important Caveats

1. These are **one-way latencies** for a single I/O operation. Throughput and queue depth matter more for sustained workloads.
2. **Tail latency** can be significantly higher than average, especially without RDMA.
3. **Hardware matters**: Enterprise NVMe controllers, PCIe 4.0/5.0, and NIC generation all affect these numbers.
4. **Workload dependent**: Sequential reads benefit more from bandwidth; random I/O is latency-bound.

{% endif %}
