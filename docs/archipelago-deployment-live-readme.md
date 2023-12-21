---
title: Archipelago-deployment-live
tags:
  - Archipelago-deployment-live
---

# Archipelago Deployment Live

A Cloud / Local production ready Archipelago Deployment using Docker and soon Kubernetes.


## What is this repo for?

Running Archipelago Commons on a live public instance using SSL with Blob/Object Storage backend 

- Cloud-based deployment, e.g. AWS/Azure under Linux
- Self-managed servers running Linux
- x86/AMD86 or ARM64/v8 CPU architectures

## What is this repo not for? 

- Running your own local/development Archipelago. For that we suggest using <https://github.com/esmero/archipelago-deployment>

## Requirements

### Minimal (not recommended for production)

- 4 Gbytes of RAM (e.g AWS EC2 t3.medium) 2 CPUs, Single SSD Drive of 100 Gbytes

### Base line for production

- 8 Gbytes of RAM (AWS EC2 t3.medium)  2 CPUs, Single SSD Drive of 100 Gbytes, optional: one magnetic Drive of 500 Gbytes for Caches/Temp files/Backups.

### Recommended for production

- 16 Gbytes of RAM (AWS EC2 m6g.xlarge - Graviton) 4 CPUs, Single SSD Drive of 200 Gbytes, optional: one magnetic Drive of 1TB for Caches/Temp files/Backups.

### OS:

- Ubuntu 22.04 /Amazon Linux 2 or Amazon Linux 2023/Debian 10 "Buster"/ AlmaLinux (Centos replacement) matching your CPU architecture (of course)
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

e.g. for Amazon Linux 2 (x86/amd64) these steps are tested:

```shell
sudo yum update -y
sudo amazon-linux-extras install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
sudo chkconfig docker on
sudo systemctl enable docker
sudo yum install -y git htop tree
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
git checkout 1.3.0
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
`Note`: There are a few extra commented lines at the end only used for: https://docs.archipelago.nyc/1.3.0/security_bots/ if you decide to go that way.

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
```

What does each key mean?

- `ARCHIPELAGO_ROOT`: the **absolute path** to your `archipelago-deployment-live` git repo in your host machine.
- `ARCHIPELAGO_EMAIL`: a valid **email**, will be used to register your SSL Certificate via Certbot.
- `ARCHIPELAGO_DOMAIN`: a valid **domain name** for your repository. This domain will be also used to request your SSL Certificate via Certbot.
- `MINIO_ACCESS_KEY`: If you are running a Cloud Service backed S3/Azure Storage this needs to be generated there. The user/IAM owner of this ACCESS KEY needs to have access to read/write the bucket you will configure in this same `.env`. If running local `min.io` whatever you set will be used.
- `MINIO_SECRET_KEY`: If you are running a Cloud Service backed S3/Azure Storage this needs to generated there. The user/IAM owner of the matching SECRET_KEY needs to have access to read/write the bucket you will configure in this same `.env` file. If running local `min.io` whatever you set will be used.
- `MYSQL_ROOT_PASSWORD`: The MYSQL 8 or Mariadb 15 password. This password will be used later also during Drupal deployment via `drush`
- `MINIO_BUCKET_MEDIA`: The name of your Persistant Storage Bucket. If using mini.io local we recommend keeping it simple, e.g. `archipelago`.
- `MINIO_FOLDER_PREFIX_MEDIA`: The `folder` (a prefix really) where your DO Storage and File storage will go inside the `MINIO_BUCKET_MEDIA` Bucket. `media/` is a _fine_ name for this one and common in archipelago deployments. **IMPORTANT:** Always terminate these with a `/`. 
- `MINIO_BUCKET_CACHE`: The name of your IIIF Cache storage Bucket. May be the same as `MINIO_BUCKET_MEDIA`. If different make sure your your `MINIO_ACCESS_KEY` and/or IAM role ACL have permission to read write to this one too.
- `MINIO_FOLDER_PREFIX_CACHE`:  The `folder` (a prefix really) where Cantaloupe will/can write its `iiif` caches. `iiifcache/` is a _lovely_ name we use a lot. **IMPORTANT:** Always terminate these with a `/`.
- `REDIS_PASSWORD`: Password for your REDIS (Drupal Cache/Queue storage) if you decide to enable the Drupal REDIS module.

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

##### Optional (expert) extra domains (does not apply to ARM64/Apple M1 Architecture):

If you have more than a single domain you may create a text file inside 
`config_storage/nginxconfig/certbot_extra_domains/your.domain.org` and write for each subdomain there an entry/line.

#### OR Running self-signed? (optional and does not apply to ARM64/Apple M1 Architecture): 

