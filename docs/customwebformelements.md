# Archipelago Custom Webform Elements

In addition to the [core elements](https://api.drupal.org/api/drupal/namespace/Drupal%21Core%21Render%21Element/8) provided by the [Drupal Webform module](https://www.drupal.org/project/webform), Archipelago also deploys a robust set of custom webform elements specific to digital repositories metadata needs and use cases.

### Computed Elements:

* Computed Metadata Transplant
  - Provides an item that takes source values (not only elements) and distributes them into other places/elements via a twig template

* Computed Token
  - Provides an item to display computed webform submission values using tokens.

* Computed Twig  
  - Provides an item to display computed webform submission values using Twig

### File Upload Elements:

  * Enhancements for Audio, Document, Image, Video file uploads
    - Backend processing for technical metadata (such as pronom, exif extraction)

  * Import Metadata from a File _(such as XML)_
    - Provides a form element for uploading, saving a file and parsing the content as metadata/webform submission data.

  * Import Metadata in CSV format from a File
    - Provides a form element for uploading, saving a file and parsing the content as metadata/webform submission data.

### Date/Time Elements:
  * Multi Format Date and Date Range
    - Provides a form element for setting/reading Dates indifferent formats suitable for metadata.

### Composite Elements:
  * Panorama Tour Builder
    - Provides a form element to build multi-scene Panorama Tour Builder with hotspots

### Linked Data:
_(*found under Composite Elements in "Add Element" menu)_

  * Library of Congress (LoC) Linked Open data
   - Provides a form element to reconciliate against [LoC Headings and similar LoD Sources](https://id.loc.gov)
    - Able to configure which LoC Autocomplete Source Provider is used:
      - Subjects (LCSH)
      - LC Name Authority File (LCNAF)
      - LC Genre/Form Terms (LCGFT
      - Thesaurus of Graphic Materials (TGN)
      - MARC list for Geographic areas
      - Filter Suggest results by RDF Type
        - Any of the Classes listed here: https://id.loc.gov/ontologies/madsrdf/v1.html

  * Multi LoD Source Agent Items
    - Provides a form element to reconciliate Agents against Multiple sources of Agents

  * Wikidata
    - Provides a form element to reconciliate against Wikidata Items and Agents
      - via [Wikidata Action API](https://www.mediawiki.org/wiki/API:Main_page)

  * Getty Vocabulary Term
    - Provides a form element to reconciliate against the [Getty Vocabularies](http://vocab.getty.edu)

  * VIAF
    - Provides a form element to reconciliate against [VIAF (OCLC) Items](http://viaf.org)

  * Location GEOJSON (Nominatim--Open Street Maps)
    - Provides a form element to collect valid location information (address, longitude, latitude, geolocation) using [Nominatim/Openstreetmap](https://nominatim.openstreetmap.org/ui/search.html) open API


#### But wait there's more!

You can review the coding behind these custom elements here:
https://github.com/esmero/webform_strawberryfield/tree/ISSUE-69/src/Element
