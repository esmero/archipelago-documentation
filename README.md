# Archipelago Commons Intro

Archipelago Commons, or simple Archipelago, is an evolving Open-Source Digital Objects Repository - DAM Server Architecture based
on the popular CMS [`Drupal8/9`](https://www.drupal.org) and released under [`GLP V.3 Licence`](https://www.gnu.org/licenses/gpl-3.0.txt). 

It is a mix of deeply integrated custom coded Drupal8 modules (made with care by us), a curated and well configured Drupal8 instance, running under a discrete and and well planned set of service containers. 
All this driven by a clear and [concise but thoughtfull planned technical roadmap](https://github.com/esmero/archipelago-deployment/issues/5). 

Archipelago was dreamt as a multi-tenat, distributed capable system (as its name suggests!) and can live isolated or in flocks of similar deployments, sharing storage, services or even better, just the discovery layer. Learn more about the different [`Software Services`](devops.md) used by Archipelago.

Its primary focus is to serve the [`GLAM community`](https://en.wikipedia.org/wiki/GLAM_(industry_sector)) by providing a flexible, 
respectful to identities and existing workflows, consistent and unified way of describing, storing, linking, exposing metadata and media assets. 

All this under a bit different concept than the one we all have been used to in recent times. 

We like to think this is not done by re-inventing the wheel, but by making sure the road is clean, leveled and with less obstacles
than before, by removing some heavy weight from the top, some unneeded ballast, plus, of course, some well positioned innovations to make the ride enjoyable. 

We also like to say that Archipelago is like a Metadata Synthetizer (LFO anyone?) and we want to give you all the knobs, parameters, inputs and outputs to make the best out of it. 
Still you can make "music" by just tapping the keyboard. 

Finally. It tries to stay humble, slim and nimble in nature with a small code base full of inline comments and `@todos`. 

To get here we had to do a full stop first. Look around. Questioning everything we knew. Research and test (repeat) and then re-Architect slowly 
on new and old assumptions, but specially new community values. 

Learn [`what makes Archipelago so different`](docs/ourtake.md) and hopefully also a well known place.


## Strawberryfields for ever

Archipelago integrates transparently into the Drupal 8 ecosystem using its Core Content Entity System (Nodes), Discovery (Search API) and in general all its Core Components plus a few well maintained external ones. 

But by design (and because we think its imperative) it takes fully charge, owns, the Metadata layer and their associated Media assets by implementing a highly
configurable and also smart JSON based Drupal field named [`strawberryfield`](https://github.com/esmero/strawberryfield/tree/8.x-1.0-beta1) that attaches to any Content. 

All these JSON's internals, keys, paths and values are dynamically exposed to the rest of the ecosystem and 
Strawberryfield even remembers it's structure as data evolves by storing JSON Paths of every little detail. 

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

Archipelago welcomes and appreciates any type of contribution, from use cases and needs, questions, documentation, devops and configuration and of course code, fixes or new features. To make the process less painful we recommend you first to read our documentation and deploy a local instance. After that please follow [this set of guidelines](docs/giveandtake.md) to help you get started.

## Caring & Coding + Fixing

* [Diego Pino](https://github.com/DiegoPino)
* [Giancarlo Birello](https://github.com/giancarlobi)

## Acknowledgments

This software is a [Metropolitan New York Library Council](metro.org) Open-Source initiative and part of the Archipelago Commons project.

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
