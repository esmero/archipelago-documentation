# Twig Templates and Archipelago

![ADOlife](../imgs/jsonupcaststar.png)

In its guts (or heart?), Archipelago does something quite simple but core to our concept of repository: it transforms in realtime the _close to your needs open schema metadata_ that lives in every `strawberryfield` as JSON into _close to other one's fixed schema needs metadata_; any destination format, using a fast, cached templating system. A templating system that is core to Drupal, called `Twig`.

### What is Twig?
Twig is a template engine for PHP part of Symfony framework.
- [Twig in Symfony](https://twig.symfony.com)
- [Twig in Drupal](https://www.drupal.org/docs/theming-drupal/twig-in-drupal)
- A template engine is processor. It allows you to mix and process _templates_ with _data_ to generate an output _document_.
	- Template: Some type of static Document (we name this a “Frame”)
	- Data: Your Archipelago Digital Object (ADO) info and your Metadata
	- Processor: Allows you to use a rich and expressive language to pick, check, iterate, transform and output your data inside the Template. We name this “casting”.
		
### Where is Twig used in Archipelago?
This templating system is exposed to Archipelago users through the UI and stored side by side in the repository as content (we named them `Metadata Display entities`, but they not only serve display needs!) so users can fully control how metadata is transformed and published without touching their individual sources.

In standard **Drupal**/8/9/10 environments, Twig drives every **Page**.
- Twig templates are normally files (.twig.html) that live in your Code.
- Modules provide Templates, Themes provide Templates

In **Archipelago**, Twig drives every aspect of **your ADO exposure** to the world.
- Strawberryfield Metadata (JSON, your Data) is passed through a Metadata Display Entity which holds:
- A Twig template (so you do not need to edit Files)
- A desired output serialization format (the Output Document)

### Twig Templates as Metadata Display Entities
Templates or recipes can be shared, exported, ingested, updated, and adapted in many ways. Fast changes are possible without having to wait for the next major release of Archipelago or your favorited Metadata Schema Specs Committee agreeing on the next or the last version. Of course, this module not only handles metadata but media assets too, extracting local or remote URIs and files from your metadata and rendering them as media viewers: books, 3D models, images, panoramas, A/V with IIIF in its soul.

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

You can learn even more about what `format_strawberryfield` can do and what many other possibilities are exposed through our templating system in this guide: [Starwberryfield Formatters](strawberryfield-formatters.md).

### Instructions and Examples
- [Working With Twig in Archipelago](../docs/workingtwigs.md)
- [Templates Shipped with Standard Archipelago Deployments](https://github.com/esmero/archipelago-deployment/tree/1.0.0-RC2/d8content/metadatadisplays)

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
