---
layout: page
title: "Commands"
---

These are the commands available to build routines.

---

{% for rule in site.data.vCurrent.commands %}
  - [{{rule.name}}](#{{ rule.name | replace: ' ', '_'}})
{% endfor %}

---
<div>
{% for rule in site.data.vCurrent.commands %}
	<div id="{{ rule.name | replace: ' ', '_'}}">
	<h3>{{ rule.name }}</h3>
	<p>{{ rule.description }}</p>
	{% if rule.inputs %}
		<h5>Inputs</h5>
		<ul>
		{% for i in rule.inputs %}
			<li>{{ i[0] }}: {{ i[1] }}</li>
		{% endfor %}
		</ul>
	{% endif %}
	{% if rule.outputs %}
  <h5>Outputs</h5>
  <ul>
	{% for i in rule.outputs %}
		<li>{{ i[0] }}: {{ i[1] }}</li>
	{% endfor %}
  </ul>
	{% endif %}
	</div>
	<br>
{% endfor %}
</div>