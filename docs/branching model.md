# GitFlow branching model
<p>Our team will be using the GitFlow branching model for each repository. For more detailed view on the model you can scroll to the bottom.</p>

# Branches
## Permanent branches

<p>
Main - protected

Development - protected 
</p>

## Temporary branches

### Based on the branching model

* Feature - used to develop new features that branches off the development branch;

* Release - help prepare a new production release; (from the development branch and must be merged back to both development and main);

* Hotfix - hotfix branches arise from a bug that has been discovered and must be resolved as soon as possible. Most of the time it is "quick and dirty" and can cause technical debt.

### Specifics

* Bugfix - branches that get created from a bug that has been discovered and must be resolved.
UNLIKE the hotfix - it's not urgent. Most likely in development since it's not urgent. Branches off from development branch;

* Rework/Refactor - branches that get created because of technical debt;

* Test - branches for experimenting safely.

## Naming convention

All temporary branches will use the kebab-case format: `<category/description-in-kebab-case>`

**Example:**

`<feature/new-form>`

## Category

The category references the type of development that the developer is workin on in the branch. It is based on the temporary branches.

**Example:**

Developer wants to create second factor authentication - feature

Developer wants to fix a bug in production - hotfix

Developer wants to fix a bug in development - bugfix

## Merging and Pull Requests

Temporary branches will inevitably (unless cancelled) be merged in `development`. There MUST NOT be any direct merges to main from feature/bugfix/test etc. branches. There must be open an pull request for merging to development.

## Pull Requests

Pull requests are mandatory for each feature (and the rest of temporary branches) in order to be accepted in development. Pull requests can be approved, returned for changes, etc. One reviewer minimum per pull request. Each pull request must contain the following:

### Title
Short description of what is being introduced - 'Feature - Reading files'

### Description
Reference the issue that this pull request will close. Details that the reviewer should know (or should be added to the documentation) before testing.

Link any issues that the pull request will close, by using the closing tags e.g. Closes #1

Part for "What should be tested". Checkboxes with short description of the functionality that should be tested.

### Development
Refer to the issue that this branch will close or is working on.

### Reviewers
Add at least one team member if not specifically stated otherwise during meeting, stand-up, etc.

### Labels
Match all labels of the issue it references. Add additional ones if you think that are necessary (technical debt, help wanted, etc.)