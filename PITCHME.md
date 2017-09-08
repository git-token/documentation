## What Are Git Best Practices

Git and GitHub allow teams to work independently yet collaboratively on software projects by allowing any number of contributors with their own local copies of the codebase to seamlessly manage a centralized remote repository with any number of branches. While this approach to software development is powerful, a tool is only as good as its users. With that in mind, let’s walk through the basics of what best practices for a ‘feature branch’ style workflow looks like on GitHub.

### 1) Make a New Branch

Branching is a way to save you from having to repair terrible, terrible application breaking changes you put into production at 1:00am on a Friday night. To that end, every repository should have at least two branches - a development branch and a master branch. These branches should be developed in parallel, with commits always being made to the development branch before being passed on to master.

In the case of a feature branch workflow, branches are also a way to manage individual project features. Any feature on your project roadmap can and should be made into its own branch that in turn can be pushed into the development-master workflow after the feature is complete. This makes it easier to keep track of specific project milestones and track bugs.

### 2) Commit Early and Often

Once you’ve created a branch it’s time to start using it. While adding commits early you can ensure that project contributors are up to date on its current state, it also forces you to keep your commits small, making it easier to pinpoint errors, fix merge conflicts, and [modularize your code](https://builttoadapt.io/standing-on-the-shoulders-of-giants-59762fb0c155). 

Note that committing early and often does not mean committing unfinished or untested code, the goal is to make your software better not worse. In addition, make sure to leave clear and concise comments on any commit you make. [This guide](https://chris.beams.io/posts/git-commit/) to commit messages is a good starting point.

### 3) Make a Pull Request

Pull requests are a way for you to discuss (and adjust) your proposed changes alongside other repository members. Not every pull request has to be ready for production, in fact pull requests can be a helpful way to get ideas or get advice when you’re stuck. 

### 4) Deploy and Merge

Once everyone is satisfied with the state of the pull request, changes should be deployed into the development branch in order to verify that they actually work as intended. If like most first attempts your branch causes issues you can simply roll back to the existing master branch.

After confirming that everything is working as intended the new code can be merged into the existing master branch.

That’s it! You should now understand the basics of one of the most popular GitHub workflows. 

## Other Considerations 

* **Tagging.** Git allows you to create ‘checkpoints’ in the history of your repository through history as important. In the feature branch workflow you should typically tag any completed feature branch.
* **Issues.** Issues are a way to track tasks, features, or bugs.Creating and managing issues on GitHub is an art. Fortunately there are some great tools like [git-labelmaker](https://github.com/himynameisdave/git-labelmaker) that can help you become a better artist. If you find that your issues are still disorganized, creating milestones is a great way to group them together under a specific project, feature, or time period (e.g. August Sprint).
* **Project Cards.** Project cards are another way to organize your issues, pull requests, and notes. When an issue is closed or a pull request is merged, the associated card will automatically be crossed out. While project cards aren’t necessary, they can help you keep better track of your long term roadmap if you’re not already using alternatives like Trello.

## Other GitHub Tools

* GitHub marketplace
* Continuous integration tools like Travis CI

## How GitToken Incentivizes Git and Software Development Best Practices

GitToken’s goal is not just to help reward developers for their work, but to help enforce Git workflow best practices like the ones listed above. GitToken encourages developers to commit early and often, and in general to communicate and coordinate with their teammates by taking action on their organization's GitHub repository, whether that means creating new branches, opening new issues and pull requests, or merging their code. 

We hope that GitToken can be one piece of a much larger community effort to make participation in the process of development collaborative and rewarding.
