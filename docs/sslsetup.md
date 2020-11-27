## How to Setup SSL for Docker/Archipelago
_*Work-In-Progress Note* This documentation page is still under construction and content may change with future updates. Please use caution when implementing any instructions referenced herein, as there may be missing steps or corresponding configuration files. Thank you for your patience as we continue to update Archipelago's documentation._

The steps found below describe one potential manual SSL configuration for Archipelago deployments. A `git clone` deployment option will be available for future releases.

### Manual Configuration Steps for an EC2 AWS Server

This process takes less than 10 minutes of reading YML files and editing a few files (described below) to get SSL running and setup with auto-renewal.

1. First, configure Certbot, following the instructions found on https://certbot.eff.org.

1. Inside a /persistent partition, establish the following folder structure.
_Note: you can keep the existing folder structure if you so choose. A benefit of the following structure is that it decouples the git clone of archipelago-deployment, which is made to be self sustainable and good for coding or smaller deployments._

```Shell
[ec2-user@ip-17x-xx-x-xxx persistent]$ ls -lah
total 64K
drwxr-xr-x 14 root           root  4.0K Oct  5 23:11 .
dr-xr-xr-x 19 root           root  275 Dec 15  2019 ..
drwxr-xr-x  8  		999  999   4096 Oct 13 20:07 db
drwxr-xr-x 13 root           root  4.0K Oct  5 23:03 drupal8
drwxr-xr-x  5           100  100   4.0K Feb 23  2020 iiifcache
drwxr-xr-x  2 root           root  4.0K Feb 23  2020 iiifconfig
drwxr-xr-x  4 root           root  4.0K Oct  5 22:45 nginx_conf
drwxr-xr-x  3 root           root  4.0K Feb 26  2019 solrconfig
drwxr-xr-x  3           8983 8983  4.0K Feb 26  2019 solrcore
```
To get to this point, create a git clone of archipelago deployment and then copy the content of the /persistent out of the repo folder into this structure. The original (or what is left) archipelago-deployment ends inside a drupal8 folder here.

