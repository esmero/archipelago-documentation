---
title: Managing Bots
tags:
  - Security
  - Bots
---

# Managing Bots

A public-facing production instance will likely encounter bad bots and other malicious traffic that will consume resources. There are many solutions available that address a variety of different needs, but we provide basic configurations and a Docker image for integrating the [NGINX Ultimate Bad Bot & Referrer Blocker](https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker).

!!! warning

    Before proceeding, please be sure to familiarize yourself with the [NGINX Ultimate Bad Bot & Referrer Blocker README](https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker).

## Deployment

1. Uncomment or add the following docker-compose environment variables, replacing any appropriate values with your own and leaving the blocker and cron disabled to start (see highlighted lines):
   ```dotenv title=".env" hl_lines="7 10"
   MSMTP_ACCOUNT=SMTP_ACCOUNT_NAME
   MSMTP_EMAIL=repositorysupport@metro.org
   MSMTP_HOST=smtp.metro.org
   MSMTP_PASSWORD=YOUR_SMTP_PASSWORD
   MSMTP_PORT=SMTP_PORT
   MSMTP_STARTTLS=on
   NGXBLOCKER_ENABLE=false
   NGXBLOCKER_CRON=00 22 * * *
   NGXBLOCKER_CRON_COMMAND=/usr/local/sbin/update-ngxblocker -x
   NGXBLOCKER_CRON_START=false
   ```
2. Uncomment or add the following lines and comment out the line for the original NGINX image:
   ```yaml title="docker-compose.yml" hl_lines="7 8 15-24 33"
   # Run docker-compose up -d
   # Docker file for Arm64 and Apple M1 machines
   version: '3.5'
   services:
     web:
       container_name: esmero-web
       # image: jonasal/nginx-certbot
       image: esmero/nginx-bot-blocker:1.1.0-multiarch
       restart: always
       environment:
         CERTBOT_EMAIL: ${ARCHIPELAGO_EMAIL}
         ENVSUBST_VARS: FQDN
         FQDN: ${ARCHIPELAGO_DOMAIN}
         NGINX_ENVSUBST_OUTPUT_DIR: /etc/nginx/user_conf.d
         MSMTP_ACCOUNT: ${MSMTP_ACCOUNT}
         MSMTP_EMAIL: ${MSMTP_EMAIL}
         MSMTP_HOST: ${MSMTP_HOST}
         MSMTP_PASSWORD: ${MSMTP_PASSWORD}
         MSMTP_PORT: ${MSMTP_PORT}
         MSMTP_STARTTLS: ${MSMTP_STARTTLS}
         NGXBLOCKER_CRON: ${NGXBLOCKER_CRON}
         NGXBLOCKER_CRON_COMMAND: ${NGXBLOCKER_CRON_COMMAND}
         NGXBLOCKER_CRON_START: ${NGXBLOCKER_CRON_START}
         NGXBLOCKER_ENABLE: ${NGXBLOCKER_ENABLE}
       ports:
         - "80:80"
         - "443:443"
       volumes:
         - ${ARCHIPELAGO_ROOT}/config_storage/nginxconfig/template:/etc/nginx/templates
         - ${ARCHIPELAGO_ROOT}/drupal:/var/www/html:cached
         - ${ARCHIPELAGO_ROOT}/data_storage/ngnixcache:/var/cache/nginx
         - ${ARCHIPELAGO_ROOT}/data_storage/letsencrypt:/etc/letsencrypt
         - ${ARCHIPELAGO_ROOT}/config_storage/nginxconfig/bots.d:/etc/nginx/bots.d
   ```
3. First pull the new image:
   ```bash
   docker compose pull
   ```

    !!! note
    
        If using an older version of docker, don't forget the hyphen:
        ```bash
        docker-compose pull
        ```

4. Now bring the Docker ensemble down and up again:
   ```bash
   docker compose down && docker compose up -d
   ```

    !!! note
    
        If using an older version of docker, don't forget the hyphen:
        ```bash
        docker-compose down && docker-compose up -d
        ```

4. Run the install script for the bot blocker in the default dry run mode and review the output:
   ```bash
   docker exec -ti esmero-web bash -c "/usr/local/sbin/install-ngxblocker"
   ```
