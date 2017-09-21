**Note:** This document is under active development. If you have any questions, comments, or suggestions feel free to reach out!

# GitToken Whitepaper v0.1

## Abstract
GitToken is a decentralized open source service that allows any GitHub repository to reward repository contributors with their own Ethereum ERC20 tokens. We hope that the ability for contributors to seamlessly fund projects they like and for open source developers to be rewarded for their work will help move the open source community and associated ethos forward.

### Table of Contents
* [1. Background and Motivation](#1-background-and-motivation)
* [2. Getting Started](#2-getting-started)
* [3. GitToken Architecture Overview](#3-gittoken-architecture)
  + [3.1  Webhook](#31-webhook)
  + [3.2. Command Line Interface](#32-command-line-interface)
  + [3.3. Dashboard](#33-dashboard)
  + [3.4. Registry & Exchange](#34-registry-&-exchange)
* [4. Token Economics](#4-token-economics)
* [5. Funding Projects with GitToken](#4-gittoken-funding)
  + [5.1. For Projects](#51-funding-project)
  + [5.2. For Contributors](#52-funding-contributor)
* [6. Project Analysis & Comparison](#4-gittoken-project-analysis)
  + [6.1. Finding Projects](#61-finding-projects)
  + [6.1. Analyzing Projects](#62-analyzing-projects)
* [7. GitToken Multiverse](#4-gittoken-muiltiverse)


# 1. Background and Motivation
Over the past 9 years GitHub has quickly become the largest host of source code in the world, managing nearly 57 million repositories and 100 million pull requests for over 26 million users. As the open source movement continues to gain momentum so to does the number of individual contributors to these repositories. In some cases repositories have over 10,000 independent contributors working on a volunteer basis.

At the same time, the emergence of cryptographic networks and assets, such as Ethereum, has created new protocols for sending and managing value. Ethereum is a cryptographic network for running distributed programs, allowing users to send peer-to-peer transactions and interact with smart contracts deployed on the global network.

Ethereum smart contracts are software applications written in a high-level scripting language and compiled into byte code to be run on a version of the Ethereum Virtual Machine (EVM). The EVM interprets the byte code instruction set and translates the program into machine code to be executed.

GitToken combines the work flows of the Git version control system leveraged by GitHub's web-based source code management platform and the Ethereum network to provide a set of open-source software tools that enable any GitHub user to issue their own ERC20 tokens to incentivize and reward contributors, and monitor the fundamentals of their projects.

Contributions are mapped to GitHub web hook events, and include but are not limited to, creating issues, committing code, merging branches, forking repositories, and reviewing code. GitToken provides a Docker image to configuring and deploy a server for listening to GitHub web hook events.

Contributors receive tokens through interacting with an organization that has configured a GitToken server instance and has setup a GitHub web hook. When a contributor creates a new event, their GitHub login username is provided in the web hook request. Each username is mapped to an Ethereum address in the GitToken contract.

Contributors verify their GitHub identity by authenticating into GitHub using their Open Authorization (OAuth) token credentials. Contributors authenticate themselves with the GitToken token contract using the GitToken server authentication URL associated with an organization. If a contributor has not yet verified their identity with the contract, their contribution rewards will be held by the contract until their identity has been verified.

The Ethereum ecosystem has adopted a de facto contract interface for transacting value on top of the Ethereum network, the ERC20 protocol. The ERC20 protocol allows tokens to be exchanged over-the-counter (OTC) with private parties using Ethereum contracts. While the standard is still evolving, many developers have used the ERC20 token to represent utility or rights in their projects and have offered tokens to the public to raise funding for open source development.

GitToken will offer GTK tokens to represent contributions made to its GitHub repositories. A portion of tokens issued are automatically auctioned to bidders by the GitToken contract upon each event.

# 2. Getting Started

GitToken isn't quite ready yet, but we're aiming for an alpha release on a private network, the Torvalds network, at the end of the month.

You can get started now by registering your organization in two simple steps using the GitToken CLI

1) Using npm (or yarn), install GitToken
`npm i -g git-token`

2) Register your account by typing
`git token register`
and following the instructions provided. This will set up your tokens and webhook for you.

Note that to register you'll need a GitHub organization and personal access token, which you can get by going to Settings > Developer Settings > Personal Access Tokens.

# 3. GitToken Architecture

GitToken can be described at a high level by the chart below. Each box represents a core component of the service. The arrows between components represent the flow of data: the solid arrows represent requests while the unfilled arrows represent responses.

![Architecture Chart](https://raw.githubusercontent.com/git-token/documentation/master/images/alpha_services_chart.png)

# 4. Token Economics

All GitToken contracts are created with an initial supply of zero `0` tokens.

Upon each valid contribution, the total supply of the tokens issued increases relative to the contribution type and corresponding reward value set by the organization.

Additionally, given special events, such as adding a new member to an organization or reaching a milestone, a reserved supply of tokens is created. The purpose of the reserve supply is to allocate a portion of tokens generated for auction. The auctioning of these tokens is determined by the organization’s milestone schedules, which can be set up using GitHub’s existing project management tools.

GitToken's token issuance and distribution model is continuous and its supply is inflationary. Inflationary forces are strongest when the rate of supply increases faster than the real output of the per unit value. This may occur when tokens are issued for events that have not been realized or hold intangible value. For example, for repositories that enable it, issuing tokens for `watch` events is probably the least difficult or easiest way to earn tokens and holds intangible value (e.g. value in the form of the community's interest in the project).

Events with intangible value can easily become an attack surface if the frequency of watch events is abnormally high relative to the frequency of other contribution events.
To mitigate this scenario and control the rate of token supply increase for intangible events, the GitToken default reward values of these intangible events should always be much smaller than rewards for events representing tangible value, like `commit`, `create`, and `pull_request` events.

A full list of the default [GitHub event](https://developer.github.com/webhooks/#events) reward values are as follows:

|Event | Reward Value | Description |
|---|---|---|
| * | 0 | Any time any event is triggered |
| commit_comment | 250 | Any time a Commit is commented on. |
| create | 2500 | Any time a Branch or Tag is created. |
| delete | 0 | Any time a Branch or Tag is deleted. |
| deployment | 5000 | Any time a Repository has a new deployment created from the API. |
| deployment_status | 100 | Any time a deployment for a Repository has a status update from the API. |
| fork | 5000 | Any time a Repository is forked. |
| gollum | 100 | Any time a Wiki page is updated. |
|installation | 250 | Any time a GitHub App is installed or uninstalled. |
| installation_repositories | 1000 | Any time a repository is added or removed from an installation. |
| issue_comment | 250 | Any time a comment on an issue is created, edited, or deleted. |
|issues | 500 | Any time an Issue is assigned, unassigned, labeled, unlabeled, opened, edited, milestoned, demilestoned, closed, or reopened. |
|label | 100 | Any time a Label is created, edited, or deleted. |
| marketplace_purchase | 0 | Any time a user purchases, cancels, or changes their GitHub Marketplace plan. |
| member | 1000 | Any time a User is added or removed as a collaborator to a Repository, or has their permissions modified. |
| membership | 1000 | Any time a User is added or removed from a team. Organization hooks only. |
| milestone | 15000 | Any time a Milestone is created, closed, opened, edited, or deleted.|
| organization | 1000 | Any time a user is added, removed, or invited to an Organization. Organization hooks only. |
| org_block | 0 | Any time an organization blocks or unblocks a user. Organization hooks only. |
| page_build | 500 | Any time a Pages site is built or results in a failed build. |
| ping | 2500 | Any time a web hook is configured. |
| project_card | 250 | Any time a Project Card is created, edited, moved, converted to an issue, or deleted. |
| project_column | 50 | Any time a Project Column is created, edited, moved, or deleted. |
| project | 1000 | Any time a Project is created, edited, closed, reopened, or deleted. |
| public | 10000 | Any time a Repository changes from private to public. |
| pull_request_review_comment | 250 | Any time a comment on a pull request's unified diff is created, edited, or deleted (in the Files Changed tab). |
| pull_request_review | 100 | Any time a pull request review is submitted, edited, or dismissed.|
| pull_request | 1000 | Any time a pull request is assigned, unassigned, labeled, unlabeled, opened, edited, closed, reopened, or synchronized (updated due to a new push in the branch that the pull request is tracking). Also any time a pull request review is requested, or a review request is removed. |
| push | 1000 | Any Git push to a Repository, including editing tags or branches. Commits via API actions that update references are also counted. This is the default event. |
| repository | 2500 | Any time a Repository is created, deleted (organization hooks only), made public, or made private. |
| release | 5000 | Any time a Release is published in a Repository. |
| status | 200 | Any time a Repository has a status update from the API |
| team | 2000| Any time a team is created, deleted, modified, or added to or removed from a repository. Organization hooks only |
| team_add | 2000 | Any time a team is added or modified on a Repository.|
| watch | 100 | Any time a User stars a Repository.|

Tokens are issued and distributed via the `rewardContributor()` and `_rewardContributor()` contract methods. The full contract can be found [here](https://github.com/git-token/contracts/blob/master/contracts/GitToken.sol).