2. Copy and paste the following to create a local copy of this file:

 <details><summary>docker-compose.yml</summary>
 **Be sure to replace youremail@gmail.com with your email address.
 <span>

 ```YAML
 version: '3.5'
 services:
   web:
     container_name: esmero-web
     image: staticfloat/nginx-certbot
     restart: always
     environment:
       CERTBOT_EMAIL: "youremail@gmail.com"
     ports:
       - "80:80"
       - "443:443"
     volumes:
       - /persistent/nginx_conf/conf.d:/etc/nginx/user.conf.d:ro
       - /persistent/nginx_conf/certbot_extra_domains:/etc/nginx/certbot/extra_domains:ro
       - /persistent/drupal8:/var/www/html:cached
     depends_on:
       - solr
       - php
     tty: true
     networks:
       - host-net
       - esmero-net
   php:
     container_name: esmero-php
     restart: always
     image: "esmero/php-7.3-fpm:latest"
     tty: true
     networks:
       - host-net
       - esmero-net
     volumes:
       - ${PWD}:/var/www/html:cached
   solr:
     container_name: esmero-solr
     restart: always
     image: "solr:7.5.0"
     tty: true
     ports:
       - "8983:8983"
     networks:
       - host-net
       - esmero-net
     volumes:
       - /persistent/solrcore:/opt/solr/server/solr/mycores:cached
       - /persistent/solrconfig:/drupalconfig:cached
     entrypoint:
       - docker-entrypoint.sh
       - solr-precreate
       - drupal
       - /drupalconfig
   # see https://hub.docker.com/_/mysql/
   db:
     image: mysql:5.7
     command: --max_allowed_packet=256M
     container_name: esmero-db
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: esmerodb
     networks:
       - host-net
       - esmero-net
     volumes:
       - /persistent/db:/var/lib/mysql:cached
   iiif:
     container_name: esmero-cantaloupe
     image: "esmero/cantaloupe-s3:4.1.6"
     restart: always
     ports:
       - "8183:8182"
     networks:
       - host-net
       - esmero-net
     volumes:
       - /persistent/iiifconfig:/etc/cantaloupe
       - /persistent/iiifcache:/var/cache/cantaloupe
 networks:
   host-net:
     driver: bridge
   esmero-net:
     driver: bridge
     internal: true
````

</span>
</details>

  _Note: This file shows how the folders in Step 1 are being used, and how SSL is being automatically deployed and renewed (without any human interaction other than starting the docker-compose and watching the logs)._

3. Now copy and paste the following to create a local copy of this file:

 <details><summary>ngnix.conf</summary>
  **Be sure to replace all instances of yoursite.org with your own domain.
 <span>

 ```Shell
 # goes into /persistent/nginx_conf/conf.d/nginx.conf
 upstream cantaloupe {
  server  esmero-cantaloupe:8182;
  }

  server {
    listen              443 ssl;
    server_name         yoursite.org;
    ssl_certificate     /etc/letsencrypt/live/yourstie.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yoursite.org/privkey.pem;

     client_max_body_size 512M; ## Match with PHP from FPM container

    root /var/www/html/web; ## <-- Your only path reference.

    fastcgi_send_timeout 120s;
    fastcgi_read_timeout 120s;
    fastcgi_pass_request_headers on;

    fastcgi_buffers 16 16k;
    fastcgi_buffer_size 32k;

    # Cantaloupe proxypass
    location /cantaloupe/ {
       proxy_set_header X-Forwarded-Proto $scheme;
       proxy_set_header X-Forwarded-Host $host;
       proxy_set_header X-Forwarded-Port $server_port;
       proxy_set_header X-Forwarded-Path /cantaloupe/;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       if ($request_uri ~* "/cantaloupe/(.*)") {
         proxy_pass http://cantaloupe/$1;
       }
    }

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    # Very rarely should these ever be accessed outside of your lan
    location ~* \.(txt|log)$ {
        deny all;
    }

    location ~ \..*/.*\.php$ {
        return 403;
    }

    location ~ ^/sites/.*/private/ {
        return 403;
    }

    # Allow "Well-Known URIs" as per RFC 5785
    location ~* ^/.well-known/ {
        allow all;
    }

    # Block access to "hidden" files and directories whose names begin with a
    # period. This includes directories used by version control systems such
    # as Subversion or Git to store control files.
    location ~ (^|/)\. {
        return 403;
    }

    location / {
        try_files $uri /index.php?$query_string; # For Drupal >= 7
    }

    location @rewrite {
        rewrite ^/(.*)$ /index.php?q=$1;
    }

    # Don't allow direct access to PHP files in the vendor directory.
    location ~ /vendor/.*\.php$ {
        deny all;
        return 404;
    }

    # Allow Modules to be updated via UI (still we believe composer is the way)    
    rewrite ^/core/authorize.php/core/authorize.php(.*)$ /core/authorize.php$1;

    # In Drupal 8, we must also match new paths where the '.php' appears in
    # the middle, such as update.php/selection. The rule we use is strict,
    # and only allows this pattern with the update.php front controller.
    # This allows legacy path aliases in the form of
    # blog/index.php/legacy-path to continue to route to Drupal nodes. If
    # you do not have any paths like that, then you might prefer to use a
    # laxer rule, such as:
    #   location ~ \.php(/|$) {
    # The laxer rule will continue to work if Drupal uses this new URL
    # pattern with front controllers other than update.php in a future
    # release.
    location ~ '\.php$|^/update.php' {
        fastcgi_split_path_info ^(.+?\.php)(|/.*)$;
        include fastcgi_params;
        # Block httpoxy attacks. See https://httpoxy.org/.
        fastcgi_param HTTP_PROXY "";
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param PHP_VALUE "upload_max_filesize=512M \n post_max_size=512M";
        proxy_read_timeout 900s;
        fastcgi_intercept_errors on;
        fastcgi_pass esmero-php:9000;
    }

     # Fighting with Styles? This little gem is amazing.
    location ~ ^/sites/.*/files/styles/ { # For Drupal >= 7
        try_files $uri @rewrite;
    }

    # Handle private files through Drupal.
    location ~ ^/system/files/ { # For Drupal >= 7
        try_files $uri /index.php?$query_string;
    }
}
```

</span>
</details>

4. Create the following folder:

```Shell
/persistent/nginx_conf/conf.d/
```

5. Place the ngnix.conf file inside the `/conf.d/` folder.

6. Create also this other folder:

```Shell
/persistent/nginx_conf/certbot_extra_domains/
```

7. Inside the `/certbot_extra_domains/` folder, create a text file named the same way as your domain (which can/or not contain additional subdomains but needs to exist).

```Shell
cat  /persistent/nginx_conf/certbot_extra_domains/yoursite.org
```

```Shell
drwxr-xr-x 2 root root 4.0K Oct  5 22:46 .
drwxr-xr-x 4 root root 4.0K Oct  5 22:45 ..
-rw-r--r-- 1 root root   48 Oct  5 22:46 yoursite.org
```

_Optionally, create additional subdomains if needed._

```Shell
cat  /persistent/nginx_conf/certbot_extra_domains/yoursite.org
subdomain.yoursite.org
anothersub.yoursite.org
```

8. Make sure you have edited the `docker-compose.yml` and `ngnix.conf` files you created to match your own information. Also make sure to also adjust the paths if you do not want the /persistent approach described in Step 1.

9. Run the following commands:

```Shell
docker -compose up -d
docker ps
```
You should see this:

```Shell
b5a04747ee06        staticfloat/nginx-certbot    "/bin/bash /scripts/…"   8 days ago          Up 8 days           0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   esmero-web
84afae094b57        esmero/php-7.3-fpm:latest    "docker-php-entrypoi…"   8 days ago          Up 8 days           9000/tcp                                   esmero-php
13a9214acfd0        esmero/cantaloupe-s3:4.1.6   "sh -c 'java -Dcanta…"   8 days ago          Up 8 days           0.0.0.0:8183->8182/tcp                     esmero-cantaloupe
044dd5bc7245        mysql:5.7                    "docker-entrypoint.s…"   8 days ago          Up 8 days           3306/tcp, 33060/tcp                        esmero-db
31f4f0f45acc        solr:7.5.0                   "docker-entrypoint.s…"   8 days ago          Up 8 days           0.0.0.0:8983->8983/tcp                     esmero-solr
```

10.SSL has now been configured for your Archipelago instance.

### User contributed documentation:
_Adding SSL to Archipelago running docker_ by [Zachary Spalding](https://github.com/senyzspalding): https://youtu.be/rfH5TLzIRIQ

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
