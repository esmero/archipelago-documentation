---
title: Documentation Technical Details
tags:
  - Documentation
---

# Documentation Technical Details

Archipelago documentation is generated using the following open source projects:

* [MkDocs](https://www.mkdocs.org/) generates the static site
* [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) provides the site's theme
* [mike](https://github.com/jimporter/mike) generates the routing for versions
* [Github Actions](https://docs.github.com/en/actions) builds and deploys the files to [Github Pages](https://docs.github.com/en/pages)

To use any advanced features not mentioned in these pages, you can look through the documentation for each of the above projects.

In addition to the pages added directly via this repository, there are some pages automatically deployed here with GitHub Actions from the following repositories:

* <https://github.com/esmero/archipelago-deployment>
* <https://github.com/esmero/archipelago-deployment-live>

Both the main READMEs and documentation in the `docs` folders for those repositories are prepended with `archipelago-deployment` and `archipelago-deployment-live` respectively and copied to the `docs` folder here with the rest of the documentation. In practice that means those pieces of documentation need to be edited in those repositories directly.

