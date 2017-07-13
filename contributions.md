# Git Contributions

GitToken provides a Docker image and Dockerfile for configuring and listening to incoming GitHub contribution events via HTTP POST requests made by an organization's GitHub webhook service.

Request data is parsed and signed by the GitToken middleware handler, and sent to the GitToken contract to create and distribute tokens to contributors.

## GitHub Webhook Events

### Configuring a Webhook

> We’ll send a POST request to the URL below with details of any subscribed events. You can also specify which data format you’d like to receive. More information can be found in our [developer documentation](https://developer.github.com/webhooks/).

Under the **settings** tab in an organization's GitHub dashboard, click **Webhook** on the left navigation section and add a new webhook.

<img src="./GitHubWebHookSetup.png" >

GitToken, by default, sets the webhook endpoint to be `/gittoken`. This endpoint is customizable in the configuration file of the GitToken Docker service. [^GTKWebHook]

Enter the url of the organization's GitHub webhook endpoint in the `payload URL` field of the webhook settings page. This is the endpoint that will receive POST requests when a contribution is made to any of an organizations' repositories.

Additionally, the organization can filter for which events they would like to receive from the web hook service. The organization may wish to listen only for `push` events, or they may wish to receive individual events, or all events. GitToken provides event handling for all event types and recommends listening for all events.

#### Ping Event

The `ping` event is the first event sent by the GitHub webhook service. Its purpose is to test the endpoint configuration and, in the context of GitToken, establish the keystore and contract for the GitToken server.

The webhook service will display a checkmark if the endpoint responds with a `200` success status. A successful response will include JSON data about the keystore account, contract creation transaction receipt, and current details about the Ethereum blockchain the GitToken server is connected to.

```javascript
{
  "accounts": { ... },
  "contract": { ... },
  "blockchain": { ... }
}
```

Otherwise, the webhook service will display an error message with either a `400` or `500` error status.

Upon receiving a ping event, the GitToken server checks if a keystore and GitToken contract already exist. If either does not exist, the GitToken server attempts to create the keystore and deploy a contract with the configured parameters provided to the GitToken server instance.

One common reason a contract may not deploy may be due to inadequate funds in the Ethereum account tied to the keystore. For the purpose of the GitToken alpha on the Ropsten testnet a faucet can be used to provide the minimum amount necessary to create a contract.

The server will respond with an error message if the contract could not be created.

### Event Types

--------------------------------------------------------------------
Event                      Reward Value Description
-------------------------- ------------ -----------------------------------------
*                                     0 Any time any event is triggered (Wildcard Event).

commit_comment                      250 Any time a Commit is commented on.

create                             2500 Any time a Branch or Tag is created.

delete                                0 Any time a Branch or Tag is deleted.

deployment                         5000 Any time a Repository has a new deployment created from the API.

deployment_status                   100 Any time a deployment for a Repository has a status update from the API.

fork                               5000 Any time a Repository is forked.

gollum                              250 Any time a Wiki page is updated.

installation                        250 Any time a GitHub App is installed or uninstalled.

installation_repositories          1000 Any time a repository is added or removed from an installation.

issue_comment                       250 Any time a comment on an issue is created, edited, or deleted.

issues                              500 Any time an Issue is assigned, unassigned, labeled, unlabeled, opened, edited, milestoned, demilestoned, closed, or reopened.

label                               100 Any time a Label is created, edited, or deleted.

marketplace_purchase                  0 Any time a user purchases, cancels, or changes their GitHub Marketplace plan.

member                             1000 Any time a User is added or removed as a collaborator to a Repository, or has their permissions modified.

membership                         1000 Any time a User is added or removed from a team. Organization hooks only.

milestone                         15000 Any time a Milestone is created, closed, opened, edited, or deleted.

organization                       1000 Any time a user is added, removed, or invited to an Organization. Organization hooks only.

org_block                             0 Any time an organization blocks or unblocks a user. Organization hooks only.

page_build                          500 Any time a Pages site is built or results in a failed build.

project_card                        250 Any time a Project Card is created, edited, moved, converted to an issue, or deleted.

project_column                       50 Any time a Project Column is created, edited, moved, or deleted.

project                            1000 Any time a Project is created, edited, closed, reopened, or deleted.

public                            10000 Any time a Repository changes from private to public.

pull_request_review_comment         250 Any time a comment on a pull request's unified diff is created, edited, or deleted (in the Files Changed tab).

pull_request_review                 100 Any time a pull request review is submitted, edited, or dismissed.

pull_request                       1000 Any time a pull request is assigned, unassigned, labeled, unlabeled, opened, edited, closed, reopened, or synchronized (updated due to a new push in the branch that the pull request is tracking). Also any time a pull request review is requested, or a review request is removed.

push                               1000 Any Git push to a Repository, including editing tags or branches. Commits via API actions that update references are also counted. This is the default event.

repository                         2500 Any time a Repository is created, deleted (organization hooks only), made public, or made private.

release                            5000 Any time a Release is published in a Repository.

status                              200 Any time a Repository has a status update from the API

team                               2000 Any time a team is created, deleted, modified, or added to or removed from a repository. Organization hooks only

team_add                           2000 Any time a team is added or modified on a Repository.

watch                               500 Any time a User stars a Repository.
--------------------------------------------------------------------




[^GTKWebHook]: e.g., the webhook endpoint for [GitToken](https://github.com/git-token)'s repositories is `https://GitToken.org/gittoken`
