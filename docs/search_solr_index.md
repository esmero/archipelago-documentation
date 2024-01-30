---
title: Search and Solr Overview
tags:
  - Search
  - Search API
  - Solr
  - Solr Index
  - Facets
  - Strawberry Key Name Providers
  - OCR
  - HOCR
---

# Search and Solr Overview

*This page is under construction. Please stay tuned for further updates and thank you for your patience as we continue to brew up more documentation.*

## Preamble + prerequisites 

Before diving into any Search and Solr Configuration, please review our [Metadata in Archipelago](metadatainarchipelago.md) overview documentation, which provides important context for understanding how the shape of your Archipelago Digital Objects/Collections (ADOs) metadata will inform your Search and Solr options and outcomes.

## Archipelago and Solr
Archipelago's latest Release (1.1.0) uses [Apache Solr 9.1](https://solr.apache.org/guide/solr/9_1/index.html), which incorporates some [major improvements and changes from Solr 8](https://solr.apache.org/guide/solr/9_1/upgrade-notes/major-changes-in-solr-9.html). Please refer to the [primary Solr documentation](https://solr.apache.org/guide/solr/9_1/index.html) for the most comprehensive and in-depth information about Solr's wide breadth of functionality and configuration options.

### Instructions and Guides

* [Strawberry Key Name Providers, Solr Field, and Facet Configuration](strawberry_key_name_providers.md)
* [Advanced Search](search_advanced.md)

## OCR Highlights

Archipelago uses [solr-ocrhighlighting v0.8.4](https://github.com/dbmdz/solr-ocrhighlighting/releases/tag/wip), built by the Development Team at the [Bavarian State Library](https://github.com/dbmdz).

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
