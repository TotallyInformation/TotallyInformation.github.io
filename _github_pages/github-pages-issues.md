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

* Looks like there may be some issues when creating pages directly from the GitHub web interface. I've managed to create several pages that show inconsistent metadata - I suspect they were all created via the web interface first. See [Issue #6661](https://github.com/jekyll/jekyll/issues/6661) and [this test page](https://totallyinformation.github.io/github_pages/test)

## Jekyll Issues

This list is about Jekyll failings, not specific to GitHub Pages.

* Jekyll documentation is, in my opinion, poor. Information is badly spread between pages. For example, trying to find a comprehensive list of variable filters that actually work with Jekyll is difficult. Some documentation is left up to the core documentation of the Liquid templating engine. However, while that is comprehensive, it applies to the use of Liquid in its original environment, not everything works in Jekyll.

* There are too many inconsistent differences between different parts of Jekyll. For example, why are there so many differences in the schema definitions for Pages, Posts and Collection Documents?
