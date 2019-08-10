# Archipelago Commons Intro

Archipelago Commons, or simply Archipelago, is an Open-Source Digital Objects Repository - DAM Server Architecture based
on the popular CMS [`Drupal8/9`](https://www.drupal.org) and released under [`GLP V.3 Licence`](https://www.gnu.org/licenses/gpl-3.0.txt).

Archipelago is a mix of deeply integrated custom coded Drupal8 modules (made with care by us) and a curated and well configured Drupal8 instance, all running under a discrete and and well planned set of service containers.
This work is driven by our [technical roadmap](https://github.com/esmero/archipelago-deployment/issues/5).

Archipelago was born of our vision for a distributed, multi-tenant system where repositories and can all exist as their own unique islands or in constellations or flocks of similar deployments. These island repositories can share storage, services, or even just a discovery layer. You can learn more about the different Software Services used by Archipelago [`here`](devops.md).

The primary focus of Archipelago is to serve the [`GLAM community`](https://en.wikipedia.org/wiki/GLAM_(industry_sector)) by providing a flexible system that is deeply respectful of identities and existing workflows, while offering a consistent and unified way to describe, store, link, and expose metadata and media assets. We have approached repository architecture a little differently than we have seen in other solutions. We like to think of Archipelago as a Metadata Synthetizer (LFO anyone?) and we want to give you all the knobs, parameters, inputs and outputs so you can make the unique sounds you want to make with it. To get here we had to start over from scratch, look and listen, and question everything we thought knew. Our first release is the culmination of a great deal of research and testing.

Finally, Archipelago tries to maintain a humble profile. We built this to be slim and nimble in nature with a small code base full of inline comments and `@todos`. Please take a moment to read more and learn [`what makes Archipelago so different`](docs/ourtake.md) and hopefully a very useful solution for you.


## Strawberryfields for ever

Archipelago integrates cleanly and transparently into the Drupal 8 ecosystem using its Core Content Entity System (Nodes), Discovery (Search API), and other general Core Components plus a few well of the very well maintained external ones.

Archipelago's unique contribution back to Drupal lies in the way that we control the metadata layer and their associated media assets. We do this by implementing a highly configurable and also smart JSON based Drupal field named [`strawberryfield`](https://github.com/esmero/strawberryfield/tree/8.x-1.0-beta1) that attaches to any kind of content. All of the JSON internals, keys, paths and values that are stored in a Strawberryfield are dynamically exposed to the rest of the Drupal ecosystem. Strawberryfield even remembers it's structure as data evolves by storing JSON Paths of every little detail.

Explore what [`strawberryfield does`](docs/strawberryfield.md), why we built it and what issues it addresses.

## Nothing is real

Archipelago includes two additional companion modules, [`webform_strawberryfield`](https://github.com/esmero/webform_strawberryfield/tree/8.x-1.0-beta1) and [`format_strawberryfield`](https://github.com/esmero/webform_strawberryfield/tree/8.x-1.0-beta1) that
extend the core metadata capabilities of strawberryfield and allow the same flexibility to be exposed during Ingest and Viewing of Digital Objects.

## Ingesting

`Webform Strawberryfield` (we had a better name) extends and integrates into the [`amazing Drupal Webform module`](https://www.drupal.org/project/webform) to allow Archipelago users
to build any possible Metadata and Media, ingest and edit, workflows directly via the UI using web forms.

By not having a hardcoded ingest method, Archipelago can be used outside the GLAM community too, as a pure Data repository, Biological Sciences, Digital Humanities, Archives or even as a mixed, mutant cross domain system.

We also added `WIKIDATA`, `LoC` and `Getty` Authority querying elements to aid in linking to external Linked Open Data Sources.

All these integrations are made to help local needs and community identities to survive the never ending race for the next Metadata Schema
and prototype, plan and grow independently of how metadata will need to be exposed yesterday or tomorrow. And we plan to add more.

Explore what other features [`webform_strawberryfield provides to help with ingesting`](docs/webform.md), reading and interacting with your Metadata during that process.

## Exposing
`Format Strawberryfield` (we had even a better name but...) deals with taking your JSON based metadata and `casting`, mashing, mixing, exposing,
displaying and transforming it to allow rich interaction for users and other systems with your Digital Objects.

In it's guts (or heart?) it does something quite simple but core to our concept of Repository:
It transforms in realtime the _close to your needs open schema metadata_ that lives in strawberryfield as JSON into _close to other one's fixed schema needs metadata_; any destination format, using a fast, cached templating system.
A templating system easy enough to master and core to Drupal, called [`Twig`](https://twig.symfony.com/doc/2.x/).

This templating system is exposed to Archipelago users through the UI and stored side by side in the repository as content
(we named them `Metadata Display entities`, but they not only serve display needs!) so users can fully control how metadata is transformed and published without touching their individual sources.

Templates or Recipes can be shared, exported, ingested, updated and adapted in many ways; fast changes are possible without having to wait for the next mayor release of Archipelago or their favourited Metadata Schema Specs Committee agreeing on the next or the last version. Of course this module not only handles metadata, but Media assets too, extracting local or remote URIs and files from your metadata and rendering them as Media Viewers: Books, 3D models, Images, Panoramas, A/V with IIIF in its soul. Discover what [`format_strawberryfield can`](format.md) do and what many other possibilities are exposed through our templating system.

# Archipelago Deployment Quickstart

To get a general glance of what is included and how it works deploy our latest beta locally using `Docker` using [this Guide](https://github.com/esmero/archipelago-deployment/blob/8.x-1.0-beta1/README.md) covering for now OSX and Ubuntu 18.04.

# Archipelago Usage Quickstart

Now that you have it running, its time to ingest your first digital Object. Follow this [Guide](docs/firstobject.md) to master the basics.

# Contributing

Archipelago welcomes and appreciates any type of contribution, from use cases and needs, questions, documentation, devops and configuration and of course code, fixes or new features. To make the process less painful we recommend you first to read our documentation and deploy a local instance. After that please follow [this set of guidelines](docs/giveortake.md) to help you get started.

## Caring & Coding + Fixing

* [Diego Pino](https://github.com/DiegoPino)
* [Giancarlo Birello](https://github.com/giancarlobi)

## Acknowledgments

This software is a [Metropolitan New York Library Council](metro.org) Open-Source initiative and part of the Archipelago Commons project.

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
