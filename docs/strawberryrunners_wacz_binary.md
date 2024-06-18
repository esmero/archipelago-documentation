---
title: Reviewing and adjusting the `warc_to_wacz` Post-Processor operation
tags:
  - Strawberry Runners
  - WACZ
  - WARC
  - Binary
  - Post-processing
  - Background Processing
---

# Reviewing and adjusting the `warc_to_wacz` Post-Processor operation

As stated on the [Strawberry Runners overview page](docs/strawberryrunners.md), the `warc_to_wacz` action uses the 'Post processor that uses a System Binary to process * files' operations.

## Wacz Page Extractor Settings

To review or adjust the configurations for the `wacz_page_extractor` operation, select `Edit` from the `Operations` menu.

In the `wacz_page_extractor` settings, you will see the following configuration options:

1. Label: 
    - Label for this Processor; which should be a unique machine-readable name
    - Can only contain lowercase letters, numbers, and underscores
    - We do not recommend changing this Label from the default `pager`. 

2. Strawberry Runner Post Processor Plugin:
    - The `Post processor that extracts/generates Indexed Page Content from WACZ in an ADO` should be selected.
    - We do not recommend changing this Plugin selection.

3. Checkbox to mark this processor plugin as active 
    - We recommend keeping this checked as `active` at all times, but you may wish to temporarily disable this if you are performing certain types of administrative review tasks such as running large test ingests where you plan on deleting the ADOs before a final ingest.
    - If you accidentally uncheck this and need to re-trigger the `pager` (and corresponding nested `ocr` action), you can use Archipelago's [Find and Replace](find_and_replace.md) to first select a specific group of Digital Objects you wish to target for Post-Processing, then select the `Trigger Strawberrry Runners process/reprocess for Archipelago Digital Objects content item` from the [Find and Replace](find_and_replace.md) `Actions menu`.

4. The JSON key that contains the desired source files:
    - By default, only the `as:document` key is selected.
    - We do not recommend changing this selection.

5. Mimetypes(s) to limit this Processor to:
    - A single Mimetype type or a comma separated list of mimetypes that qualify to be Processed.
    - Default mimetype for this Processor is: 'application/warc'	  
	
!!! warning "Advanced OCR/HOCR Settings"

    We do not recommend making changes to the follow settings unless you are the System Administrator.


6. The system path to the binary that will be executed by this processor.
    - A full system path to a binary present in the same environment your PHP runs
    - Default is: '/usr/bin/wacz'

7. Any additional argument your executable binary requires.
    - Any arguments your binary requires to run. Use %file as replacement for the file if the executable requires the filename to be passed under a specific argument. Use %outfile if the binary is intended to generate a new file and the output is going to be a file entity. If you know the extension please add it in the form of %outfile.extension
    - Default is: 'create %file -t -o %outfile.wacz'

8. The expected and desired output of this processor.
    - If the output is just data and "One or more Files" is selected all data will be dumped into a file and handled as such.
    - Default is set to 'One or more Files'

9. Where and how the output will be used.
    - Default is set to 'A new file to be attached to the source ADO'
    - The 'As Input for another processor Plugin' selection will only have an effect if another Processor is setup to consume this output.
    - Other optional selections include:
       - In the same Source Metadata, as a child structure of each Processed file
       - In the same Source Metadata but inside its own, top level, "as:flavour" subkey based on the given machine name of the current plugin
       - A new file to be attached to the source ADO
       - As Input for another processor Plugin
       - In a Search API Document using the Strawberryfield Flavor Data Source (e.g used for HOCR highlight)

11. Timeout in seconds for this process.
    - Default is set to: 99
    - If the process runs out of time it can still be processed again.

12. Order or execution in the global chain.
    - Default is set to: 0

___

Return to the main [Strawberry Runners](strawberryrunners.md) or the [Archipelago Documentation main page](index.md).
