# Archipelago Software Services

![ADOlife](../imgs/architecture.png)

At the core of the Archipelago philosophy is our commitment to both simplicity and flexibility.

### Under the hood, Archipelago's architecture is:
 - [Drupal (CMS)](https://www.drupal.org/)
 - [Solr (Search)](https://lucene.apache.org/solr/)
 - [Cantaloupe (Image Server)](https://cantaloupe-project.github.io/)
 - [S3 Storage (Mini.io or any other S3 flavor)](https://min.io/)

Installation is entirely [Dockerized](https://www.docker.com) and scripted with [easy-to-follow directions](https://github.com/esmero/archipelago-deployment/blob/1.0.0-RC2/README.md).
* Docker containers are as follows:
   Container | Purpose | Description
   --- | --- | ---
   esmero-web | NGNIX | Routes calls to esmero-php
   esmero-php | PHP-FPM | Has all binaries for postprocessing/exif/ocr/etc. Runs PHP code. 
   esmero-db | Database | AMD and INTEL processors: MYSQL 8<br />ARM processors: MariaDB 
   esmero-minio | Storage                     | S3 API compatible Backend file and ADO as file storage. In a local it will do all the S3 stuff, on a live instance it can server as file routing to AWS S3, Azure Blob Storage, etc. 
   esmero-solr | Solr |Currently version 8.8.2
   esmero-nlp | Natural Language Processing | NLP64 server for entity extraction, language detection, etc. 

_Information related to non-Dockerized installation and configruation can be found here: [Traditional Installation Notes](traditional-install.md)_

### Strawberryfield Modules at the heart of every Archipelago:
  - [Strawberryfield](https://github.com/esmero/strawberryfield)
  - [Format Strawberryfield](https://github.com/esmero/format_strawberryfield)
  - [Webform Strawberryfield](https://github.com/esmero/webform_strawberryfield)
  - [Strawberry Runners](https://github.com/esmero/strawberry_runners)
  - [Archipelago Multi-Importer (AMI)](https://github.com/esmero/ami)

Documentation related to the Strawberryfield modules can be found here: [Strawberryfields Forever](strawberryfields.md)

### Archipelago also extends these powerful tools:
  - [Annotorius](https://github.com/recogito/annotorious)
  - [Drupal Webform Module](https://www.drupal.org/project/webform)
  - [International Image Interoperability Framework (IIIF)](https://iiif.io/)
  - [Internet Archive BookReader](https://github.com/internetarchive/bookreader)
  - [Mirador](https://projectmirador.org)
  - [Pannellum](https://github.com/mpetroff/pannellum)
  - [Replayweb.page Webarchive Player](https://github.com/webrecorder/replayweb.page)
  - [Solr OCR Highlighting](https://github.com/dbmdz/solr-ocrhighlighting)
  - Twig Templating [In Symfony](https://twig.symfony.com) / [In Drupal](https://www.drupal.org/docs/theming-drupal/twig-in-drupal)

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
