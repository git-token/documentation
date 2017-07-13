---
references:
- id: octoverse
  author:
    - family: GitHub et. al.
  title: The state of the Octoverse 2016
  container-title: 'https://octoverse.github.com/'
  accessed:
    year: 2016
    month: 12
    day: 31
- id: github2017
  author:
    - family: GitHub et. al.
  title: Celebrating nine years of GitHub with an anniversary sale
  container-title: 'https://github.com/blog/2345-celebrating-nine-years-of-github-with-an-anniversary-sale'
  accessed:
    year: 2017
    month: 7
    day: 12
- id: Buterin2017
  author:
    - family: Buterin et. al.
  title: A Next-Generation Smart Contract and Decentralized Application Platform
  container-title: 'https://github.com/ethereum/wiki/wiki/White-Paper'
  accessed:
    year: 2017
    month: 7
    day: 12
- id: git2017
  author:
    - family: Git, et. al.
  title: gitworkflows - An overview of recommended workflows with Git
  container-title: 'https://git-scm.com/docs/gitworkflows'
  accessed:
    year: 2017
    month: 7
    day: 11
- id: gnosis2017
  author:
    - family: Aitken
  title: Gnosis' Prediction Market Scores $12.5M In 'Record-Breaking' Crypto Auction
  container-title: 'https://www.forbes.com/sites/rogeraitken/2017/04/24/gnosis-prediction-market-scores-12-5m-in-record-breaking-crypto-auction/#3afec93ce87d'
  accessed:
    year: 2017
    month: 7
    day: 11
---
# Introduction


Over the past 9 years GitHub has quickly become the largest host of source code in the world, managing nearly 57 million repositories and 100 million pull requests for over 26 million users. [@github2017] As the open source movement continues to gain momentum so to does the number of individual contributors to these repositories. In some cases these repositories had over 10,000 contributors. [@octoverse]

At the same time, the emergence of cryptographic networks and assets, such as Ethereum, has created new protocols for sending and managing value. Ethereum is a cryptographic network for running distributed programs; allowing users of Ethereum to send peer-to-peer (P2P) transactions and interact with smart contracts deployed on the network.

Ethereum smart contracts are software applications written in a high-level scripting language and compiled into byte code to be run on a version of the Ethereum Virtual Machine (EVM). The EVM interprets the byte code instruction set and translates the the program into machine code to be executed. [@Buterin2017]

GitToken combines the work flows of Git version control system leveraged by GitHub's web-based source code management platform and the Ethereum network to provide a set of open-source software tools and programs to enable any GitHub user to issue their own ERC20 tokens to incentivize and reward contributors, and monitor the fundamentals of their projects by integrating token generation with git contributions.

Contributions are mapped to GitHub web hook events, and include but are not limited to, creating issues, committing code, merging branches, forking repositories, and reviewing code. GitToken provides a Docker container to deploy a server for configuring and listening to GitHub web hook events.

Contributors receive tokens through interacting with an organization that has configured a GitToken server and has setup a GitHub web hook. When a contributor creates a new event, her/his GitHub login username is provided in the web hook request. Each username is mapped to an Ethereum address in the GitToken contract.

Contributors verify their GitHub identity by authenticating into GitHub using their Open Authorization (OAuth) token credentials. Contributors authenticate themselves with the token contract using the GitToken server authentication URL associated with an organization. For example, the authentication URL for the GitToken GTK token contract is [`https://GitToken.org/auth/github`](https://GitToken.org/auth/github). If a contributor has not yet verified their identity with the contract, their contribution rewards will be held by the contract until their identity has been verified.

The Ethereum ecosystem has adopted a de facto contract interface for transacting value on top of the Ethereum network, the ERC20 protocol.  The ERC20 protocol allows tokens to be exchanged over-the-counter (OTC) with private parties using Ethereum contracts. While the standard is still evolving, many developers have used the ERC20 token to represent utility or rights in their projects and have offered tokens to the public to raise funding for open source development. [@gnosis2017]

[GitToken](https://gittoken.org) will offer GTK tokens to represent contributions made to the organization's GitHub [repositories](https://github.com/git-token). A portion of tokens issued are automatically auctioned to bidders by the GitToken contract upon each event.

---
