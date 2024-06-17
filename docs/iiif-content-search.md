---
title: IIIF Content Search API Integration
tags:
  - IIIF
  - IIIF Server Settings Form
  - IIIF Content Search API
  - Solr
  - Solr Fields
  - Solr Index
---

# IIIF Content Search API Integration

Beginning in release 1.4.0, Archipelago features IIIF Content Search API integration with attendant default configurations and settings.

Through a non-trifling amount of code and maths, Archipelago speaks the IIIF Content Search API language using data from your Archipelago's Digital Objects, to enable you to search within Mirador (or other supported viewers) for specific hits within OCR, VTT file, or manually created textual annotations. 

Please also see the related [IIIF Server Settings Form](iiif_server_settings.md), and Strawberry Runners guides for [Reviewing and adjusting the `pager` and `ocr` Post-Processor operations](strawberryrunners_pager_ocr.md) and [Reviewing and Adjusting the `subtitle` Post-Processor operations](strawberryrunners_subtitle.md).

## 1. IIIF Manifest Templates

First, Archipelago's default IIIF Manifest templates explicitly state that they support the 3 versions of IIIF Content Search APIS in the 'service' key.

## 2. API Endpoints Exposure

Next, in the default Exposed Metadata Endpoints API Endpoints (generated from the IIIF templates), Archipelago provides the specific structure needed for the IIIF Content Search API. Archipelago passes the data about “the template containing it”, the IIIF API version, if simple or advanced, and the Archipelago Digital Object resource UUID we are searching against (the one that contains the RAW data feeding the template, or at least the Top level/parent one of that).

## 3. Pathway into and out of the Solr Index

Then, Archipelago's backend re-creates the IIIF manifest using this data (basically repeats what the client did before), but uses JMESPATHs to extract just what is needed, flipping the order of the structure and putting IIIF Image IDs, as "top keys" referencing canvases and their #xywh selectors, if present.

Using this transformed data, Archipelago's backend search is able to be limited to OCR generated only by those images (important as Archipelago repositories can contain millions of OCR'd documents). Archipelago's internal search then returns natively, via the [Bavarian State Library’s Solr OCR highlight plugin](https://github.com/dbmdz/solr-ocrhighlighting/), the relevant hits within a specified ADO. These are then reprocessed to be IIIF compliant (W3C) annotations and then reverted back to results as “canvases with images”.

## Things to keep in mind

- To make this performant, Archipelago uses two levels of caches that get invalidated automatically on any "ingredient" used modification.

- Archipelago can also tell the backend to use a "different" template than the one used at the front (Mirador), allowing you to define which "canvases" are searchable. This is not a normal use case, but still a valid one. And you can, per resource, have complex logic and/or different Viewers, even on a one by one basis.

### Acknowledgements

Archipelago's developers would like to extend our gratitude to our community, especially to [Mike](https://github.com/digitaldogsbody) and [Johannes](https://github.com/jbaiter) for their work and help, and everyone else in the IIIF and repository communities for all the amazing tools, viewers, specs and cookbook examples.

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

