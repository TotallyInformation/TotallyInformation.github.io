---
---
# Node-RED Questions and Answers

<ul>
{% for item in site.nr_qa %}
  <li>
    <a href="{{ item.url }}">{{ item.title | replace:'_',' ' }}</a> ({{ item.date | default: site.time | date: "%Y-%m-%d %H:%M" }})
    <p>{% if item.description %}
        {{ item.description }}
    {% else %}
        {{ item.excerpt | strip_html }}
    {% endif %}</p>
  </li>
{% endfor %}
</ul>

## TEST

{% for item in site.nr_qa %}
    <pre><code>
    {{ item | first | jsonify }}
    </code></pre>
{% endfor %}
