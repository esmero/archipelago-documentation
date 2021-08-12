# Documentation Menu
* [What is Archipelago?](https://github.com/esmero/archipelago-documentation#archipelago-commons-intro)
* [Code of Conduct](https://github.com/esmero/archipelago-documentation#code-of-conduct)
* [Instructions and Guides](https://github.com/esmero/archipelago-documentation#instructions-and-guides)
* [Archipelago Presentations and Documents](https://github.com/esmero/archipelago-documentation#archipelago-presentations-and-documents)
* [Contributing](https://github.com/esmero/archipelago-documentation#contributing)

# Archipelago Commons Intro

Archipelago Commons, or simply Archipelago, is an evolving Open Source Digital Objects Repository / DAM Server Architecture based on the popular CMS [`Drupal8/9`](https://www.drupal.org) and released under [`GLP V.3 License`](https://www.gnu.org/licenses/gpl-3.0.txt).

Archipelago is a mix of deeply integrated custom-coded Drupal modules (made with care by us) and a curated and well-configured Drupal instance, running under a discrete and well-planned set of service containers.

Archipelago was dreamt as a multi-tenant, distributed, capable system (as its name suggests!) and can live isolated or in flocks of similar deployments, sharing storage, services, or -- even better -- just the discovery layer. Learn more about the different [`Software Services`](docs/devops.md) used by Archipelago.

Archipelago's primary focus is to serve the greater [`GLAM community`](https://en.wikipedia.org/wiki/GLAM_(industry_sector)) by providing a flexible, consistent, and unified way of describing, storing, linking, exposing metadata and media assets. We respect identities and existing workflows. We endeavor to design Archipelago in ways that empower communities of every size and shape.

Finally, Archipelago tries to stay humble, slim, and nimble in nature with a small code base full of inline comments and `@todos`. All of our work is driven by a clear and [concise but thoughtful planned technical roadmap --updated in tandem with new releases](https://github.com/esmero/archipelago-deployment/issues/103).

Learn what makes Archipelago so different and hopefully also a well known place:
* [Archipelago's Philosophy & Guiding Principles](docs/ourtake.md)
* [Strawberryfields Forever](docs/strawberryfields.md)
* [Software Services](docs/devops.md)

# Code of Conduct
The Archipelago Commons community is dedicated to providing a welcoming and positive experience for all participants. To ensure this can be met for all parties, please review our [Code of Conduct](CODE_OF_CONDUCT.md).

# Instructions and Guides

## Archipelago Deployment Quickstart
To get a general glance of what is included and how it works, follow [this guide](https://github.com/esmero/archipelago-deployment/blob/1.0.0-RC2/README.md) to deploy our latest beta locally using `Docker` with OSX or Ubuntu 18/20.04.

## Archipelago 101 (and up)
Now that you have a general understanding of [Archipelago's architecture](https://github.com/esmero/archipelago-documentation#archipelago-commons-intro), and have a [local Archipelago](https://github.com/esmero/archipelago-deployment/blob/1.0.0-RC2/README.md) running, the following guides will help you master the basics.

_Site Administration & Configuration_
* [Archipelago's File Persistence Strategy](docs/archifilepersistencestrategy.md)
* [How to Setup SSL for Docker/Archipelago _(*Work-In-Progress Documentation)_](docs/sslsetup.md)
* [Debugging PHP in Archipelago](docs/xdebug.md)
* [Creating Display Modes](docs/createdisplaymodes.md)
* [Strawberryfield Formatters](docs/strawberryfield-formatters.md)
* [Configuration for Google Sheets API](docs/googleapi.md)
* [General Q&A](docs/generalqa.md)
  * [Twig Modules Configuration](docs/generalqa.md#twig-modules-configuration), [SMTP Configuration](docs/generalqa.md#smtp-configuration), [Min.io Logging](https://github.com/esmero/archipelago-documentation/blob/1.0.0-RC2/docs/generalqa.md#minio-logging)
* [Annotations in Archipelago](docs/annotations.md)

_Digital Objects and Collections Creation, Metadata and Cataloging, General Workflows_
* [Ingesting Your First Object](docs/firstobject.md)
* [Webforms In Archipelago](docs/webforms.md)
  * [How to Create a Webform as an Input Method for Archipelago Digital Objects (ADO)](docs/webformsasinput.md)
  * [Customizing Webforms: Modifying allowable file extensions](docs/modifyingfileextensionsinwebform.md)
  * [Archipelago Custom Webform Elements](docs/customwebformelements.md)
* [Twig Templates and Archipelago](docs/metadatatwigs.md) _(how Twig is used in Archipelago)_
  * [Working With Twig in Archipelago](docs/workingtwigs.md) _(getting started with custom Twig templates)_
* [Archipelago Multi-Importer (AMI)](docs/ami.md)
  * [Spreadsheet Formatting Overview](docs/AMIspreadsheet.md)
  * [Ingesting Digital Objects and Collections using Spreadsheets or Google Sheets](docs/AMIviaSpreadsheets.md) 
  * [Using the Islandora 7 Solr Importer plugin](docs/I7solrImporter.MD)

# Archipelago Presentations and Documents
* [Archipelago : an empathic Digital Repository Architecture](https://tinyurl.com/archipelago-brief-presentation)
* [Working with Archipelago Multi-Importer](https://tinyurl.com/workingwithAMI)
* [Twig Templates and Archipelago](http://tinyurl.com/archipelagoandtwig)
* [Archipelago Digital Objects Repository (an) architecture to last (DrupalCon North America 2021)](https://tinyurl.com/archipelagoDCNA)
* [Archipelago 1.0.0-RC2 Specs and features](https://tinyurl.com/ArchipelagoRC2specs)

# Archipelagos in the Wild
Explore Archipelago instances running free across digital realms.
* [Archipelago Community Showcase](docs/inthewild.md)
* [METRO + Archipelago](http://archipelago.nyc)

# Contributing
Archipelago welcomes and appreciates any type of contribution, from use cases and needs, questions, documentation, devops and configuration and -- of course -- code, fixes, or new features. To make the process less painful, we recommend you first to read our documentation and deploy a local instance. After that please follow [this set of guidelines](docs/giveortake.md) to help you get started.

## Caring & Coding + Fixing
* [Diego Pino](https://github.com/DiegoPino)
* [Giancarlo Birello](https://github.com/giancarlobi)
* [Allison Lund](https://github.com/alliomeria)

## Acknowledgments
This software is a [Metropolitan New York Library Council](https://metro.org) Open-Source initiative and part of the Archipelago Commons project.

## License
[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
