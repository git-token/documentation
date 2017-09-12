Note: This document is under active development. If you have any questions, comments, or suggestions feel free to reach out!

# GitToken Whitepaper v0.1

### Abstract
This document describes a smart contract platform built on top of [Ethereum](https://ethereum.org/), a globally distributed computing network.
The suggested Proof-of-Code scheme will enable open source projects to reward contributors, fund their projects, and seamlessly trade  [ERC20](https://theethereum.wiki/w/index.php/ERC20_Token_Standard) tokens representing the market's appraisal of the value of a given open source project.

### Table of Contents
* [1. Background and Overview](#1-background)
* [2. Features & Getting Started](#2-features)
* [3. GitToken Architecture Overview](#3-gittoken-architecture)
  + [3.1. GitToken Webhook](#31-webhook)
  + [3.2. GitToken CLI](#32-CLI)
  + [3.2. GitToken Dashboard](#33-dashboard)
  + [3.3. GitToken Registry & Exchange)](#34-registry)
* [4. Token Economics](#4-gittoken-economics)
* [5. Funding Projects with GitToken](#4-gittoken-funding)
  + [5.1. For Projects)](#51-funding-project)
  + [5.2. For Contributors)](#52-funding-contributor)
* [6. Project Analysis & Comparison](#4-gittoken-project-analysis)
  + [6.1. Finding Projects)](#61-finding-projects)
  + [6.1. Analyzing Projects)](#62-analyzing-projects)
* [7. GitToken Multiverse](#4-gittoken-muiltiverse)


# 1. Background and Motivation
Over the past 9 years GitHub has quickly become the largest host of source code in the world, managing nearly 57 million repositories and 100 million pull requests for over 26 million users. As the open source movement continues to gain momentum so to does the number of individual contributors to these repositories. In some cases these repositories had over 10,000 contributors.

At the same time, the emergence of cryptographic networks and assets, such as Ethereum, has created new protocols for sending and managing value. Ethereum is a cryptographic network for running distributed programs; allowing users of Ethereum to send peer-to-peer (P2P) transactions and interact with smart contracts deployed on the global network.

Ethereum smart contracts are software applications written in a high-level scripting language and compiled into byte code to be run on a version of the Ethereum Virtual Machine (EVM). The EVM interprets the byte code instruction set and translates the program into machine code to be executed.

GitToken combines the work flows of Git version control system leveraged by GitHub's web-based source code management platform and the Ethereum network to provide a set of open-source software tools and programs to enable any GitHub user to issue their own ERC20 tokens to incentivize and reward contributors, and monitor the fundamentals of their projects by integrating token generation with git contributions.

Contributions are mapped to GitHub web hook events, and include but are not limited to, creating issues, committing code, merging branches, forking repositories, and reviewing code. GitToken provides a Docker image to configuring and deploy a server for listening to GitHub web hook events.

Contributors receive tokens through interacting with an organization that has configured a GitToken server instance and has setup a GitHub web hook. When a contributor creates a new event, her/his GitHub login username is provided in the web hook request. Each username is mapped to an Ethereum address in the GitToken contract.

Contributors verify their GitHub identity by authenticating into GitHub using their Open Authorization (OAuth) token credentials. Contributors authenticate themselves with the GitToken token contract using the GitToken server authentication URL associated with an organization. If a contributor has not yet verified their identity with the contract, their contribution rewards will be held by the contract until their identity has been verified.

The Ethereum ecosystem has adopted a de facto contract interface for transacting value on top of the Ethereum network, the ERC20 protocol. The ERC20 protocol allows tokens to be exchanged over-the-counter (OTC) with private parties using Ethereum contracts. While the standard is still evolving, many developers have used the ERC20 token to represent utility or rights in their projects and have offered tokens to the public to raise funding for open source development.

GitToken will offer GTK tokens to represent contributions made to the organization's GitHub repositories. A portion of tokens issued are automatically auctioned to bidders by the GitToken contract upon each event.
