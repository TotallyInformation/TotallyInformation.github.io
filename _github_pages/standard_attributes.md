---
description: 'These are the default attributes (variables) that you get with each part of the Jekyll domain.'
comments: true
---
{% include header.html %}

{{ site.collections.github_pages.title }}

{{ site.collections.github_pages.description }}

## Global


## Site


## Pages
- content
- dir
- name
- path
- url

## Posts
- categories tags
- content
- date
- dir
- draft?
- excerpt
- excerpt_separator
- id
- next previous
- path
- title
- url

## Collections


## Files

Files can be accessed via `site.static_files`. They are typically kept in the `assets` folder.

- `path` - The relative path to the file, e.g. `/assets/img/image.jpg`
- `modified_time` - The `Time` the file was last modified, e.g. `2016-04-01 16:35:26 +0200`
- `name` - The string name of the file e.g. `image.jpg` for `image.jpg`
- `basename` - The string basename of the file e.g. `image` for `image.jpg`
- `extname` - The extension name for the file, e.g.  `.jpg` for `image.jpg`

While front-matter variables can't be added directly, they can be made accessible via the `defaults` section of `_config.yml`.

{% raw %}
```
defaults:
  - scope:
      path: "assets/img"
    values:
      image: true
```
{% endraw %}

{% include footer.html %}
