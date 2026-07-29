---
title: IIIF mediated Viewers
tags:
  - Strawberryfield Formatters
  - Display Formatters
  - IIIF
---

# IIIF mediated Viewers

Formatters that use IIIF twig templates to process media and generate IIIF manifests:

- Strawberry Field Media Formatter using OpenSeadragon for IIIF media
- Strawberry Field Simple Image Formatter using IIIF
- Strawberry Field Media Formatter using the Mirador * IIIF Viewer plugin
- Strawberry Field Paged Formatter using IABook Readerplugin
- Strawberry Field 3D Model Formatter
- Strawberry Field Panorama Formatter using Pannellum * and IIIF
- Strawberry Field PDF Formatter for IIIF served PDFs
- Strawberry Field Media Formatter using the Universal (UV) * IIIF Viewer plugin

!! note "Recommended prerequisite documentation"

    Please also see our '[Primer on Display Modes](primerdisplaymodes.md)' and '[Strawberryfield Formatters](strawberryfield-formatters.md)' guides for more information about Display Modes and Formatters.

## Strawberry Field Media Formatter using OpenSeadragon for IIIF media

This formatter displays media using the [OpenSeadragon](https://openseadragon.github.io) for IIIF viewer.

You can find the default example of the `Strawberry Field Media Formatter using OpenSeadragon for IIIF media` settings configuration form at:
- `admin/structure/types/manage/digital_object/display/digital_object_viewmode_fullitem`
- Through the `Manage` menu > `Structure` > Content types > Digital Object > Manage display > Select 'Digital Object Full View'

Click on the small settings cogwheel on the right hand side of the listed formatter fields to review the formatter settings.

For this formatter, site administrators have multiple options to adjust the standard display: defining Max Height + Width, defining the use and look/feel of annotations, grouping of media files, use of custom UI/UX icons, and defining the source IIIF Twig templates and Exposed Metadata Endpoints.
	
??? info "Default configurations used"

    - Use IIIF Global Urls? Yes.
    - IIIF Media Server base URI: http://localhost:8183/iiif/2
    - IIIF Media Server Internal base URI: http://esmero-cantaloupe:8182/iiif/2
    - Limited to the following file upload JSON Keys: Fetch from any available
    - Embargo Alternate upload JSON Keys: Do not provide alternate files when embargoed
    - Viewer for embargoed Objects is hidden
    - This formatter still needs to be setup
    - Displays Zoomable Media from JSON using a IIIF server and the OpenSeadragon viewer.
    - Use a single Viewer for multiple media: 1
    - Enable W3C WebAnnotations: 1
    - Show thumbnail navigation bar: 1
    - Media fetched from JSON "as:image" key
    - Maximum size: 100% x 520 pixels

## Strawberry Field Simple Image Formatter using IIIF

There is not an example use of this formatter in default Archipelago deployments. You can find the option for this formatter listed in the Formatters dropdown list for any pre-configured display mode + formatter setup. This `Strawberry Field Simple Image Formatter using IIIF` is best used for simple displays of single images where you want to provide a IIIF-mediated image that links back to the original source Node (aka: Archipelago Digital Object, ADO). 

For this formatter, the settings available for this formatter are very slim: specifying the source JSON Keys for the media files, setting the number of files to be used, enable linking behavior, defining Max Height + Width, using global IIIF URLs, and enabling embargo hiding.

## Strawberry Field Media Formatter using the Mirador * IIIF Viewer plugin

See [separate guide for this formatter here](mirador_iiif_formatter.md), and some related notes [here](metadatainarchipelago.md#strawberry-field-media-formatter-using-the-mirador-iiif-viewer-plugin).

## Strawberry Field Paged Formatter using IABook Readerplugin

Archipelago provides a formatter integration for the popular [Internet Archive Bookreader](https://github.com/internetarchive/bookreader). 

You can find the default IA Bookreader Formatter settings configuration form at:
- `admin/structure/types/manage/digital_object/display/digital_object_with_book_reader`
- Through the `Manage` menu > `Structure` > Content types > Digital Object > Manage display > Select 'Digital Object with Bookreader'

Select the small settings cogwheel to the right of the `Erdbeere` field + `Strawberry Field Paged Formatter using IABook Readerplugin` to review the custom IA Bookreader Formatter settings.


??? info "Default configurations used"

    - Use IIIF Global Urls? Yes.
    - IIIF Media Server base URI: https://dcmny.org/cantaloupe/iiif/2
    - IIIF Media Server Internal base URI: http://esmero-cantaloupe:8182/iiif/2
    - Limited to the following file upload JSON Keys: Fetch from any available
    - Embargo Alternate upload JSON Keys: Do not provide alternate files when embargoed
    - Viewer for embargoed Objects is visible
    - Displays Paged Media from JSON using a IIIF server and the IABook Reader viewer.
    - Pages processed by the "IIIF Presentation API 2.1 Manifest" Metadata Data Display template
    - Base IA Reader images URL: https://cdn.jsdelivr.net/gh/internetarchive/bookreader@4.40.3/BookReader/images/
    - Maximum size: 100% x 900 pixels
    - JSON key providing Book Cover configuration: hascover. Please note: By default IA Book reader will treat any resource as a book with a Book Cover. If this JSON key is present in your Strawberryfield JSON field it will be evaluated as boolean. If the value is or evaluates to "False", when seeing a resource with opposing pages (2up), two opposing pages will be shown initially (a spread) instead of treating the first as the cover. By default the value of this is "TRUE" which is the normal behavior of this IIIF Viewer.

## Strawberry Field 3D Model Formatter

You can find the default 3D Model Formatter settings configuration form at:
- `admin/structure/types/manage/digital_object/display/digital_object_with_3d_viewer`
- Through the `Manage` menu > `Structure` > Content types > Digital Object > Manage display > Select 'Digital Object with 3D Viewer'

Select the small settings cogwheel to the right of the `Erdbeere` field + `Strawberry Field 3D Model Formatter` to review the custom 3D Model Formatter settings.


??? info "Default configurations used"

    - Use IIIF Global Urls? Yes.
    - IIIF Media Server base URI: http://localhost:8183/iiif/2
    - IIIF Media Server Internal base URI: http://esmero-cantaloupe:8182/iiif/2
    - Limited to the following file upload JSON Keys: Fetch from any available
    - Embargo Alternate upload JSON Keys: Do not provide alternate files when embargoed
    - Viewer for embargoed Objects is hidden
    - Displays 3D Models from JSON using the JSM Modeller Library
    - Media fetched from JSON "as:model" key
    - Inverted Up Axis: "YES"
    - Number of 3D Models: "1"
    - Maximum size: 100% x 720 pixels

## Strawberry Field Panorama Formatter using Pannellum * and IIIF

Archipelago provides a formatter integration for the [Pannellum](https://github.com/mpetroff/pannellum) open source panorama viewer for the web.

You can find the default Pannellum Formatter settings configuration form at:
- `admin/structure/types/manage/digital_object/display/digital_object_with_pannellum_panorama`
- Through the `Manage` menu > `Structure` > Content types > Digital Object > Manage display > Select 'Digital Object with Pannellum Panorama'

Select the small settings cogwheel to the right of the `Erdbeere` field + `Strawberry Field Formatter using Pannellum * and IIIF` to review the custom 3D Model Formatter settings.


??? info "Default configurations used"

    - Use IIIF Global Urls? Yes.
    - IIIF Media Server base URI: https://dcmny.org/cantaloupe/iiif/2
    - IIIF Media Server Internal base URI: http://esmero-cantaloupe:8182/iiif/2
    - Limited to the following file upload JSON Keys: Fetch from any available
    - Embargo Alternate upload JSON Keys: Do not provide alternate files when embargoed
    - Viewer for embargoed Objects is hidden
    - Displays Panoramas from JSON using Pannellum and a IIIF server endpoint
    - Media fetched from JSON "as:image" key
    - Number of images: "1"
    - Maximum size: 100% x 520 pixels


## Strawberry Field PDF Formatter for IIIF served PDFs

Archipelago provides a formatter integration for a lightweight, browser driven [Javascript PDF Reader](https://github.com/mozilla/pdf.js).

You can find the default PDF Formatter settings configuration form at:
- `admin/structure/types/manage/digital_object/display/digital_object_with_pdf`
- Through the `Manage` menu > `Structure` > Content types > Digital Object > Manage display > Select 'Digital Object with PDF Viewer'

Select the small settings cogwheel to the right of the `Erdbeere` field + `Strawberry Field PDF Formatter for IIIF served PDFs` to review the custom 3D Model Formatter settings.


??? info "Default configurations used"

    - Use IIIF Global Urls? Yes.
    - IIIF Media Server base URI: https://dcmny.org/cantaloupe/iiif/2
    - IIIF Media Server Internal base URI: http://esmero-cantaloupe:8182/iiif/2
    - Limited to the following file upload JSON Keys: Fetch from any available
    - Embargo Alternate upload JSON Keys: Do not provide alternate files when embargoed
    - Viewer for embargoed Objects is hidden
    - Document fetched from JSON "as:document" key
    - Initial page: "1"
    - Maximum size: 100% x auto


## Strawberry Field Media Formatter using the Universal (UV) * IIIF Viewer plugin

Archipelago provides a formatter integration for [Universal Viewer](https://github.com/universalviewer/universalviewer).

There is not an example use of this formatter in default Archipelago deployments. You can find the option for this formatter listed in the Formatters dropdown list for any pre-configured display mode + formatter setup, labeled as `Strawberry Field Formatter using the Universal (UV) * IIIF Viewer plugin` in the formatter dropdown menu.

For this formatter, the settings available for configuring include: selecting the Exposed Metadata Endpoints used to generate the Manifests, , defining Max Height + Width, specifying the source JSON Keys for the media files, setting the number of files to be used, setting for enabling embargo hiding, and a checkbox to set whether to use global IIIF URLs or not.

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
