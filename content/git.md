# **git Cheat Sheet** 

# Table of Contents

- [Basics](#basics)
  - [Clone a remote repository](#clone-a-remote-repository)
  - [Create a new branch](#create-a-new-branch)
    - [Option 1 (old)](#option-1-old)
    - [Option 2 (new)](#option-2-new)
  - [Push the new branch to the remote](#push-the-new-branch-to-the-remote)
  - [Fetch the latest changes](#fetch-the-latest-changes)
    - [git fetch --all](#git-fetch-all)
    - [git fetch origin](#git-fetch-origin)
  - [Pull remote changes](#pull-remote-changes)
  - [Publish remote repository (from local)](#publish-remote-repository-from-local)

- [Typical Workflow](#typical-workflow)
  - [1. Commit or stash your local changes](#1-commit-or-stash-your-local-changes)
  - [2. Fetch the latest changes](#2-fetch-the-latest-changes)
  - [3.Synchronize with the remote branch](#3-synchronize-with-the-remote-branch)

- [Branches](#branches)
  - [View both local and remote branches](#view-both-local-and-remote-branches)
  - [To see a list of all remote branches](#to-see-a-list-of-all-remote-branches)
  - [To see a list of all local branches](#to-see-a-list-of-all-local-branches)
  - [Delete a branch](#delete-a-branch)
  - [Force delete it](#force-delete-it)
  - [Switch to a different branch](#switch-to-a-different-branch)

- [Advanced](#advanced)
  - [Remove the folder from tracking](#remove-the-folder-from-tracking)
  - [Untrack files and delete them from the remote repository](#untrack-files-and-delete-them-from-the-remote-repository)
  - [Match ALL local branches with remote (Hard-match)](#match-all-local-branches-with-remote-hard-match)
  - [Match ALL local branches with remote (Soft-match / rebase)](#match-all-local-branches-with-remote-soft-match--rebase)
  - [Cleaning up local branches that no longer exist on the remote repository](#cleaning-up-local-branches-that-no-longer-exist-on-the-remote-repository)
  - [Undo the most recent commit](#undo-the-most-recent-commit)

<br>

# Basics

## **Clone a remote repository**

```bash
cd /path/to/your/directory
git clone https://github.com/Locaria/CAP_Quality_Checks.git
```

## **Create a new branch**

### Option 1 (old)

```bash
git checkout -b <new_branch_name> origin/<remote_branch_name>
```
*Replace <new_branch_name> with the name of the new branch you want to create, and <remote_branch_name> with the name of the remote branch (like main or develop).*

For example, if you want to create a branch called feature/new-feature based on origin/main:

```bash
git checkout -b feature/new-feature origin/main
```

### Option 2 (new)

```bash
git switch -c <new_branch_name>
```

## **Push the new branch to the remote** if you want to track it

```bash
git push -u origin <new_branch_name>
```
*This sets up tracking for the branch so future git pull and git push commands automatically use the correct remote branch.*

## Fetch the latest changes

### **`git fetch --all`**

Fetches changes from **all remotes** in your repository.
```bash
git fetch --all
```
*If your repository has multiple remotes (e.g., origin, upstream, or others), it updates all of them.
Commonly used when working with forks or multiple remote repositories.*

### **`git fetch origin`**

Fetches changes only from the **origin remote**.
```bash
git fetch origin
```
*This is the most common scenario when you have just one remote repository (origin is usually the default remote name).*

## Pull remote changes
To pull the latest changes from the remote repository:
```bash
git pull
```
*By default, this command pulls from the branch you’re currently on and updates your local branch with the changes from the remote.*

## Publish remote repository (from local)

```bash
git remote add origin https://github.com/example/repository.git
git push -u origin main
```
<br>

# Typical Workflow
## **1. Commit or stash your local changes**
### **Option 1: Commit your changes**
#### Stage your changes
```bash
git add .
```

#### Commit your changes:
```bash
git commit -m "Your commit message"
```

### **Option 2: Stash your changes (if you’re not ready to commit)**
#### Stash your changes to temporarily save them:
```bash
git stash
```

## **2. Fetch the latest changes**
### Fetch the latest updates from the remote repository
```bash
git fetch origin
```

## **3. Synchronize with the remote branch**

### **Option 1: Push the changes to remote**
#### Merge the remote branch into your local branch
```bash
git push origin <branch_name>
```
*Push your local commits to the remote repository.*

### **Option 2: Merge the changes**
#### Merge the remote branch into your local branch
```bash
git merge origin/<updates/branch_name>
```
*This will merge your teammate's changes into your local branch and create a merge commit if there are new commits.*

### **Option 3: Rebase (if you want a linear history)**
#### Rebase your branch to include your teammate's changes
```bash
git checkout branch_name
git rebase main
```
*Rebase takes all the commits from example-branch that are not in main, temporarily removes them, updates feature-branch to match main, and then re-applies your commits on top of the updated main. This can help resolve commit issues, if remote and local are not in synchronization. During rebasing, if there are conflicts, Git will pause the process and you’ll need to resolve the conflicts manually.*

<br>

# **Branches**

### View both local and remote branches

```bash
git branch -a
```

### To see a list of all remote branches

```bash
git branch -r
```

### To see a list of all local branches

```bash
git branch
```

### Delete a branch

```bash
git branch -d <branch_name>
```

### Force delete it

```bash
git branch -D <branch_name>
```
*Useful command to delete all local branches that are not in remote (GitHub) anymore. Handle with care though, it will also delete any local changes you made in those branches, if any.*

### Switch to a different branch

```bash
git switch <branch_name>
```
<br>

# Advanced

<aside>
💡

[Useful Git / GitHub commands](https://docs.google.com/document/d/1j3SHgeLiR6kdmNSvS5QN-MYl86JTX0TT3spQ8uzJVO0/edit?tab=t.0#heading=h.cxuw0rkpsl3e)

</aside>

### Remove the folder from tracking
```bash
git rm -r --cached <folder_name/>
```
*Even if you add it to .gitignore, Git will still track the folder if it was committed before. To untrack it run the previous command*

### Untrack files and delete them from the remote repository
```bash
find . -name EXAMPLE.FILE -print0 | xargs -0 git rm -f --ignore-unmatch
git add .gitignore
git commit -m "Remove EXAMPLE.FILE files and update .gitignore"
git push
```
*Useful if we accidentally pushed and synched a file we don’t want to sync. Make sure to add the respective file to .gitignore after untracking or it will be tracked again on the next add+commit.*

### Match ALL local branches with remote **(Hard-match)**
```bash
for branch in $(git branch -r | grep -v '\->'); do
    git branch --track "${branch#origin/}" "$branch" 2>/dev/null || true
    git checkout "${branch#origin/}"
    git reset --hard "$branch"
done
```
*This script ensures that all local branches are synchronized with the remote branches by resetting them to the remote state. Be careful, this will discard all local changes that have not yet been pushed to remote.*

### Match ALL local branches with remote **(Soft-match / rebase)**
```bash
for branch in $(git branch -r | grep -v '\->'); do
    git branch --track "${branch#origin/}" "$branch" 2>/dev/null || true
    git checkout "${branch#origin/}"
    git pull --rebase
done
```
*This script tracks all remote branches by creating corresponding local branches. For each branch, it switches to the local version and then pulls changes from the remote branch using rebase, which integrates the latest remote changes while preserving local commits.*

### Cleaning up local branches that no longer exist on the remote repository
```bash
git fetch --all --prune
for branch in $(git branch | sed 's/^..//'); do
    if ! git show-ref --verify --quiet refs/remotes/origin/$branch; then
        echo "Deleting local branch $branch as it no longer exists on remote"
        git branch -D $branch
    fi
done
```
*Handle with care, as this will delete all your local branches and changes that don’t exist on remote. Make sure to save your existing work before running this.*

### Undo the most recent commit
```bash
git reset HEAD~1
```
*It moves the changes from the most recent commit back to the staging area without deleting them. This allows you to modify or recommit the changes as needed. You can modify the number in git reset HEAD~<number> to go back multiple commits.*
