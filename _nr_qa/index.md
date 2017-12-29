---
---
# Node-RED Questions and Answers

<ul>
{% for item in site.nr_qa %}
  <li>
    <a href="{{ item.url }}">{{ item.title }}</a> {% if item.date %}({{ item.date }}){% endif %}
    <p>{% if item.description %}{{ item.excerpt }}{% else %}{{ item.excerpt }}{% endif %}</p>
  </li>
{% endfor %}
</ul>

## TEST

{% for item in site.nr_qa | first %}
    <ul>
    {% for i in item %}
        <li>{{ i }}</li>
    {% endfor %}
    </ul>
{% endfor %}
