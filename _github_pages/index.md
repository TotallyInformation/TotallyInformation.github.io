---
---
# GitHub Pages and Jekyll/Liquid

Hints and tips on using Jekyll for publishing to GitHub Pages.

<ul>
{% for item in site.github_pages %}
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

{% include footer.html %}