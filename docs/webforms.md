---
title: Webforms in Archipelago
tags:
  - Webform
  - Form Mode
  - Webform Elements
---

# Webforms in Archipelago

The [Webform Strawberryfield](https://github.com/esmero/webform_strawberryfield) module provides [Drupal Webform](https://www.drupal.org/project/webform) ( == awesome piece of code) integrations for StrawberryField so you can really have control over your Metadata ingests. These custom elements provide Drupal Webform integrations for Archipelago’s StrawberryField so you can have fine grained and detailed control over your Metadata ingests and edits.

## Where to Find

You can access Webforms in Archipelago at:
- `Admin > Structure > Webforms`
- `/admin/structure/webform/`

![Default Archipelago Webforms](images/DefaultArchipelagoWebforms.png)

### Instructions and Guides

* [How to Create a Webform as an Input Method for Archipelago Digital Objects (ADO)](webformsasinput.md)
* [Customizing Webforms: Modifying allowable file extensions](modifyingfileextensionsinwebform.md)
* [Archipelago Custom Webform Elements](customwebformelements.md)
* [Using Archipelago's 'Webform LoD from CSV attached to an ADO suggest'](WebformLoDfromCSV.md)

### Archipelago Default Example Webforms

Use these webforms or their elements to create a custom webform for your own repository/project needs:

* Archipelago Default Deployment Webforms
    * [Descriptive Metadata](https://github.com/esmero/archipelago-deployment/blob/1.3.0/config/sync/webform.webform.descriptive_metadata.yml)
        * [Corresponding Schema.org Type Options](https://github.com/esmero/archipelago-deployment/blob/1.3.0/config/sync/webform.webform_options.schema_org_creative_works.yml)

    * [Digital Object Collection](https://github.com/esmero/archipelago-deployment/blob/1.3.0/config/sync/webform.webform.digital_object_collection.yml)
        * [Corresponding Schema.org Type Options](https://github.com/esmero/archipelago-deployment/blob/1.3.0/config/sync/webform.webform_options.schema_org_cw_collections.yml)

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
