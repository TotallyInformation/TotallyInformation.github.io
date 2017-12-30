GitHub pages use Jekyll which in turn uses [Liquid](http://www.rubydoc.info/gems/liquid).
Liquid provides the variable filters that can be used to amend/format the variable for output.
This is a more comprehensive list than you will find in most places, it is combined from multiple lists including the [source documentation](http://www.rubydoc.info/gems/liquid/Liquid/StandardFilters).

- abs - absolute value
- append - append a string e.g. `{{ 'foo' | append:'bar' }} #=> 'foobar'`
- capitalize - capitalize words in the input sentence
- ceil - rounds a number up to the nearest integer, e.g. {{ 4.6 | ceil }} #=> 5
- compact - Remove nils within an array provide optional property with which to check for nil.
- concat - ?? Not documented! ??
- date - reformat a date ([syntax reference](http://docs.shopify.com/themes/liquid-documentation/filters/additional-filters#date))

  Reformat a date using Ruby's core Time#strftime( string ) -> string
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
  e.g. `{{ undefined_variable | default: "Default value" }} #=> "Default value"`
- divided_by - integer division e.g. `{{ 10 | divided_by:3 }} #=> 3`
- downcase - convert an input string to lowercase
- escape - html escape a string
- escape_once - returns an escaped version of html without affecting existing escaped entities
- first - get the first element of the passed in array
- floor - rounds a number down to the nearest integer, e.g. `{{ 4.6 | floor }} #=> 4`
- group_by - group elements from array by given property: `{{ site.posts | group_by:"category" }}`
- join - join elements of the array with certain character between them
- jsonify -- convert data to JSON: `{{ site.data.dinosaurs | jsonify }}`
- last - get the last element of the passed in array
- lstrip - strips all whitespace from the beginning of a string
- map - map/collect an array on a given property
- markdownify - convert markdown to HTML
- minus - subtraction e.g. `{{ 4 | minus:2 }} #=> 2`
- modulo - remainder, e.g. `{{ 3 | modulo:2 }} #=> 1`
- newline_to_br - replace each newline (`\n`) with html break
- plus - addition e.g. `{{ '1' | plus:'1' }} #=> 2, {{ 1 | plus:1 }} #=> 2`
- prepend - prepend a string e.g. `{{ 'bar' | prepend:'foo' }} #=> 'foobar'`
- remove - remove each occurrence e.g. `{{ 'foobarfoobar' | remove:'foo' }} #=> 'barbar'`
- remove_first - remove the first occurrence e.g. `{{ 'barbar' | remove_first:'bar' }} #=> 'bar'`
- replace - replace each occurrence e.g. `{{ 'foofoo' | replace:'foo','bar' }} #=> 'barbar'`
- replace_first - replace the first occurrence e.g. `{{ 'barbar' | replace_first:'bar','foo' }} #=> 'foobar'`
- reverse - reverses the passed in array
- round - rounds input to the nearest integer or specified number of decimals e.g. `{{ 4.5612 | round: 2 }} #=> 4.56`
- rstrip - strips all whitespace from the end of a string
- size - return the size of an array or string
- slice - slice a string. Takes an offset and length, e.g. {{ "hello" | slice: -3, 3 }} #=> llo
- sort - sort elements of the array
- sort_natural - Sort elements of an array ignoring case if strings provide optional property with which to sort an array of hashes or drops.
- split - split a string on a matching pattern e.g. {{ "a~b" | split:"~" }} #=> ['a','b']
- strip - strips all whitespace from both ends of the string
- strip_html - strip html from string
- strip_newlines - strip all newlines (`\n`) from string
- times - multiplication e.g `{{ 5 | times:4 }} #=> 20`
- truncate - truncate a string down to x characters. It also accepts a second parameter that will append to the string
  e.g. `{{ 'foobarfoobar' | truncate: 5, '.' }} #=> 'foob.'`
- truncate - truncate a string down to x characters
- truncatewords - truncate a string down to x words
- uniq - remove duplicate elements from an array, optionally using a given property to test for uniqueness
- upcase - convert an input string to uppercase
- url_decode - url decode a string
- url_encode - url encode a string
- where - select elements from array with given property value: `{{ site.posts | where:"category","foo" }}`


{% include footer.md %}