Only if you are not running a fully qualified domain you wish a valid/signed. We **really DO not** recommend this route. IF you plan on using this deployment for local testing or running on non SSL please go for <https://github.com/esmero/archipelago-deployment> which delivers the same experience in less than 20 minutes deployment time.

Generate a self signed Cert

```shell
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout data_storage/selfcert/private/nginx.key -out data_storage/selfcert/certs/nginx.crt 
sudo openssl dhparam -out data_storage/selfcert/dhparam.pem 4096
cp deploy/ec2-docker/docker-compose-selfsigned.yml deploy/ec2-docker/docker-compose.yml
```

Note: Self signed docker-compose.yml file is setup to use min.io with local storage

```yaml
    volumes:
      - ${ARCHIPELAGO_ROOT}/data_storage/minio-data:/data:cached
```

This folder will be created by min.io. If you are using a secondary Drive (e.g. magnetic) you can modify your `deploy/ec2-docker/docker-compose.yml` to use a folder there, e.g.

```yaml
    volumes:
      - /persistentinotherdrive/data_storage/minio-data:/data:cached
```

Make sure your logged in user can read/write to it.

NOTE: If you want to use AWS S3 storage for the self signed version replace the minio Service yaml block with this [Service Block](https://github.com/esmero/archipelago-deployment-live/blob/e90cf7701f1ae8e0a580a0901aaadb669baa21fd/deploy/ec2-docker/docker-compose-aws-s3.yml#L108-L125) in your new `deploy/ec2-docker/docker-compose.yml`. You can mix and match services and even remove all `:cached` statements for _improved_ R/W volumen performance.

### Step 4. First Run

#### First Permissions

```shell
sudo chown 8183:8183 config_storage/iiifconfig/cantaloupe.properties
sudo chown -R 8183:8183 data_storage/iiifcache
sudo chown -R 8183:8183 data_storage/iiiftmp
sudo chown -R 8983:8983 data_storage/solrcore
```

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

#### Composer and Drupal

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

Once done, execute our setup script that will prepare your Drupal `settings.php` and bring some of the `.env` enviromental variables to the Drupal environment. 

```shell
docker exec -ti esmero-php bash -c 'scripts/archipelago/setup.sh'
```

And now you can deploy Drupal! 

**IMPORTANT:** Make sure you replace in the following command inside `root:MYSQL_ROOT_PASSWORD` the `MYSQL_ROOT_PASSWORD` string with the **value** you used/assigned in your `.env` file for `MYSQL_ROOT_PASSWORD`. And replace `ADMIN_PASSWORD` with a password that is safe and you won't forget! That passwords is for your Drupal super user (uid:0).

```shell
docker exec -ti -u www-data esmero-php bash -c "cd web;../vendor/bin/drush -y si --verbose --existing-config --db-url=mysql://root:MYSQL_ROOT_PASSWORD@esmero-db/drupal --account-name=admin --account-pass=ADMIN_PASSWORD -r=/var/www/html/web --sites-subdir=default --notify=false;drush cr;chown -R www-data:www-data sites;"
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
If you make modifications to your `Twig templates`, that command will **replace** the ones shipped by us with fresh copies overwriting all your modifications. Only run to restore larger errors or when needing to update **everything** ones with newer versions and you don't care for your own customization. Please read https://docs.archipelago.nyc/1.3.0/utility_scripts/ for more ways of managing exporting/importing Metadata Display Entities (Twig templates).

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
docker exec -ti esmero-php bash -c "drush -y config-set format_strawberryfield.iiif_settings pub_server_url https://your.domain.org/cantaloupe/iiif/2"
```

Finally Done! Now you can log into your new Archipelago using `https` and start exploring. Thank you for following this guide!

## Deployment on ARM64/v8(Graviton, Apple M1) system:

This applies to AWS `m6g` and `t3g` Instances and is documented inline in this guide. Please open an [ISSUE](https://github.com/esmero/archipelago-deployment-live/issues) in this repository if you run into any problems.
Please review <https://github.com/esmero/archipelago-deployment-live/blob/1.3.0/deploy/ec2-docker/docker-compose-aws-s3-arm64.yml> for more info.

### How do I know my Architecture?

Run 

```shell
uname -m 
```

- For an `x86(64 bit)` processor system output will be `x86_64`
- For an `ARM(64 bit)` processor system output will be `aarch64`

## Caring & Coding + Fixing + Testing

* [Diego Pino](https://github.com/DiegoPino)
* [Allison Lund](https://github.com/alliomeria)
* [Giancarlo Birello](https://github.com/giancarlobi)

## Acknowledgments

This software is a [Metropolitan New York Library Council](https://metro.org) Open-Source initiative and part of the Archipelago Commons project.

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
