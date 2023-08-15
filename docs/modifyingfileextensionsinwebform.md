---
title: Customizing Webforms: Modifying allowable file extensions
tags:
  - Webform
  - Webform Elements
  - File Extensions
---

# Customizing Webforms: Modifying allowable file extensions

A guide to walk users through how to modify the Webform `Descriptive Metadata` to allow additional file extensions to be ingested into Archipelago. This is the default Webform with Archipelago by following [archipelago-deployment](https://github.com/esmero/archipelago-deployment).

## Context

When creating an Archipelago Digital Object (ADO), on Step 4 of the ingest, `Attach Files`, there is a step during the ingest to upload the files associated with your ADO. There will be a section on the Webform outlining the maximum number of files allowed, the maximum file size allowed, and the allowed file extensions that can be uploaded.

Let's say we are creating an ADO with the media type `DigitalDocument` and this ADO contains a data set saved as a `csv` file, but when we get to Step 4 of the ingest workflow we find that `csv` is not an allowed file extension. Fortunately, Archipelago has no restrictions on what file extensions can be uploaded, but some use cases will require a little configuring to fit a specific need. This guide will walk users through the steps to modify the default Webform, `Descriptive Metadata`, to allow additional file extensions to be included during an ingest.

**Prerequisites for following this guide:**

- Running instance of Archipelago (on http://localhost:8001 if you followed the [deployment guide](archipelago-deployment-readme.md#archipelago-docker-deployment) verbatim)
- Admin credentials

## Let's begin!

### Managing Webforms

Once logged in as `admin`, the first thing we need to do is navigate to the Webforms page so we can edit the Webform `Descriptive Metadata.` Click on `Manage`, then `Structure` and when the page loads, scroll down and click `Webforms`.

![Manage > Structure > Webforms](images/modifyingfileextensionsinwebform/02_manage-structure-webforms.jpg)

This is where all of the Webforms inside your Archipelago live. For this guide we're going to edit the Webform `Descriptive Metadata`. Go ahead and click `Build` under the `OPERATIONS` column for `Descriptive Metadata`.

![Descriptive Metadata](images/modifyingfileextensionsinwebform/03_webform-page.jpg)

### Step 3: Editing Elements

Here we see all of the elements in `Descriptive Metadata`; *Title*, *Media type*, *Description*, *Linked Data* elements, etc. The element that we want to edit is `Upload Associated Documents` as this is the field you will use to upload `pdf`, `doc`, `rtf`, `txt`, etc. files during the ingest workflow. Click on `Edit` under the `OPERATIONS` column.

![Upload Associated Documents element](images/modifyingfileextensionsinwebform/04_upload-associated-documents.jpg)

A new screen will pop up named `Edit Upload Associated Documents element`. This is where you can configure the maximum number of values (under `ELEMENT SETTINGS`), the maximum file size and also edit the allowed file extensions for this element, which is what we'll be doing. The latter both exist under `FILE SETTINGS` section, highlighted in the screenshot below.

![Edit Upload Associated Documents element](images/modifyingfileextensionsinwebform/05_edit-upload-associated-documents-element.jpg)

When you scroll down you'll see the `Allowed file extensions` field. This is where we will add the `csv` file extension. **Please note:** All file extensions are separated by a space; no `,` or `.` between the values.

Once you've added all the file extensions your project needs, scroll down to the bottom of `Edit Upload Associated Documents element` and click `Save`.

![Add csv](images/modifyingfileextensionsinwebform/06_add-csv.jpg)

**This next step is imperative for saving your changes**, scroll to the bottom of your elements list page and click `Save elements` in order to persist all changes made.

![Save Elements](images/modifyingfileextensionsinwebform/07_save-elements.jpg)

### Complete

Woohoo! Now when you are ingesting a `DigitalDocument` object, you will be able to add `csv` files! 🍓

![Complete](images/modifyingfileextensionsinwebform/08_complete.jpg)

### Recap

When logged in as an admin, we go to *Manage > Structure > Webforms* and click on `Build` under the `OPERATIONS` column of `Descriptive Metadata` (shortcut: /admin/structure/webform/manage/descriptive_metadata). Then we click on `Upload Associated Documents` to edit the element, scroll down to the *Allowed file extensions* field and add `csv` without `.` or `,` separating the values. Click `Save` at the bottom of the `Edit Upload Associated Documents element` page and then `Save elements` at the bottom of the Webform page.

### That was helpful, but...

??? info "How do I upload a `wav` or `aiff` file for "MusicRecording" or an `mov` file for a \"Movie\"?"

    The steps are virtually the same as what is outlined in this guide! The difference here is that instead of editing `Upload Associated Documents`, you will need to edit the field element that is associated with your ADO's media type. For example, with Media type `MusicRecording`, you will edit `Upload Audio File`, for `Movie`, will edit `Videos`.

??? info "How do I know which element in Descriptive Metadata to edit per media type?"

    When editing an element inside `Descriptive Metadata`, at the top of the window `Edit Upload Associated Documents element` (see Step 3 for a recap on how to get here) there is a tab next to `General` titled `Conditions`. Inside of `Conditions` we have `CONDITIONAL LOGIC` which is where the Webform is told which `Media type` needs this element to be visible in the Webform. In the example below, we know that the field element `Upload Associated Documents` will be visible when `DigitalDocument`, `Thesis` and `Book` are the selected `Media type`.
    
    This is also the place you can add new logic or delete present logic by clicking the `+` or `-` next to the `TRIGGER/VALUE` to create new conditionals.
    
    ![Conditionals](images/modifyingfileextensionsinwebform/09_conditionals.jpg)

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
