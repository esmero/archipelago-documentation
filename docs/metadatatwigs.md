# Twig Templates and Archipelago

![ADOlife](../imgs/jsonupcaststar.png)

Archipelago uses a fast, cached templating system that is core to Drupal, called `Twig`. In its guts (or its heart?) Archipelago uses this system to transform the _close to your needs open schema metadata_ that lives in every `strawberryfield` as JSON into _close to other one's fixed schema needs metadata_. This is quite simple, but it is an essential component of our vision of how a repository should manage metadata.

### What is Twig?
Twig is a template engine for PHP part of Symfony framework.
- [Twig in Symfony](https://twig.symfony.com)
- [Twig in Drupal](https://www.drupal.org/docs/theming-drupal/twig-in-drupal)
- A template engine is a processor. It allows you to mix and process _templates_ with _data_ to generate an output _document_.
	- Template: Some type of static Document (we name this a “Frame”)
	- Data: Your Archipelago Digital Object (ADO) info and your Metadata
	- Processor: Allows you to use a rich and expressive language to pick, check, iterate, transform and output your data inside the Template. We refer to this as “casting”.
		
### Where is Twig used in Archipelago?
This templating system is exposed to Archipelago users through the UI, and is stored in the repository as content. This setup empowers users to fully control how metadata is transformed and published without touching their individual sources or needing to manage hard-coded configurations. We named these readily accessible and powerful templates `Metadata Display entities`, but they serve more than just display needs.

Twig drives **every** Page in a Drupal 8/9/10 environment.
- Twig templates are normally files (.twig.html) that live in your Code.
- Modules provide Templates, Themes provide Templates

Twig drives **every** aspect of your ADO exposure to the world in Archipelago and even batch Ingest.
- Strawberryfield Metadata (JSON, your Data) is passed through a **Metadata Display Entity** which holds:
	- A Twig template (so you do not need to edit Files)
	- A desired output serialization format (the Output Document)

### Twig Templates as Metadata Display Entities
Templates or recipes can be shared, exported, ingested, updated, and adapted in many ways. This means you can make changes quickly without having to wait for the next major release of Archipelago or your favorite Metadata Schema Specs Committee’s agreement to implement the next or the last version. This module not only handles metadata but media assets as well. It will extract local or remote URIs and files from your metadata and render them as media viewers: books, 3D models, images, panoramas, A/V, all with IIIF in its soul.

**Metadata Display Entities** are used for:
- Display:
	- ADO landing pages (via Drupal Field Formatter)
	- IIIF or JSON driven viewers (via Drupal Field Formatter and using Exposed Metadata Endpoints)
	- Map Formatter (Drupal Field Formatter)
	- Custom Blocks (Drupal Views)
	- Search Result Displays (Drupal Views)
	- Collection and Creative Work Series (old compound) displays (Drupal Views)
- Machinable Output
	- Exposed Metadata Endpoints (Standalone URLs to access metadata)
- Batch Ingest
	- AMI Ingest: To transform your CSV data (one row == DATA) to Strawberry field JSON to generate an ADO

### Twig Templates Shipped with Archipelago
**Archipelago Ships with:**
- IIIF Manifest V3 for Images  (JSON-LD) Metadata Display
- IIIF Manifest V2 for Images and Documents (JSON-LD) Metadata Display
- GEOJSON (JSON) Metadata Display
- A LoD Display (HTML) Metadata Display
- A General ADO Description (HTML) Metadata Display
- A Schema.org (JSON-LD) Metadata Display
- A Thumbnails (HTML) Metadata Display
- An AMI (JSON) Ingest Template
etc.

You can find these templates here:
- On Github: [Templates Shipped with Standard Archipelago Deployments](https://github.com/esmero/archipelago-deployment/tree/1.0.0-RC2/d8content/metadatadisplays)
- In your local instance: http://localhost:8001/metadatadisplay/list

**Archipelago (the humans) will keep adding and refining these with every release.**

### Instructions and Examples

While a lot of core needs and use cases are covered with the Twig Templates shipped with Archipelago, you may want to **add more Input elements** to your Webforms, which in turn will generate **new JSON Values**, which in turn you may want to **show/expose to end users**.

**Knowing** (even if you do not plan to) how to edit or create your own **Twig templates** is important.

- This guide covers the Basics of [Working With Twig in Archipelago](../docs/workingtwigs.md)
- This section contains [Full Examples of Common Use Cases](../docs/workingtwigs.md#full-examples-for-common-uses-cases)
- This section covers a [Recommended Workflow](../docs/workingtwigs.md#a-recommended-workflow)
- You may also want learn more about what `format_strawberryfield` can do and what many other possibilities are exposed through our templating system in this guide: [Strawberryfield Formatters](strawberryfield-formatters.md).

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
