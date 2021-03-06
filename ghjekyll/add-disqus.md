---
title: 'Add Disqus to Jekyll pages'
description: 'Example code to add to every page as a footer to enable the Disqus comments service.'
comments: true
testFm: testing front matter variables
date: 2018-01-02 17:11:00
---

Create `footer.html` in your site `_includes` folder or include this in the default layout in `_layouts`.

{% raw %}
```html
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

Note that the default `minima` theme for Jekyll/GitHub pages includes a setting for this in _config.yml but currently does **not** include the appropriate include file to let you use it!

Also note that you have very little control over the styling. In particular, the width is set to 100% so make sure that this code is wrapped in a div that is limited to your normal content width.
