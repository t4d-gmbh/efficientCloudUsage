```{include} ../README.md
:end-before: <!-- include-before -->
```

{% if build == "slides" %}
:::{admonition} Authors
:class: note, margin
Dr. Jonas I. Liechti  
Dr. Matteo Delucchi
:::

{% else %}
### Authors

**Dr. Jonas I. Liechti**  
**Dr. Matteo Delucchi**  
{% endif %}


### Content
```{toctree}
:maxdepth: {% if build == "slides" %}1{% else %}4{% endif %}
{% if build == "slides" %}:numbered:{% endif %}

content/index
```
