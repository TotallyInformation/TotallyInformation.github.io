---
title: List of problems and issues when using Jekyll with GitHub Pages
description: >
  While sites generated using GitHub pages can use Jekyll to enhance them, there are some limitations
  and, it seems, a number of problems.
comments: true
date: 2018-01-05 17:05:00
---

## Advantages (not matched by alternatives)

* The Jekyll build process runs on the GitHub back end, no need for the horrible (on Windows anyway) installation of Ruby and Jekyll.

* GitHub provides an existing set of data about the owning organisation/person and their repos.

Sorry, can't think of any others that aren't matched by alternative tools.

## GitHub Pages Issues

* Build failures generate error messages that are worse than useless. Often not even identifying the page that the build failed on.

* Looks like there may be some issues when creating pages directly from the GitHub web interface. I've managed to create several pages that show inconsistent metadata - I suspect they were all created via the web interface first. See [Issue #6661](https://github.com/jekyll/jekyll/issues/6661) and [this test page](https://totallyinformation.github.io/github-pages/test). You can see that the test page is not able to reference its frontmatter metadata.

* Jekyll uses Kramdown to render markdown files. This does not quite match the default GFM format. While this is improving, the version used is typically behind the current one, often by a fair margin.

## Minima Theme Issues

While the Minima theme is pretty good, it has a few issues of its own.

* The documentation states that all you need to do to enable Disqus is to add your shortcode to `_config.yml`. However, this is not quite correct as Disqus in only added to posts and not to pages or collection documents, nor to the home page.

* The main menu does not work quite as expected. If a page doesn't have a title, it is ignored, not terribly safe an assumption since an xml document might have a title but shouldn't be shown or perhaps a page needed in the menu doesn't have/need a title.

## Jekyll Issues

This list is about Jekyll failings, not specific to GitHub Pages.

* The sort filter does not work correctly. I have some code that includes {%raw%}`{% for item in collection | sort: "title" %}`{%endraw%} but several entries are incorrectly sorted - [example](/github-pages/).

* Jekyll documentation is, in my opinion, poor. Information is badly spread between pages. For example, trying to find a comprehensive list of variable filters that actually work with Jekyll is difficult. Some documentation is left up to the core documentation of the Liquid templating engine. However, while that is comprehensive, it applies to the use of Liquid in its original environment, not everything works in Jekyll.

* There are too many inconsistent differences between different parts of Jekyll. For example, why are there so many differences in the schema definitions for Pages, Posts and Collection Documents?

* In the `page` object, not all properties are set as one might expect. `page.date` for example is not set unless you create it manually in the frontmatter. Again, not all page properties actually apply to "Pages", some only apply to posts or collection documents. Highly confusing.