5. The script will output the changes that are going to be made. Review them carefully and ensure that they are ok to make. Then run the command with the execute flag:
   ```bash
   docker exec -ti esmero-web bash -c "/usr/local/sbin/install-ngxblocker -x"
   ```
6. Run the setup script for the bot blocker in the default dry run mode and review the output:
   ```bash
   docker exec -ti esmero-web bash -c "/usr/local/sbin/setup-ngxblocker -v /etc/nginx/templates -e .copy"
   ```
7. The script will output the NGINX configuration changes that are going to be made. Review them carefully and ensure that they are ok to make. Then run the command with the execute flag:
   ```bash
   docker exec -ti esmero-web bash -c "/usr/local/sbin/setup-ngxblocker -v /etc/nginx/templates -e .copy -x"
   ```
8. Enable the bot blocker and cron (if applicable):
   ```dotenv title=".env" hl_lines="7 10"
   MSMTP_ACCOUNT=SMTP_ACCOUNT_NAME
   MSMTP_EMAIL=repositorysupport@metro.org
   MSMTP_HOST=smtp.metro.org
   MSMTP_PASSWORD=YOUR_SMTP_PASSWORD
   MSMTP_PORT=SMTP_PORT
   MSMTP_STARTTLS=on
   NGXBLOCKER_ENABLE=true
   NGXBLOCKER_CRON=00 22 * * *
   NGXBLOCKER_CRON_COMMAND=/usr/local/sbin/update-ngxblocker -x
   NGXBLOCKER_CRON_START=true
   ```

    !!! note
    
        If `MSMTP_EMAIL` is blank and cron is enabled the flag for sending email notifications will be skipped.

9. Bring the Docker ensemble down and back up again:
   ```bash
   docker compose down && docker compose up -d
   ```

    !!! note
    
        If using an older version of docker, don't forget the hyphen:
        ```bash
        docker-compose down && docker-compose up -d
        ```

10. Test that it is working by following the "TESTING" section (STEP 10) in the official documentation: <https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker>

## Deployment with Upgrade

If looking to use this solution as part of an upgrade (from 1.0.0 to 1.1.0, for example) we recommend coming back to the above steps after successfully completing the upgrade. After the upgrade, you will only need to add the environment variables and docker compose configurations and follow the steps as detailed above.

## Advanced Configuration

Because our Docker containers only persist our mounted files and folders, any advanced configurations may require overriding the files generated by our esmero-web container on boot. For example, the above `setup-ngxblocker` script is normally responsible for writing the following include lines:

```nginx
include /etc/nginx/bots.d/blockbots.conf;
include /etc/nginx/bots.d/ddos.conf;
```

