# Min.io Logging

**Q:** How can I see my minio (S3) docker container's realtime traffic and requests?

**A:** For standard demo deployments, mini.io storage server runs on the `esmero-minio` docker container. Steps are:

1. Install the `mc` binaries (minio client) for your platform following [this instructions](https://docs.min.io/docs/minio-client-quickstart-guide.html). e.g for OSX run on your terminal:

```SHELL
brew install minio/stable/mc
mc alias set esmero-minio http://localhost:9000 user password
```

with `http://localhost:9000` being your current machines mini.io URL and exposed port,  `user` being your username (defaults to `minio`) and your original choosen `password` (defaults to `minio123`)

2. Run a `trace` to watch realtime activity on your terminal:

```SHELL
mc admin trace -v -a --debug  --insecure --no-color esmero-minio
```

_Note: `mc` client is also AWS S3 compatible and can be used to move/copy/delete files on the local instance and to/from a remote AWS storage._

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
