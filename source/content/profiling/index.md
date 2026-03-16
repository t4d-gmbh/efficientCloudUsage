# <i class="fa-solid fa-magnifying-glass-chart"></i> Profiling

{% if slide %}
<!-- BUILDING THE SLIDES -->
```{toctree}
:maxdepth: 1

./tools
./runtime
./runtime_infra
./runtime_application
./runtime_userspace
./practice

```

{% else %}
<!-- BUILDING THE PAGES -->

```{include} ./tools.md
```
```{include} ./runtime.md
```
```{include} ./runtime_infra.md
```
```{include} ./runtime_application.md
```
```{include} ./runtime_userspace.md
```
```{include} ./practice.md
```

{% endif %}
