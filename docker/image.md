#### Docker Image

`docker pull gittoken/express-server`

##### Description

GitToken provides a NodeJS Express application for handling GitHub web hook events and distributing tokens for git contributions.

The image is intended to be run with `docker-compose` where an [environment variables](https://docs.docker.com/compose/env-file/) (`.env`) file is provided to the `docker-compose.yml` `env_file` field. The `.env` file customizes the values for the GitToken server instance.

```bash
# Example environment variables (.env) file

WEB3_PROVIDER                  = "http://127.0.0.1:8545"

IS_GITHUB_WEBHOOK              = true

GITTOKEN_DIRECTORY_PATH        = "/gittoken-server"

GITTOKEN_KEYSTORE_FILENAME     = ".keystore"

GITTOKEN_CONTRACT_FILE         = "contract.json"

GITTOKEN_FAUCET_ACTIVE         = false

GITTOKEN_CONTRACT_OWNER        = "0x0000000000000000000000000000000000000000"

GITTOKEN_CONTRACT_OWNER_EMAIL  = "your_email@your_organization.website"

GITTOKEN_CONTRACT_ORGANIZATION = "Your Organization"

GITTOKEN_CONTRACT_SYMBOL       = "SYMBOL"

GITTOKEN_CONTRACT_DECIMALS     = 8

GITTOKEN_API_SESSION_SECRET    = 'SOMETHINGFANTASTIC'

GITHUB_API_ID                  = "YOUR_GITHUB_APPLICATION_CLIENT_ID"

GITHUB_API_SECRET              = "YOUR_GITHUB_APPLICATION_CLIENT_SECRET"

GITHUB_CALLBACK_URL            = "https://your_organization.website/auth/github/callback"

```
