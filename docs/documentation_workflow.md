---
title: GitHub Workflow
tags:
  - Documentation
  - Contributing
  - Examples
---

# GitHub Workflow - Two Ways

### Route A - GitHub via web interface

1. First, either open a new Issue in the [Archipelago Documentation Github Repo](https://github.com/esmero/archipelago-documentation) describing the new documentation you would like to contribute, or reply to an existing Issue that you would like to address. Tag `@alliomeria` to request feedback. Since our team is consistently working on new and updating existing documentation, please wait for review and guidance before proceeding with creating the new documentation you described in your new Issue or comments on an existing documentation Issue.
2. [Fork the documentation repo](https://github.com/esmero/archipelago-documentation). If you're not familiar with forking [see this guide](https://docs.github.com/en/get-started/quickstart/fork-a-repo).
3. In your local fork of Archipelago's documentation, before proceeding with making changes to existing or adding new documentations guides, make sure you first select 'Sync Fork' and that the local fork branch you are working on is up-to-date with the same branch in the main documentation repo.
4. Working in your synced, up-to-date local fork, proceed with navigating to the existing documentation guides you would like to edit, or proceed with creating a new guide as related to the Issue you followed up on in Step 1.
5. Committ your changes (whether edits or new additions) and add notes descrbing what is part of the committed changes.
6. If this is new documentation add it to the `nav` section of the `mkdocs.yml` configuration file at the root of the repo. For example:
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
7. [Create a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork) and link to the Issue by tagging it, e.g. `Resolves #100`.
8. Please tag `@alliomeria` when you are ready for your contributed documentation to be reviewed. Our team will review and provide feedback, and let you know when your contributed documentation is merged. Thank you in advance for your time and efforts to create documentation for Archipelago's community!


### Route B - GitHub via local desktop client with optional local build and preview

This pathway is not necessary for most documentation contributions.

1. First, either open a new Issue in the [Archipelago Documentation Github Repo](https://github.com/esmero/archipelago-documentation) describing the new documentation you would like to contribute, or reply to an existing Issue that you would like to address. Tag `@alliomeria` to request feedback. Since our team is consistently working on new and updating existing documentation, please wait for review and guidance before proceeding with creating the new documentation you described in your new Issue or comments on an existing documentation Issue.
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
6. If this is new documentation add it to the `nav` section of the `mkdocs.yml` configuration file at the root of the repo. For example:
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
7. To view the changes locally, first install the Python libraries using the Python package manager pip:
   ```shell
   pip install mkdocs-material mike git+https://github.com/jldiaz/mkdocs-plugin-tags.git mkdocs-git-revision-date-localized-plugin mkdocs-glightbox
   ```
   You may need to install Python on your machine. [Download Python](https://www.python.org/downloads/) or use your favorite operating system package manager such as Homebrew. 

8. Now you can build the site locally, e.g. for the documentation using the 1.5.0 branch:
   ```shell
   mike deploy 1.5.0
   mike set-default 1.5.0
   ```
   If you create a new branch to match the issue number as in step 3, you would use your branch instead of 1.5.0. For example, a branch of ISSUE-129.
   ```shell
   mike deploy ISSUE-129
   mike set-default ISSUE-129
   ```
9. Start the web server:
   ```shell
   mike serve
   ```
10. Check the results in your browser by going to: <http://localhost:8000>
11. If everything looks good, you can push to your forked repo issue branch:
    ```shell
    git add .
    git commit -m "Create new docs with useful information."
    git push origin ISSUE-100
    ```
12. [Create a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork) and link to the Issue by tagging it, e.g. `Resolves #100`.

13. Please tag `@alliomeria` when you are ready for your contributed documentation to be reviewed. Our team will review and provide feedback, and let you know when your contributed documentation is merged. Thank you in advance for your time and efforts to create documentation for Archipelago's community!

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
