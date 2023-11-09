---
title: About this Documentation
tags:
  - Documentation
  - Contributing
---

# About this Documentation

This documentation was generated with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/). The repo/branch is at <https://github.com/esmero/archipelago-documentation/tree/1.3.0>, and the site is built using the following Github workflow: <https://github.com/esmero/archipelago-documentation/blob/1.3.0/.github/workflows/ci.yml>.

## Contributing

1. First, please see [this guide](giveortake.md) to set up the repo and branch locally and to create an issue and corresponding pull request.
2. Install mkdocs-material and mike: `pip install mkdocs-material mike`.
3. Make changes to local
4. Run `mike delete --all && mike deploy 1.0.0 default && mike set-default 1.0.0 && mike serve` to see and test changes.
5. Follow the above guide to contribute the changes back.
