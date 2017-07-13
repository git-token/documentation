# GitToken Services

## Web API Server

GitToken provides an Express Node.JS application for handling HTTP post requests from GitHub web hook services.

The GitToken server consists of composable GitToken Node libraries and packages; including an Express middleware library that can be used in existing Express applications.

Additionally, GitToken provides a Dockerfile and Docker image to quickly deploy a GitToken server instance, using an environmental variables file and Docker Compose (`docker-compose up --build -d`) to deploy the server.

@import "../docker/index.md"
