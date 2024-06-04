---
layout: page
title: "Terminology"
---


<style>
img {
  display: block;
  margin: 0 auto;
}

</style>


{% for rule in site.data.vCurrent.terminology %}
  - [{{rule.name}}](#{{ rule.name | replace: ' ', '_'}})
{% endfor %}

---

<div class="rule-list">
{% for rule in site.data.vCurrent.terminology %}
	<div id="{{ rule.name | replace: ' ', '_'}}">
	<h3>{{ rule.name }}</h3>
	{% for d in rule.data %}
		<p>{{ d.description }}</p>
		<div><img src="{{ site.terminology_img }}/{{ d.img }}" /></div>
	{% endfor %}
	</div>
{% endfor %}
</div>