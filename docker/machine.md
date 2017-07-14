#### Machine

[`docker-machine`](https://docs.docker.com/machine/) is a command line interface (CLI) tool provided by Docker to deploy containers and images on remote servers and virtual machines.

Using `docker-machine` with `docker-compose` is the preferred method for setting up a GitToken server instance.

##### Configuring a Docker Machine

Configuring a Docker machine requires a remote server to deploy the GitToken server to. The following example demonstrates configuring a basic generic (host agnostic) server.

```bash
# Create a new docker machine
docker-machine create \
  # Specify the `generic` driver
  --driver generic \
  # Supply the public network IP address of the machine
  --generic-ip-address=___.___.___.___ \
  # Provide the path to the private SSH key of the
  # current user for authenticating into the machine
  --generic-ssh-key ~/.ssh/id_rsa \
  # Name the machine
  machine_name
```
