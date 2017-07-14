# GitHub Integrations

GitToken provides a Docker image and Dockerfile for configuring a NodeJS Express application for listening to incoming GitHub contribution events via HTTP POST requests made by an organization's GitHub webhook service.

Additionally, the GitToken Express application establishes Open Authorization (OAuth) endpoints for authenticating contributors with their GitHub login credentials and verifying the contributor with the GitToken contract instance.

Request data is parsed and signed by the GitToken middleware handler, and sent to the GitToken contract to create and distribute tokens to contributors.

@import "../webhook/github_webhook_events.md"
@import "../gittoken/authentication.md"
