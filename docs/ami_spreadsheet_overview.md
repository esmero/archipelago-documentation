---
title: Spreadsheet Formatting Overview
tags:
  - AMI
  - Archipelago Multi Importer
---

## Spreadsheet Formatting Overview

There are multiple ways a spreadsheet/CSV file can be structured to work with AMI, depending on the data transformation and mapping you will be using.

- For most standard AMI ingests, each Row of your spreadsheet/CSV will correspond to a single Digital Object, Creative Work Series (Compound Object Parent) or Collection.
- Columns in your spreadsheet/CSV can be mapped to different data (files) and metadata elements (label, description, subjects, etc.).

- It is recommended that different types of files are placed into separate columns--"images", "documents", "models", "videos", "audios", "texts".
    - Filepaths can point to remote files, to existing files within your docker container, s3 (or other storage type/location that is accessible to Archipelago), and to paths within zip files.
        - Example path for existing file within docker container:
            `/var/www/html/d8content/myAMIimage.jpg`
        - Example s3 path:
            `s3://myAMIuploads/myAMIdocument.pdf`
        - Example remote filepath:
            `https://dogsaregreat.edu/dogs.tiff`
    - **Multiple files (of the same type) can be placed in a single cell, separated by a semicolon ( ; ).**
    - For Digital Objects comprised of multiple types of files, such as an Oral History Interview with an audio file and a PDF transcript file, you can place different file types within different corresponding columns for the same Row.
    - It is recommended that filepaths are copied/stored as plain (non-hyperlinked) formatted text.

- **Every spreadsheet/CSV file should contain the following Columns:**
    - `type`
        - the Digital Object or Digital Object Collection Type, such as 'Photograph' or 'Collection'
    - `label`
        - the title of the Digital Object or Collection
    - **Soft-requirement** `node_uuid`
        - this can be empty
        - if empty, Archipelago will automatically generate UUIDs
        - can be used with existing UUIDs during migrations
    - **Soft-requirement** `sequence_id` for Creative Work Series (compound) children objects
        - this is used to determine the sequence order for children objects within a Creative Work Series (compound) object
        - should be an integer only (ie, '1' and not 'Page 1')
        - if not present, the objects will present in the original ingest order
        - we strongly recommend mapping the correct sequence using 'sequence_id'

- **Recommended Columns:**
    - Files as defined above
        - "images", "documents", "models", "videos", "audios", "text"
        - .warc/.wacz files should be placed in a column "upload_associated_warcs"
    - `ismemberof` and/or `ispartof` (and/or whatever predicate corresponds with the relationship you are mapping)
        - these columns can be used to connect related objects using the object-to-object relationship that matches your needs
        - these columns can hold 3 types of values
            - empty (no value)
            - an integer to connect an object to another object's corresponding row in the same spreadsheet/CSV
              * Ex: Row 2 corresponds to a Digital Object Collection; for a Digital Object corresponding to Row 3, the 'ismemberof' column contains a value of '2'. The Digital Object in Row 3 would be ingested as a member of the Digital Object Collection in Row 2.
            - a UUID to connect with an already ingested object
    - Metadata - for all the rich, detailed information associated with your Digital Objects and Collections
        - Every Column header will become a JSON Key and each cell a JSON value for that Key
        - You can use direct JSON snippets such as:

            ```json
            [{"uri": "http://id.loc.gov/authorities/subjects/sh95008857","label": "Digital libraries"}]
            ```
        - If you have an advanced twig template with the necessary logic, you can place data in cells that can be parsed and structured in various ways (such as multiple values separated by semicolons split accordingly, capitalization of values based on defined patterns, etc.)

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
