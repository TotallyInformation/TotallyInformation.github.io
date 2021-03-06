---
title: A Page to test things
description: GitHub pages use Jekyll but not all features are available. This page is for testing what works and what doesn\'t
comments: false
layout: home
borderColor2: silver
category: test
date: 2018-01-02 14:20:00
testFm: testing front matter variables
---

{% assign borderColor = 'silver' %}

<div style="border:1px solid {{ borderColor }}">
  <p>Do we get site variables defined in _config.yml? Not everything.</p>
  <p>
    &#10004; 'site.remote-theme': --{{ site.remote-theme }}--<br>
    &#10004; 'site.title': --{{ site.title }}--<br>
    &#10004; 'site.disqus.shortname': --{{ site.disqus.shortname }}--<br>
    &times;  'site.collections.nr-qa.title': --{{ site.collections.nr-qa.title }}-- : Nothing from collections is directly accessible.<br>
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>&#10004; Do we get the Jekyll Environment data? Yes</p>
  <p>
    &#10004; 'jekyll.environment': --{{ jekyll.environment }}-- : should give "production" on GitHub Pages.<br>
    &#10004; 'jekyll.version': --{{ jekyll.version }}--<br>
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>Do we get all the page variables for this {% if page.collection %}collection document{% else %}Page{% endif %}? Nope.</p>
  <p>
    &#10004; 'page.url': --{{ page.url }}--<br>
    &#10004; 'page.layout': --{{ page.layout }}-- : Default for this collection is "page" but set to "home" in front matter - should show "home"<br>
    &#10004; 'page.path': --{{ page.path }}--<br>
    &times;  'page.date': --{{ page.date }}-- : Should be auto-set according to docs but apparently not.<br>
  </p>

  <p>Custom variables set in _config.yml or front matter? <b>Only available to actual Pages and NOT TO COLLECTION DOCUMENTS!</b></p>
  <p>
    &times;  'page.title': --{{ page.title }}-- : This is set in this page's front matter, it should be available! <br>
    &times;  'page.description': --{{ page.description }}-- : This is set in this page's front matter, it should be available! <br>
    &times;  'page.comments': --{{ page.comments }}-- : Is set in this page as false but is, instead showing true which is the default<br>
    &times;  'page.borderColor2': --{{ page.borderColor2 }}--: Is set in this page's front matter, it should be available!<br>
  </p>

  <p>Local assigned variables? Yes.</p>
  <p>
    &#10004; 'borderColor': --{{ borderColor }}-- : Is set in an assignment tag so is available.<br>
  </p>

  <p>Only available in collection documents{% if page.collection %} like this{% endif %}</p>
  <p>
    &#10004; 'page.id': --{{ page.id }}-- : Should be set for documents in a collection or a post{% if page.collection %} so should be available here (collection document){% endif %}.<br>
    &#10004; 'page.collection': --{{ page.collection }}-- : Should be set for documents in a collection or a post{% if page.collection %} so should be available here (collection document){% endif %}.<br>
  </p>

  <p>Only available in actual pages (not a collection document{% if page.collection %} like this{% endif %})</p>
  <p>
    &#10004; 'page.dir': --{{ page.dir }}--<br>
    &#10004; 'page.name': --{{ page.name }}--<br>
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>&#10004; Do we get the forloop variables?</p>
  <p>
  {% for item in site.github-pages %}
    {% if forloop.first == true %}
      Yes, we do! First time through! {{ forloop.index }}, {{ forloop.index0 }}, {{ forloop.rindex }}, {{ forloop.rindex0 }}, {{ forloop.length }}<br>
    {% elsif forloop.last == true %}
      This is the last iteration! {{ forloop.index }}, {{ forloop.index0 }}, {{ forloop.rindex }}, {{ forloop.rindex0 }}
    {% else %}
      Not the first time. {{ forloop.index }}, {{ forloop.index0 }}, {{ forloop.rindex }}, {{ forloop.rindex0 }}<br>
    {% endif %}
  {% else %}
    Urm, nope!
  {% endfor %}
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>Color filters? Nope, they don't work</p>
  <p>
    &times; <span style="background-color:{{ '#7ab55c' | color_to_rgb }}">Should change #7ab55c to <code>rgb(122, 181, 92)</code></span> <br>
    &times; #7ab55c colour brightness (should be 153.21): {{ '#7ab55c' | color_brightness }} <br>
    &times; <span style="background-color:{{ '#7ab55c' | color_modify: 'red', 255 }}">Modify: Should modify green to orange</span> <br>
    &times; <span style="background-color:{{ '#7ab55c' | color_lighten: 30 }}">Lighten: Should modify green to pale green</span>
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>Money filters? Nope, they don't work</p>
  <p>
    &times; {{ 145 | money }} - should give '£145.00' or similar<br>
    &times; {{ 145 | money_with_currency }} - should give '145 GBP' or similar
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>Number filters? Some work, some don't</p>
  <p>
    &times; {{ 145 | at_most: 100 }} - 'at_most: 100' should give '100' from an input of 145<br>
    &times; {{ 45 | at_least: 100 }} - 'at_least: 100' should give '100' from an input of 45 <br>
    &#10004; {{ 4.6 | ceil }} - 'ceil' should give 5 from an input of 4.6 <br>
    &#10004; {{ 345 | divided_by: 10 }} - 'divided_by' should give 34 from an input of 345
  </p>
