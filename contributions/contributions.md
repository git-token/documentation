# Git Contributions

GitToken provides a Docker image and Dockerfile for configuring and listening to incoming GitHub contribution events via HTTP POST requests made by an organization's GitHub webhook service.

Request data is parsed and signed by the GitToken middleware handler, and sent to the GitToken contract to create and distribute tokens to contributors.

@import "../webhook/github_webhook_events.md"
