# How to Setup SSL for a local Archipelago Deployment (NOT Archipelago Deployment Live)

_*Note:* For a complete production deployment with SSL please follow the [Archipelago Deployment Live](https://github.com/esmero/archipelago-deployment-live) strategy. This documentation is only meant for power users that need to expose a local/development deployment via a qualified Domain Name and SSL._

The steps found below describe a potential manual SSL configuration for a local Archipelago deployment exposed to the internet using a qualified domain name and SSL. We will not move the Database, Solr or the actual Drupal Deployment folder in this documentation, only enable SSL using the existing local deployment file structure.

### Manual Configuration Steps for a Linux Server

Assuming you already have an [Archipelago deployment](https://github.com/esmero/archipelago-deployment) that is exposed via HTTP via port `8001`, this process takes less than 10 minutes of reading YML files and editing the files (described below) to get SSL running and setup with auto-renewal.

First: Make sure your Domain name is correctly resolving against your Server's Public IP address. We will use the same NGINX Docker container and setup our live instances uses, that includes an automatic request and renovation of SSL Certificates against https://certbot.eff.org.

1. Open a terminal and `cd` into your cloned Archipelago Deployment github repository base folder. We will create , inside the /persistent folder (which already exists,) an extra folder structure via the following command.
    ```Shell
      mkdir -p persistent/nginxconfig/template
    ```
2. Inside the new `template` folder create a new file named `nginx.conf.template` with the following content.

    ??? info "nginx.conf.template"
   
         ```Shell
         upstream cantaloupe {
          server esmero-cantaloupe:8182;
          keepalive 32;
         }

         server {
            listen              443 ssl;
            server_name         ${FQDN};
            ssl_certificate     /etc/letsencrypt/live/${FQDN}/fullchain.pem;
            ssl_certificate_key /etc/letsencrypt/live/${FQDN}/privkey.pem;
            client_max_body_size 512M; ## Match with PHP from FPM container

            root /var/www/html/web; ## <-- Your only path reference.

           if ($http_user_agent ~* (go\-http|zgrab|mj12bot|meta\-externalagent|oai\-searchbot|tiktok|tiktokspider|lplinkcheck|netcrawl|npbot|AliyunSecBot|bytedance|bytespider|gptbot|semrush|petalbot|amazonbot|ahrefsbot|zhanzhang|semrushbot|brightbot|imagesiftbot|barkrowler|friendlycrawler|turnitin|claudebot|python-requests|aiohttp|semanticscholarbot|go\-http\-client|facebookexternalhit)) {
                  return 444;
            }

            fastcgi_send_timeout 120s;
            fastcgi_read_timeout 120s;
            fastcgi_pass_request_headers on;
            proxy_connect_timeout 60s;
            proxy_send_timeout 120s;
            proxy_read_timeout 300s;

            fastcgi_buffers 16 16k;
            fastcgi_buffer_size 32k;
    
            # Please adapt to your needs
            proxy_buffers 16 16k;  
            proxy_buffer_size 16k;

            #gzip
            gzip on;
            gzip_disable "msie6";

            gzip_vary on;
            gzip_proxied any;
            gzip_comp_level 6;
            gzip_buffers 16 8k;
            gzip_http_version 1.1;
            gzip_min_length 256;
            gzip_types
              application/atom+xml
              application/geo+json
              application/javascript
              application/x-javascript
              application/json
              application/ld+json
              application/manifest+json
              application/rdf+xml
              application/rss+xml
              application/xhtml+xml
              application/xml
              font/eot
              font/otf
              font/ttf
              image/svg+xml
              text/css
              text/javascript
              text/plain
              text/xml;

            # Cantaloupe proxypass
            location /cantaloupe/ {
               proxy_http_version 1.1;
               proxy_set_header   "Connection" "";
               proxy_set_header X-Forwarded-Proto $scheme;
               proxy_set_header X-Forwarded-Host $host;
               proxy_set_header X-Forwarded-Port $server_port;
               proxy_set_header X-Forwarded-Path /cantaloupe/;
               proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
               proxy_read_timeout 120s; 
               if ($request_uri ~* "/cantaloupe/(.*)") {
                 proxy_pass http://cantaloupe/$1;
               }
            }

          # Tus upload to allow resumes 
          location ~* /webform_strawberry/tus_upload {
                proxy_request_buffering  off;
                proxy_buffering          off;
                proxy_http_version       1.1;
                try_files $uri $uri/ /index.php?$request_uri;
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

            # Very rarely should .txt and .log ever be accessed
            # But now we allow /do/ download endpoints to serve .txt and .log

           location ~* ^(?!/do/.+/(file|metadata)/(?!\.(txt|log)$)).*\.(txt|log)$ {
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
                fastcgi_keep_conn on;
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
            # Enforce clean URLs
            # Removes index.php from urls like www.example.com/index.php/my-page --> www.example.com/my-page
            # Could be done with 301 for permanent or other redirect codes.
            if ($request_uri ~* "^(.*/)index\.php/(.*)") {
                return 307 $1$2;
            }
         }
         ```       

2. Edit your existing `docker-compose.yml` file and replace ONLY! the `web:` container definition with the following snippet, making sure you preseve the original identation. Replace `youremail@domainname.org` with your own (real) email and `domainname.org` with your fully qualified domain name. You can also use a subdomain if that is the case.

    ??? info "docker-compose.yml"
    
        ```YAML
        web:
          container_name: esmero-web
          image: jonasal/nginx-certbot
          restart: always
          environment:
            CERTBOT_EMAIL: youremail@domainname.org
            ENVSUBST_VARS: FQDN
            FQDN: domainname.org
            NGINX_ENVSUBST_OUTPUT_DIR: /etc/nginx/user_conf.d
          ports:
              - "80:80"
              - "443:443"
          volumes:
              - ${PWD}/persistent/nginxconfig/template/nginx.conf.template:/etc/nginx/templates/nginx.conf.template:ro
              - ${PWD}/web:/var/www/html/web:cached
              - ${PWD}/persistent/letsencrypt:/etc/letsencrypt
          depends_on:
            - solr
            - php
            - db
        ```

3. Before restarting anything, make sure both ports `443` and `80` are open if you are running behind a firewall. Port `80` and the `.well-known` URI location are used by `Certbot` to validate/confirm the SSL certificate request. 

4. Run the following commands, line by line (and replace `docker-compose` with `docker compose` if running on an Apple Mac or modern Windows PC):

    ```Shell
    docker-compose down
    docker-compose pull
    docker-compose up -d
    docker ps
    ```

    You should see amongst output lines the new NGINX container with an output similar to this:
    
    ```Shell
    1d7414a12387        jonasal/nginx-certbot        "/docker-entrypoint.…"   1 minute ago       Up 1 minute       0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   esmero-web
    ```

Double check your nginx logs (```docker logs -f esmero-web -n 100```) for any errors or if you are getting an 500 error when accesing your Archipelago publicly via `https` under your new domain. 

5. Access your website using `https://yourdomain.org` and verify you can log-in. Once logged in, navigate to `/admin/config/archipelago/iiif` and under `Base URL of your IIIF Media Server public accessible from the Outside World.` change `localhost:8183/iiif/2` to `https://yourdomain.org/cantaloupe/iiif/2`. Save. 

6. SSL has now been configured for your Archipelago Local instance. If you plan on running long term a production machine, please use the [Archipelago Deployment Live](https://github.com/esmero/archipelago-deployment-live) strategy. We, as a community, do not support this strategy long term, mostly because there is no proper separation between configurations/data and Drupal itself.

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
