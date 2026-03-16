# <i class="fa-solid fa-rotate-left"></i> Recap: HPC Clusters

```{admonition} Part 1 Refresher
:class: tip, margin

Revisiting core concepts from [Part 1](https://t4d-gmbh.github.io/utilizingSharedResources/).
```

{% if slide %}
<!-- BUILDING THE SLIDES -->
```{toctree}
:maxdepth: 1

./clusterSetup
./sharedParallelFilesystem
./featuresAndLimitations
./interaction
./sshSecurity
```

{% else %}
<!-- BUILDING THE PAGES -->

```{include} ./clusterSetup.md
```
```{include} ./sharedParallelFilesystem.md
```
```{include} ./featuresAndLimitations.md
```
```{include} ./interaction.md
```
```{include} ./sshSecurity.md
```

{% endif %}
