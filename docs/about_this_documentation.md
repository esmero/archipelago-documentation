---
title: About This Documentation
tags:
  - Documentation
  - Contributing
  - Examples
---

# About This Documentation

This document serves as both a guide and a template for creating a documentation page. You can use the processed/published page of this as a visual example for what the resulting HTML/CSS/Javascript looks like, and you can copy and modify this Markdown file locally to contribute documentation.

## Sources

Archipelago documentation is generated using the following open source projects:

* [MkDocs](https://www.mkdocs.org/) generates the static site
* [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) provides the site's theme
* [mike](https://github.com/jimporter/mike) generates the routing for versions
* [Github Actions](https://docs.github.com/en/actions) builds and deploys the files to [Github Pages](https://docs.github.com/en/pages)

To use any advanced features for which we don't include examples in this document, you can look through the documentation for each of the above projects.

In addition to the pages added directly via this repo, there are some pages automatically deployed here with Github Actions from the following repos:

* <https://github.com/esmero/archipelago-deployment>
* <https://github.com/esmero/archipelago-deployment-live>

Both the main READMEs and documentation in the `docs` folders for those repos are prepended with `archipelago-deployment` and `archipelago-deployment-live` respectively and copied to the `docs` folder here with the rest of the documentation. In practice that means those pieces of documentation need to be edited in those repos directly.

## How to Contribute

1. Find an issue to resolve or create a new issue [here](https://github.com/esmero/archipelago-documentation/issues).
2. [Fork the documentation repo](https://github.com/esmero/archipelago-documentation). If you're not familiar with forking [see this guide](https://docs.github.com/en/get-started/quickstart/fork-a-repo).
3. Create an issue branch in your forked repo. For example, if the issue you're resolving is ISSUE-100:
   ```shell
   git checkout -b ISSUE-100
   ```
4. Copy this template to create a new piece of documentation:
   ```shell
   cp docs/about_this_documentation.md docs/new_documentation.md
   ```
5. Make your changes to the copied markdown file.
5. If this is new documentation add it to the `nav` section of the `mkdocs.yml` configuration file at the root of the repo. For example:
   ```yaml hl_lines="7"
   nav:
     - Home: index.md
     - About Archipelago:
       - Archipelago's Philosophy & Guiding Principles: ourtake.md
       - Strawberryfields Forever: strawberryfields.md
       - Software Services: devops.md
       - New Documentation: new_documentation.md
     - Code of Conduct: CODE_OF_CONDUCT.md
     - Instructions and Guides:
       - Archipelago-Deployment:
         - Start: archipelago-deployment-readme.md
         - Installing Archipelago Drupal 9 on OSX (macOS): archipelago-deployment-osx.md
         - Installing Archipelago Drupal 9 on Ubuntu 18.04 or 20.04: archipelago-deployment-ubuntu.md
         - Installing Archipelago Drupal 9 on Windows 10/11: archipelago-deployment-windows.md
         - Adding Demo Archipelago Digital Objects (ADOs) to your Repository: archipelago-deployment-democontent.md
   ...
   ```
6. To view the changes locally, first install the Python libraries:
   ```shell
   pip install mkdocs-material mike git+https://github.com/jldiaz/mkdocs-plugin-tags.git
   ```
7. Now you can build the site locally, e.g. for the documentation for 1.0.0:
   ```shell
   mike deploy 1.0.0
   ```
8. Start the web server:
   ```shell
   mike serve
   ```
9. Check the results in your browser by going to: <http://localhost:8000>
10. If everything looks good, you can push to your forked repo issue branch:
    ```shell
    git add .
    git commit -m "Create new docs with useful information."
    git push origin ISSUE-100
    ```
11. [Create a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork) and link to the issue by tagging it, e.g. `Resolves #100`.

## Examples

Below are some examples of some elements that are currently in use on the site.

### Images

Images are located in the `docs/images` folder. You can add new ones there and link to them by relative path. For example, if you added `new_documentation_image.jpg`, you would embed it like so:

```markdown
![New Documentation Image](images/new_documentation_image.jpg)
```

### Admonitions

Markup:

```markdown
??? question "What is a collapsible admonition?"
 
    This is a collapsible admonition. It can have a title, and it collapses so as not to interrupt the flow the of the document, but it provides useful information as needed.
```

Result:

??? question "What is a collapsible admonition?"
 
    This is a collapsible admonition. It can have a title, and it collapses so as not to interrupt the flow the of the document, but it provides useful information as needed.

You can read more about admonitions with further examples in the [Material for MkDocs documentation](https://squidfunk.github.io/mkdocs-material/reference/admonitions/).

### Code blocks

Markup for a code block with a title and highlighted lines:

````markdown
```html+twig title="HTML in a TWIG template" hl_lines="8 9 10"
{% for image in attribute(data, 'as:image')|slice(0,1) %}
 {% if image["flv:exif"] %}
   {% set height = image["flv:exif"].ImageHeight%}
 {% else  %}
   {% set width = 1200 %}
 {% endif %}
   {% set imageurl =  IIIFserverurl ~ image['url']|replace({'s3://':''})|replace({'private://':''})|url_encode %}
<a href="{{ nodeurl }}" title="{{ data.label }}" alt="{{ data.label }}">
<img src="{{ imageurl }}/pct:5,5,90,30/,400/0/default.jpg" class="d-block.w-auto" alt="{{ image.name }}" height="400px" style="min-width:1200px">
</a> 
{% endfor %}
```
````

Result:

```html+twig title="HTML in a TWIG template" hl_lines="8 9 10"
{% for image in attribute(data, 'as:image')|slice(0,1) %}
 {% if image["flv:exif"] %}
   {% set height = image["flv:exif"].ImageHeight%}
 {% else  %}
   {% set width = 1200 %}
 {% endif %}
   {% set imageurl =  IIIFserverurl ~ image['url']|replace({'s3://':''})|replace({'private://':''})|url_encode %}
<a href="{{ nodeurl }}" title="{{ data.label }}" alt="{{ data.label }}">
<img src="{{ imageurl }}/pct:5,5,90,30/,400/0/default.jpg" class="d-block.w-auto" alt="{{ image.name }}" height="400px" style="min-width:1200px">
</a> 
{% endfor %}
```
