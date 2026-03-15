# <i class="fa-solid fa-server"></i> HPC Clusters at UZH

```{admonition} Two Systems
:class: tip, margin
UZH researchers have access to both a local Science Cluster and a national Supercomputer.
```

UZH provides its researchers with two distinct HPC systems, each designed for different scales of computation.

{% if slide %}
<!-- BUILDING THE SLIDES -->
```{toctree}
:maxdepth: 3

./overview
./scienceCluster
./scienceClusterStorage
./supercomputer
./access

```

{% else %}
<!-- BUILDING THE PAGES -->

```{include} ./overview.md
```
```{include} ./scienceCluster.md
```
```{include} ./scienceClusterStorage.md
```
```{include} ./supercomputer.md
```
```{include} ./access.md
```

{.smaller}
**Sources**:
<https://docs.s3it.uzh.ch/cluster/overview/>
<https://docs.s3it.uzh.ch/cluster/resources/>
<https://docs.s3it.uzh.ch/cluster/data/>
<https://docs.cscs.ch/clusters/>
<https://docs.s3it.uzh.ch/supercomputer/overview/>
{% endif %}
