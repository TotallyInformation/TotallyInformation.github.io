---
description: 'These are the default attributes (variables) that you get with each part of the Jekyll domain.'
comments: true
---

## Front Matter

[Front matter](https://jekyllrb.com/docs/frontmatter/) is a simple YAML-based specification for adding metadata to Jekyll files. You can add it to pages, posts and collections. It goes at the top of the file between two lines containing `---`.

There are some standard front matter attributes:

- `layout` If set, this specifies the layout file to use. Use the layout file name without the file extension. Layout files must be placed in the  `_layouts` directory.

   Using `null` will produce a file without using a layout file. However this is overridden if the file is a post/document and has a layout defined in the front matter defaults. Starting from version 3.5.0, using `none` in a post/document will produce a file without using a layout file regardless of front matter defaults. Using `none` in a page, however, will cause Jekyll to attempt to use a layout named "none".

- `permalink` If you need your processed blog post URLs to be something other than the site-wide style (default /year/month/day/title.html), then you can set this variable and it will be used as the final URL.

- `published` Set to `false` if you don’t want a specific _post_ to show up when the site is generated.

- `date` A date here overrides the date from the name of the _post_. This can be used to ensure correct sorting of posts. A date is specified in the format YYYY-MM-DD HH:MM:SS +/-TTTT; hours, minutes, seconds, and timezone offset are optional.

- `category`, `categories` Instead of placing _posts_ inside of folders, you can specify one or more categories that the post belongs to. When the site is generated the post will act as though it had been set with these categories normally. Categories (plural key) can be specified as a YAML list or a space-separated string.

- `tags` Similar to categories, one or multiple tags can be added to a _post_. Also like categories, tags can be specified as a YAML list or a space-separated string.

- `title` While not specifically a standard, many templates make use of page/post titles
- `description` While not specifically a standard, many templates make use of page/post descriptions. Start the description value with "> " to enable multi-line text, indent the multi-line text.



## Global Variables
- `site` Site wide information + configuration settings from  _config.yml. See below for details.
- `page` Page specific information + the YAML front matter. Custom variables set via the YAML Front Matter will be available here. See below for details.
- `layout` Layout specific information + the YAML front matter. Custom variables set via the YAML Front Matter in layouts will be available here.
- `content` _In layout files_, the rendered content of the Post or Page being wrapped. **Not defined in Post or Page files**.
- `paginator` When the paginate configuration option is set, this variable becomes available for use. See [Pagination](https://jekyllrb.com/docs/pagination/) for details.

## site.xxxx Variables
- `site.time` The current time (when you run the jekyll command).

- `site.pages` A list of all _Pages_.

- `site.posts` A reverse chronological list of all _Posts_.

- `site.related_posts` If the page being processed is a _Post_, this contains a list of up to ten related Posts. By default, these are the ten most recent posts. For high quality but slow to compute results, run the  jekyll command with the --lsi (latent semantic indexing) option. Also note GitHub Pages does not support the lsi option when generating sites.

- `site.static_files` A list of all _static files_ (i.e. files not processed by Jekyll's converters or the Liquid renderer). Each file has three properties: path,  modified_time and extname.

- `site.html_pages` A subset of `site.pages` listing those which end in `.html`.

- `site.html_files` A subset of `site.static_files` listing those which end in `.html`.

- `site.collections` A list of all the _collections_.

- `site.data` A list containing the data loaded from the YAML files located in the `_data` directory.

- `site.documents` A list of all the _documents_ in every _collection_.

- `site.categories.[CATEGORY]` The list of all _Posts_ in category [CATEGORY].

- `site.tags.[TAG]` The list of all _Posts_ with tag [TAG].

- `site.url` Contains the url of your site as it is configured in the _config.yml. For example, if you have url: `http://mysite.com` in your configuration file, then it will be accessible in Liquid as  site.url. For the development environment there is an exception, if you are running jekyll serve in a development environment  site.url will be set to the value of host, port, and SSL-related options. This defaults to url: `http://localhost:4000`.

- `site.[CONFIGURATION_DATA]` All the variables set via the command line and your _config.yml are available through the site variable. For example, if you have foo: bar in your configuration file, then it will be accessible in Liquid as site.foo. Jekyll does not parse changes to _config.yml in watch mode, you must restart Jekyll to see changes to variables.

## page.xxxx Variables

- `page.content` The content of the Page, rendered or un-rendered depending upon what Liquid is being processed and what page is.

- `page.title` The title of the Page.

- `page.excerpt` The un-rendered excerpt of the Page.

- `page.url` The URL of the Post without the domain, but with a leading slash, e.g. /2008/12/14/my-post.html

- `page.date` The Date assigned to the Post. This can be overridden in a Post’s front matter by specifying a new date/time in the format YYYY-MM-DD HH:MM:SS (assuming UTC), or YYYY-MM-DD HH:MM:SS +/-TTTT (to specify a time zone using an offset from UTC. e.g. 2008-12-14 10:30:00 +0900).

- `page.id` An identifier unique to a document in a _Collection_ or a _Post_ (useful in RSS feeds). e.g.  /2008/12/14/my-post /my-collection/my-document

- `page.categories` The list of categories to which this _post_ belongs. Categories are derived from the directory structure above the _posts directory. For example, a post at /work/code/_posts/2008-12-24-closures.md would have this field set to ['work', 'code']. These can also be specified in the YAML Front Matter. Available to collection documents but not to Pages(?).

- `page.tags` The list of tags to which this post belongs. These can be specified in the YAML Front Matter.

- `page.path` The path to the _raw_ post, page or collection document. Example usage: Linking back to the page or post’s source on GitHub. This can be overridden in the YAML Front Matter.

- `page.next` The next post relative to the position of the current post in site.posts. Returns nil for the last entry.

- `page.previous` The previous post relative to the position of the current post in site.posts. Returns nil for the first entry.

### These are _not_ described in the Jekyll docs section on Pages but are certainly available
- dir - Only available to _pages_, not to collection documents.
- name - Only available to _pages_, not to collection documents.
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

While front-matter variables can't be added directly to files, they can be made accessible via the `defaults` section of `_config.yml`.

{% raw %}
```
defaults:
  - scope:
      path: "assets/img"
    values:
      image: true
```
{% endraw %}

