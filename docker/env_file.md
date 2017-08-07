##### Environment Variables File

The `.env` file customizes the values for the GitToken server instance.

```env
# Example environment variables (.env) file

# NOTE Web3 Provider must be a public IP address,
# and not associated with localhost.
#
# i.e. The IP address must not be 127.0.0.1 or 0.0.0.0
#
# If a Docker container is running an Ethereum client on the machine,
# use the IP Address associated with that container's public network IP address
# or the bound port of the service on the machine.

WEB3_PROVIDER=http://___.___.___.___:8545

IS_GITHUB_WEBHOOK=true

GITTOKEN_DIRECTORY_PATH=/gittoken-server

GITTOKEN_KEYSTORE_FILENAME=.keystore

GITTOKEN_CONTRACT_FILE=contract.json

GITTOKEN_FAUCET_ACTIVE=false

GITTOKEN_CONTRACT_OWNER=0x0000000000000000000000000000000000000000

GITTOKEN_CONTRACT_OWNER_USERNAME=your_github_username

GITTOKEN_CONTRACT_ORGANIZATION=Your Organization

GITTOKEN_CONTRACT_SYMBOL=SYMBOL

GITTOKEN_CONTRACT_DECIMALS=8

GITTOKEN_API_SESSION_SECRET=SOMETHINGFANTASTIC

GITHUB_API_ID=YOUR_GITHUB_APPLICATION_CLIENT_ID

GITHUB_API_SECRET=YOUR_GITHUB_APPLICATION_CLIENT_SECRET

GITHUB_API_CALLBACK_URL=https://your_organization.website/auth/github/callback

```
