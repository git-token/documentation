# Economics of GitToken

### Overview

GitToken software issues Ethereum tokens whenever a valid GitHub web hook event is received corresponding to a contribution made toward a GitHub organization.

All GitToken contracts are created with an initial supply of zero `0` tokens. Upon each valid contribution, the total supply of the tokens issued increases, relative to the contribution type and corresponding reward value.

Additionally, given special events, such as adding a new member to an organization or reaching a milestone, a reserved supply of tokens is created. The purpose of the reserve supply is to allocate a supply of tokens for auction. The auctioning of these tokens is determined by the organization's milestone schedule(s). These schedules may be organized by the contributors of the project using GitHub's existing user interfaces.

Consequently, the GitToken token issuance and distribution model is continuous; and therefore its supply is inflationary. Inflationary forces are strongest when the rate of supply increases faster than the real output of the per unit value. This may occur when tokens are issued for events that have not been realized or hold intangible value.

For example, issuing tokens for `watch` events is probably the least difficult or easiest way to earn GitToken tokens; simply click the `Star` repository button on a GitToken enabled GitHub repository.

This event may hold intangible value as it may represent future word of mouth marketing or other channels of connection. However, it may also become an attack surface, if the frequency of watch events is abnormally high relative to the frequency of other contribution events.

To mitigate this scenario and control the rate of token supply increase for intangible events, the GitToken default reward values of these intangible events are relatively smaller compared to events representing tangible value, like `commit`, `create` and `pull_request` events.

For example, `watch` events issue `100 tokens` by default, compared to `2500 tokens` for a `pull_request` or `create` event.

### Setting Custom Reward Values

For certain projects it may make sense to change the default reward values for contribution events.

