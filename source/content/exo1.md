## ✏️ Exercise 1 ✏️

```{epigraph}
{.centered}
**Conainer Deployment**
```

```{epigraph}
{.centered}
*Recall:*
```

::::{grid} 1 3 3 3
:gutter: 2

:::{grid-item-card} CI/CD Builds
Container builds can be automated, facilitating distribution an access:

```bash
apptainer pull env.sif oras://ghcr.io/<owner>/<env-sif>:1.0.0
```
:::
:::{grid-item-card} Dedicated build environment
A dedicated build environment can facilitate development, testing and debugging.

Object storage can facilitate the transport of container binaries from the build to the production environment.
:::
:::{grid-item-card} Local builds
A container build is a computational workload and should be treated as such, also on an HPC cluster.

```{admonition} Detached build environments
:class: warning
A conatiner build should be as independent from its build environment as possible.
```
:::
::::

{.centered}
Head over to the [HelloWorld](https://github.com/pSciComp/exoHelloWorld) repository and have a look at [`HPC - Exo 1`](https://github.com/pSciComp/exoHelloWorld/blob/main/exercises/hpc/Exo_1.md).
