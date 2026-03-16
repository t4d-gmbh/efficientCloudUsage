### Application-Level (Native Instrumentation)

{% if slide %}

* **Mechanism:** Telemetry hooks are inserted directly into the application's runtime environment or memory allocator.
* **Advantage:** Eliminates temporal misalignment by directly correlating hardware utilization with specific computational operations. Optimal for granular hardware analysis (e.g., accelerator memory tracking).
* **Primary Tools:** PyTorch Profiler, Score-P, HPCToolkit.
* **Principle:** The application programming interface (API) is instrumented to intercept and record resource allocation events natively during execution.
{% endif %}

{% if page %}
Telemetry hooks are inserted directly into the application's runtime environment or memory allocator. This methodology eliminates temporal misalignment by directly correlating hardware utilization with specific computational operations. It is the optimal approach for granular hardware analysis, specifically for accelerator memory tracking.

* **Primary Tools:** PyTorch Profiler, Score-P, HPCToolkit.
* **Underlying Principle:** The application programming interface (API) is instrumented to intercept and record resource allocation events natively during execution.
{% endif %}

**Example (PyTorch GPU Memory Profiling):**

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
