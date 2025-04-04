---
title: Notes for Adapting Archipelago's Default OAI-PMH Configurations for an ILS/Discovery Layer
tags:
  - Metadata API Module
  - Open API
  - OAI-PMH
---

# Notes for Adapting Archipelago's Default OAI-PMH Configurations for an ILS/Discovery Layer

This set of notes can be used to assist with using Archipelago's Metadata API Module and provided default OAI-PMH configurations to expose Archipelago collections and objects to an ILS/Discovery Layer. 

Please refer to the [main Metadata API Module documentation](open_api.md) for more information about the provided default configurations and templates.

## Example of using the Metadata API Module

In the example scenario described below, an institution has the Metadata API Module installed and configured in their Archipelago instance already, and is using the Archipelago default OAI-PMH Dublin Core (DC) Twig template without any changes for their local metadata schema.

### Open Archives Initiative Protocol for Metadata Harvesting (OAI-PMH)

As found on the [Open Archives Initiative Protocol for Metadata Harvesting](https://www.openarchives.org/pmh/) homepage, "(OAI-PMH) is a low-barrier mechanism for repository interoperability. Data Providers are repositories that expose structured metadata via OAI-PMH. Service Providers then make OAI-PMH service requests to harvest that metadata. OAI-PMH is a set of six verbs or services that are invoked within HTTP."

### Understanding the Archipelago Digital Object and OAI-PMH API URL format

In Archipelago, the URL used for OAI-PMH requests is defined by code and a configurable Drupal View. The View limits the returned data to a singular collection by default. The collection value is the `ismemberof` ID found in an Archipelago Digital Object Collections's Raw JSON metadata. For every Archipelago Digital Object/Collection, the URL will be formatted to include the UUID of the ADO/Collection like so: `http(s)://yourdomain.ext/do/{node_uuid}/`

Per the standards requried for OAI-PMH and the configured site base URL, the OAI-PMH URL will always look something like the following:


```shell
https://yourdomain.ext/ap/api/oai_pmh/oai?verb=ListRecords&set=SET-UUID&metadataPrefix=oai_dc
```

### Configuring Archipelago to output the collection you want to share

#### Update the default OAI-PMH View to specify your chosen Collection

1. Go to any digital object that is part of your Collection as a logged in user with the proper permissions to view 'ADO Tools' OR the 'Raw JSON' Formatter.
2. Either select the 'ADO Tools' tab at the top of a digital object page OR open the 'Raw JSON' section found at the bottom of a digital object page. Search the JSON metadata for "ismemberof". You will see a numeric ID, make a note of that ID to use in the next step.
3. With your `ismemberof` ID in hand, go to the related OAI-PMH View:
  - `Structure -> Views`, and then select Edit for the View titled "OAI exposed entity reference".
  - In the Filter Criteria section of the View, find the Filter named "Content datasource: 🍓 Strawberry (Descriptive Metadata source) » entity_sbf_entity_reference_ismemberof » ID (= 25)"
  - Select that Filter and change the ID value to the one you have just made note of. In the following screenshot, the ID has been changed to "292".

  ![Updating the Filter ID](images/view-metadata-entity_sbf_entity_reference_ismemberof.png)

#### Identify the UUID of the chosen Collection

1. Navigate to the top level Collection that you identified before, the one you just set the node ID for in the OAI-PMH corresponding Drupal View.
2. Retrieve the UUID of the Collection. As stated earlier on this page, by default, for every Archipelago Digital Object/Collection, the URL will be formatted to include the UUID of the ADO/Collection like so: `http(s)://yourdomain.ext/do/{node_uuid}/`. You can retrieve that UUID from the default URL. You may also have the UUID value included on your default Metadata Display Twig Template for Collections already. You can also use the 'ADO Tools' tab to identifty `"node_uuid"`. 

#### Craft your specified OAI-PMH URL

With your View updated to reference the chosen Collection's Drupal Node ID and your Archipelago UUID ("node_uuid" value), you can now form your needed OAI-PMH API URL. Set your `node_uuid` value as the set parameter, as shown in this example (but replacing with your chose Collection's UUID value):

```shell
https://YOUR-DOMAIN/api/oai_pmh/oai?verb=ListRecords&set=67acd47d-3cfd-427d-ada0-79b918e028e5&metadataPrefix=oai_dc
```

The Metadata API module will use the provided UUID value to identify the chosen Collection as the ['set' parameter defined in the OAI-PMH API specs](https://www.openarchives.org/OAI/openarchivesprotocol.html#Set).

```shell
                        {
                            "name": "set",
                            "in": "query",
                            "description": "",
                            "required": false,
                            "deprecated": false,
                            "style": "form",
                            "explode": true,
                            "schema": {
                                "type": "string",
                                "format": "uuid"
                            }
```

#### Permissions to view the OAI-PMH REST URL

In the likely event that you are planning to share the URL to be referenced by another system, such as your instution's ILS/Discovery Layer, note that by default only the 'administrator' role has permissions set to access the URL. You need to update the default View permissions by selecting which additional roles you want to be able to access the OAI-PMH URL. You can review and update the appropriate the Metadata API permissions at `~/admin/people/permissions`.

__

Return to the [Archipelago Documentation main page](index.md).
