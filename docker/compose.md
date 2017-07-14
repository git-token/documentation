#### Compose

[`docker-compose`](https://docs.docker.com/compose/) is a command line interface (CLI) tool for composing docker service deployments.

Deploying a GitToken server instance is intended to be done using a `docker-compose.yml` file to define the configuration of the service.

Use the [`gittoken/express-server:v1`](https://hub.docker.com/r/gittoken/) Docker image to build the GitToken service instance from.

Configuration parameters of a GitToken server instance are passed to the container using an [environment variables](#environment-variables-file) file.

GitToken server instance requires both ports 1324 and 1325 to be publicly exposed and available to the host machine. The host machine may map the default ports to different ports, according to the needs of the developer. For example, the developer may instead map ports `3000:1324` to bind the service to port 3000 on the host machine.



```yml
# Example docker-compose.yml configuration using the
# GitToken express-server Docker image

# Use docker-compose v3
version: '3.0'
# Define services
services:
  # Define gittoken service
  gittoken:
    # Use the GitToken Docker image to build from
    image: "gittoken/express-server:v1"
    # Define an environment variables file path
    # relative to the Docker machine file system
    env_file:
      - /gittoken-server/gittoken.env
    # Expose port 1324 for the Express Server
    # Expose port 1325 for the WebSocket Server
    ports:
      - 1324:1324
      - 1325:1325
    # Mount the local volume of the Docker machine
    # to the container volume
    volumes:
      - /gittoken-server/:/gittoken-server/
```
