---
title: 'Jekyll/Liquid Variable Filters'
description: 'A comprehensive list of filters available to modify/format Jekyll variables.'
comments: true
---

GitHub pages use Jekyll which in turn uses [Liquid](https://help.shopify.com/themes/liquid).
Liquid provides the variable filters that can be used to amend/format the variable for output.
This is a more comprehensive list than you will find in most places, it is combined from multiple lists including the [source documentation](http://www.rubydoc.info/gems/liquid/Liquid/StandardFilters). For more details on these filters, see the [official filter documentation](https://help.shopify.com/themes/liquid/filters).

See also: [Tags]({% link gitHub-jekyll/liquid-tags.md %}), [Attributes]({% link github-jekyll/standard-attributes.md %}), [Test Page]({% link github-jekyll/test2.md %})

- abs - absolute value
- absolute_url - Prepend the `url` and `baseurl` value to the input. e.g. {% raw %}`{{ "/assets/style.css" | absolute_url }} #=> http://example.com/my-baseurl/assets/style.css`{% endraw %}
- append - append a string e.g. {% raw %}`{{ 'foo' | append:'bar' }} #=> 'foobar'`{% endraw %}
- array_to_sentence_string - Convert an array into a sentence. Useful for listing tags. Optional argument for connector. e.g. {% raw %}`{{ page.tags | array_to_sentence_string: 'or' }} #=> foo, bar, or baz`{% endraw %}
- capitalize - capitalize words in the input sentence
- ceil - rounds a number up to the nearest integer, e.g. {% raw %}`{{ 4.6 | ceil }} #=> 5`{% endraw %}
- compact - Remove nils within an array provide optional property with which to check for nil.
- [concat](https://help.shopify.com/themes/liquid/filters/array-filters#concat) - Concatenates (combines) an array with another array
- cgi_escape - Replaces any special characters with appropriate `%XX` replacements. CGI escape normally replaces a space with a plus `+` sign.

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

- date_to_long_string - Format a date to long format. e.g. `07 November 2008`
- date_to_rfc822 - Convert a Date into the RFC-822 format used for RSS feeds.
- date_to_string - Convert a date to short format. e.g. `07 Nov 2008`
- date_to_xmlschema - Convert a Date into XML Schema (ISO 8601) format.

- default - returns the given variable unless it is null or the empty string, when it will return the given value,
  e.g. {% raw %}`{{ undefined_variable | default: "Default value" }} #=> "Default value"`{% endraw %}
- default_errors -
- default_pagination -

- divided_by - integer division e.g. {% raw %}`{{ 10 | divided_by:3 }} #=> 3`{% endraw %}
- downcase - convert an input string to lowercase
- escape - html escape a string
- escape_once - returns an escaped version of html without affecting existing escaped entities
- first - get the first element of the passed-in array
- floor - rounds a number down to the nearest integer, e.g. {% raw %}`{{ 4.6 | floor }} #=> 4`{% endraw %}
- __gist__ (provided by [jekyll-gist plugin](https://github.com/jekyll/jekyll-gist)) - display a gist. e.g. {% raw %}`{% gist c08ee0f2726fd0e3909d %}`{% endraw %}

- group_by - group elements from array by given property: {% raw %}`{{ site.posts | group_by:"category" }}`{% endraw %}
- group_by_exp - Group an array's items using a Liquid expression. e.g. {% raw %}`{{ site.members | group_by_exp:"item","item.graduation_year | truncate: 3, \"\"" }} #=> [{"name"=>"201...", "items"=>[...]},{"name"=>"200...", "items"=>[...]}]`{% endraw %}

- [highlight](https://help.shopify.com/themes/liquid/filters/additional-filters#highlight) - Wraps words inside search results with an HTML `<strong>` tag with the class highlight if it matches the submitted search.terms. Has to be used in a search form. **Does this work in Jekyll?** Not to be confused with the `highlight` **tag**.
- highlight_active_tag - Wraps a tag link in a `<span>` with the class active if that tag is being used to filter a collection.

- img_tag - generate an img html tag

- inspect - Convert an object into its String representation for debugging. Returns a JSON stringified version of a variable so use as {% raw %}`<pre>{{ page | inspect }}</pre>`{% endraw %} - possibly references Ruby's inspect function?

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
- normalize_whitespace - Replace any occurrence of whitespace with a single space.
- number_of_words - Count the number of words in some text.
- placeholder_svg_tag
- plus - addition e.g. {% raw %}`{{ '1' | plus:'1' }} #=> 2, {{ 1 | plus:1 }} #=> 2`{% endraw %}
- pop - Remove an entry from the end of an array. Non-destructive, makes a copy of the array and changes that.
- prepend - prepend a string e.g. {% raw %}`{{ 'bar' | prepend:'foo' }} #=> 'foobar'`{% endraw %}
- push - Add entry to the end of an array. Non-destructive, makes a copy of the array and changes that.
- relative_url - Prepend the baseurl value to the input. Useful if your site is hosted at a subpath rather than the root of the domain. e.g. {% raw %}`{{ "/assets/style.css" | relative_url }} #=> /my-baseurl/assets/style.css`{% endraw %}
- remove - remove each occurrence e.g. {% raw %}`{{ 'foobarfoobar' | remove:'foo' }} #=> 'barbar'`{% endraw %}
- remove_first - remove the first occurrence e.g. {% raw %}`{{ 'barbar' | remove_first:'bar' }} #=> 'bar'`{% endraw %}
- replace - replace each occurrence e.g. {% raw %}`{{ 'foofoo' | replace:'foo','bar' }} #=> 'barbar'`{% endraw %}
- replace_first - replace the first occurrence e.g. {% raw %}`{{ 'barbar' | replace_first:'bar','foo' }} #=> 'foobar'`{% endraw %}
- reverse - reverses the passed in array
- round - rounds input to the nearest integer or specified number of decimals e.g. {% raw %}`{{ 4.5612 | round: 2 }} #=> 4.56`{% endraw %}
- rstrip - strips all whitespace from the end of a string
- sample - Pick a random value from an array. Optionally provide a number to pick multiple values.
- sassify - Convert a Sass-formatted string into CSS.
- script_tag - generate a script html tag
- scssify - Convert a Scss-formatted string into CSS.
- shift - Add entry to the start of an array. Non-destructive, makes a copy of the array and changes that.
- size - return the size of an array or string
- slice - slice a string. Takes an offset and length, e.g. {% raw %}`{{ "hello" | slice: -3, 3 }} #=> llo`{% endraw %}
- slugify - Convert a string into a lowercase URL "slug". [Optionally takes a style parameter](https://jekyllrb.com/docs/templates/#options-for-the-slugify-filter): none, raw, default, pretty, ascii, latin. [Definitions for permalink styles](https://jekyllrb.com/docs/permalinks/#builtinpermalinkstyles)

- sort - sort elements of the array
- sort_natural - Sort elements of an array ignoring case if strings provide optional property with which to sort an array of hashes or drops.

- smartify - Convert "quotes" into “smart quotes.”
- split - split a string on a matching pattern e.g. {% raw %}`{{ "a~b" | split:"~" }} #=> ['a','b']`{% endraw %}

- strip - strips all whitespace from both ends of the string
- strip_html - strip html from string
- strip_newlines - strip all newlines (`\n`) from string

- stylesheet_tag - generate a stylesheet html tag
- times - multiplication e.g {% raw %}`{{ 5 | times:4 }} #=> 20`{% endraw %}
- to_integer - Convert a string or boolean to integer.

- truncate - truncate a string down to x characters. It also accepts a second parameter that will append to the string
  e.g. {% raw %}`{{ 'foobarfoobar' | truncate: 5, '.' }} #=> 'foob.'`{% endraw %}
- truncatewords - truncate a string down to x words

- uniq - remove duplicate elements from an array, optionally using a given property to test for uniqueness
- unshift - Remove an entry from the start of an array. Non-destructive, makes a copy of the array and changes that.
- upcase - convert an input string to uppercase

- uri_escape - Percent encodes any special characters in a URI. URI escape normally replaces a space with `%20`. Reserved characters will not be escaped.
- url_decode - url decode a string
- url_encode - url encode a string

- where - select elements from array with given property value: {% raw %}`{{ site.posts | where:"category","foo" }}`{% endraw %}
- where_exp - Select all the objects in an array where the expression is true. Jekyll v3.2.0 & later. e.g. {% raw %}`{{ site.members | where_exp:"item","item.graduation_year == 2014" }} {{ site.members | where_exp:"item","item.graduation_year < 2014" }} {{ site.members | where_exp:"item","item.projects contains 'foo'" }}`{% endraw %}

- xml_escape - Escape some text for use in XML.

## Liquid filters that don't work in Jekyll

- These math filters don't work:

    - at_least - Limits a number to a minimum value.
    - [at_most](https://help.shopify.com/themes/liquid/filters/math-filters#at_most) - Limits a number to a maximum value.

- Not all of the string filters work:

    - The hashing filters don't work
    - camelcase
    - pluralize
    - time_tag

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