</div>


<div style="border:1px solid {{ borderColor }}">
  <p>String filters? Some work, some don't</p>
  <p>
    &#10004; {{ "capitalize me" | capitalize }} - 'capitalize' should give 'Capitalize me'<br>
    &#10004; {{ "Hello, world. Goodbye, world." | remove: "world" }} - 'remove' should give 'Hello, . Goodbye, .'<br>
    &#10004; {{ "hello" | slice: 1, 3 }} - 'slice' should give 'ell'<br>
    &#10004; {{ "<h1>Hello</h1> World" | strip_html }} - 'strip_html' should give 'Hello World'<br>
    &times; {{ "coming-soon" | camelcase }} - 'camelcase' should give 'ComingSoon'<br>
    &times; {{ "ShopifyIsAwesome!" | sha1 }} - 'sha1' should give 'c7322e3812d3da7bc621300ca1797517c34f63b6'<br>
    &times; {{ 10 | pluralize: 'item', 'items' }} - 'pluralize' should give '10 items'<br>
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>Utility filters? Some work, some don't</p>
  <p>
    &times; {{ site.date | time_tag }} - 'time_tag' should give a date/time string wrapped in html time tags<br>
  </p>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>jsonify? See the dev console for the following:</p>
  <pre>
    {% raw %}
    &lt;script>
        (function() &#123;
            // Dump the page object to a JS variable - note we have to strip or escape the html
            var jk = &#123;&#123; page | jsonify | strip_html &#125;&#125;&#59;
            console.log(jk)
        &#125;)()&#59;
    &lt;/script>
    {% endraw %}
  </pre>
</div>

<!--
<div style="border:1px solid {{ borderColor }}">
  <p>This page object</p>
  <pre>{{ page | inspect }}</pre>
</div>

<div style="border:1px solid {{ borderColor }}">
  <p>site object</p>
  <pre>{{ site | inspect }}</pre>
</div>
-->

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        console.log('--PAGE (jsonify)--', jk_page)
        var jk_site = {{ site | jsonify | strip_html }};
        console.log('--SITE (jsonify)--', jk_site)
        var jk_pages = {{ site.pages | jsonify | strip_html }};
        console.log('--SITE.PAGES (jsonify)--', jk_pages)
        //var layout = {{ layout | jsonify }}
        //console.log('--LAYOUT (jsonify)--', layout)
    })();
</script>

