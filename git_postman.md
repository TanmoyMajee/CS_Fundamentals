# Comprehensive Git and GitHub Interview Notes

## Introduction to Git and GitHub

### What is Git?
- **Git** is a distributed version control system that helps you track changes in your code and collaborate with others.

### What is GitHub?
- **GitHub** is a web-based platform that uses Git for version control and provides a graphical interface and collaboration features.

### Difference Between Git and GitHub
- **Git** is the version control tool you use locally, while **GitHub** is an online service for hosting and managing Git repositories.

## Core Git Concepts

### Branching
- Branching is essentially creating a copy of the main project. This lets you work on new features or bug fixes separately without altering the main codebase. Once you're done, you can merge your branch back into the main branch using a git merge. This keeps the development organized and the main codebase stable

### Merging
- **Merging** is the process of combining changes from one branch into another. It often creates a merge commit to tie the histories together.

### Merge Conflicts
- **Merge conflicts** occur when changes in different branches conflict with each other. You must manually resolve these conflicts by editing the files and choosing which changes to keep before completing the merge.

### Resolving Conflicts
- When a merge conflict happens, Git will mark the conflicting areas in the affected files. You resolve the conflicts by editing those files, removing the conflict markers, and deciding which changes to keep.

### Creating a Repository
- **Initialization**: Use `git init` to create a new Git repository in your current directory.
- **Cloning**: Use `git clone <repository-url>` to create a local copy of an existing repository.

### Basic Branching Commands
- Creating a Branch: git branch <branch-name>
- Switching Branches: git checkout <branch-name> or git switch <branch-name> (modern usage).
- Creating and Switching to a New Branch: git checkout -b <branch-name> or git switch -c <branch-name>.

### Merging Branches
-Merging a Branch: First, switch to the branch you want to merge into (e.g., git checkout main), then run git merge <branch-name> to merge the changes.



| Feature        | `git reset`                                                          | `git revert`                                                             |
| -------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Purpose        | Moves the branch pointer to a previous commit and can alter history. | Creates a new commit that undoes changes from a previous commit.         |
| History Impact | Can rewrite commit history (especially with `--hard`).               | Preserves the history by adding a new commit.                            |
| Use Case       | Useful for undoing changes in a private branch.                      | Useful for undoing changes in a public branch without rewriting history. |


## Git Commands for Undoing Changes

### `git reset` vs. `git revert`
- **`git reset`**: Moves the branch pointer to a previous commit and can modify the working directory and staging area.
  - **Soft Reset**: Keeps changes in the staging area.
  - **Hard Reset**: Discards all changes after the specified commit.

- **`git revert`**: Creates a new commit that undoes the changes introduced by a previous commit without altering the existing commit history.

## Fetching and Pulling Changes

### `git fetch` vs. `git pull`
- **`git fetch`**: Downloads changes from the remote repository but does not merge them into your local branch.
- **`git pull`**: Fetches changes from the remote repository and automatically merges them into your current branch.

## Additional Important Topics

### `.gitignore`
- **Purpose**: Specify files and directories that should not be tracked by Git, such as build artifacts or sensitive information.

### Branching Strategies
- **Git Flow**: A branching model using feature branches and multiple long-lived branches like `master` and `develop`.
- **GitHub Flow**: A simpler model with a main branch and short-lived feature branches that are merged frequently.

### `git rebase`
- **Purpose**: Reapply commits from one branch onto another base, creating a linear history.

### `git stash`
- **Purpose**: Temporarily save uncommitted changes so you can work on something else and reapply them later.
### `git cherry-pick` (continued)
- **Use Case**: Useful when you want to bring a single commit from another branch into your current branch without merging the entire branch.

### `git reset`, `git revert`, and Undoing Changes (continued)
- **Detailed Example**:
  - `git reset --soft <commit>`: Moves the branch pointer and keeps changes in the staging area.
  - `git reset --hard <commit>`: Moves the branch pointer and discards all changes after that commit.
  - `git revert <commit>`: Creates a new commit that undoes the specified commit.

### Resolving Merge Conflicts (continued)
- **Steps**:
  1. Identify conflicted files (Git will mark them).
  2. Edit the files to resolve conflicts.
  3. Remove conflict markers and decide on the final code.
  4. Commit the resolved files.

### Working with Remotes
- **Adding a Remote**: `git remote add origin <url>`
- **Pushing Changes**: `git push origin <branch>` to push changes to a remote branch.
- **Pulling Changes**: `git pull` to fetch and merge changes from the remote.





# Postman Quick Notes

## What is Postman?
Postman is a popular tool for API testing. It allows you to send API requests and see the responses, which helps in debugging and testing your endpoints.

## How to Send a Basic Request
1. Open Postman.
2. Select the HTTP method (e.g., GET, POST).
3. Enter the API endpoint URL.
4. Click the "Send" button to see the response.

## Organizing Requests
- You can organize your API requests into collections and folders to keep them neat and run related requests together easily.

## Using Environment Variables
- Postman allows you to create environment variables so you can switch between different setups (like dev, staging, and production) without manually changing URLs.

## Testing and Validating Responses
- You can write tests in Postman using JavaScript to automatically check response status codes, response times, or other conditions.

---

