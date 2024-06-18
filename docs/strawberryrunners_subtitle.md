---
title: Strawberry Runners Post-Processing
tags:
  - Strawberry Runners
  - Subtitles
  - VTT
  - HOCR
  - OCR
  - Post-processing
  - Background Processing
---

# Reviewing and adjusting the `subtitle` Post-Processor operations 

As stated on the [Strawberry Runners overview page](docs/strawberryrunners.md), the `subtitle` action extracts textual values from subtitle/transcript VTT files and generates time/space transmuted OCR. This transmuted OCR can be used to search within a time-based video or audio file's corresponding subtitle/transcript VTT file(s), then navigate to the matching time of the video or audio file within a media viewer.

We strongly recommend using caution when making any adjustments to the default `subtitle` configurations as this may result in unexpected issues with the transmuted OCR values in your Solr Index. Also, the `subtitle` Strawberry Runner Post-Processor needs to used with corresponding related default IIIF Configuration Form Settings. 

# Subtitle Settings

To review or adjust the configurations for the `subtitle` operation, select `Edit` from the `Operations` menu.

In the `subtitle` settings, you will see the following configuration options:

![Strawberry Runners Pager](images/sbr_pager.png)

1. Label: 
    - Label for this Processor; which should be a unique machine-readable name
    - Can only contain lowercase letters, numbers, and underscores
    - We do not recommend changing this Label from the default `pager`. 

2. Strawberry Runner Post Processor Plugin:
    - The `Post processor that extracts/generates Ordered Sequences of files/pages/children using Files present in an ADO` should be selected.
    - We do not recommend changing this Plugin selection.

3. Checkbox to mark this processor plugin as active 
    - We recommend keeping this checked as `active` at all times, but you may wish to temporarily disable this if you are performing certain types of administrative review tasks such as running large test ingests where you plan on deleting the ADOs before a final ingest.
    - If you accidentally uncheck this and need to re-trigger the `pager` (and corresponding nested `ocr` action), you can use Archipelago's [Find and Replace](find_and_replace.md) to first select a specific group of Digital Objects you wish to target for Post-Processing, then select the `Trigger Strawberrry Runners process/reprocess for Archipelago Digital Objects content item` from the [Find and Replace](find_and_replace.md) `Actions menu`.

4. ADO type(s) to limit this processor to:
    - A single ADO type or a comma delimited list of ado types that qualify to be Processed.
    - Leave empty to apply to all ADOs. If you do not provide any specific ADO types here, the processor will be applied for all ADOs with the JSON keys selected in the next step.
    - Default ADO types specified are: 'Document,Book,Article'	
    - You may wish to add additional types of document/multiple-paged type of ADOs to this list that are custom to you Archipelago environment. 

5. The JSON key that contains the desired source files:
    - By default, the `as:image` and `as:document` keys are selected.
    - We do not recommend changing this selection.

6. Mimetypes(s) to limit this Processor to:
    - A single Mimetype type or a comma separated list of mimetypes that qualify to be Processed.
    - Leave empty to apply any file.
    - Default mimetypes are: 'application/pdf,image/tiff,image/jpeg,image/jp2'	

7. Within the ADO's metadata, the JSON key that contains the language in ISO639-3 (3 letter) format to be used for OCR/NLP processing via Tesseract.
    - Default JSON key specified is: 'language_iso639_3'

8. Please provide a default language in ISO639-3 (3 letter) format. If none is provided we will use 'eng'.
    - Default language specified is: 'eng'

9. Timeout in seconds for this process.
    - If the process runs out of time it can still be processed again
    - Default selection is: 10

10. Order or execution in the global chain.
    - Default selection is: 0

### Related IIIF Configuration Form and IIIF Content Search Guides

* [IIIF Configuration Form](iiif_server_settings.md)
* [IIIF Content Search API Overview](iiif-content-search.md)

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the main [Strawberry Runners](strawberryrunners.md) or the [Archipelago Documentation main page](index.md).
