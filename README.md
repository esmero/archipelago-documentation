# Documentation Menu
* [What is Archipelago?](https://github.com/esmero/archipelago-documentation#archipelago-commons-intro)
* [Code of Conduct](https://github.com/esmero/archipelago-documentation#code-of-conduct)
* [Instructions and Guides](https://github.com/esmero/archipelago-documentation#instructions-and-guides)
* [Archipelagos in the Wild](https://github.com/esmero/archipelago-documentation#archipelagos-in-the-wild)
* [Contributing](https://github.com/esmero/archipelago-documentation#contributing)

# Archipelago Commons Intro

Archipelago Commons, or simply Archipelago, is an evolving Open Source Digital Objects Repository / DAM Server Architecture based on the popular CMS [`Drupal8/9`](https://www.drupal.org) and released under [`GLP V.3 License`](https://www.gnu.org/licenses/gpl-3.0.txt).

Archipelago is a mix of deeply integrated custom-coded Drupal8 modules (made with care by us) and a curated and well-configured Drupal8 instance, running under a discrete and and well-planned set of service containers.

Archipelago was dreamt as a multi-tenant, distributed, capable system (as its name suggests!) and can live isolated or in flocks of similar deployments, sharing storage, services, or -- even better -- just the discovery layer. Learn more about the different [`Software Services`](docs/devops.md) used by Archipelago.

Archipelago's primary focus is to serve the greater [`GLAM community`](https://en.wikipedia.org/wiki/GLAM_(industry_sector)) by providing a flexible, consistent, and unified way of describing, storing, linking, exposing metadata and media assets. We respect identities and existing workflows. We endeavor to design Archipelago in ways that empower communities of every size and shape.

Finally, Archipelago tries to stay humble, slim, and nimble in nature with a small code base full of inline comments and `@todos`. All of our work is driven by a clear and [concise but thoughtful planned technical roadmap --updated in tandem with new releases](https://github.com/esmero/archipelago-deployment/issues/80).

Learn what makes Archipelago so different and hopefully also a well known place:
* [Archipelago's Philosophy & Guiding Principles](docs/ourtake.md)
* [Strawberryfields Forever](docs/strawberryfields.md)
* [Software Services](docs/devops.md)

# Code of Conduct
The Archipelago Commons community is dedicated to providing a welcoming and positive experience for all participants. To ensure this can be met for all parties, please review our [Code of Conduct](CODE_OF_CONDUCT.md).

# Instructions and Guides

## Archipelago Deployment Quickstart
To get a general glance of what is included and how it works, follow [this guide](https://github.com/esmero/archipelago-deployment/blob/1.0.0-RC1/README.md) to deploy our latest beta locally using `Docker` with OSX or Ubuntu 18.04.

## Archipelago 101 (and up)
Now that you have Archipelago running, the following guides will help you master the basics.
* [Ingesting Your First Object](docs/firstobject.md)
* [General Q&A](docs/generalqa.md)
* [Webforms In Archipelago _(*Work-In-Progress Documentation)_](docs/webforms.md)
* [Creating Display Modes](docs/createdisplaymodes.md)
* [Managing Metadata Displays with Twig _(*Work-In-Progress Documentation)_](docs/metadatatwigs.md)
* [Debugging PHP in Archipelago](docs/xdebug.md)
* [Archipelago's File Persistence Strategy](docs/archifilepersistencestrategy.md)
* [How to Setup SSL for Docker/Archipelago _(*Work-In-Progress Documentation)_](docs/sslsetup.md)

## Archipelagos in the Wild
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
