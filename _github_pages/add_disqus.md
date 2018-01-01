---
title: 'Add Disqus to Jekyll pages'
description: 'Example code to add to every page as a footer to enable the Disqus comments service.'
comments: true
---

Create `footer.html` in your site `_includes` folder or include this in the default layout in `_layouts`.

{% raw %}
```
{% if page.comments %}
<div id="disqus_thread"></div>
<script>
    var disqus_config = function () {
        this.page.url = '{{ page.url | absolute_url }}'
        this.page.identifier = '{{ page.id }}'
    };
    (function() { var d = document, s = d.createElement('script');
    s.src = 'https://<<set_this_from_disqus_settings>>.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date()); (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}

<div>Updated: {{ page.date | default: site.time | date: "%Y-%m-%d %H:%M" }} <a href="#top">Top</a></div>
```
{% endraw %}

