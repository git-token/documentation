### Event Types & Reward Values

The following events are monitored by the GitHub webhook. These events can be filtered on the webhook setting page for submitting POST requests to the endpoint GitToken service.


Additionally, the following reward values can be set by the contract owner by submitting a signed transaction to the `setRewardValue` method of the contract.

```solidity
/**
 * @dev GitToken contract owner(s) may set individual reward values for
 * contribution events
 * @param _rewardValue uint256 Value of reward type
 * @param _rewardType string Name of reward type
 * @returns success bool Returns true after successfully updating the
 * contract
 */
function setRewardValue(
  uint256 _rewardValue,
  string _rewardType
) onlyOwner public returns (bool success) {
  gittoken.rewardValues[_rewardType] = _rewardValue;
  return true;
}
```

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
