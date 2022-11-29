---
title: Utility Scripts
tags:
  - Bash
  - Scripts
  - DevOps
---

# Utility Scripts

If you've already followed deployment guides for [archipelago-deployment](archipelago-deployment-readme.md) and [archipelago-deployment-live](archipelago-deployment-live-readme.md), you may have used some shell scripts that archipelago provides. The scripts are available in the `scripts/archipelago/` and `drupal/scripts/archipelago/` folders respectively.

## Import/Export Metadata Display Twig Templates

Metadata Display Entity Twig Templates can be exported out of and imported into both local and remote deployments with the following script: `import_export.sh`. The script can be run interactively or non-interactively.

!!! warning "Docker host vs. Docker container"

    Because the script uses the Docker `.env` file for the JSONAPI user and URL by default, we recommend running this directly on the host.

### Interactive Mode

Running the script interactively will guide you through a number of prompts to configure and import or export to an existing folder or to one which will be created.

```shell
./import_export.sh -n
```

### Non-interactive Mode

To run the command non-interactively provide the required and optional parameters with the necessary arguments as needed.

!!! info "Options for Non-interactive Mode"

    `-i` or `-e` (required)

    &nbsp;&nbsp;&nbsp;&nbsp;Import or export, respectively, Metadata Entity Display Twig Templates from a local folder.

    `-s path` (required)

     &nbsp;&nbsp;&nbsp;&nbsp;The absolute path of the local folder to export to or import from.

    `-j path/filename` (only required if the `.env` file containing the JSONAPI user and password is in a non-standard location)

    &nbsp;&nbsp;&nbsp;&nbsp;The absolute path to the `.env` file containing the JSONAPI user and password.

    `-d url` (required if URL is not in `.env` file or importing to or exporting from a remote deployment)

    &nbsp;&nbsp;&nbsp;&nbsp;The URL of the archipelago deployment.

    `-k` (optional)

    &nbsp;&nbsp;&nbsp;&nbsp;Keep any existing files ending with `.json` in the specified folder (the default is to delete) before exporting.

!!! info "JSONAPI User"

    The JSONAPI user credentials, by default, will be read from the `.env` files in the following locations (relative to the root of the deployment):

    | Deployment | File Location |
    | --- | --- |
    | archipelago-deployment-live | `./deploy/ec2-user/.env` |
    | archipelago-deployment | `./.env` |

    A separate file can also be passed as an argument using the `-j` option.

    ```shell title=".env"
    JSONAPI_USER=jsonapi
    JSONAPI_PASSWORD=jsonapi
    ```

!!! example "Exporting from local archipelago-deployment-live"

    ```shell
    ./import_export.sh -e -s /home/ec2-user/metadatadisplay_export
    ```
    After logging into the archipelago-deployment-live host, the above command will delete any files with the `.json` extension if the destination folder exists. Otherwise, the folder will be created. The JSON user credentials and domain from the `.env` file will then be used to download the files so please make sure these are set.

!!! example "Exporting from local archipelago-deployment"

    ```shell
    ./import_export.sh -e -s /home/user/metadatadisplay_export -d http://localhost:8001
    ```
    This will work the same way as the above example, but the URL is passed as an argument in this case since the `.env` file will not contain (in most cases) the domain. As above, the JSON user credentials will have to be set in the `.env` file.

!!! example "Exporting from remote archipelago-deployment-live"

    ```shell
    ./import_export.sh -e -s /home/user/metadatadisplay_export -d https://archipelago.nyc
    ```
    This is essentially the same as the example directly above, except that in this case the JSON user credentials in the `.env` file will have to be set to the ones used to access the remote instance.

!!! example "Importing locally into archipelago-deployment-live"

    ```shell
    ./import_export.sh -i -s /home/user/metadatadisplay_import
    ```
    This is essentially the same as the first example above, except that the import option (`-i`) is used. The folder name is changed for the sake of example, but you can use the same folder that was used for exporting.

!!! example "Importing locally into archipelago-deployment"

    ```shell
    ./import_export.sh -i -s /home/user/metadatadisplay_import -d http://localhost:8001
    ```
    As in the example directly above, this corresponds to the example for exporting with a local archipelago-deployment instance, except that the import option (`-i`) is used.

!!! example "Importing from local instance into remote archipelago-deployment-live"

    ```shell
    ./import_export.sh -i -s /home/user/metadatadisplay_import -d https://archipelago.nyc
    ```
    In this example, the locally exported files are being imported into a remote instance. As in the above examples with remote instances, the JSON user credentials need to be set in the `.env` file to those with access to the remote instance.

## Automatic Deployment Script

If you're frequently deploying locally with archipelago-deployment, you may want to use the automated deployment script available at `scripts/archipelago/devops/auto_deploy.sh`. The script is interactive and can be called from the root of the deployment, e.g. `/home/user/archipelago-deployment/`:

!!! example "Automatic Deployment"

    scripts/archipelago/devops/auto_deploy.sh

Follow the prompts and select your options to complete the deployment.
