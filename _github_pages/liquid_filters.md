---
title: 'Jekyll/Liquid Variable Filters'
description: 'A comprehensive list of filters available to modify/format Jekyll variables.'
comments: true
---
{% include header.html %}

GitHub pages use Jekyll which in turn uses [Liquid](https://help.shopify.com/themes/liquid).
Liquid provides the variable filters that can be used to amend/format the variable for output.
This is a more comprehensive list than you will find in most places, it is combined from multiple lists including the [source documentation](http://www.rubydoc.info/gems/liquid/Liquid/StandardFilters). For more details on these filters, see the [official filter documentation](https://help.shopify.com/themes/liquid/filters).

- abs - absolute value
- append - append a string e.g. {% raw %}`{{ 'foo' | append:'bar' }} #=> 'foobar'`{% endraw %}
- capitalize - capitalize words in the input sentence
- ceil - rounds a number up to the nearest integer, e.g. {% raw %}`{{ 4.6 | ceil }} #=> 5`{% endraw %}
- compact - Remove nils within an array provide optional property with which to check for nil.
- [concat](https://help.shopify.com/themes/liquid/filters/array-filters#concat) - Concatenates (combines) an array with another array
- date - reformat a date ([syntax reference](http://docs.shopify.com/themes/liquid-documentation/filters/additional-filters#date))

  Reformat a date using Ruby's core `Time#strftime( string ) -> string`
  <pre>
    %a - The abbreviated weekday name (``Sun'')
    %A - The  full  weekday  name (``Sunday'')
    %b - The abbreviated month name (``Jan'')
    %B - The  full  month  name (``January'')
    %c - The preferred local date and time representation
    %d - Day of the month (01..31)
    %H - Hour of the day, 24-hour clock (00..23)
    %I - Hour of the day, 12-hour clock (01..12)
    %j - Day of the year (001..366)
    %m - Month of the year (01..12)
    %M - Minute of the hour (00..59)
    %p - Meridian indicator (``AM''  or  ``PM'')
    %s - Number of seconds since 1970-01-01 00:00:00 UTC.
    %S - Second of the minute (00..60)
    %U - Week  number  of the current year,
            starting with the first Sunday as the first
            day of the first week (00..53)
    %W - Week  number  of the current year,
            starting with the first Monday as the first
            day of the first week (00..53)
    %w - Day of the week (Sunday is 0, 0..6)
    %x - Preferred representation for the date alone, no time
    %X - Preferred representation for the time alone, no date
    %y - Year without a century (00..99)
    %Y - Year with century
    %Z - Time zone name
    %% - Literal ``%'' character
  </pre>
  See also: http://www.ruby-doc.org/core/Time.html#method-i-strftime

- default - returns the given variable unless it is null or the empty string, when it will return the given value,
  e.g. {% raw %}`{{ undefined_variable | default: "Default value" }} #=> "Default value"`{% endraw %}
- default_errors -
- default_pagination -
- divided_by - integer division e.g. {% raw %}`{{ 10 | divided_by:3 }} #=> 3`{% endraw %}
- downcase - convert an input string to lowercase
- escape - html escape a string
- escape_once - returns an escaped version of html without affecting existing escaped entities
- first - get the first element of the passed in array
- floor - rounds a number down to the nearest integer, e.g. {% raw %}`{{ 4.6 | floor }} #=> 4`{% endraw %}
- group_by - group elements from array by given property: {% raw %}`{{ site.posts | group_by:"category" }}`{% endraw %}
- highlight -
- highlight_active_tag
- img_tag - generate an img html tag
- join - join elements of the array with certain character between them
- [json](https://help.shopify.com/themes/liquid/filters/additional-filters#json) - Converts a string into JSON format.

  You do not have to wrap the Liquid output in quotations - the json filter will add them in. The json filter will also escape quotes as needed inside the output. The json filter can also used to make Liquid objects readable by JavaScript.

- jsonify - convert data to JSON: {% raw %}`{{ site.data.dinosaurs | jsonify }}`{% endraw %}
- last - get the last element of the passed in array
- lstrip - strips all whitespace from the beginning of a string
- map - map/collect an array on a given property
- markdownify - convert markdown to HTML
- minus - subtraction e.g. {% raw %}`{{ 4 | minus:2 }} #=> 2`{% endraw %}
- modulo - remainder, e.g. {% raw %}`{{ 3 | modulo:2 }} #=> 1`{% endraw %}
- newline_to_br - replace each newline (`\n`) with html break
- placeholder_svg_tag
- plus - addition e.g. {% raw %}`{{ '1' | plus:'1' }} #=> 2, {{ 1 | plus:1 }} #=> 2`{% endraw %}
- prepend - prepend a string e.g. {% raw %}`{{ 'bar' | prepend:'foo' }} #=> 'foobar'`{% endraw %}
- remove - remove each occurrence e.g. {% raw %}`{{ 'foobarfoobar' | remove:'foo' }} #=> 'barbar'`{% endraw %}
- remove_first - remove the first occurrence e.g. {% raw %}`{{ 'barbar' | remove_first:'bar' }} #=> 'bar'`{% endraw %}
- replace - replace each occurrence e.g. {% raw %}`{{ 'foofoo' | replace:'foo','bar' }} #=> 'barbar'`{% endraw %}
- replace_first - replace the first occurrence e.g. {% raw %}`{{ 'barbar' | replace_first:'bar','foo' }} #=> 'foobar'`{% endraw %}
- reverse - reverses the passed in array
- round - rounds input to the nearest integer or specified number of decimals e.g. {% raw %}`{{ 4.5612 | round: 2 }} #=> 4.56`{% endraw %}
- rstrip - strips all whitespace from the end of a string
- script_tag - generate a script html tag
- size - return the size of an array or string
- slice - slice a string. Takes an offset and length, e.g. {% raw %}`{{ "hello" | slice: -3, 3 }} #=> llo`{% endraw %}
- sort - sort elements of the array
- sort_natural - Sort elements of an array ignoring case if strings provide optional property with which to sort an array of hashes or drops.
- split - split a string on a matching pattern e.g. {% raw %}`{{ "a~b" | split:"~" }} #=> ['a','b']`{% endraw %}
- strip - strips all whitespace from both ends of the string
- strip_html - strip html from string
- strip_newlines - strip all newlines (`\n`) from string
- stylesheet_tag - generate a stylesheet html tag
- time_tag
- times - multiplication e.g {% raw %}`{{ 5 | times:4 }} #=> 20`{% endraw %}
- truncate - truncate a string down to x characters. It also accepts a second parameter that will append to the string
  e.g. {% raw %}`{{ 'foobarfoobar' | truncate: 5, '.' }} #=> 'foob.'`{% endraw %}
- truncate - truncate a string down to x characters
- truncatewords - truncate a string down to x words
- uniq - remove duplicate elements from an array, optionally using a given property to test for uniqueness
- upcase - convert an input string to uppercase
- url_decode - url decode a string
- url_encode - url encode a string
- where - select elements from array with given property value: {% raw %}`{{ site.posts | where:"category","foo" }}`{% endraw %}

## Liquid filters that don't work in Jekyll

- These math filters don't work:

    - at_least - Limits a number to a minimum value.
    - [at_most](https://help.shopify.com/themes/liquid/filters/math-filters#at_most) - Limits a number to a maximum value.

- Not all of the string filters work:

    - The hashing filters don't work
    - camelcase
    - pluralize

- Not all of the utility filters work:

    - weight_with_unit - Formats a number with the default weight unit. The unit can be overridden by passing it into the filter.
    - format_address


- The money filters don't appear to work.

- The colour filters don't seem to work:

    - [color_brightness](https://help.shopify.com/themes/liquid/filters/color-filters#color_brightness) - Calculates the perceived brightness of the given color. Uses [W3C recommendations for calculating perceived brightness](https://www.w3.org/TR/AERT#color-contrast), for the purpose of ensuring adequate contrast.
    - color_darken - Darkens the input color. Takes a value between 0 and 100 percent.
    - color_desaturate - Desaturates the input color. Takes a value between 0 and 100 percent.
    - color_extract - Extracts a component from the color. Valid components are alpha, red, green, blue, hue, saturation and lightness.
    - color_lighten - Lightens the input color. Takes a value between 0 and 100 percent.
    - color_mix - Blends together two colors. Blend factor should be a value value between 0 and 100 percent.
    - color_modify - Modifies the given component of a color. {% raw %}`{{ '#7ab55c' | color_modify: 'red', 255 }} #=> #ffb55c`{% endraw %}
    - color_saturate - Saturates the input color. Takes a value between 0 and 100 percent.
    - color_to_hex - Converts a CSS color string to hex6 format.
    - color_to_hsl - Converts a CSS color string to CSS `hsl()` format.
    - [color_to_rgb](https://help.shopify.com/themes/liquid/filters/color-filters#color_to_rgb) - Converts a CSS color string to CSS `rgb()` format.



{% include footer.html %}
