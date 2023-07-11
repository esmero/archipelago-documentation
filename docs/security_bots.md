---
title: Managing Bots
tags:
  - Security
  - Bots
---

# Managing Bots

A public-facing production instance will likely encounter bad bots and other malicious traffic that will consume resources. There are many solutions available that address a variety of different needs, but we provide basic configurations and a Docker image for integrating the [NGINX Ultimate Bad Bot & Referrer Blocker](https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker).

## Deployment

1. Uncomment or add the following docker-compose environment variables, replacing any appropriate values with your own (leave the blocker and cron disabled to start):
   ```dotenv title=".env"
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
   ```shell
   docker compose pull
   ```

    !!! note
    
        If using an older version of docker, don't forget the hyphen:
        ```shell
        docker-compose pull
        ```

4. Now bring the Docker ensembler down and up again:
   ```shell
   docker compose down && docker compose up -d
   ```

    !!! note
    
        If using an older version of docker, don't forget the hyphen:
        ```shell
        docker-compose down && docker-compose up -d
        ```

4. Run the install script for the bot blocker in the default dry run mode and review the output:
   ```shell
   docker exec -ti esmero-web bash -c "/usr/local/sbin/install-ngxblocker"
   ```
5. If everything looks ok, run with the execute flag:
   ```shell
   docker exec -ti esmero-web bash -c "/usr/local/sbin/install-ngxblocker -x"
   ```
6. Run the setup script for the bot blocker in the default dry run mode and review the output:
   ```shell
   docker exec -ti esmero-web bash -c "/usr/local/sbin/setup-ngxblocker -v /etc/nginx/templates -e .copy"
   ```
7. If everything looks ok, run with the execute flag:
   ```shell
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
   ```shell
   docker compose down && docker compose up -d
   ```

    !!! note
    
        If using an older version of docker, don't forget the hyphen:
        ```shell
        docker-compose down && docker-compose up -d
        ```

10. There is a small bug that causes environment substitution to fail on the NGINX container so we need to run it manually:
    ```shell
    docker exec -ti esmero-web bash -c "/docker-entrypoint.d/20-envsubst-on-templates.sh"
    ```
11. Now check that the NGINX configurations generated from the template are valid:
    ```shell
    docker exec -ti esmero-web bash -c "nginx -t"
    ```
    You should see something like the below output:
    ```shell
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful 
    ```
12. Restart the NGINX service:
    ```shell
    docker exec -ti esmero-web bash -c "nginx -s reload"
    ```
13. Test that it is working by following the "TESTING" section (STEP 10) in the official documentation: <https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker>
