# Linux Users - Docker Builder Prune

The `docker builder prune` command is used to clean up unused builder resources within Docker, which are artifacts left behind during the image building process. Builders are used to create images from a set of source code and configuration files. Pruning these resources can help free up disk space and improve the efficiency of the Docker environment.


`docker-builder-prune.sh`

```shell
#!/bin/bash

docker builder prune --all --force --verbose

echo "Done!"

```


