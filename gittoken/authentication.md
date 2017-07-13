## GitHub Open Authorization (OAuth)

To receive tokens, contributors must verify her/his identity with the organization's GitToken contract.

<img height="600" align="right" src="../images/GitHubOAuth.png" >

The GitToken NodeJS Express application provides a GitHub OAuth endpoint for contributors to authenticate themselves with the organization's GitToken contract instance.

Additionally, GitToken provides a web application user interface to enable contributors to easily verify their identity.

By default authentication requests are handled at the GitToken server endpoint `/auth/github`.



A contributor verifies their identity by associating an Ethereum address to their OAuth GitHub credentials using the web application user interface.
