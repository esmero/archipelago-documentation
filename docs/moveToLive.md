# Moving from `archipelago-deployment` to `archipelago-deployment-live`

## What is this documentation for?

If you have been using/running/populating with Archipelago Digital Objects an instance that was setup using our simpler to deploy but harder to customize [archipelago-deployment](https://github.com/esmero/archipelago-deployment) strategy 
and can't wait to move to this one, meant for a larger (and somehow easier to maintain and upgrade on the long run) but (wait!) you do not want to ingest again, setup again, configure users, etc. You already did that! 
If all that applies this is your documentation.

## What is this documentation not for?

To install an archipelago-deployment-live from scratch or to keep (forever) syncing between the two deployment options in a quantum phase shifting eternum like a Timecrystal.

## Requirements

- An instance of archipelago (working, tested) installed using `archipelago-deployment` as a basis
- Basic knowledge and instincts on how to run Terminal Commands, copy files, run `composer`, `drush`, linux Permissions and `git` of course.
- Patience. You can't skip steps here. Also Patience again since you may have stretched (good) your current instance to do way more we thought it could. 
- Time to read main Documentation of this Repo to have a basic knowledge of how this is deployed. Recommended even if you are not going to deploy one from scratch here.
- For Shell Commands documented here please copy line by line. Not the whole block.

## Differences between both deployments strategies. 

In a nutshell: `archipelago-deployment-live` uses a different folder structure moving configuration storage, data storage outside of your webroot and allows a much finer control of your settings (safer) and docker containers.
In a nutshell inside the first nutshell: `archipelago-deployment-live` also ignores more files so keeping customized versions, your own packages, your own settings around and version controlled is much easier.
lastly: `archipelago-deployment-live` makes more use of Cloud Services, e.g so if you have been running `min.io` as local mounted storage you may now consider to move storage (files) to a cloud service like AWS S3

## Commonalities between both deployments strategies.

In a nutshell: Since both run same code and use the same Docker Containers the data is actually the same, all is just persisted in different places.


## Getting the new repo in place

First you need to clone this repository and (hopefully) store in the same parent folder to your current `archipelago-deployment` one. For the purpose
of this tutorial we will assume you have `archipelago-deployment` cloned in this location: `$HOME/archipelago-deployment`

Locate your archipelago-deployment folder in your terminal. Do an `ls` to make sure you can see the folder (not the content) and run

```Shell
git clone https://github.com/esmero/archipelago-deployment-live
cd archipelago-deployment-live
git checkout 1.0.0-RC3
cd ..
cd archipelago-deployment
```

Now you have side by side `$HOME/archipelago_deployment` and `$HOME/archipelago-deployment-live`

This will give you the base structure. 

Before touching anything let's start by generating a backup of your current deployment (safety first)

## Backing up

### Step 1:

Shutdown your `docker-compose` ensemble. Inside your original `archipelago-deployment` folder run this:

```Shell
docker-compose down
````

### Step 2:

Verify all containers are actually down.

```Shell
docker ps
```
The following command should return an empty listing. If anything is still running, wait a little longer and run the following comman again.

### Step 3:

Now let's tar.gz the whole ensemble with data and configs. As an example we will save this into your `$HOME` folder. 
As a good practice we append the **current date** (YEAR-DAY-MONTH) to the filename. Here we assume today is September 1st of 2021.

```Shell
sudo tar -czvpf $HOME/archipelago-deployment-backup-20210109.tar.gz ../archipelago-deployment
```

The process may take a few minutes. Now let's verify all is there and the `tar.gz` is not corrupt.

```Shell
tar -tvvf $HOME/archipelago-deployment-backup-20210109.tar.gz 
```

You will see a listing of files. If corrupt (do you have enough space? did your ssh connection drop?) you will see:

```
tar: Unrecognized archive format
```

Done! If you are running a public instance we can allow ourselves to start docker again to avoid down times.

```Shell
docker-compose up -d
````

## The directory structures
Now that you backed all up we can spend some minutes looking at both directory structures.

If you observe both deployment strategies side by side you will inmediatelly notice the most important similarities and also differences.

<table>
	<tr>
		<th>archipelago-deployment Live</th>
		<th>archipelago-deployment</th>
 	</tr>
	<tr>
  		<td valign="top"><pre>.
├── config_storage
│   ├── iiifconfig
│   ├── nginxconfig
│   ├── nginxconfig_selfcert
│   ├── php-fpm
│   └── solrconfig
├── data_storage
│   ├── db
│   ├── iiifcache
│   ├── iiiftmp
│   ├── letsencrypt
│   ├── minio-data
│   ├── ngnixcache
│   ├── selfcert
│   ├── solrcore
│   └── solrlib
├── deploy
│   ├── azure-kubernetes
│   ├── ec2-docker
│   └── kubernetes
├── docs
└── drupal
│   ├── config
│   ├── d8content
│   ├── docs
│   ├── drush
│   ├── patches
│   ├── persistent
│   ├── private
│   ├── scripts
│   ├── vendor
│   ├── web
│   └── xdebug
</pre></td>
   		<td valign="top"><pre>.
├── config
│   └── sync
├── d8content
│   └── metadatadisplays
├── docs
├── drush
│   ├── Commands
│   └── sites
├── nginxconfigford8
├── patches
├── persistent
│   ├── db
│   ├── iiifcache
│   ├── iiifconfig
│   ├── miniodata
│   ├── solrconfig
│   ├── solrcore
│   └── solrlib
├── private
│   └── webform
├── scripts
│   ├── archipelago
│   └── composer
├── vendor
│   ├── archipelago
│   ├── asm89
│   ├── aws
│   ├── behat
│   ├── bin
│   ├── brick
│   ├── chi-teck
│   ├── composer
│   ├── consolidation
│   ├── container-interop
│   ├── cweagans
│   ├── data-values
│   ├── dflydev
│   ├── doctrine
│   ├── drupal
│   ├── drush
│   ├── easyrdf
│   ├── egulias
│   ├── enlightn
│   ├── erusev
│   ├── evenement
│   ├── ezyang
│   ├── fabpot
│   ├── fileeye
│   ├── firebase
│   ├── frictionlessdata
│   ├── google
│   ├── graham-campbell
│   ├── grasmash
│   ├── guzzlehttp
│   ├── instaclick
│   ├── jcalderonzumba
│   ├── jean85
│   ├── jmikola
│   ├── justinrainbow
│   ├── laminas
│   ├── league
│   ├── lsolesen
│   ├── maennchen
│   ├── markbaker
│   ├── masterminds
│   ├── mglaman
│   ├── mhor
│   ├── mikey179
│   ├── mixnode
│   ├── ml
│   ├── monolog
│   ├── mtdowling
│   ├── myclabs
│   ├── nesbot
│   ├── nette
│   ├── nikic
│   ├── paragonie
│   ├── pear
│   ├── phar-io
│   ├── phenx
│   ├── phpdocumentor
│   ├── phplang
│   ├── phpmailer
│   ├── phpoffice
│   ├── phpoption
│   ├── phpseclib
│   ├── phpspec
│   ├── phpstan
│   ├── phpunit
│   ├── professional-wiki
│   ├── psr
│   ├── psy
│   ├── ralouphie
│   ├── ramsey
│   ├── react
│   ├── sebastian
│   ├── seld
│   ├── sirbrillig
│   ├── solarium
│   ├── squizlabs
│   ├── stack
│   ├── strawberryfield
│   ├── swaggest
│   ├── symfony
│   ├── symfony-cmf
│   ├── theseer
│   ├── twbs
│   ├── twig
│   ├── typo3
│   ├── vlucas
│   ├── web64
│   ├── webflo
│   ├── webmozart
│   ├── wikibase
│   ├── wikimedia
│   └── zaporylie
├── web
│   ├── core
│   ├── libraries
│   ├── modules
│   ├── profiles
│   ├── sites
│   └── themes</pre></td>
 	</tr>
</table>

## The Data

Let's start by focusing on the `data`, in our case Database, Solr and File (S3 + Private) storage. Collapsing here a few folder will make this easier to read. 
Marked with a `*` are matching folders that contain DB, Solr Core, the S3 min.io data (if you are using local storage) and also Drupal's very own `private` folder

<table>
	<tr>
		<th>archipelago-deployment Live</th>
		<th>archipelago-deployment</th>
 	</tr>
	<tr>
  		<td valign="top"><pre>.
├── config_storage
├── data_storage
│   ├── * db *
│   ├── iiifcache
│   ├── iiiftmp
│   ├── letsencrypt
│   ├── * minio-data *
│   ├── ngnixcache
│   ├── selfcert
│   ├── solrcore
│   └── solrlib
├── deploy
├── docs
└── drupal
│   ├── config
│   ├── d8content
│   ├── docs
│   ├── drush
│   ├── patches
│   ├── persistent
│   ├── * private *
│   ├── scripts
│   ├── vendor
│   ├── web
│   └── xdebug
</pre></td>
   		<td valign="top"><pre>.
├── config
├── d8content
├── docs
├── drush
├── nginxconfigford8
├── patches
├── persistent
│   ├── * db *
│   ├── iiifcache
│   ├── iiifconfig
│   ├── * miniodata *
│   ├── solrconfig
│   ├── * solrcore *
│   └── solrlib
├── * private *
├── scripts
├── vendor
├── web
</pre></td>
 	</tr>
</table>

### Copying the Data into the new Structure

To do so we need to stop docker again. This is needed because Databases sometimes keep an open Change Log and Locks in place and if there is any interaction, cron running, your data may end corrupted.

#### Step 1:

Shutdown your `docker-compose` ensemble. Inside your original `archipelago-deployment` folder run this:

```Shell
docker-compose down
````

#### Step 2:

Verify all containers are actually down.

```Shell
docker ps
```

#### Step 3:

We will copy DB, min.io (File and ADO storage as files) and Drupal's private (temporary files, caches) folders to its new place

```
sudo cp -rpv persistent/db ../archipelago-deployment-live/data_storage/db
sudo cp -rpv persistent/solrcore ../archipelago-deployment-live/data_storage/solrcore
sudo cp -rpv persistent/miniodata ../archipelago-deployment-live/data_storage/minio-data
sudo cp -rpv private ../archipelago-deployment-live/drupal/private
```

Running `-rpv` will copy verbosely, recursively while preserving original permissions.

Done!

You can now start docker-compose again

```Shell
docker-compose up -d
````

## The Web

Collapsing again a few folder to aid in readability we can now focus on your actual Drupal/Archipelago Code/Web and settings. To be honest (we are) you can easily reinstall and restore all this via `composer` but we can also
Move folders as a learning experience/time and bandwith experience. Marked with a `*` are matching folders you want to copy over.

<table>
	<tr>
		<th>archipelago-deployment Live</th>
		<th>archipelago-deployment</th>
 	</tr>
	<tr>
  		<td valign="top"><pre>.
├── config_storage
├── data_storage
├── deploy
├── docs
└── drupal
│   ├── * config *
│   ├── d8content
│   ├── docs
│   ├── drush
│   ├── patches
│   ├── persistent
│   ├── private
│   ├── scripts
│   ├── * vendor *
│   ├── * web *
│   └── xdebug
</pre></td>
   		<td valign="top"><pre>.
├── * config *
│   └── sync
├── d8content
│   └── metadatadisplays
├── docs
├── drush
│   ├── Commands
│   └── sites
├── nginxconfigford8
├── patches
├── persistent
├── private
├── scripts
├── * vendor *
├── * web *
</pre></td>
 	</tr>
</table>

### Copying the Web into the new Structure

No need to stop docker again. We can do this while your archipelago is still running 

#### Step 1:

We will copy all important folders over. From your `archipelago-deployment` folder run:

```
sudo cp -rpv vendor ../archipelago-deployment-live/drupal/vendor
sudo cp -rpv web ../archipelago-deployment-live/drupal/web
sudo cp -rpv config ../archipelago-deployment-live/drupal/config
```

And also a few files selectively we know you are very fond of!

```
sudo cp -rpv composer.json ../archipelago-deployment-live/drupal/composer.json
sudo cp -rpv composer.lock ../archipelago-deployment-live/drupal/composer.lock
```
Done!

## SSL, Enviromentals, Configurations, Settings and Docker

We are almost done. But `archipelago-deployment-live` has a different, safer way of defining SSL Certs, credentials and global settings for your Archipelago. 
We will start first by copying settings as they are (most likely not very safe) and then we can update passwords/etc to make your system better prepared for the world.

To learn more about these general settings please read [this section of the parent Documentation](../README.md#step-3-setup-your-enviromental-variables-for-dockerservices) (who likes duplicated documentation? nobody)
The gist here is (after reading, please do not skip) is we need to add our service definitions into an `.env` file.

Coming from `archipelago-deployment` means and assuming you are running AWS Linux 2 using the suggested locations in this document, and you have a vanilla deployment and you followed [these instructions](https://github.com/esmero/archipelago-deployment/blob/1.0.0-RC2/docs/ubuntu.md)) that your values for `$HOME/archipelago-deployment-live/deploy/ec2-docker/.env` will be the following:

```
ARCHIPELAGO_ROOT=/home/ec2-user/archipelago-deployment-live
ARCHIPELAGO_EMAIL=your@validemail.org
ARCHIPELAGO_DOMAIN=your.domain.org
MINIO_ACCESS_KEY=minio
MINIO_SECRET_KEY=minio123
MYSQL_ROOT_PASSWORD=esmero-db
MINIO_BUCKET_MEDIA=archipelago
MINIO_FOLDER_PREFIX_MEDIA=/
MINIO_BUCKET_CACHE=archipelago
MINIO_FOLDER_PREFIX_CACHE=/
```

If you plan on staying on local storage driven `min.io` 
`MINIO_BUCKET_CACHE` and `MINIO_FOLDER_PREFIX_CACHE` are not going to be used. If you are planning on moving your Storage from local to Cloud driven please replace with the right (e.g AWS IAM keys and Secrets + bucket names and prefixes (folders)).
Again, refer to the [parent Documentation](../README.md#step-3-setup-your-enviromental-variables-for-dockerservices) for setting this up

Once you have that in place (double check, if something goes wrong here we can always fine tune and fix again) we need to decide on a new docker-compose file and
you may need to customize it depending on your choices, current and future needs.

If you already have an SSL certificate and its provided by `CertBot` you can either copy the certs from your current system (will totally depend on your setup since archipelago-deployment does not provide out of the box SSL Certs) to
`$HOME/archipelago-deployment-live/data_storage/letsencrypt`

A normal folder structure for that is:

```
.
├── accounts
│   └── acme-v02.api.letsencrypt.org
│       └── directory
│           └── cac9f8218ef18e4f11ec053785bbf648
│               ├── meta.json
│               ├── private_key.json
│               └── regr.json
├── archive
│   ├── your.domain.org
│       ├── cert1.pem
│       ├── chain1.pem
│       ├── fullchain1.pem
│       └── privkey1.pem
│ 
├── csr
│   ├── 0000_csr-certbot.pem
│   ├── 0001_csr-certbot.pem
│   ├── 0002_csr-certbot.pem
│   └── 0003_csr-certbot.pem
├── keys
│   ├── 0000_key-certbot.pem
│   ├── 0001_key-certbot.pem
│   ├── 0002_key-certbot.pem
│   └── 0003_key-certbot.pem
├── live
│   ├── README
│   └── your.domain.org
│      ├── cert.pem -> ../../archive/your.domain.org/cert1.pem
│      ├── chain.pem -> ../../archive/your.domain.org/chain1.pem
│      ├── fullchain.pem -> ../../archive/your.domain.org/fullchain1.pem
│      ├── privkey.pem -> ../../archive/your.domain.org/privkey1.pem
│      └── README
├── renewal
│   └── your.domain.org.conf
│   
└── renewal-hooks
    ├── deploy
    ├── post
    └── pre
```

Or, if your SSL cert is up to renewal you can just let Archipelago request it for you. Renewal will happen auto-magically and you may never ever need to worry about that in the future.

Finally. Let's adapt the docker-compose file we need to our previous (but still current!) `archipelago-deployment` reality

Run, for x86/AMD. For ARM64/Apple M1 please check the [parent Documentation](../README.md#deployment-on-arm64v8graviton-apple-m1-system))

```
cp $home/archipelago-deployment-live/deploy/ec2-docker/docker-compose-aws-s3.yml $home/archipelago-deployment-live/deploy/ec2-docker/docker-compose.yml
nano $home/archipelago-deployment-live/deploy/ec2-docker/docker-compose.yml
```

And replace the content with this slightly modified version. Note, we really only changed lines after this comment `# THIS DIFFERS FROM THE NORMAL ONE...`

```
# Run docker-compose up -d

version: '3.5'
services:
  web:
    container_name: esmero-web
    image: staticfloat/nginx-certbot
    restart: always
    environment:
      CERTBOT_EMAIL: ${ARCHIPELAGO_EMAIL}
      ENVSUBST_VARS: FQDN
      FQDN: ${ARCHIPELAGO_DOMAIN}
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ${ARCHIPELAGO_ROOT}/config_storage/nginxconfig/conf.d:/etc/nginx/user.conf.d
      - ${ARCHIPELAGO_ROOT}/config_storage/nginxconfig/certbot_extra_domains:/etc/nginx/certbot/extra_domains:ro
      - ${ARCHIPELAGO_ROOT}/drupal:/var/www/html:cached
      - ${ARCHIPELAGO_ROOT}/data_storage/ngnixcache:/var/cache/nginx
      - ${ARCHIPELAGO_ROOT}/data_storage/letsencrypt:/etc/letsencrypt
    depends_on:
      - solr
      - php
      - db
    tty: true
    networks:
      - host-net
      - esmero-net
  php:
    container_name: esmero-php
    restart: always
    image: "esmero/php-7.4-fpm:1.0.0-RC2-multiarch"
    tty: true
    networks:
      - host-net
      - esmero-net
    volumes:
      - ${ARCHIPELAGO_ROOT}/config_storage/php-fpm/www.conf:/usr/local/etc/php-fpm.d/www.conf
      - ${ARCHIPELAGO_ROOT}/drupal:/var/www/html:cached
    environment:
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MINIO_BUCKET_MEDIA: ${MINIO_BUCKET_MEDIA}
      MINIO_FOLDER_PREFIX_MEDIA: ${MINIO_FOLDER_PREFIX_MEDIA}
  solr:
    container_name: esmero-solr
    restart: always
    image: "solr:8.8.2"
    tty: true
    ports:
      - "8983:8983"
    networks:
      - host-net
      - esmero-net
    volumes:
      - ${ARCHIPELAGO_ROOT}/data_storage/solrcore:/var/solr/data
      - ${ARCHIPELAGO_ROOT}/config_storage/solrconfig:/drupalconfig
      - ${ARCHIPELAGO_ROOT}/data_storage/solrlib:/opt/solr/contrib/archipelago/lib
    entrypoint:
      - docker-entrypoint.sh
      - solr-precreate
      - drupal
      - /drupalconfig
  db:
    image: mysql:8.0.22
    command: mysqld --default-authentication-plugin=mysql_native_password  --max_allowed_packet=256M
    container_name: esmero-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    networks:
      - host-net
      - esmero-net
    volumes:
      - ${ARCHIPELAGO_ROOT}/data_storage/db:/var/lib/mysql
  nlp:
    container_name: esmero-nlp
    restart: always
    image: "esmero/esmero-nlp:1.0"
    ports:
      - "6400:6400"
    networks:
      - host-net
      - esmero-net
  iiif:
    container_name: esmero-cantaloupe
    image: "esmero/cantaloupe-s3:4.1.9RC"
    restart: always
    ports:
      - "8183:8182"
    networks:
      - host-net
      - esmero-net
    environment:
      AWS_ACCESS_KEY_ID: ${MINIO_ACCESS_KEY}
      AWS_SECRET_ACCESS_KEY: ${MINIO_SECRET_KEY}
      # THIS DIFFERS FROM THE STANDARD ONE AND ENABLES LOCAL FILESYSTEM CACHE INSTEAD OF AWS S3 one
      CACHE_SERVER_DERIVATIVE: FilesystemCache
      S3SOURCE_BASICLOOKUPSTRATEGY_BUCKET_NAME: ${MINIO_BUCKET_MEDIA}
      S3SOURCE_BASICLOOKUPSTRATEGY_PATH_PREFIX: ${MINIO_FOLDER_PREFIX_MEDIA}
      S3CACHE_BUCKET_NAME: ${MINIO_BUCKET_CACHE} 
      S3CACHE_OBJECT_KEY_PREFIX: ${MINIO_FOLDER_PREFIX_CACHE} 
      XMS: 2g
      XMX: 4g
    volumes:
      - ${ARCHIPELAGO_ROOT}/config_storage/iiifconfig:/etc/cantaloupe
      - ${ARCHIPELAGO_ROOT}/data_storage/iiifcache:/var/cache/cantaloupe
      - ${ARCHIPELAGO_ROOT}/data_storage/iiiftmp:/var/cache/cantaloupe_tmp
  minio:
    container_name: esmero-minio
    restart: always
    image: minio/minio:latest
    volumes:
      - ${ARCHIPELAGO_ROOT}/data_storage/minio-data:/data:cached
    ports:
      - "9000:9000"
      - "9001:9001"
    networks:
      - host-net
      - esmero-net
    environment:
      MINIO_HTTP_TRACE: /tmp/minio-log.txt
      MINIO_ROOT_USER: ${MINIO_ACCESS_KEY}
      MINIO_ROOT_PASSWORD: ${MINIO_SECRET_KEY}
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY}
      # THIS DIFFERS FROM THE STANDARD ONE AND ENABLES LOCAL MINIO INSTEAD OF AWS S3 one 
    command: server /data --console-address ":9001"
networks:
  host-net:
    driver: bridge
  esmero-net:
    driver: bridge
    internal: true

```

Press CNTRL-X and you are done.
Now the final test!!

## Shutdown the old one, start the new one

So we are ready. Testing may be a hit-miss thing here. Did we cover all steps? did a command failed? Good thing is we can start the new ensemble and all our old one 
will survive and we can come back over and over until we are ready. Let's try!

We will start by shutting down the running docker-ensemble

```
cd $HOME/archipelago-deployment
docker-compose down
```

Now let's go to our new deployment. Docker starts here in a different folder


```
cd $HOME/archipelago-deployment-live/deploy/ec2-docker
docker-compose up
```

You may notice we removed the `-d`. Why? We want to see all the messages and notice/mark/copy any errors. E.g did the SSL CERT load correctly? Did the MYSQL import worked out?
To avoid shutting it down while all starts, please open another Terminal and type:

```Shell
docker ps
```
And look at the up-times. Do you see any Containers restarting? (Where Created and the Status differ for a lot and Status keeps reset to 0?) A healthy deployment
will look similar to this (

```
CONTAINER ID   IMAGE                                    COMMAND                  CREATED          STATUS         PORTS                                      NAMES
f794c25db64c   esmero/cantaloupe-s3:4.1.9RC2-arm64      "sh -c 'java -Dcanta…"   6 seconds ago    Up 3 seconds   0.0.0.0:8183->8182/tcp                     esmero-cantaloupe
5b791445720f   jonasal/nginx-certbot                    "/docker-entrypoint.…"   6 seconds ago    Up 3 seconds   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   esmero-web
e38fbbd86edf   esmero/esmero-nlp:1.0.1-RC2-arm64        "/usr/local/bin/entr…"   11 seconds ago   Up 6 seconds   0.0.0.0:6400->6400/tcp                     esmero-nlp
c84a0a4d43e9   minio/minio:latest                       "/usr/bin/docker-ent…"   11 seconds ago   Up 6 seconds   0.0.0.0:9000-9001->9000-9001/tcp           esmero-minio
3ec176a960c3   esmero/php-7.4-fpm:1.0.0-RC2-multiarch   "docker-php-entrypoi…"   11 seconds ago   Up 6 seconds   9000/tcp                                   esmero-php
e762ad7ea5e2   solr:8.8.2                               "docker-entrypoint.s…"   11 seconds ago   Up 6 seconds   0.0.0.0:8983->8983/tcp                     esmero-solr
381166d61f8c   mariadb:10.5.10-focal                    "docker-entrypoint.s…"   11 seconds ago   Up 6 seconds   3306/tcp      
```

If you feel all seems to be fine open a browser window and visit your website. See if you can log in, see ADOs. 
If not please you can momentarely shut down this new docker-ensemble and restart the older one. Nothing is lost!
Then with time and tea/coffee and fresh eyes come back and re-trace your steps. 95% of the issues are incorrect values in the `.env` file. The other 5% may be on us.
If you run into any trouble please get in touch!

Happy deploying!

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback, or open an ISSUE in this [Archipelago Deployment Live Repository](https://github.com/esmero/archipelago-deployment-live/).

Return to [Archipelago Live Deployment](../README.md).