Because the script is unable to place them in the correct part of our `nginx.conf.template` file, which in turn generates our `nginx.conf` file (see [Using environment variables in nginx configuration](https://github.com/docker-library/docs/tree/master/nginx#using-environment-variables-in-nginx-configuration-new-in-119)), our own script adds (when `NGINXBLOCKER_ENABLE=true`) or removes (when `NGINXBLOCKER_ENABLE=false`) the lines to an empty file, which in turn is statically included in our main `nginx.conf.template` file. One option provided by `setup-ngxblocker` is to exclude (`-d`) the DDOS rule. In our case, we need to manually override the lines in our template file to reproduce this behavior:

!!! example

    ```nginx title="nginx.conf.template" hl_lines="25 26"
    upstream cantaloupe {
      server esmero-cantaloupe:8182;
    }
    
    server {
        listen              443 ssl;
        server_name         ${FQDN};
        ssl_certificate     /etc/letsencrypt/live/${FQDN}/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/${FQDN}/privkey.pem;
        client_max_body_size 1536M; ## Match with PHP from FPM container
    
        root /var/www/html/web; ## <-- Your only path reference.
    
        fastcgi_send_timeout 120s;
        fastcgi_read_timeout 120s;
        fastcgi_pass_request_headers on;
    
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        
        # Please adapt to your needs
        proxy_buffers 16 16k;  
        proxy_buffer_size 16k;
    
        #include /etc/nginx/conf.d/bots.include;
        include /etc/nginx/bots.d/blockbots.conf;
    ```

    !!! note

        Keep in mind that from this point, when disabling/enabling the bot blocker via the environment variable, you'll also need to uncomment/comment the added line. 

Another more generally applicable approach is to override files that are part of the docker image:

!!! example

    Our bash script ([setup_bot_blocker.sh](https://github.com/esmero/archipelago-docker-images/tree/1.1.0/esmero-web/setup_bot_blocker.sh)) is triggered by and runs just before the startup script ([start_nginx_certbot.sh](https://github.com/JonasAlfredsson/docker-nginx-certbot/blob/master/src/scripts/start_nginx_certbot.sh)) for the NGINX Certbot image. For any advanced needs involving custom startup behavior, our script can be modified and overwridden:

    1. First, we'll copy the script from the running Docker container onto our host:
       ```bash
       docker cp esmero-web:/scripts/setup_bot_blocker.sh drupal/scripts/archipelago/
       ```
    2. Then we mount the file to override the container's file:
       ```yaml title="docker-compose.yml" hl_lines="34"
       # Run docker-compose up -d
       # Docker file for Arm64 and Apple M1 machines
       version: '3.5'
       services:
         web:
           container_name: esmero-web
           # image: jonasal/nginx-certbot
           image: esmero/nginx-bot-blocker:1.1.0-multiarch
           restart: always
           environment:
             CERTBOT_EMAIL: ${ARCHIPELAGO_EMAIL}
             ENVSUBST_VARS: FQDN
             FQDN: ${ARCHIPELAGO_DOMAIN}
             NGINX_ENVSUBST_OUTPUT_DIR: /etc/nginx/user_conf.d
             MSMTP_ACCOUNT: ${MSMTP_ACCOUNT}
             MSMTP_EMAIL: ${MSMTP_EMAIL}
             MSMTP_HOST: ${MSMTP_HOST}
             MSMTP_PASSWORD: ${MSMTP_PASSWORD}
             MSMTP_PORT: ${MSMTP_PORT}
             MSMTP_STARTTLS: ${MSMTP_STARTTLS}
             NGXBLOCKER_CRON: ${NGXBLOCKER_CRON}
             NGXBLOCKER_CRON_COMMAND: ${NGXBLOCKER_CRON_COMMAND}
             NGXBLOCKER_CRON_START: ${NGXBLOCKER_CRON_START}
             NGXBLOCKER_ENABLE: ${NGXBLOCKER_ENABLE}
           ports:
             - "80:80"
             - "443:443"
           volumes:
             - ${ARCHIPELAGO_ROOT}/config_storage/nginxconfig/template:/etc/nginx/templates
             - ${ARCHIPELAGO_ROOT}/drupal:/var/www/html:cached
             - ${ARCHIPELAGO_ROOT}/data_storage/ngnixcache:/var/cache/nginx
             - ${ARCHIPELAGO_ROOT}/data_storage/letsencrypt:/etc/letsencrypt
             - ${ARCHIPELAGO_ROOT}/config_storage/nginxconfig/bots.d:/etc/nginx/bots.d
             - ${ARCHIPELAGO_ROOT}/drupal/scripts/archipelago/setup_bot_blocker.sh:/scripts/setup_bot_blocker.sh
       ```
    3. Then we modify our script copy (we'll reproduce the same behavior from the previous example but incorporate it into our script):
    ```bash title="drupal/scripts/archipelago/setup_bot_blocker.sh" hl_lines="36-38 42-44"
    #!/bin/bash
    
    set -e
    
    if [ ! -z "${MSMTP_EMAIL}" ]; then
        envsubst < /root/.msmtprc.template > /root/.msmtprc
    fi
    
    if [ "${NGXBLOCKER_CRON_START}" = true ]; then
        if [ ! -z "${MSMTP_EMAIL}" ]; then
          CRON_COMMAND="${NGXBLOCKER_CRON} ${NGXBLOCKER_CRON_COMMAND} -e ${MSMTP_EMAIL}"
        else
          CRON_COMMAND="${NGXBLOCKER_CRON} ${NGXBLOCKER_CRON_COMMAND} -n"
        fi
        echo "${CRON_COMMAND}" | crontab - &&
        /etc/init.d/cron start
    fi
    
    if [ ! -f /etc/nginx/templates/bots.include.copy ]; then
        touch /etc/nginx/templates/bots.include.copy
    fi
    if [ ! -f /etc/nginx/templates/bots.include.template ]; then
        touch /etc/nginx/templates/bots.include.template
    fi
    
    if [ "${NGXBLOCKER_ENABLE}" = true ]; then
        if [ ! -L /etc/nginx/conf.d/botblocker-nginx-settings.conf ]; then
            ln -s /etc/nginx/bots_settings_conf.d/botblocker-nginx-settings.conf /etc/nginx/conf.d/botblocker-nginx-settings.conf
        fi
        if [ ! -L /etc/nginx/conf.d/globalblacklist.conf ]; then
            ln -s /etc/nginx/bots_settings_conf.d/globalblacklist.conf /etc/nginx/conf.d/globalblacklist.conf
        fi
        if ! grep -q blockbots.conf /etc/nginx/templates/bots.include.copy; then
            echo "include /etc/nginx/bots.d/blockbots.conf;" >> /etc/nginx/templates/bots.include.copy
        fi
        #if ! grep -q ddos.conf /etc/nginx/templates/bots.include.copy; then
        #    echo "include /etc/nginx/bots.d/ddos.conf;" >> /etc/nginx/templates/bots.include.copy
        #fi
        if ! grep -q blockbots.conf /etc/nginx/user_conf.d/bots.include; then
            echo "include /etc/nginx/bots.d/blockbots.conf;" >> /etc/nginx/user_conf.d/bots.include
        fi
        #if ! grep -q ddos.conf /etc/nginx/user_conf.d/bots.include; then
        #    echo "include /etc/nginx/bots.d/ddos.conf;" >> /etc/nginx/user_conf.d/bots.include
        #fi
        cp /etc/nginx/templates/bots.include.copy /etc/nginx/templates/bots.include.template
    else
        >|/etc/nginx/templates/bots.include.template
        >|/etc/nginx/user_conf.d/bots.include
        if [ -L /etc/nginx/conf.d/botblocker-nginx-settings.conf ]; then
            rm /etc/nginx/conf.d/botblocker-nginx-settings.conf
        fi
        if [ -L /etc/nginx/conf.d/globalblacklist.conf ]; then
            rm /etc/nginx/conf.d/globalblacklist.conf
        fi
    fi
    ```
    4. Next we remove the `include` line from the existing files:
       ```bash
       docker exec -ti esmero-web bash -c "sed -i '/include \/etc\/nginx\/bots.d\/ddos.conf/d' /etc/nginx/templates/bots.include.copy"
       ```
       ```bash
       docker exec -ti esmero-web bash -c "sed -i '/include \/etc\/nginx\/bots.d\/ddos.conf/d' /etc/nginx/templates/bots.include.template"
       ```
       ```bash
       docker exec -ti esmero-web bash -c "sed -i '/include \/etc\/nginx\/bots.d\/ddos.conf/d' /etc/nginx/user_conf.d/bots.include"
       ```
    5. Finally we bring the Docker ensemble down and back up again to propagate the changes in our container:
       ```bash
       docker compose down && docker compose up -d
       ```

        !!! note
        
            If using an older version of docker, don't forget the hyphen:
            ```bash
            docker-compose down && docker-compose up -d
            ```

    The above is an example of a more complicated customization, but it's a pattern that can be used more generally throughout the Docker containers, i.e.:

    1. Copy the file that needs to be overwridden from the Docker container to the host and make custom changes.
    2. Mount the file from the host to the location within the docker container, e.g.:
       ```yaml
       - ${ARCHIPELAGO_ROOT}/LOCATION_ON_HOST/CUSTOMIZED_FILE_ON_HOST:/LOCATION_IN_DOCKER_CONTAINER/FILE_IN_DOCKER_CONTAINER
       ```
    3. Bring the Docker ensemble down and bring it up again.
