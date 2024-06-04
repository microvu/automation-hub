---
layout: page
title: "Axioms"
---

<style>
#content>div {
  max-width: 100rem;
}

img {
  display: block;
  margin: 0 auto;
}
</style>

These rules serve as a basis for creating automation routines.
They are enforced by the software so do not worry about breaking the rules; it helps to be familiar with them though.

---

{% for rule in site.data.vCurrent.axioms %}
  - [{{rule.name}}](#{{ rule.name | replace: ' ', '_'}})
{% endfor %}

---

<div class="rule-list">
{% for rule in site.data.vCurrent.axioms %}
	<div id="{{ rule.name | replace: ' ', '_'}}">
	<h3>{{ rule.name }}</h3>
	{% for d in rule.data %}
		<p>{{ d.description }}</p>
		<div><img src="{{ site.baseurl }}/img/axioms/{{ d.img }}" /></div>
	{% endfor %}
	</div>
{% endfor %}
</div>