While this is technically allowed by the GitToken contract using the [`setRewardValue()`](https://github.com/git-token/contracts/blob/master/contracts/GitToken.sol#L337)and [`setReservedValue()`](https://github.com/git-token/contracts/blob/master/contracts/GitToken.sol#L356) methods, it is encouraged to consider the impact of setting arbitrary reward values for events, and consider the ramifications of such reward values in the context of the goals and mission of the project before changing the default values.

```solidity
/**
   * @dev Set the reward value for a GitHub web hook event
   * @param _rewardValue uint256 Number of tokens to issue given a rewardType,
   * @param _rewardType  string  GitHub web hook event,
   * @return             bool    Returns boolean value if method is called;
   */
  function setRewardValue(
    uint256 _rewardValue,
    string _rewardType
  )
    onlyOwner
    public
    returns (bool)
  {
    gittoken.rewardValues[_rewardType] = _rewardValue;
    RewardValueSet(_rewardType, '', _rewardValue, now);
    return true;
  }
```
```solidity
  /**
   * @dev Set the reserved value for a GitHub web hook event subtype
   * @param _reservedValue uint256 Number of tokens to issue given a reservedType,
   * @param _rewardType    string  GitHub web hook event,
   * @param _reservedType  string  GitHub web hook event subtype (action; e.g. `organization` -> `member_added`),
   * @return               bool    Returns boolean value if method is called;
   */
  function setReservedValue(
    uint256 _reservedValue,
    string _rewardType,
    string _reservedType
  )
    onlyOwner
    public
    returns (bool)
  {
    gittoken.reservedValues[_rewardType][_reservedType] = _reservedValue;
    RewardValueSet(_rewardType, _reservedType, _reservedValue, now);
    return true;
  }
```

### Rate of Change of the Token Supply

The purchasing power, or exchange rate, of the per unit value of the token should factor in the rate of change of the token supply.

<a href="https://www.codecogs.com/eqnedit.php?latex=\\&space;Rv&space;=&space;\textrm{Reward&space;Value}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}&space;\\&space;Rb&space;=&space;\textrm{Reward&space;Bonus}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}&space;\\&space;RSv&space;=&space;\textrm{Reserved&space;Supply&space;Value}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}&space;\\\\&space;_{n}&space;=&space;\textrm{Contribution&space;Event&space;Nonce}&space;\\&space;s_{n}\subseteq&space;s'_{n}&space;\textrm{where&space;}_{n}\ne0&space;\\\\&space;s_{n}&space;=&space;\textrm{Initial&space;Token&space;Supply}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}\\&space;s'_{n}&space;=&space;\textrm{Ending&space;Token&space;Supply}&space;=&space;\sum_{n=1}^{\infty}Rv_{n}&plus;Rb_{n}&plus;RSv_{n}&space;\\&space;\Delta{s_{n_{r}}}&space;=&space;\textrm{Periodic&space;Rate&space;of&space;Supply&space;Change}&space;=&space;\frac{s'_{n}}{s_{n}}&space;-&space;1&space;\\\\&space;s_{r_{Gavg}}&space;=&space;\textrm{Geometric&space;Average&space;Rate&space;of&space;Supply&space;Change}&space;=&space;\prod_{n=1}^{\infty}(1&plus;\Delta{s_{n_{r}}})^{1/n}-1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\\&space;Rv&space;=&space;\textrm{Reward&space;Value}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}&space;\\&space;Rb&space;=&space;\textrm{Reward&space;Bonus}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}&space;\\&space;RSv&space;=&space;\textrm{Reserved&space;Supply&space;Value}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}&space;\\\\&space;_{n}&space;=&space;\textrm{Contribution&space;Event&space;Nonce}&space;\\&space;s_{n}\subseteq&space;s'_{n}&space;\textrm{where&space;}_{n}\ne0&space;\\\\&space;s_{n}&space;=&space;\textrm{Initial&space;Token&space;Supply}&space;=&space;\mathbb{N}\left&space;\{&space;\right.0,1,2,3,...\left.&space;\right&space;\}\\&space;s'_{n}&space;=&space;\textrm{Ending&space;Token&space;Supply}&space;=&space;\sum_{n=1}^{\infty}Rv_{n}&plus;Rb_{n}&plus;RSv_{n}&space;\\&space;\Delta{s_{n_{r}}}&space;=&space;\textrm{Periodic&space;Rate&space;of&space;Supply&space;Change}&space;=&space;\frac{s'_{n}}{s_{n}}&space;-&space;1&space;\\\\&space;s_{r_{Gavg}}&space;=&space;\textrm{Geometric&space;Average&space;Rate&space;of&space;Supply&space;Change}&space;=&space;\prod_{n=1}^{\infty}(1&plus;\Delta{s_{n_{r}}})^{1/n}-1" title="\\ Rv = \textrm{Reward Value} = \mathbb{N}\left \{ \right.0,1,2,3,...\left. \right \} \\ Rb = \textrm{Reward Bonus} = \mathbb{N}\left \{ \right.0,1,2,3,...\left. \right \} \\ RSv = \textrm{Reserved Supply Value} = \mathbb{N}\left \{ \right.0,1,2,3,...\left. \right \} \\\\ _{n} = \textrm{Contribution Event Nonce} \\ s_{n}\subseteq s'_{n} \textrm{where }_{n}\ne0 \\\\ s_{n} = \textrm{Initial Token Supply} = \mathbb{N}\left \{ \right.0,1,2,3,...\left. \right \}\\ s'_{n} = \textrm{Ending Token Supply} = \sum_{n=1}^{\infty}Rv_{n}+Rb_{n}+RSv_{n} \\ \Delta{s_{n_{r}}} = \textrm{Periodic Rate of Supply Change} = \frac{s'_{n}}{s_{n}} - 1 \\\\ s_{r_{Gavg}} = \textrm{Geometric Average Rate of Supply Change} = \prod_{n=1}^{\infty}(1+\Delta{s_{n_{r}}})^{1/n}-1" /></a>

#### Monitoring Token Supply Rates of Change

The GitToken software does not target a rate of change in the token supply or an inflation rate. However, as a project matures, the rate of change in the token supply should become stable.

Short-terms "jumps" or "spikes" in the rate of change of the token supply may indicate changes in the projects' organization, reward values, or other meaningful events.

## Token Distribution

### Reward Values

Tokens are issued and distributed via the [`rewardContributor()`](https://github.com/git-token/contracts/blob/master/contracts/GitToken.sol#L395) and [`_rewardContributor()`](https://github.com/git-token/contracts/blob/master/contracts/GitTokenLib.sol#L124) contract methods

```solidity
/**
   * @dev Reward contributor when a GitHub web hook event is received
   * @param  _username     string GitHub username of contributor
   * @param  _rewardType   string GitHub web hook event
   * @param  _reservedType string GitHub web hook event subtype (action; e.g. `organization` -> `member_added`)
   * @param  _rewardBonus  uint   Number of tokens to send to contributor as a bonus (used for off-chain calculated values)
   * @param  _deliveryID   string GitHub delivery ID of web hook request
   * @return               bool   Returns boolean value if method is called
   */
  function rewardContributor(
    string _username,
    string _rewardType,
    string _reservedType,
    uint _rewardBonus,
    string _deliveryID
  )
  onlyOwner
  public
  returns (bool) {
    require(gittoken._rewardContributor(_username, _rewardType, _reservedType, _rewardBonus, _deliveryID));
    address _contributor = gittoken.contributorAddresses[_username];
    uint _value = gittoken.rewardValues[_rewardType].add(_rewardBonus);
    uint _reservedValue = gittoken.reservedValues[_rewardType][_reservedType];

    Contribution(_contributor, _username, _value, _reservedValue, now, _rewardType);

    return true;
  }
```

```solidity
/**
   * @dev Internal rewardContributor method for GitToken contract rewardContributor method
   * @param  self   Data    Use the Data struct as the contract storage and reference
   * @param  _username     string GitHub username of contributor
   * @param  _rewardType   string GitHub web hook event
   * @param  _reservedType string GitHub web hook event subtype (action; e.g. `organization` -> `member_added`)
   * @param  _rewardBonus  uint   Number of tokens to send to contributor as a bonus (used for off-chain calculated values)
   * @param  _deliveryID   string GitHub delivery ID of web hook request
   * @return               bool   Returns boolean value when called from the parent contract;
   */
  function _rewardContributor (
    Data storage self,
    string _username,
    string _rewardType,
    string _reservedType,
    uint _rewardBonus,
    string _deliveryID
  ) internal returns (bool) {
    // Calculate total reward value for contribution event
    uint _value = self.rewardValues[_rewardType].add(_rewardBonus);

    // Calculate reserved value for contribution event
    uint _reservedValue = self.reservedValues[_rewardType][_reservedType];

    // Get the contributor Ethereum address from GitHub username
    address _contributor = self.contributorAddresses[_username];

    // If no value is created, then throw the transaction;
    if(_value == 0 && _reservedValue == 0){
      throw;
      // If the GitHub web hook event ID has already occured, then throw the transaction;
    } else if(self.receivedDelivery[_deliveryID] == true) {
      throw;
    } else {
      // Update totalSupply with the added values created, including the reserved supply for auction;
      self.totalSupply = self.totalSupply.add(_value).add(_reservedValue);

      // Add to the balance of reserved tokens held for auction by the contract
      self.balances[address(this)] = self.balances[address(this)].add(_reservedValue);

      // If the contributor is not yet verified, increase the unclaimed rewards for the user until the user verifies herself/himself;
      if (_contributor == 0x0){
        self.unclaimedRewards[_username] = self.unclaimedRewards[_username].add(_value);
      } else {
        // If the contributor's address is set, update the contributor's balance;
        self.balances[_contributor] = self.balances[_contributor].add(_value);
      }

      // Set the received deliveries for this event to true to prevent/mitigate event replay attacks;
      self.receivedDelivery[_deliveryID] = true;

      // Return true to parent contract
      return true;
    }
  }
```

The amount of tokens issued and distributed corresponds to the type of contribution made to the GitHub repository. The following table provides the default token reward values that are associated with each GitHub web hook event.

NOTE: The following reward values are subject to change; We are still modeling token distributions and running [simulated and live tests](https://gittoken.org/). Please refer to the [GitToken.sol contract](https://github.com/git-token/contracts/blob/master/contracts/GitToken.sol#L38) for the current default values.



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

### Reward Bonuses

#### Go passed the default values

### Reserved Supply Values

#### Auction

### Utility & Scarcity of GitTokens


_DISCLAIMER: This article does not constitute investment advise, a solicitation or offer to
buy or sell any financial security or instrument._
