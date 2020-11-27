# Debugging PHP in Archipelago

This document describes how to enable Xdebug for local PHP development using the PHPStorm IDE and a docker container running the Archipelago `esmero-php:development` image. It involves interacting with the [esmero/archipelago-docker-images](https://github.com/esmero/archipelago-docker-images) repo and the [esmero/archipelago-deployment](https://github.com/esmero/archipelago-deployment) repo.

## Part 1: Docker
1. Run the following commands from your `/archipelago-deployment` directory:

   `docker-compose down` \
   `docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d`

   This version of `docker-compose up` uses an override file to modify our services. `docker-compose.dev.yml` we now have an extra PHP container called `esmero-php-debug`. To stop the containers in the future, run `docker-compose -f docker-compose.yml -f docker-compose.dev.yml down`.

   So we have reloaded the containers and now you are ready for Part 2.

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

 # Set up Browser Bookmarklets

 1. We are still in the `archipelago-deployment` repository. Find the `/xdebug` folder. Inside is a file  `js-bookmarks-for-xdebug.txt`. This text file contains Javascript that you can copy and paste into your browser's bookmarks manager. Paste the JS snippet into the URL for the bookmark. Create 2 bookmarks: Start, and Stop.
     ![Debug](../imgs/xdebug/bookmarks.png)
     ![Debug](../imgs/xdebug/bookmarks2.png)   
     ![Debug](../imgs/xdebug/bookmarks3.png)   

 We will use these bookmarks to Start and Stop debugging sessions from the browser.

 # Actually Debugging!
 1. Hit the button (top right bar of PHPStorm) that looks like a telephone, for `Start Listening for PHP Debug Connections`.

      ![Debug](../imgs/xdebug/telephone.png)

 2. Now, you can use `Run > Debug` and select the `docker` named configuration that we created in the previous steps. The debugging console will appear. It will say it is waiting for incoming connection from 'archipelago'
 .
       ![Debug](../imgs/xdebug/waiting.png)

 3. Right now the debugging session is not enabled. Browse to  `localhost:8001`. Hit the `Start Debugging` bookmark you created in the previous session. This loads a cookie called `XDEBUG_SESSION`, with the value `archipelago` that can communicate with PHPStorm.

  4. Now set a breakpoint in your code, and refresh the page.
 If you have breakpoints set, either manually, or from leaving "Break at first line in PHP scripts" checked, you should have output now in the debugger.

 5. If you are done actively debugging, it is best to hit the `Stop Debugging` bookmark we also created in the previous step. This will greatly improve speed and performance for your app in development. When you need to debug, just turn on debugging using the bookmarks again.

 6. If you would like to see the output of your xdebug logs, run the following script:
 `docker exec -ti esmero-php bash -c 'tail -f /tmp/xdebug.log > /proc/1/fd/2'`

    Then, you can use the typical docker logs command on the `esmero-php` container, and you will see the xdebug output:
    `docker logs esmero-php -f`


Xdebug makes accessing variables in Drupal kind of great. Many possibilities, including debugging for Twig templates. Happy debugging!

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
