# Pokyny pro dodavatele

Tato stránka dokumentuje některé z technických rozhodnutí, které
[Ministerstvo spravedlnosti](https://www.justice.cz) udělalo a které po svých dodavatelích vyžaduje.

Všechny služby musí splňovat [Standard pro tvorbu služeb]({{ '/standards/standard-pro-tvorbu-sluzeb' | relative_url }}), proti kterému budou vyhodnocovány.

## Standardy

{% assign standards = site.pages
  | where: "standard", true
  | group_by: "category" %}

{% for standard_group in standards %}
{% if standard_group.name != "" %}
### {{ standard_group.name }}
{% else %}
### Obecné standardy
{% endif %}

{% for standard in standard_group.items %}
- [{{ standard.title }}]({{ standard.url | relative_url }})
{% endfor %}
{% endfor %}

## Principy

{% assign principle_groups = site.pages
  | where: "principle", true %}

{% for principle in principle_groups %}
- [{{ principle.title }}]({{ principle.url | relative_url }})
{% endfor %}


## Návody

{% assign guides = site.pages
  | where: "guide", true
  | group_by: "category" %}

{% for guide_group in guides %}
{% if guide_group.name != "" %}
### {{ guide_group.name }}
{% else %}
### Obecné návody
{% endif %}

{% for guide in guide_group.items %}
- [{{ guide.title }}]({{ guide.url | relative_url }})
{% endfor %}
{% endfor %}