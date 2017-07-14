#### Docker Image

Docker images are cached snapshots of built Docker containers. They provide a minimal amount of data, which may be composed of immutable layers of cached snapshot data from other images, to form a container environment runnable by docker machines.

GitToken provides public Docker images for services on [Docker Hub](https://hub.docker.com/r/gittoken/).

##### Using an Image

GitToken provides a NodeJS Express application for handling GitHub web hook events and distributing tokens for git contributions.

From within a Docker machine, running the CLI `docker pull gittoken/express-server` will grab the GitToken `express-server` image for running a GitToken server instance.

The image is intended to be run with `docker-compose` where an [environment variables](https://docs.docker.com/compose/environment-variables/) (`.env`) file is provided to the `docker-compose.yml` `env_file` field.

@import "env_file.md"
