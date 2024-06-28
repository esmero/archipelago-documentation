---
title: Metadata API Module
tags:
  - Metadata API Module
  - Open API
  - OAI-PMH
---

# Metadata API Module

Archipelago's Metadata API Module is a special Strawberryfield Formatter Module that implements Metadata API Endpoints and uses Views and Metadata Display Entities as a configuration option to provide dynamic APIs based on Metadata present in each Archipelago Digital Object (or Node) that contains a Strawberryfield type of field (JSON). 

Beginning with Archipelago 1.4.0, Archipelago is shipped with a standard implementation of a set of OAI-PMH functions. This includes a full Open API Configuration for OAI-PMH in the Metadata API Module, a corresponding OAI-PMH View, and two corresponding OAI-PMH Metadata Display Templates (Item, and a Wrapper to output object data in Dublin Core). All of these components are necessary to enable OAI-PMH functionality in your Archipelago. Please see the notes below that further explain the specific OAI-PMH `verbs` (request and response types) supported by Archipelago's default configuration. Please see the [OAI Protocol for Metadata Harvesting Specifications](https://www.openarchives.org/OAI/openarchivesprotocol.html) for more information about OAI-PMH functions.

You can find the Metadata API Module Configuration Form:

- Through the `Manage` menu > `Configuration` > `Archipelago` > `Configure Metadata APIs`
- Directly at `/admin/config/archipelago/metadataapi` 

![Metadata API Module Page](images/metadata-api-page.png)

## Default OAI-PMH Configuration Form

In Archipelago's default `oai_pmh` Metadata API Endpoint, the following [OAI-PMH verbs (requests and responses)](https://www.openarchives.org/OAI/openarchivesprotocol.html#ProtocolMessages) are supported:

- GetRecord
- Identify
- ListIdentifiers
- ListMetadataFormats
- ListRecords
- ListSets




??? info "Click to see the Default OAI-PMH Open API Config"

    ```JSON title="JSON of Default OAI-PMH Open API Config"
    {
        "openapi": "3.0.2",
        "info": {
            "title": "Test API",
            "version": "1.0.0"
        },
        "paths": {
            "http://localhost:8001/ap/api/oai_pmh/{oai}": {
                "get": {
                    "parameters": [
                        {
                            "name": "verb",
                            "in": "query",
                            "description": "",
                            "required": true,
                            "deprecated": false,
                            "style": "form",
                            "explode": true,
                            "schema": {
                                "enum": [
                                    "GetRecord",
                                    "Identify",
                                    "ListIdentifiers",
                                    "ListMetadataFormats",
                                    "ListRecords",
                                    "ListSets"
                                ],
                                "type": "string"
                            }
                        },
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
                        },
                        {
                            "name": "identifier",
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
                        },
                        {
                            "name": "oai",
                            "in": "path",
                            "description": "",
                            "required": true,
                            "deprecated": false,
                            "style": "simple",
                            "explode": false,
                            "schema": {
                                "type": "string"
                            }
                        },
                        {
                            "name": "page",
                            "in": "query",
                            "description": "",
                            "required": false,
                            "deprecated": false,
                            "style": "form",
                            "explode": true,
                            "schema": {
                                "type": "integer"
                            }
                        },
                        {
                            "name": "metadataPrefix",
                            "in": "query",
                            "description": "",
                            "required": false,
                            "deprecated": false,
                            "style": "form",
                            "explode": true,
                            "schema": {
                                "enum": [
                                    "oai_dc",
                                    "mods"
                                ],
                                "type": "string"
                            }
                        },
                        {
                            "name": "from",
                            "in": "query",
                            "description": "",
                            "required": false,
                            "deprecated": false,
                            "style": "form",
                            "explode": true,
                            "schema": {
                                "type": "string",
                                "format": "date"
                            }
                        },
                        {
                            "name": "until",
                            "in": "query",
                            "description": "",
                            "required": false,
                            "deprecated": false,
                            "style": "form",
                            "explode": true,
                            "schema": {
                                "type": "string",
                                "format": "date"
                            }
                        }
                    ]
                }
            }
        }
    }
    ```


### Reviewing and Editing the Default OAI-PMH Configuration Form

![Metadata API OAI-PMH Default Form](images/metadata-api-oai-pmh-form.png)

## Default OAI PMH View

- Through the `Manage` menu > `Structure` > `Views` > `OAI_exposed_entity_reference`
- Directly at `/admin/structure/views/view/oai_exposed_entity_reference` 

Note that the default configuration limits the OAI-PMH queries to members of the Archipelago Demo Collection: Biodiversity Heritage Library Collection (Node #25 as ingested via the default AMI Demo Set). 

__

Return to the [Archipelago Documentation main page](index.md).
