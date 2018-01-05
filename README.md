# [Development Information website for Totally Information](https://totallyinformation.github.io)

This is a quick information website from Julian Knight @ Totally Information.
It will be used for keeping track of various hints, tips and knowledge regarding development.

It will continue to be a work in progress for a long time no doubt as I learn more about Jekyll and
GitHub Pages. I will also, over time, move information from my personal notebooks (Microsoft OneNote)
to here.

Most of the pages should have the Disqus comment service enabled so please raise issues there first.
Otherwise, feel free to raise a [GitHub issue](https://github.com/TotallyInformation/TotallyInformation.github.io/issues) as well.

Regards, Julian Knight, [Totally Information](https://www.totallyinformation.com).

## To Do

- Add link to directly edit page on GitHub if logged in as TI
- Include this README to the top index.md page
- Fix site menu
- Add link to license in footer
- Add links to my other sites
- Fix the feed
- Extend and improve the repo list

  * Move to an include
  * Sort by recent changes if possible
  * Include description if possible

# Index

{% include collection_doc_lister.html collection='nr_qa' %}

{% include collection_doc_lister.html collection='github_pages' %}

{% include page_lister.html dir="/vscode/" %}

## Totally Information's Public Code Repositories

<table>
    {% assign repos =  site.github.public_repositories | sort: "name" %}
    {% tablerow repository in repos cols:1 %}
        <a hre="{{ repository.html_url }}">{{ repository.name }}</a>
    {% endtablerow %}
</table>

