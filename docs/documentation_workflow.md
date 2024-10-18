---
title: GitHub Workflow
tags:
  - Documentation
  - Contributing
  - Examples
---

# GitHub Workflow

1. Find an issue to resolve or create a new issue [here](https://github.com/esmero/archipelago-documentation/issues).
2. [Fork the documentation repo](https://github.com/esmero/archipelago-documentation). If you're not familiar with forking [see this guide](https://docs.github.com/en/get-started/quickstart/fork-a-repo).
3. Create an issue branch in your forked repo. For example, if the issue you're resolving is ISSUE-100:
   ```shell
   git checkout -b ISSUE-100
   ```
4. Copy this template to create a new piece of documentation:
   ```shell
   cp docs/documentation_template.md docs/new_documentation.md
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
6. To view the changes locally, first install the Python libraries using the Python package manager pip:
   ```shell
   pip install mkdocs-material mike git+https://github.com/jldiaz/mkdocs-plugin-tags.git mkdocs-git-revision-date-localized-plugin mkdocs-glightbox
   ```
   You may need to install Python on your machine. [Download Python](https://www.python.org/downloads/) or use your favorite operating system package manager such as Homebrew. 

7. Now you can build the site locally, e.g. for the documentation using the 1.4.0 branch:
   ```shell
   mike deploy 1.4.0
   mike set-default 1.4.0
   ```
   If you create a new branch to match the issue number as in step 3, you would use your branch instead of 1.4.0. For example, a branch of ISSUE-129.
   ```shell
   mike deploy ISSUE-129
   mike set-default ISSUE-129
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

