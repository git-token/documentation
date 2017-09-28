**Note:** This document is under active development. If you have any questions, comments, or suggestions feel free to reach out!

# GitToken Guide v0.1

## Abstract
GitToken is a decentralized open source service that allows any GitHub repository to reward repository contributors with their own distinct Ethereum ERC20 tokens. We hope that the ability for contributors to seamlessly fund projects they like and for open source developers to be rewarded for their work will help move the open source community and associated ethos forward.

### Table of Contents
* [1. Background and Motivation](#1-background-and-motivation)
* [2. Getting Started](#2-getting-started)
* [3. GitToken Architecture Overview](#3-gittoken-architecture)
  + [3.1  Webhook](#31-webhook)
  + [3.2. Command Line Interface](#32-command-line-interface)
  + [3.3. Dashboard](#33-dashboard)
  + [3.4. Registry & Exchange](#34-registry-&-exchange)
* [4. Token Economics](#4-token-economics)
* [5. Funding Projects with GitToken](#5-funding-projects-with-gittoken)
  + [5.1. For Projects](#51-funding-project)
  + [5.2. For Contributors](#52-funding-contributor)
* [6. Project Analysis & Comparison](#6-project-analysis-and-comparison)
* [7. GitToken Multiverse](#7-gittoken-muiltiverse)


# 1. Background and Overview
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
`npm i -g git-token@alpha`

2) Register your account by typing
`git token register`

3) Follow the instructions provided to set up your tokens and webhook

Note that to register you'll need a GitHub organization and personal access token, which you can get by going to Settings > Developer Settings > Personal Access Tokens.

The GIF below provides a brief overview of what the process above should look like.

# 3. GitToken Architecture

GitToken can be described at a high level by the chart below. Each box represents a core component of the service. The arrows between components represent the flow of data: the solid arrows represent requests while the unfilled arrows represent responses.

![Architecture Chart](https://raw.githubusercontent.com/git-token/documentation/master/images/alpha_services_chart.png)


# 4. Token Economics

## Overview

All GitToken contracts are created with an initial supply of zero `0` tokens.

Upon each valid contribution, the total supply of the tokens issued increases relative to the contribution type and corresponding reward value set by the organization.

Additionally, given special events, such as adding a new member to an organization or reaching a milestone, a reserved supply of tokens is created. The purpose of the reserve supply is to allocate a portion of tokens generated for auction. The auctioning of these tokens is determined by the organization’s milestone schedules, which can be set up using GitHub’s existing project management tools.

Tokens issued by the GitToken service token are issued continuously and their supplies are inflationary. Inflationary forces are strongest when the rate of supply increases faster than the real output of the per unit value. This may occur when tokens are issued for events that have not been realized or hold intangible value. For example `watch` events are probably the easiest way to earn tokens. GitToken does not target a rate of change in the token supply or an inflation rate automatically. However, as a project matures, the rate of change in the token supply should become stable.

`watch` and similar events are easy in part because they have intangible value (e.g. value in the form of the community's interest in the project). Events with intangible value can easily become an attack surface if their frequency is abnormally high relative to other contribution events. To mitigate this scenario and control the rate of token supply increase for intangible events, the GitToken default reward values of intangible events is much smaller than rewards for events representing tangible value, like `commit`, `create`, and `pull_request` events.

## Setting Custom Reward Values

Any organization using the GitToken service can fully customize their event rewards. If so desired, an organization could set all reward values to `0` and only issue bonuses as described later on.

While this is technically allowed by the GitToken contract via the `setRewardValue()` and `setReservedValue()` methods, it is encouraged to consider the impact of setting arbitrary reward values for events, and consider the ramifications of such reward values in the context of the goals and mission of the project before changing the default values.

We hope that as projects test GitToken on our private network and Ropsten we can iterate on our default values to find those enabling the most productive repositories.

For completeness, a full list of [GitHub event](https://developer.github.com/webhooks/#events) and their default reward values are as follows:

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

# 5. Funding Projects with GitToken


## For Projects

Once a project's integrated GitToken into its workflow and has contributors receiving the rewards specified the first priority is to make sure contributors are really getting rewarded for the work they're doing, and that requires fundraising.

While code often speaks for itself, there are a few other things you can do to help ensure your project gets traction. Note that many of these are obvious, but in our estimation worth restating.

**Summarize your vision**

What does your project do? It sounds like an easy question to answer but many projects don't answer it. Here are some succinct examples from some of the most popular open source projects on GitHub (a larger list can be found [here](https://gitstar-ranking.com/)):

>The https://freeCodeCamp.org open source codebase and curriculum. Learn to code and help nonprofits.
-- freeCodeCamp

>The most popular HTML, CSS, and JavaScript framework for developing responsive, mobile first projects on the web.
-- Bootstrap

>A declarative, efficient, and flexible JavaScript library for building user interfaces.
-- React

**Foster community**

Open source is all about collaboration and collaboration goes hand in hand with community. Let your community know how they can help. GitToken has a [community section](https://github.com/git-token/community) for more general questions and comments so that anyone can get involved.

Stay engaged in discussions your community. Not only does it help ensure that everyone is on the same page regarding the current state of the project but it allows you to ensure the direction of the project is more accurately shaped by the community's wants and needs.

**Establish project guidelines**

Guidelines help the community organize itself without constant guidance from project maintainers.

In our case, we've set up a `CONTRIBUTING.md` document to help contributors get an idea of current guidelines. Our goal is to outline the workflows we are currently following and hence the workflows that contributors might also be encouraged to follow in order to create the least friction. Currently this involves working on the basis of a [feature branching model](http://nvie.com/posts/a-successful-git-branching-model/).

Our `CONTRIBUTING.md` also includes a Code of Conduct that explicitly states what is and is not encouraged or tolerated in the community. Having a Code of Conduct and calling out bad behavior at every step is critical to having an engaged and productive community.

## For Contributors

Contributors are the lifeblood of open source. In GitToken enabled projects contributors can provide either their time or donations to projects as they wish.

Contributors offering their time just need to make sure they follow all the guidelines a project has outlined. Note that these may be different than the guidelines set by GitToken itself.

Contributors donating directly to projects should make sure to conduct due diligence on their own and using GitToken tools like the terminal to ensure that the project is meeting its goals. No one wants to donate to organizations that are likely to misspend the funds, [which are more pervasive than you'd think](http://www.tampabay.com/americas-worst-charities/).

# 6. Project Analysis and Comparison

GitToken isn't about a single open source project, it's about the open source ecosystem. To that end we wanted to make sure there was a way to allow anyone to search for and analyze projects. We hope that this will help the open source community understand the underlying value behind what's being built and incentivize the community to seek out and contribute to projects that the community values. As a starting point, we've opted to make this possible via the GitToken Terminal, part of the GitToken CLI.

Users who have installed GitToken using `npm i -g git-token@alpha` will be able to run `git token terminal` to explore GitToken enabled projects and see key statistics around their associated tokens.
