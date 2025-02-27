# Git & Version Control Reference Guide

## Table of Contents
1. [Configuring Git](#configuring-git)
2. [Creating and Cloning Repositories](#creating-and-cloning-repositories)
3. [Staging and Committing](#staging-and-committing)
4. [Branching](#branching)
    - [Switching Branches](#switching-branches)
    - [Creating and Switching](#creating-and-switching)
    - [Key Differences](#key-differences)
5. [Merging Branches](#merging-branches)
6. [Viewing History and Status](#viewing-history-and-status)
7. [Pushing and Pulling](#pushing-and-pulling)
    - [Understanding `origin` and `-u`](#understanding-origin-and--u)
    - [Push Commands](#push-commands)
    - [Pull Commands](#pull-commands)
    - [Push and Pull Scenarios](#push-and-pull-scenarios)
8. [Resolving Merge Conflicts](#resolving-merge-conflicts)
9. [Undoing Changes](#undoing-changes)
    - [Undo Last Commit but Keep Changes](#undo-last-commit-but-keep-changes)
    - [Undo Last Push and Commit](#undo-last-push-and-commit)
    - [Remove Commit Entirely](#remove-commit-entirely)
10. [Cherry-Pick Commands](#cherry-pick-commands)
11. [Fetch and Merge Remote Changes](#fetch-and-merge-remote-changes)
12. [Additional Useful Commands](#additional-useful-commands)
    - [Stashing Changes](#stashing-changes)
    - [Viewing Differences](#viewing-differences)
    - [Resetting Files](#resetting-files)
    - [Tagging](#tagging)
    - [Working with Remotes](#working-with-remotes)
    - [Rebasing](#rebasing)

---

## Configuring Git

Set up your user information for all local repositories.

- **Set User Name**
    ```sh
    git config --global user.name "Your Name"
    ```
    Sets the username for your commits.

- **Set User Email**
    ```sh
    git config --global user.email "your.email@example.com"
    ```
    Sets the email for your commits.

---

## Creating and Cloning Repositories

- **Initialize a New Repository**
    ```sh
    git init
    ```
    Initializes a new Git repository in the current directory.

- **Clone an Existing Repository**
    ```sh
    git clone <repository-url>
    ```
    Clones an existing repository from a remote source.

---

## Staging and Committing

- **Add File(s) to Staging Area**
    ```sh
    git add <file>
    ```
    Adds a file to the staging area.

- **Add All Changes**
    ```sh
    git add .
    ```
    Adds all changes in the current directory to the staging area.

- **Commit Changes**
    ```sh
    git commit -m "commit message"
    ```
    Commits the staged changes with a message.

---

## Branching

Branches allow you to work on different versions of your code simultaneously.

- **List Branches**
    ```sh
    git branch
    ```
    Lists all local branches.

- **Create a New Branch**
    ```sh
    git branch <branch-name>
    ```
    Creates a new branch called `<branch-name>`.

### Switching Branches

- **Switch to an Existing Branch**
    ```sh
    git checkout <branch-name>
    ```
    Switches to an existing branch named `<branch-name>`.

    - **Example**:
        ```sh
        git checkout main
        ```
        Switches to the `main` branch.

### Creating and Switching

- **Create and Switch to a New Branch**
    ```sh
    git checkout -b <branch-name>
    ```
    Creates a new branch and switches to it.

    - **Example**:
        ```sh
        git checkout -b feature-branch
        ```
        Creates and switches to `feature-branch`.

### Key Differences

- **`git checkout <branch-name>`**
    - **Purpose**: Switch to an existing branch.
    - **Example**: `git checkout main`

- **`git checkout -b <branch-name>`**
    - **Purpose**: Create a new branch and switch to it.
    - **Example**: `git checkout -b hotfix`

---

## Merging Branches

Integrate changes from one branch into another.

- **Merge a Branch into Current Branch**
    ```sh
    git merge <branch-name>
    ```
    Merges `<branch-name>` into the current branch.

    - **Example**:
        ```sh
        git merge feature-branch
        ```
        Merges `feature-branch` into the current branch.

---

## Viewing History and Status

- **Show Commit History**
    ```sh
    git log
    ```
    Displays the commit history for the current branch.

- **Show Commit History (One Line)**
    ```sh
    git log --oneline
    ```
    Displays a simplified commit history.

- **Show Current Status**
    ```sh
    git status
    ```
    Shows the status of changes as untracked, modified, or staged.

---

## Pushing and Pulling

Interact with remote repositories to share changes.

### Understanding `origin` and `-u`

- **`origin`**
    - The default name of the remote repository when you clone or add a remote.
    - Refers to the URL of the remote repository.

- **`-u` or `--set-upstream`**
    - Sets the upstream (tracking) reference for the current branch.
    - Simplifies future `git push` and `git pull` commands by linking local and remote branches.

### Push Commands

- **Push Changes to Remote Repository**

    - **Push and Set Upstream**
        ```sh
        git push -u origin <branch-name>
        ```
        Sets the upstream reference and pushes changes to the remote branch.

        - **Explanation**:
            - After this command, you can simply use `git push` for subsequent pushes.

    - **Push to Specified Remote and Branch**
        ```sh
        git push origin <branch-name>
        ```
        Pushes changes to the specified branch on the remote named `origin`.

    - **Push to Upstream Branch**
        ```sh
        git push
        ```
        Pushes changes to the upstream branch set for the current local branch.

### Pull Commands

- **Pull Changes from Remote Repository**

    - **Pull from Upstream**
        ```sh
        git pull
        ```
        Pulls changes from the upstream tracking branch.

    - **Pull from Specified Remote and Branch**
        ```sh
        git pull origin <branch-name>
        ```
        Fetches and merges changes from `origin/<branch-name>` into the current branch.

### Push and Pull Scenarios

#### Initial Push and Set Upstream

- **Command**
    ```sh
    git push -u origin main
    ```
- **What Happens**
    - Pushes the local `main` branch to `origin/main`.
    - Sets `origin/main` as the upstream for `main`.
    - Future pushes can be done with `git push`.

#### Subsequent Pushes

- **Command**
    ```sh
    git push
    ```
- **What Happens**
    - Pushes changes to the upstream branch (`origin/main`).

#### Pulling Changes

- **Command**
    ```sh
    git pull
    ```
- **What Happens**
    - Pulls changes from the upstream branch (`origin/main`) into the current branch.

---

## Resolving Merge Conflicts

Handle situations where Git cannot automatically merge changes.

- **Attempt to Merge**
    ```sh
    git merge <branch-name>
    ```
    Merges `<branch-name>` into the current branch.

- **If Conflicts Occur**
    1. **Identify Conflicts**
        - Conflicted files are marked in the output and in the files themselves.

    2. **Resolve Conflicts**
        - Edit the files to resolve differences.
        - Look for `<<<<<<<`, `=======`, `>>>>>>>` markers.

    3. **Add Resolved Files**
        ```sh
        git add <file>
        ```
        Stages the resolved files.

    4. **Commit the Merge**
        ```sh
        git commit -m "Merge branch 'branch-name' resolving conflicts"
        ```
        Finalizes the merge after resolving conflicts.

---

## Undoing Changes

Correct mistakes by undoing commits and changes.

### Undo Last Commit but Keep Changes

- **Soft Reset**
    ```sh
    git reset --soft HEAD~1
    ```
    Moves the HEAD pointer to the previous commit but keeps changes staged.

### Undo Last Push and Commit

- **Revert Last Commit**
    ```sh
    git revert HEAD
    git push origin <branch-name>
    ```
    Creates a new commit that undoes the changes of the last commit and pushes it.

### Remove Commit Entirely

- **Hard Reset and Force Push**
    ```sh
    git reset --hard HEAD~1
    git push origin <branch-name> --force
    ```
    - **Warning**: This rewrites history and can cause issues for others collaborating on the repository.

---

## Cherry-Pick Commands

Apply specific commits from one branch to another.

- **Apply a Specific Commit**
    ```sh
    git cherry-pick <commit-hash>
    ```
    - **Example**:
        ```sh
        git cherry-pick a1b2c3d4
        ```
    - Applies the commit with hash `a1b2c3d4` to the current branch.

- **Steps**
    1. **Find the Commit Hash**
        - Use `git log` to locate the commit.

    2. **Execute Cherry-Pick**
        - Run `git cherry-pick <commit-hash>`.

---

## Fetch and Merge Remote Changes

Retrieve updates from the remote without merging immediately.

- **Fetch from Remote**
    ```sh
    git fetch origin
    ```
    Retrieves updates from `origin`.

- **Merge Fetched Changes**
    ```sh
    git merge origin/<branch-name>
    ```
    Merges fetched changes into the current branch.

- **Alternatively, Pull Directly**
    ```sh
    git pull origin <branch-name>
    ```
    Fetches and merges in one command.

---

## Additional Useful Commands

### Stashing Changes

Save changes temporarily without committing.

- **Stash Uncommitted Changes**
    ```sh
    git stash
    ```

- **List Stashes**
    ```sh
    git stash list
    ```

- **Apply Stashed Changes**
    ```sh
    git stash apply
    ```

- **Apply and Remove Stashed Changes**
    ```sh
    git stash pop
    ```
    Applies the stash and removes it from the stash list.

### Viewing Differences

- **Show Differences Between Working Directory and Staging Area**
    ```sh
    git diff
    ```

- **Show Differences Between Staging Area and Last Commit**
    ```sh
    git diff --staged
    ```

- **Show Differences Between Two Commits**
    ```sh
    git diff <commit1> <commit2>
    ```

### Resetting Files

- **Unstage a File**
    ```sh
    git reset <file>
    ```
    Removes the file from staging but leaves the working directory unchanged.

- **Discard Changes in Working Directory**
    ```sh
    git checkout -- <file>
    ```
    Reverts the file in the working directory to match the last commit.

### Tagging

Create markers for specific points in your commit history.

- **Create an Annotated Tag**
    ```sh
    git tag -a v1.0 -m "Version 1.0"
    ```

- **List Tags**
    ```sh
    git tag
    ```

- **Push Tags to Remote**
    ```sh
    git push origin --tags
    ```

### Working with Remotes

Manage remote repositories.

- **List Remote Repositories**
    ```sh
    git remote -v
    ```

- **Add a New Remote**
    ```sh
    git remote add <name> <url>
    ```
    - **Example**:
        ```sh
        git remote add upstream https://github.com/other/repo.git
        ```

- **Remove a Remote**
    ```sh
    git remote remove <name>
    ```
    - **Example**:
        ```sh
        git remote remove upstream
        ```

### Rebasing

Integrate changes by moving a sequence of commits to a new base commit.

- **Rebase Current Branch onto Another**
    ```sh
    git rebase <branch-name>
    ```
    Applies commits of current branch onto `<branch-name>`.

- **Interactive Rebase**
    ```sh
    git rebase -i HEAD~n
    ```
    Allows you to edit, squash, or reorder commits.

    - **Example**:
        ```sh
        git rebase -i HEAD~3
        ```
        Interactively rebases the last 3 commits.

---

Feel free to use this guide to assist you as you delve deeper into Git and version control. By practicing these commands and understanding their effects, you'll become proficient in managing your code and collaborating effectively.

Let me know if you need further explanations on any of these commands or concepts!
