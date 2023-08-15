---
title: Text Based Find and Replace
tags:
  - Advanced Batch Find and Replace
  - Advanced Find and Replace
  - Find and Replace
  - Search and Replace
  - Metadata
  - Review
---

# Text Based Find and Replace

The text-based find and replace is case-sensitive and space-sensitive, and while it's the most simple of the actions, it's quite powerful. For this reason it's important to be very precise and target only what's intended. Below are a guide and some examples of use cases for this action.

!!! note

    Please refer to the main [Find and Replace documentation page](find_and_replace.md) for a general overview of [where to find within your Archipelago](find_and_replace.md#where-to-find), a [general overview of default options](find_and_replace.md#main-page-overview) and [important notes and workflow recommendations](find_and_replace.md#important-notes--workflow-recommendations).

## Step-by-Step Guide

1. Go to `Tools > Advanced Batch Find and Replace`.
2. Identify and/or Filter the Digital Objects (ADOs) that need updates by Searching and/or using the available [Facets](find_and_replace.md#default-facets-configured).
3. Either select all (includes results that appear on additional pages) by toggling `Select / deselect all results (all pages, x total)` or toggle the buttons for individual objects.
4. Expand the `► Raw Metadata (JSON)` for some of the objects and double-check that the text being searched is only targeting what is intended and that the replacement text makes sense.
5. Select `Text based find and replace Metadata for Archipelago Digital Objects content item` from the `Action` dropdown.
6. If selecting objects individually, expand the `Selected X items` and review the list.
7. Press the `Apply to selected items` button (don't worry, nothing will happen yet).
8. Add the values for search (`JSON Search String`) and replace (`JSON Replacement String`).
9. If you're absolutely certain about the replacement you have targeted, uncheck the 'only simulate and debug affected JSON' option and select `Apply`.

    !!! Note "only simulate and debug affected JSON"

        This option, which is selected by default, will simulate the action and show the list of objects that would be affected, along with the number of modifications for each object and a total number of results processed. For each JSON key and value affected the *modifications* will count 2:

        1 for the deletion of the current key and value

        +

        1 for the creation of the modified key and value 

## Use Cases and Examples

!!! example "Replacing a JSON key"

    **Use Case**: A JSON key is currently singular but should be plural.

    ```json title="JSON key example with empty array value"
    ...
    "myKey": [],
    ...
    ```

    ```json title="JSON key example with array values"
    ...
    "myKey": ["strawberries","blueberries","blackberries"],
    ...
    ```

    Follow [the steps above](#step-by-step-guide) and use the following for the search and replace values:

    ```text title="Search Value"
    "myKey":
    ```

    ```text title="Replace Value"
    "myKeys":
    ```

    !!! tip

        By using quotes and the colon instead of `myKey` only, we avoid unintentionally replacing other instances of the text within the JSON.

    After applying the changes, we have the following key:

    ```json title="JSON key example with empty array value after update"
    ...
    "myKeys": [],
    ...
    ```

    ```json title="JSON key example with array values after update"
    ...
    "myKeys": ["strawberries","blueberries","blackberries"],
    ...
    ```

!!! example "Replacing a JSON value"

    **Use Case**: After a batch ingest, it was discovered that JSON values across ADOs in multiple keys contain the same typo: `Agnes Meyerhoff` (two fs) instead of `Agnes Meyerhof`.

    ```json title="JSON value example with typo" hl_lines="7 18"
    ...
    "creator_lod": [
        {
            "name_uri": "",
            "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/art",
            "agent_type": "personal",
            "name_label": "Meyerhoff, Agnes",
            "role_label": "Artist"
        },
        {
            "name_uri": "",
            "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/col",
            "agent_type": "personal",
            "name_label": "Messenger, Maria, , 1849-1937",
            "role_label": "Collector"
        }
    ],
    "description": "Inscription on mount: \"Meyerhoff, Agnes \\ Frankfurt - a\/M. \\ Inv el-lith \\ Painter.\" Inscription on verso: \"Agnes Meyerhoff \\ Frankfurt a\/M \\ inv. [at?] lith. \\ [maker in?]\".",
    ...
    ```

    Follow [the steps above](#step-by-step-guide) and use the following for the search and replace values:

    ```text title="Search Value"
    Meyerhoff, Agnes
    ```

    ```text title="Replace Value"
    Meyerhof, Agnes
    ```

    After applying the changes, we have the following values:

    ```json title="JSON values after update" hl_lines="7 18"
    ...
    "creator_lod": [
        {
            "name_uri": "",
            "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/art",
            "agent_type": "personal",
            "name_label": "Meyerhof, Agnes",
            "role_label": "Artist"
        },
        {
            "name_uri": "",
            "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/col",
            "agent_type": "personal",
            "name_label": "Messenger, Maria, , 1849-1937",
            "role_label": "Collector"
        }
    ],
    "description": "Inscription on mount: \"Meyerhof, Agnes \\ Frankfurt - a\/M. \\ Inv el-lith \\ Painter.\" Inscription on verso: \"Agnes Meyerhoff \\ Frankfurt a\/M \\ inv. [at?] lith. \\ [maker in?]\".",
    ...
    ```

!!! example "Replacing a JSON value with escape characters"

    **Use Case**: The URL for a website that appears in multiple keys needs to be updated from `http://hubblesite.org` to `https://hubblesite.org`.

    ```json title="JSON value example with URL after update"
    ...
    "rights": "This digital image may be used for educational or scholarly purposes without restriction. Commercial and other uses of the item are prohibited without prior written permission from the NASA and the Space Telescope Science Institute (STScI). For more information, please visit the NASA and the Space Telescope Science Institute's Copyright web page at [http:\/\/hubblesite.org\/copyright](http:\/\/hubblesite.org\/copyright).",
    ...
    "description": "\"The largest NASA Hubble Space Telescope image ever assembled, this sweeping bird’s-eye view of a portion of the Andromeda galaxy (M31) is the sharpest large composite image ever taken of our galactic next-door neighbor. Though the galaxy is over 2 million light-years away, The Hubble Space Telescope is powerful enough to resolve individual stars in a 61,000-light-year-long stretch of the galaxy’s pancake-shaped disk. ... The panorama is the product of the Panchromatic Hubble Andromeda Treasury (PHAT) program. Images were obtained from viewing the galaxy in near-ultraviolet, visible, and near-infrared wavelengths, using the Advanced Camera for Surveys and the Wide Field Camera 3 aboard Hubble. This cropped view shows a 48,000-light-year-long stretch of the galaxy in its natural visible-light color, as photographed with Hubble's Advanced Camera for Surveys in red and blue filters July 2010 through October 2013.\" -full description available at: [http:\/\/hubblesite.org\/image\/3476\/gallery\/73-phat](http:\/\/hubblesite.org\/image\/3476\/gallery\/73-phat).",
    ...
    ```
   
    Follow [the steps above](#step-by-step-guide) and use the following for the search and replace values:

    ```text title="Search Value"
    http://hubblesite.org
    ```

    ```text title="Replace Value"
    https://hubblesite.org
    ```

    !!! note

        You'll notice that the escape characters for the forward slash (`\/`), which appear in the raw JSON, do not need to be included in the search or replace values.

    After applying the changes, we have the following values:

    ```json title="JSON value example with URL after update"
    ...
    "rights": "This digital image may be used for educational or scholarly purposes without restriction. Commercial and other uses of the item are prohibited without prior written permission from the NASA and the Space Telescope Science Institute (STScI). For more information, please visit the NASA and the Space Telescope Science Institute's Copyright web page at [https:\/\/hubblesite.org\/copyright](https:\/\/hubblesite.org\/copyright).",
    ...
    "description": "\"The largest NASA Hubble Space Telescope image ever assembled, this sweeping bird’s-eye view of a portion of the Andromeda galaxy (M31) is the sharpest large composite image ever taken of our galactic next-door neighbor. Though the galaxy is over 2 million light-years away, The Hubble Space Telescope is powerful enough to resolve individual stars in a 61,000-light-year-long stretch of the galaxy’s pancake-shaped disk. ... The panorama is the product of the Panchromatic Hubble Andromeda Treasury (PHAT) program. Images were obtained from viewing the galaxy in near-ultraviolet, visible, and near-infrared wavelengths, using the Advanced Camera for Surveys and the Wide Field Camera 3 aboard Hubble. This cropped view shows a 48,000-light-year-long stretch of the galaxy in its natural visible-light color, as photographed with Hubble's Advanced Camera for Surveys in red and blue filters July 2010 through October 2013.\" -full description available at: [https:\/\/hubblesite.org\/image\/3476\/gallery\/73-phat](https:\/\/hubblesite.org\/image\/3476\/gallery\/73-phat).",
    ...
    ```

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the main [Find and Replace documentation page](find_and_replace.md) or the [Archipelago Documentation main page](index.md).