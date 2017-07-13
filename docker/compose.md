#### Compose

```yml
# Example docker-compose.yml configuration using the
# GitToken express-server Docker image

version: '3.0'
services:
  gittoken:
    # Define an environment variables file
    env_file:
      - /gittoken-server/gittoken.env
    ports:
      - 1324:1324
      - 1325:1325
    image: "gittoken/express-server:v1"
    volumes:
      - /gittoken-server/:/gittoken-server/
```
