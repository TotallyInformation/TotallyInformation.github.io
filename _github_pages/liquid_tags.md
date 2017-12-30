---
---
This is a list of `tags` that can be used in Jekyll. Tags appear within {% raw %}`{% tagname %}`{% endraw %} markup.
Matching end-tags are shown in brackets.

GitHub pages use Jekyll which in turn uses [Liquid](https://help.shopify.com/themes/liquid).
See also the [official tags reference](https://help.shopify.com/themes/liquid/tags).

### Control flow tags
- [case](https://help.shopify.com/themes/liquid/tags/control-flow-tags#case-when) (when, else, endcase) - a switch statement
- if (else, elsif, endif) - execute block only if the condition is met
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

* [tablerow](https://help.shopify.com/themes/liquid/tags/iteration-tags#tablerow) (endtablerow) - Generate rows for an HTML table.

  Includes parameters: `cols` (defines the number of columns), `limit`, `offset`, `range`. See the documentation for details

### Theme/rendering tags
* comment
* form
* include
* javascript
* layout
* paginate
* raw
* schema
* section
* stylesheet

### Variable tags
* assign
* capture
* decrement
* increment

Notes:

- Note the odd spelling of `elsif` (e.g. no second `e`)
- Conditional tests support `and` and `or` to link conditions together
  e.g. {%raw}`{% if line_item.grams > 20000 and customer_address.city == 'Ottawa' %}`{% endraw %}
