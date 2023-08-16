---
title: Using Webform Find and Replace 
tags:
  - Advanced Batch Find and Replace
  - Webform Find and Replace
  - Search and Replace
---

# Webform Find and Replace

Webform Find and Replace enables you to search against values found within defined Webform elements to apply metadata replacements with targeted care. Below are a guide and an example use case for this Action.

!!! note

    Please refer to the main [Find and Replace documentation page](find_and_replace.md) for a general overview of [where to find within your Archipelago](find_and_replace.md#where-to-find), a [general overview of default options](find_and_replace.md#main-page-overview) and [important notes and workflow recommendations](find_and_replace.md#important-notes--workflow-recommendations).

### Important Notes about Different Webform Elements

!!! warning "Maximum Length as Defined by your Webform Element Configuration OR Theme Defaults"

    For certain text-based webform element types, the maximum field length (`maxlength`) defined in your specific webform element configurations will be enforced during Webform Find and Replace operations. If no maximum length is defined, the Admin Theme will enforce a maximum length of 128 characters. Please see our main [Webforms documentation](webforms.md) for information about configuring webforms in Archipelago.

!!! note "Some Complex Webform Elements Not Available"

    Please note that some complex webform elements are not available for use with Webform Find and Replace. Any webform element that requires user interactions (such as the Nominatim Open Street Maps lookup/query and selection) is not available for usage. The different file upload webform elements are also not available for use with Webform Find and Replace.

## Step by Step Guide

1. Go to `Tools > Advanced Batch Find and Replace`.
2. Identify and/or Filter the Digital Objects (ADOs) that need updates by Searching and/or using the available [Facets](find_and_replace.md#default-facets-configured).
   - If using the 'Ingest Method Service URL' for Faceting, please note that using the Find and Replace Webform functionality to update ADOs will cause the original AMI Set URL identified in this Facet to be overwritten and replaced with the specified Webform used during the replacement execution.
3. Either select all (includes results that appear on additional pages) by toggling `Select / deselect all results (all pages, x total)` or toggle the buttons for individual objects.
4. Expand the `► Raw Metadata (JSON)` for some of the objects and double-check that the metadata field and value you are targeting for replacement is present.
5. Select `Webform find-and-replace Metadata for Archipelago Digital Objects content item` from the `Action` dropdown.
6. If selecting objects individually, expand the `Selected X items` and review the list.
7. Press the `Apply to selected items` button (don't worry, nothing will happen yet).  
8. On the Webform Find and Replace Action Configuration page, you will need to select the configuration options to be applied for your selected ADOs.
   - From the dropdown:
     1. Select which Webform you want to use
     2. Select which Form element you want to use
     3. Select which value to search for in the [chosen form element] JSON key
     4. Select which value to replace with in the [chosen form element] JSON key
   - See the [Simulation Mode notes here](find_and_replace/#simulation-mode) for information about this optional simulation/test mode.
9. If you're absolutely certain about the replacement you have targeted, uncheck the 'only simulate and debug affected JSON' option and select `Apply`.
10. After the operation is executed, [check your changes](find_and_replace/#checking-your-changes).

## Use Case Example

In the following example configuration, for the selected 'Senju no oubashi (Senju great bridge)' object, the *Media Type (type JSON key)* value of "Visual Artwork" will be replaced with the `type` JSON key value of "Photograph".

* Selection of Single ADO and `Webform find-and-replace` Action

    ![Webform Find and Replace Selection and Action](images/SelectionAndAction_WebformFAR_2022-12.jpg)

* Webform Find and Replace Form
  
    ![Action Configuration](images/ActionConfiguration_WebformFAR_2022-12.jpg)

* Confirmation of Successfully Executed Changes

    ![Successfully Executed Changes](images/Success_WebformFAR_2022-12.jpg)
___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the main [Find and Replace documentation page](find_and_replace.md) or the [Archipelago Documentation main page](index.md).
