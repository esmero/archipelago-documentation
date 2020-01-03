# Debugging PHP in Archipelago

This document describes how to enable Xdebug for local PHP development using the PHPStorm IDE and a docker container running the Archipelago `esmero-php:development` image. It involves interacting with the [esmero/archipelago-docker-images](https://github.com/esmero/archipelago-docker-images) repo and the [esmero/archipelago-deployment](https://github.com/esmero/archipelago-deployment) repo.

## Part 1: Docker
1. Build the development image with Xdebug enabled. Run the following from the `/archipelago-docker-images/esmero-php-fmp` folder:

    `sudo docker build -f Dockerfile.dev -t esmero-php:development . --no-cache`
    
    This image is based on top of our main `esmero-php` image with a few additions.

2. Run the following commands from your `/archipelago-deployment` directory:
   
   `docker-compose down` \
   `docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d`
   
   This version of `docker-compose up` uses an override file to modify the image being used by our PHP service. `docker-compose.dev.yml` uses the `esmero-php:development` tagged image that we built in step 1. So we have rebuilt the containers and now you are ready for Part 2.

## Part 2: PHPStorm

1. In PHPStorm, open your `archipelago-deployment` project.
 
2. Go to `Preferences > Languages & Frameworks > PHP > Debug`. In this window there is an Xdebug section. Use these settings:
    - Debug port: `9999`. (do NOT use the default, 9000)
    - Can accept external connections: yes, select checkbox
    - (optional) Break at first line in PHP scripts: uncheck. If you leave this selected, you will have to manually step through a breakpoint from Drupal's main index.php file on every request, which is quite annoying. However, leaving this box checked can be useful for making sure the connection is working at first, before you have set any internal breakpoints.
    
    Your settings should look like this. Hit APPLY and OK.
    ![Debug](../imgs/xdebug/debug-settings.png)    

3. Go to `Preferences > Languages & Frameworks > PHP > Servers`. We will create a new server here. Use these settings:
    - Name: `docker-debug-server`
    - Host: `localhost`
    - Port: `8001`
    - Use path mappings: yes, select the checkbox
    - Under project files, select the top-level `archipelago-deployment` directory in the `File/Directory` column.
    - In the `Absolute path on the server` add `/var/www/html`
    
    Hit APPLY and OK and close the window.
  
    ![Debug](../imgs/xdebug/server-settings-2.png)    
 
 4. Go to `Run > Edit Configurations`. Hit the `+` Button to create a new PHP Remote Debug. Name whatever you want, I called mine `docker`. Use these settings:
    - Filter debug connection by IDE Key: yes, select the checkbox
    - Server: select `docker-debug-server` from dropdown (we created this in step 3)
    - IDE Key: `archipelago` (this matches the key set in our container)
    
    ![Debug](../imgs/xdebug/edit-configurations.png)    
 
 5. Validate your connection. With  `Run > Edit Configurations` still open, you can hit the link that says "Validate". Use these settings in the following validation window:
    - Path to create validation script `<your local path>/archipelago-deployment/web`
    - Url to validation script: `http://localhost:8001`
    
    Hit VALIDATE. You should get a series of green check marks. If you get a warning about missing `php.ini` file, that is OK, our file has a different name in the container (`xdebug.ini`) and is still being read correctly.
    ![Debug](../imgs/xdebug/validate-2.png)    

 
 # Actually Debugging!
 1. Hit the button (top right bar of PHPStorm) that looks like a telephone, for `Start Listening for PHP Debug Connections`.
 
      ![Debug](../imgs/xdebug/telephone.png) 
      
 2. Now, you can use `Run > Debug` and select the `docker` named configuration that we created in the previous steps. The debugging console will appear. It will say it is waiting for incoming connection from 'archipelago'
 .
       ![Debug](../imgs/xdebug/waiting.png) 

 3. Go to your Archipelago at `localhost:8001` and refresh the page. If you have breakpoints set, either manually, or from leaving "Break at first line in PHP scripts" checked, you should have output now in the debugger. If you have no breakpoints set, at least the "Waiting for incoming" message will now say "Connected".
 
 4. If you would like to see the output of your xdebug logs, run the following script:
 `docker exec -ti esmero-php bash -c 'tail -f /tmp/xdebug.log > /proc/1/fd/2'`
 
    Then, you can use the typical docker logs command on the `esmero-php` container, and you will see the xdebug output:
    `docker logs esmero-php -f`
    
 
Xdebug makes accessing variables in Drupal kind of great. Many possibilities, including debugging for Twig templates. Happy debugging!

 ----
 #### Questions? Please ask our [Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) and we will respond.
 
 
 
 
 

