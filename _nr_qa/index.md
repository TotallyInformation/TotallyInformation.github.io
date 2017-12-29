---
---
# Node-RED Questions and Answers

<ul>
{% for item in site.nr_qa %}
  <li>
    <a href="{{ item.url }}">{{ item.title | replace:'_',' ' }}</a> ({{ item.date | default: site.time }})
    <p>{% if item.description %}
        {{ item.excerpt }}
    {% else %}
        {{ item.excerpt }}
    {% endif %}</p>
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

{!
    <li>next</li>

    <li>path</li>

    <li>output</li>

    <li>url</li>

    <li>content</li>

    <li>previous</li>

    <li>relative_path</li>

    <li>id</li>

    <li>collection</li>

    <li>excerpt</li>

    <li>draft</li>

    <li>categories</li>

    <li>layout</li>

    <li>title</li>

    <li>slug</li>

    <li>ext</li>

    <li>tags</li>
!}
