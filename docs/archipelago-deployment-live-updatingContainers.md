---
title: Updating containers in an Archipelago-deployment-live instance
tags:
  - Github
  - Docker
  - Archipelago-deployment-live
---

# How to update your Docker containers

From time to time you may have a need to update the containers themselves. Primarily this is done for security releases.

## 1. Update docker-compose.yml

The first thing you need to do is to edit your `docker-compose.yml` file and replace the version of the container with the new one you wish to use.

Navigate to your `docker-compose.yml` file and open it to edit. On Debian installs it would look like this:

```shell
  cd /usr/local/archipelago/deploy/archipelago-deployment-live/deploy/ec2-docker
  vi docker-compose.yml
```

You want to change the image line to reflect the name of the new image you wish to use:

```yaml
image: esmero/php-7.4-fpm:1.0.0-RC3-multiarch
```

might become:

```yaml
image: esmero/php-8.0-fpm:1.1.0-multiarch
```
  
Save your change. If use vi like in the above it would look like this:

```shell
  :wq
```

## Pull the new image(s)

Docker Compose will now allow us to grab the new image(s) while your current system is running:

```shell
  docker-compose pull
```

## Stop and restart the container

It is necesary to stop and start the container or the current image will continue to be used:

```shell
  docker-compose stop container-name
```

Wait for it to stop. Then bring it back up:

```shell
docker-compose up -d 
```

It is important to use the -d flag or you will have your live instance stuck in your terminal. You want it to run in the background. The `-d` flag stands for *detached*.

### Down and up

If you are more comfortable having the all the containers go down and up you can do that with the following:

```shell
docker-compose down
docker-compose up
```

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback, or open an ISSUE in this [Archipelago Deployment Live Repository](https://github.com/esmero/archipelago-deployment-live/).

Return to [Archipelago Live Deployment](archipelago-deployment-live-readme.md).
