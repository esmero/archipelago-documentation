---
title: Text Base Find and Replace
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
2. Filter on the objects that need updates by searching and/or faceting.
3. Either select all (includes results that appear on additional pages) by toggling `Select / deselect all results (all pages, x total)` or toggle the buttons for individual objects.
4. Expand the `► Raw Metadata (JSON)` for some of the objects and double-check that the text being searched is only targeting what is intended and that the replacement text makes sense.
5. Select `Text based find and replace Metadata for Archipelago Digital Objects content item` from the `Action` dropdown.
6. If selecting objects individually, expand the `Selected X items` and review the list.
7. Press the `Apply to selected items` button (don't worry, nothing will happen yet).
8. Add the values for search (`JSON Search String`) and replace (`JSON Replacement String`).
9. If you're absolutely certain about the replacement, uncheck `only simulate and debug affected JSON` and `Apply`.

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

    **Use Case**: After a batch ingest, it was discovered that JSON values across ADOs in multiple keys contain the same typo: `Agnes Meyerhoff` instead of `Anges Meyerhof`.

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
