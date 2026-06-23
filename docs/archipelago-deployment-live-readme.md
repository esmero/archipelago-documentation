---
title: Archipelago-deployment-live
tags:
  - Archipelago-deployment-live
  - Drupal 10
---

# Archipelago Deployment Live

A Cloud / Local production ready Archipelago 1.7.0 Deployment (Drupal 10) using Docker,
For Drupal 11 (IOHO less stable) please follow the [Archipelago 2.1.0 Deployment Live](https://github.com/esmero/archipelago-deployment-live/blob/2.1archipelago-deployment-live-readme.md) guide. Same features but different code (and more work for us!).

Last updated: Jun 17th 2026 for Drupal 10.6.11 and Archipelago 1.7.0 (and 2.1.0) release day!

Previously updated: for 1.6.0, May 22nd 2026 for https://www.drupal.org/psa-2026-05-18 (Drupal 10.6.9)

Previously updated: for 1.6.0, December 10th 2025.


## What is this repo for?

Running Archipelago Commons on a live public instance using SSL with Blob/Object Storage backend 

- Cloud-based deployment, e.g. AWS/Azure under Linux
- Self-managed servers running Linux
- x86/AMD86 or ARM64/v8 CPU architectures

## What is this repo not for? 

- Running your own local/development Archipelago. For that we suggest using <https://github.com/esmero/archipelago-deployment>

### What is inside this dumpling for 1.7.0? (new section)

- minio.io (latest) for local or Routed S3 with Console. We recommend disabling it and using directly a cloud managed S3 for better performance and full multipart upload compliance if deciding to go for the route (default) version.
- Updated Apache Solr 10.0 with custom built and updated wizardly Solr OCR Highlight library [v0.10](https://github.com/dbmdz/solr-ocrhighlighting) coded and maintained by the Development Team at the [Bavarian State Library](https://github.com/dbmdz). Thanks Johannes Baiter and team.
- MySQL 8.4(amd64/x86) or MariaDB 12.3 (Arm64/M1/M2/M3/M4/M5)
- Updated NGINX 1.31.1
- Updated Custom PHP-FPM 8.3 multi architecture, fine-tuned for Drupal 10/11 , WARC to WACZ processing, Tesseract 5 with JP2 support, PDFAlto(what a pain to build!) and latest Composer 2.x, Drush 13.x-dev, FFMPEG, FIDO, plus (NEW) Audiowave for Waveform to JSON extraction and BWFmetaedit for WAV files holding BWF metadata (Checksumming and other extras per stream).
- Natural Language Processing via NLPWEB64 multi architecture with FastText Language detection (Thanks Mike Bennett!) or alternatively Machine learning/ML containers/APIs. (Image similarity: YOLO,MobileNet,ViT(New),Insightface and Text transformer: SBERT) differentiated for arm64 and amd/intel/64
- New Cantaloupe 6.0.6 (our own versioning, but based on latest `dev` upstream) on Java 26, multi architecture, IIIF2/3 Server with precise Video Frame, PDF extraction, PDF Tiling support, new Jetty with tons of community and custom fixes.
- A Skeleton Project setup to run latest Version of Drupal 10 (10.6.11), Updated Archipelago Chiloe Base theme based on Bootstrap 5 with Light/Dark Mode (for those late night dwellers) and Strawberry Field modules on 2.1.0.
- Complete support for Apple Silicon M1/M2/M3/M4/M5 Machines and in general arm64 architecture Chips like Raspberry Pi 4, with specially built arm64 docker containers. The only differences now between deployment strategies is the DB. Blazing fast OCR.
- A freshly baked Anubis with bugfixes, more Bot control and new options for rules. Also faster.

## Requirements

### Minimal (not recommended for production)

- 4 Gbytes of RAM (e.g AWS EC2 t3.medium) 2 CPUs, Single SSD Drive of 100 Gbytes

### Minimal for production

- 8 Gbytes of RAM (AWS EC2 t3.medium)  2 CPUs, Single SSD Drive of 100 Gbytes, optional: one magnetic Drive of 500 Gbytes for Caches/Temp files/Backups.

### Recommended for production

- 16-32 Gbytes of RAM or more (AWS EC2 m6g.xlarge 2xlarge - Graviton) 4 CPUs, Single SSD Drive of 200 Gbytes, optional: one magnetic Drive of 1TB for Caches/Temp files/Backups.

### OS:

- Ubuntu 22.04 LTS /Ubuntu 24.04 LTS/Amazon Linux 2023/Debian 10 "Buster" matching your CPU architecture (of course)
- Most recent `Docker` running as a service and `docker-compose` or `docker compose`

### Skills and the important human aspect

- Basic Unix/Linux terminal skills and a root/sudo account
- `Docker` basics knowledge and how to manage packages in your System
- Knowledge of how to manage AWS/Azure or any other cloud-based provider for Computing Instances and S3 compatible Object Storage system and the associated permissions credentials to do so.
- To be patient. Please read everything. Do not skip steps. Do not only copy and paste commands. Explanations give context and might help you troubleshoot issues.

Basically this guide is meant for humans with basic to medium `DevOps` background or humans with patience that are willing to troubleshoot, ask, and try again when that background is not (yet) enough. And we are here to help.

## Deployment on Linux/X86/AMD system

### Step 1:

Deploy your base system

Make sure your Firewall/AWS Security group has these ports open for everyone to access

- 443 (NGINX SSL)
- 80 (NGINX HTTP)
And protected/modally open for your own development/testing/administration
- 8183 (Cantaloupe)
- 8983 (Solr)
- 6400 (NLP64)
- 9001 (Minio)
- 22 (so you can ssh into your machine)

Setup your system using your favorite package manager with 

- Docker
- git
- htop
- tree
- docker-compose or `docker compose` (V2) if running the most recent docker service. See https://docs.docker.com/compose/migrate/

e.g. for Amazon Linux 2023 (x86/amd64) these steps are tested:

```shell
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
sudo chkconfig docker on
sudo systemctl enable docker
sudo yum install -y git htop tree jq perl-JSON-PP
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo reboot
```

Reboot is needed to allow Docker to take full control over your OS resources.

### Step 2:

In your location of choice clone this repo

```shell
git clone https://github.com/esmero/archipelago-deployment-live
cd archipelago-deployment-live
git checkout 1.7.0
```

### Step 3. Setup your enviromental variables for Docker/Services

#### Setup Enviromentals

Setup your deployment enviromental variables by copying the template

```shell
cp deploy/ec2-docker/.env.template deploy/ec2-docker/.env
```
and editing it

```shell
nano deploy/ec2-docker/.env
```

The content of that file would be similar to this. 
`Note`: There are a few extra commented lines at the end only used for: https://docs.archipelago.nyc/1.7.0/security_bots/ if you decide to go that way, but also not needed if running `anubis`.

```env
ARCHIPELAGO_ROOT=/home/ec2-user/archipelago-deployment-live
ARCHIPELAGO_EMAIL=your@validemail.org
ARCHIPELAGO_DOMAIN=your.domain.org
MINIO_ACCESS_KEY=THE_S3_AZURE_OR_LOCAL_MINIO_KEY
MINIO_SECRET_KEY=THE_S3_AZURE_OR_LOCAL_MINIO_SECRET
MYSQL_ROOT_PASSWORD=YOUR_MYSQL_PASSWORD_FOR_ARCHIPELAGO
MINIO_BUCKET_MEDIA=THE_NAME_OF_YOUR_S3_BUCKET_FOR_PERSISTEN_STORAGE
MINIO_FOLDER_PREFIX_MEDIA=media/
MINIO_BUCKET_CACHE=THE_NAME_OF_YOUR_S3_BUCKET_FOR_IIIF_STORAGE
MINIO_FOLDER_PREFIX_CACHE=iiifcache/
REDIS_PASSWORD=YOUR_REDIS_PASSWORD
PHP_MEMORY_LIMIT=MEGABYTES_OF_MEMORY_ASSIGNED_TO_PHP_WEB
PHP_CLI_MEMORY_LIMIT=MEGABYTES_OF_MEMORY_ASSIGNED_TO_PHP_CLI_DRUSH_HYDROPONICS
ANUBIS_PRIVATE_KEY=ed25519_PRIVATE_KEY
```

What does each key mean?

- `ARCHIPELAGO_ROOT`: the **absolute path** to your `archipelago-deployment-live` git repo in your host machine.
- `ARCHIPELAGO_EMAIL`: a valid **email**, will be used to register your SSL Certificate via Certbot.
- `ARCHIPELAGO_DOMAIN`: a valid **domain name** for your repository. This domain will be also used to request your SSL Certificate via Certbot.
- `MINIO_ACCESS_KEY`: If you are running a Cloud Service backed S3/Azure Storage this needs to be generated there. The user/IAM owner of this ACCESS KEY needs to have access to read/write the bucket you will configure in this same `.env`. If running local `min.io` whatever you set will be used.
- `MINIO_SECRET_KEY`: If you are running a Cloud Service backed S3/Azure Storage this needs to generated there. The user/IAM owner of the matching SECRET_KEY needs to have access to read/write the bucket you will configure in this same `.env` file. If running local `min.io` whatever you set will be used.
- `MYSQL_ROOT_PASSWORD`: The MYSQL 8 or Mariadb 15 password. This password will be used later also during Drupal deployment via `drush`
- `MINIO_BUCKET_MEDIA`: The name of your Persistant Storage Bucket. If using mini.io local we recommend keeping it simple, e.g. `archipelago`.
- `MINIO_FOLDER_PREFIX_MEDIA`: The `folder` (a prefix really) where your DO Storage and File storage will go inside the `MINIO_BUCKET_MEDIA` Bucket. `media/` is a _fine_ name for this one and common in archipelago deployments. **IMPORTANT:** Always terminate these with a `/`. Drupal will only have access to files inside this prefix. Means a URL like s3://myfile.pdf requires myfile.pdf to exist inside that bucket and inside that prefix.
- `MINIO_BUCKET_CACHE`: The name of your IIIF Cache storage Bucket. May be the same as `MINIO_BUCKET_MEDIA`. If different make sure your your `MINIO_ACCESS_KEY` and/or IAM role ACL have permission to read write to this one too.
- `MINIO_FOLDER_PREFIX_CACHE`:  The `folder` (a prefix really) where Cantaloupe will/can write its `iiif` caches. `iiifcache/` is a _lovely_ name we use a lot. **IMPORTANT:** Always terminate these with a `/`.
- `REDIS_PASSWORD`: Password for your REDIS (Drupal Cache/Queue storage) if you decide to enable the Drupal REDIS module.
- `PHP_MEMORY_LIMIT`: PHP's (esmero-php) Memory limit in Megabytes without the "M" at the end(so just a number). Defaults to 1024
- `PHP_CLI_MEMORY_LIMIT`: PHP's (esmero-php) client (the php command, drush and hydropinics) Memory limit in Megabytes without the "M" at the end(so just a number) t. Defaults to PHP_MEMORY_LIMIT. If you are going to enabled Search API Background indexing, new since 1.5.0, then this number should be ideally 2048 so the queue can render/ingest/and do its Drupal magic.
- `ANUBIS_PRIVATE_KEY`: Since 1.5.0. Because we (and you) do not like ML Bots (or people making money out of your data and re-selling the output as canned/simplified answers), you should put the output of ```openssl rand -hex 32``` in this key.

`IMPORTANT NOTE`: For AWS EC2. If you selected an `IAM role` for your server when setting it up/deploying it, `min.io` will use the AWS EC2-backed internal API to request access to your S3. This means the ROLE itself needs to have read/write access (ACL) to the given Bucket(s) and your key/secrets won't be able to override that. Please do not ignore this note. It will save you a LOT of frustration and coffee. You can also run an EC2 instace without a given IAM and in that case just the ACCESS_KEY/SECRET will matter.

Now that you know, you also know that these values **should not be shared** and this `.env` file **should not be committed/kept in version control**. Please be careful.

`docker-compose` will read this `.env` and start all services for you based on its content.

Once you have modified this you are ready for your first big decision. 

#### Running a fully qualified domain you wish a valid/signed certificate for AMD/INTEL Architecture?

This means you will use the `docker-compose-aws-s3.yml`. Do the following:

```shell
cp deploy/ec2-docker/docker-compose-aws-s3.yml deploy/ec2-docker/docker-compose.yml
```

#### Running a fully qualified domain you wish a valid/signed certificate for ARM64/Apple M1 Architecture?

This means you will use the `docker-compose-aws-s3-arm64.yml`. Do the following:

```shell
cp deploy/ec2-docker/docker-compose-aws-s3-arm64.yml deploy/ec2-docker/docker-compose.yml
```

### Step 4. First Run

Be sure you are in your archipelago-deployment-live (Git) base folder.

#### First Permissions


```shell
sudo chown 8183:8183 config_storage/iiifconfig/cantaloupe.properties
sudo chown -R 8183:8183 data_storage/iiifcache
sudo chown -R 8183:8183 data_storage/iiiftmp
sudo chown -R 8983:8983 data_storage/solrcore
```

#### Second, Choices (so many)

Archipelago ships (since 1.5.0) with a custom [Anubis](https://anubis.techaro.lol/) and for this release also a freshly renewed (new tag) one. Anubis is an OSS Application Firewall/middleware that will alliviate some very valid concerns (and late night server hiccups, even costs related issues) related to AI/ML/Bot swarms and unwanted traffic. But you need to choose. And you need to choose now. Want it enabled immediately? Later on? In any case we need to do some setup. Not hard. Let's get started.

##### Important Note About Troubleshooting Anubis Configurations

To help keep usage of Anubis effective, we are intentionally not detailing all of the configuration options and devops-related troubleshooting in a live, public document. Please reach out over the Archipelago Slack/email for assistance with troubleshooting Anubis for your Archipelago instance.

##### Anubis needs some rules (even if you decide not to run them yet)

The following files are needed by Anubis and mounted on start. if you get this wrong a `docker logs -f esmero-anubis` will tell you the rules are not present and traffic will be blocked. Don't get it wrong!

```
cp config_storage/anubisconfig/allow.yaml.default config_storage/anubisconfig/allow.yaml
cp config_storage/anubisconfig/deny.yaml.default config_storage/anubisconfig/deny.yaml
cp config_storage/anubisconfig/botPolicies.yaml.default config_storage/anubisconfig/botPolicies.yaml 
```

We also want your Internet domain to be setup here. Replace in the following command `your.domain.org` with the domain you setup in your `.env` file and run:

```
sed -i 's/http:\/\/esmero-web/https:\/\/your.domain.org/g' config_storage/anubisconfig/allow.yaml 
```

And! Make sure (please) you did not skip the documentation (did you? gosh.) and by doing so also forgot to setup the ANUBIS_PRIVATE_KEY key in `.env`?

If you indeed forgot, run:

```openssl rand -hex 32```

Edit `.env` and add the output of that command to the `ANUBIS_PRIVATE_KEY=` enviromental key. 

##### Anubis wants to know if they are invited

In old times you would just trust our default NGINX template setup. We do know you ended modifying it and tunning it. So. Now you have two templates. One that invites Anubis to intercept all traffic and help with bots, one that works as before and only includes our most basic IP Deny and Agent based rules (which are also present in Anubi's one).

If you want Anubis to be intercepting NGINX traffic from the moment your services go up, please do this

```
cp config_storage/nginxconfig/template/nginx.conf.template.anubis config_storage/nginxconfig/template/nginx.conf.template
```

If you want to run NGINX first without any intercepting please do. You can always come back to this later on.

```
cp config_storage/nginxconfig/template/nginx.conf.template.default config_storage/nginxconfig/template/nginx.conf.template
```
#### IMPORTANT!! (IP Based embargoes and Anubis)

If you decided to run Anubis, you must be aware that because of how it acts as "middleware" in NGINX, the client IP, Port and other original Client Information are going to be sent via headers from NGINX to PHP on the backend. By default Drupal and Symfony will not trust those headers (nor its origin) and that will intefere with IP Based embargo Bypass Logic as defined on the Format Strawberry Field Module (if you set that up). If you don't act on this, still any IP based embargoed ADOs will be secure. Without making the changes recommended below, your site's embargoed objects will be *so secure* that literally nobody other than a logged in (via Drupal) user will be able to access them because the Client IP that PHP will see is the one of the NGINX Container (inside Docker).

Only IF you are running Anubis (don't do this if not - DANGER -), and to ensure trustable information from the Forwarded IP headers is decoded as trusted "Client IP" on PHP, please edit your `drupal\web\sites\default\settings.php` and ensure that you replace the following `if (PHP_SAPI !== 'cli')` statement with the following snippet (or if recently deployed just comment/uncomment what is there already).

```PHP
if (PHP_SAPI !== 'cli') {
  $settings['reverse_proxy'] = TRUE;
  # $settings['reverse_proxy_addresses'] = [@\$_SERVER['REMOTE_ADDR']];
  # If Running Anubis via NGINX, as Documented in this release, comment (or keep commented out) the previous line
  # and (keep) uncomment The two following Lines. Add/Replace Any Private IP Ranges under which your Docker Containers Run. 
  # The ranges set there are the most common ones found for Docker Networks, but could be different if you customized it.
  # You can also disable some of the trusted headers for extra security (most important one is the .
  $settings['reverse_proxy_addresses'] = ['10.0.0.0/8','192.168.0.0/16', '172.16.0.0/12'];
  $settings['reverse_proxy_trusted_headers'] = \Symfony\Component\HttpFoundation\Request::HEADER_X_FORWARDED_FOR | \Symfony\Component\HttpFoundation\Request::HEADER_X_FORWARDED_HOST | \Symfony\Component\HttpFoundation\Request::HEADER_X_FORWARDED_PORT | \Symfony\Component\HttpFoundation\Request::HEADER_X_FORWARDED_PROTO | \Symfony\Component\HttpFoundation\Request::HEADER_FORWARDED;
}
```

That is all. DONE! Danke!

#### Actual first run

Time to spin our docker containers for the first time. We will start all without going into background so log/error checking is easier, especially if you have selected a Valid/Signed Cert choice and also want to be sure S3 keys/access are working.

```shell
cd deploy/ec2-docker
docker-compose up
```

You will see a lot of things happening. Check for errors/problems/clear alerts and give all a minute or so to start. 
Ok, let's assume your setup managed to request a valid signed SSL cert, you will see a nice message!

```shell
- Congratulations! Your certificate and chain have been saved at:XXXXX
   Your certificate will expire on 20XX-XX-XX. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
```

Archipelago will do that for you whenever it's about to expire so no need to deal with this manually, even when `docker-compose` restarts.

Now press CTRL+C. `docker-compose` will shutdown gracefully. Good!

### Step 5. Deploy Drupal 10

#### Composer

Copy the shipped default composer.default.json to composer.json and composer.default.lock to composer.lock (ONLY if you are installing from scratch):

```shell
cp ../../drupal/composer.default.json ../../drupal/composer.json
cp ../../drupal/composer.default.lock ../../drupal/composer.lock
```

Start Docker again

```shell
docker-compose up -d
```

Wait a few seconds and run:

```shell
docker exec -ti esmero-php bash -c "chown -R www-data:www-data private"
docker exec -ti esmero-php bash -c "chown -R www-data:www-data web/sites"
docker exec -ti esmero-php bash -c "composer install"
```

Composer install will take a little while and bring all your PHP libraries.

Note: Because `composer install` uses the shipped composer.lock and sometimes (inside the same shipped version) we fix bugs, it is always safe and recommended to run afterwards an extra `"Let's check, if under the archipelago and strawberryfield defined versions, there is newer code"`. We try to keep `composer.default.lock` always updated when bug fixes (after release/pre next release cycle) happen, but one never knows when this documentation is being followed or if you cloned things weeks ago, so no harm on checking.

Run the following command to fetch any last minute (if any) updates of our core modules:

```shell
docker exec -ti esmero-php bash -c "composer update archipelago/* strawberryfield/*"
```

#### Note on Composer, Github and VCS repositories

for this release we had to patch Drupal and contributed modules a lot. We waited patiently for the maintainers to merge/accept or even review our changes, but sometimes people are busy (for months!), so we had opt for fetching versions from forks and our own diverging (and very well tested) modifications. You might want to peek into the `composer.json` to see where/what is being fetched from other sources. That said. When you are running `composer update` and specially, if you do that very often, `Github` might ask you to generate a token (which requires you to have a Github account) to access the APIs without throttling limits. The message you will see might be like this:

```shell
GitHub API limit (60 calls/hr) is exhausted, could not fetch https://api.github.com/repos/DiegoPino/tableschema-php/commits/6d36f14a53c627aa12bca5837218f7e5fb0cfba7. Create a GitHub OAuth token to go over the API rate limit. You can also wait until 202X-XX-XX XX:XX:XX for the rate limit to reset.

You need to provide a GitHub access token.
Tokens will be stored in plain text in "/root/.config/composer/auth.json" for future use by Composer.
Due to the security risk of tokens being exfiltrated, use tokens with short expiration times and only the minimum permissions necessary.

Carefully consider the following options in order:

1. When you don't use 'vcs'  type 'repositories'  in composer.json and do not need to clone source or download dist files
from private GitHub repositories over HTTPS, use a fine-grained token with read-only access to public information.
Use the following URL to create such a token:
https://github.com/settings/personal-access-tokens/new?name=Composer+on+XXXX

2. When all relevant _private_ GitHub repositories belong to a single user or organisation, use a fine-grained token with
repository "content" read-only permissions. You can start with the following URL, but you may need to change the resource owner
to the right user or organisation. Additionally, you can scope permissions down to apply only to selected repositories.
https://github.com/settings/personal-access-tokens/new?contents=read&name=Composer+on+XXXX

3. A "classic" token grants broad permissions on your behalf to all repositories accessible by you.
This may include write permissions, even though not needed by Composer. Use it only when you need to access
private repositories across multiple organisations at the same time and using directory-specific authentication sources
is not an option. You can generate a classic token here:
https://github.com/settings/tokens/new?scopes=repo&description=Composer+on+XXXX

```

If you hit this limit (will never happen during a `composer install`), please follow the link on `1.`. It will generate a token (copy it please) that you can paste (it please) into the terminal. Then press enter. That is all. Tokens last some time so you won't be bothered again.

#### Now onto Drupal

Once done, execute our setup script that will prepare your Drupal `settings.php` and bring some of the `.env` enviromental variables to the Drupal environment. 

```shell
docker exec -ti esmero-php bash -c 'scripts/archipelago/setup.sh'
```

And now you can deploy Drupal! 

**IMPORTANT:** Make sure you replace in the following command inside `root:MYSQL_ROOT_PASSWORD` the `MYSQL_ROOT_PASSWORD` string with the **value** you used/assigned in your `.env` file for `MYSQL_ROOT_PASSWORD`. And replace `ADMIN_PASSWORD` with a password that is safe and you won't forget! That passwords is for your Drupal super user (uid:1). 

```shell
docker exec -ti -u www-data esmero-php bash -c "cd web;../vendor/bin/drush -y si --verbose --existing-config --extra=--skip-ssl --db-url=mysql://root:MYSQL_ROOT_PASSWORD@esmero-db/drupal --account-name=admin --account-pass=ADMIN_PASSWORD -r=/var/www/html/web --sites-subdir=default --notify=false;drush cr;chown -R www-data:www-data sites;"
```

### Step 6. Users and initial Content.

After installation is done (may take a few) you can install initial users and assign them roles. Copy each line separately. A missing permission will not let you ingest the initial Metadata Displays and AMI set.

```shell
docker exec -ti esmero-php bash -c 'drush ucrt demo --password="demo"; drush urol metadata_pro "demo"'
```

```shell
docker exec -ti esmero-php bash -c 'drush ucrt jsonapi --password="jsonapi"; drush urol metadata_api "jsonapi"'
```

```shell
docker exec -ti esmero-php bash -c 'drush urol administrator "admin"'
```

Before ingesting the base content we need to make sure we can access your `JSON-API` on for your new domain. That means we need to change internal urls (`https://esmero-web`) to the new valid SSL driven ones. This is easy:

On your host machine (no need to `docker exec` these ones), replace first in the following command `your.domain.org` with the domain you setup in your `.env` file. Go to (cd into) **your base git clone folder** (Important: **YOUR BASE CLONE FOLDER**) and then run

```shell
 sed -i 's/http:\/\/esmero-web/https:\/\/your.domain.org/g' drupal/scripts/archipelago/deploy.sh
 sed -i 's/http:\/\/esmero-web/https:\/\/your.domain.org/g' drupal/scripts/archipelago/update_deployed.sh
```

Now your `deploy.sh` and `update_deployed.sh` are update and ready. Let's ingest some Twig Templates, an AMI Set, menus and a Blocks.

```shell
docker exec -ti esmero-php bash -c 'scripts/archipelago/deploy.sh'
```

**IMPORTANT:**  `update_deployed.sh` is not needed when deploying for the first time and totally **discouraged** on a customized Archipelago. 
If you make modifications to your `Twig templates`, that command will **replace** the ones shipped by us with fresh copies overwriting all your modifications. Only run to restore larger errors or when needing to update **everything** ones with newer versions and you don't care for your own customization. Please read https://docs.archipelago.nyc/1.7.0/utility_scripts/ for more ways of managing exporting/importing Metadata Display Entities (Twig templates).

### Step 7. Set your public IIIF server URL to your actual domain

By default archipelago ships with a public facing and an internal facing IIIF Server URLs configured. These urls are used by a number of IIIF enabled viewers and need to be changed to reflect your new reality (a real Domain name and a proxied path!). These settings belong to the `strawberryfield/format_strawberryfield` module. 

First check your current settings:

```shell
docker exec -ti esmero-php bash -c "drush config-get format_strawberryfield.iiif_settings"
```

You will see the following:

```shell
pub_server_url: 'http://localhost:8183/iiif/2'
int_server_url: 'http://esmero-cantaloupe:8182/iiif/2'
```

Let's modify `pub_server_url`. Replace in the following command `your.domain.org` with the domain you defined in your `.env` file. 
NOTE: We are passing the `-y` flag to `drush` avoid that way having to answer "yes".

```shell
docker exec -ti esmero-php bash -c "drush config-set -y format_strawberryfield.iiif_settings pub_server_url https://your.domain.org/cantaloupe/iiif/2"
```

Almost Done! Now you can log into your new Archipelago using `https` and start exploring. 

### Run any Pending Search API Tasks (New to Drupal Search API 1.4.x)

This is new to us and specific to this version of Drupal's Search API.
Please, once logged in, navigate to `/admin/config/search/search-api>` and press, if present,
the "Execute pending tasks" button (in blue). This is new behavior (during a deployment from cero) for Drupal Search API.
It should take less tan a second and will inform the Search Index that there are indeed no OCRs/VTTs or ML annotations (strawberry flavors)
in the system yet (something that was never an issue before). 
We discussed this issue with the maintainers, they are aware, but can not provide a solution for now.

And Done Done! Thank you for following this guide!

## Deployment on ARM64/v8(Graviton, Apple M1) system:

This applies to AWS `m6g` and `t3g` Instances and is documented inline in this guide. Please open an [ISSUE](https://github.com/esmero/archipelago-deployment-live/issues) in this repository if you run into any problems.
Please review <https://github.com/esmero/archipelago-deployment-live/blob/1.7.0/deploy/ec2-docker/docker-compose-aws-s3-arm64.yml> for more info.

### How do I know my Architecture?

Run 

```shell
uname -m 
```

- For an `x86(64 bit)` processor system output will be `x86_64`
- For an `ARM(64 bit)` processor system output will be `aarch64`

## Caring & Coding + Fixing + Testing

* [Diego Pino](https://github.com/DiegoPino)
* [Allison Sherrick](https://github.com/alliomeria)

### Historic Core Contributors (Same Caring)

* [Giancarlo Birello](https://github.com/giancarlobi)

## Acknowledgments

This software is a [Metropolitan New York Library Council](https://metro.org) Open-Source initiative and part of the Archipelago Commons project.

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
