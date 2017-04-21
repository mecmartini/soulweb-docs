# Develpment Workflow

In this section is described the development workflow to adopt to handle the parallel development on our projects.

## Gitflow

The [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) defines a strict branching model designed around the project release. It's based on [Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow).

The core idea behind this workflow is that all feature development should take place in a dedicated branch. This encapsulation makes it easy for multiple developers to work on a particular feature without disturbing the main codebase.

The central repository will have two main branches:

* `master` branch stores the official release history
* `develop` branch serves as an integration branch for features during development

### 1. Begin start to developing a new feature

Before to start developing a feature, you need an isolated branch to work on. You can request a new branch with the following command:

    git checkout -b issue-#1060 develop

This checks out a branch called `issue-#1060` based on `develop`, and the -b flag tells `Git` to create the branch if it doesn’t already exist.

Note that the feature branch should have descriptive names:

* `issue` the title of the issue on your ticketing system
* `#1060` the ID of the issue on your ticketing system

As example:

    git checkout -b animated-main-menu-#8091 develop

### 2. Developing a feature

When you feature branch is up you can develop your code and building up the feature with as many commits as necessary.

When you need to commit your code you must follow a safe sequence to be sure to merge it without breaking the code and your local `drupal` installation:

    # export drupal configuration
    drush cex -y
    # commit code
    git add ...
    git commit -m '...'
    # merge
    git pull
    # update dependencies
    composer install
    # clear drush cache
    drush cc drush
    # update database
    drush updb -y --entity-updates
    # import drupal configuration
    drush cim -y
    # clear drupal cache
    drush cr
    # push code
    git push


### 3. Finish developing a feature

When the feature is complete you must file a pull request letting the rest of the team know your work is done and ready to be merged on `develop` branch.

[Pull requests](https://help.github.com/articles/about-pull-requests/) let you tell others about changes you've pushed. Once a pull request is opened, you can discuss and review the potential changes with collaborators and add follow-up commits before the changes are merged into the repository.

The pull request is managed through `github` UI. For more information, see [Creating a pull request](https://help.github.com/articles/creating-a-pull-request/).