# Totally Information: Development Hub

This site is used to publish development-related information.

## Node-RED

Node-RED is a flow-based programming tool.

<ul>
{% for item in site.nr_qa %}
  <li>
    <a href="{{ item.url }}">{{ item.title }}</a> ({{ item.date }})
    <p>{{ item.description }}</p>
  </li>
{% endfor %}
</ul>

## Collections List

{% for item in site.collections | where_exp: "label", "label <> posts" | first  %}
  <h2><a href="{{ item.url }}">{{ item.title }}</a></h2>
{% endfor %}

## Pages List

<ul>
{% for item in site.nr_qa %}
  <li>
    <a href="{{ item.url }}">{{ item.title }}</a>
    <p>{{ item.description }}</p>
  </li>
{% endfor %}
</ul>

Last Update: {{ site.time }}
