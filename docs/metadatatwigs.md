# Managing Metadata Displays with Twig

### Gist of Twigs in Archipelago
_(*If you entered this page from the Strawberry field, this introductory section will feel quite familiar to you.)_

In its guts (or heart?), Archipelago does something quite simple but core to our concept of repository: it transforms in realtime the _close to your needs open schema metadata_ that lives in every `strawberryfield` as JSON into _close to other one's fixed schema needs metadata_; any destination format, using a fast, cached templating system. A templating system that is core to Drupal, called `Twig`:
- [Twig in Symfony](https://twig.symfony.com)
- [Twig in Drupal](https://www.drupal.org/docs/theming-drupal/twig-in-drupal)

This templating system is exposed to Archipelago users through the UI and stored side by side in the repository as content (we named them `Metadata Display entities`, but they not only serve display needs!) so users can fully control how metadata is transformed and published without touching their individual sources.

Templates or recipes can be shared, exported, ingested, updated, and adapted in many ways. Fast changes are possible without having to wait for the next major release of Archipelago or your favorited Metadata Schema Specs Committee agreeing on the next or the last version. Of course, this module not only handles metadata but media assets too, extracting local or remote URIs and files from your metadata and rendering them as media viewers: books, 3D models, images, panoramas, A/V with IIIF in its soul.

You can learn more about what [format_strawberryfield](strawberryfield-formatters.md) can do and what many other possibilities are exposed through our templating system.

### Instructions and Guides
*_Stay tuned, more to be shared with/following 1.0.0-RC2 release_

* [Explanation of how Twig-Casting Works](tbd.md)
* [Guide to basic adjustments to the default Object Description Twig template](tbd.md)

### Examples
- [Templates Shipped with Standard Archipelago Deployments](https://github.com/esmero/archipelago-deployment/tree/1.0.0-RC2/d8content/metadatadisplays)

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
