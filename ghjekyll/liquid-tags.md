---
title: 'Jekyll/Liquid Tags'
description: 'This is a list of `tags` that can be used in Jekyll.'
comments: true
---

Tags appear within {% raw %}`{% tagname %}`{% endraw %} markup.
Matching end-tags are shown in brackets.

GitHub pages use Jekyll which in turn uses [Liquid](https://help.shopify.com/themes/liquid).
See also the [official tags reference](https://help.shopify.com/themes/liquid/tags).

{% comment %}See also: [Filters]({% link ghjekyll/liquid-filters.md %}), [Tags]({% link ghjekyll/liquid-tags.md %}), [Attributes]({% link ghjekyll/standard-attributes.md %}), [Test Page]({% link ghjekyll/test2.md %}){% endcomment %}

### Control flow tags
- [case](https://help.shopify.com/themes/liquid/tags/control-flow-tags#case-when) (when, else, endcase) - a switch statement
- if (else, elsif, endif) - execute block only if the condition is met

  Note the odd spelling of `elsif` (e.g. no second `e`)

  Conditional tests support `and` and `or` to link conditions together
  e.g. {% raw %}`{% if line_item.grams > 20000 and customer_address.city == 'Ottawa' %}`{% endraw %}

- raw (endraw) - stops Liquid from parsing the content so that you can include double braces for example.
- unless (endunless) - execute block only if the condition is **not** met

### Iteration tags
* [cycle](https://help.shopify.com/themes/liquid/tags/iteration-tags#cycle) - must be used in a for-loop. Cycles through the parameters
  e.g. {% raw %}`{% cycle 'a', 'b' %} {% cycle 'a', 'b' %} {% cycle 'a', 'b' %}  #=> a b a`{% endraw %}

  Can also specify a group name - see the documentation for more complex examples.

* for (endfor, else, break, continue) - basic loop.

  For-loop parameters:
  * Limit the iterations using the `limit` parameter
    e.g. {% raw %}`{% for item in numbers limit:2 %} {{ item }} {% endfor %}`{% endraw %}

  * Use the `offset` parameter to start at a particular index.

  * Use the _range_ parameter to create a number range to loop over, range numbers can be variables.
    e.g. {% raw %}`{% for i in (1..100) %} {{ i }} {% endfor %}`{% endraw %}

  * Use the `reversed` parameter to reverse the order of the loop

  For-loop variables - in a for-loop, we get the `forloop` object ([ref](https://help.shopify.com/themes/liquid/objects/for-loops)):
  * forloop.first/last - true when the loop is at the first/last iteration
  * forloop.index/index0 - 1/0-based index number for the current iteration
  * forloop.rindex/rindex0 - 1/0-based reverse index number for the current iteration
  * forloop.length - the length of the array being looped over

* [tablerow](https://help.shopify.com/themes/liquid/tags/iteration-tags#tablerow) (endtablerow) - Generate rows for an HTML table.

  Includes parameters: `cols` (defines the number of columns), `limit`, `offset`, `range`, `first`, `col`, `col_first` and others. See the [documentation](https://help.shopify.com/themes/liquid/objects/tablerow) for details

### Theme/rendering tags
* comment (endcomment)
* form
* [include](https://jekyllrb.com/docs/includes/)
* include_relative
* javascript
* layout
* paginate
* raw (endraw) - temporarily disables tag processing. Useful for generating content (eg, Mustache, Handlebars) which uses conflicting syntax.
* schema
* section
* stylesheet

* [highlight](https://jekyllrb.com/docs/templates/#code-snippet-highlighting) - render a code block with syntax highlighting, optional line numbers. Note that filters are processed inside code blocks so wrap the block with {% raw %}{% raw %}{% endraw %} and endraw tags if it contains curly braces. To find the appropriate identifier to use for the language you want to highlight, look for the “short name” on the [Rouge wiki](https://github.com/jayferd/rouge/wiki/List-of-supported-languages-and-lexers) or the [Pygments’ Lexers page](http://pygments.org/docs/lexers/).

* [link](https://jekyllrb.com/docs/templates/#links) - create an html link to another page in the Jekyll site. **WARNING: MUST CONTAIN A VALID SOURCE DOCUMENT NAME including folder and source extension otherwise the Jekyll build fails - on GitHub an immensely unhelpful generic error message is produced that does not allow you to identify the document causing the issue.**

### Variable tags
* assign - Assign a value to a variable. e.g. {% raw %}`{% assign name = 'freestyle' %}`{% endraw %}
* capture (endcapture) - Combine a number of outputs and assign to a variable.
  e.g. {% raw %}`{% capture attribute_name %}{{ item.title | handleize }}-{{ i }}-color{% endcapture %}`{% endraw %}
* decrement
* increment

