Git Commands
============

- [Git Commands](#git-commands)
  * [Basic commands in Bash](#basic-commands-in-bash)
  * [SSH Config](#ssh-config)
  * [Configure Git](#configure-git)
  * [Getting & Creating Projects](#getting---creating-projects)
  * [Basic Snapshotting](#basic-snapshotting)
  * [Commit Status](#commit-status)
  * [Playing With Commits](#playing-with-commits)
  * [Branching](#branching)
  * [Merging](#merging)
  * [Rebase](#rebase)
  * [Sharing & Updating Projects](#sharing---updating-projects)

_A list of my commonly used Git commands -- Ronny_


## Basic commands in Bash

| Command | Description |
| ------- | ----------- |
| `ls` |list files or folders in a directory|
| `ls -al` |list all files or folders in a directory including hidden|
| `mv [target_name] [new_name]` |Rename target file or folder|


## SSH Config

| Command | Description |
| ------- | ----------- |
| `ssh-keygen -t rsa -b 4096 -C "Comment"` |Create ssh key pair|
| `eval "$(ssh-agent -s)"` |start the ssh-agent|
| `ssh-add ~/.ssh/id_rsa` |Add id_rsa key|


## Configure Git

| Command | Description |
| ------- | ----------- |
| `git config --global alias.<any_name> 'git command'` |Set a global git alias|
| `git config --global --list` |list all Git config|
| `git config --global user.name "user_name"` |Set user_name globally in pc |
| `git config --global user.email "email"` |Set email globally in pc |
| `git config --global diff.tool p4merge` |set default diff.tool as p4merge|
| `git config --global difftool.p4merge.path "path.exe` |add p4merge path|
| `git config --global difftool.prompt false` |prevent confirmation to open p4merge|
| `git config --global merge.tool p4merge` |set default merge.tool as p4merge|
| `git config --global mergetool.p4merge.path "path.exe` |add p4merge path|
| `git config --global mergetool.prompt false` |prevent confirmation to open p4merge|


## Getting & Creating Projects

| Command | Description |
| ------- | ----------- |
| `git init` | Initialize a local Git repository |
| `rm -rf .git` | Uninitialize a local Git repository |
| `git clone ssh://git@github.com/[username]/[repository-name].git` | Clone a remote repository |
| `git remote add origin git@github.com/[username]/[repository-name].git` |add a remote repository|
| `git push -u origin master` |push project to new remote repository|

## Basic Snapshotting

| Command | Description |
| ------- | ----------- |
| `git status` | Check status |
| `git add [file-name.txt]` | Add a file to the staging area |
| `git add .` | Add all new and changed files to the staging area |
| `git commit -m "[commit message]"` | Commit changes |
| `git commit -am "[commit message]"` | Express Commit changes |
| `git pull origin [active-branch-name]` |Updates you with remote repository|
| `git push origin [active-branch-name]` |Push all changes to remote repository |
| `git rm -r [file-name.txt]` | Remove a file (or folder) |

## Commit Status

| Command | Description |
| ------- | ----------- |
| `git log --oneline` |Shows commits in the active branch|
| `git log --oneline --graph --decorate --all` |Shows all branches in tree structure|
| `git log --diff-filter=D --summary` |see log of deleted files|
| `git checkout [commit_id]~1 [fileName]` |Pull only the file changes, under that commit|
| `git checkout [commit_id]` |go back to the commit id|
| `git revert HEAD` |revert a commit|
| `git reset` |removes all files from staging area|
| `git reset --hard` |removes all files from staging area and undo modification|
| `git reset [commit_id]` |reset the repository to commit_id still files will be in modified state|
| `git reset [commit_id] --hard` |reset the repository to commit_id & no files will be in modified state|


## Playing With Commits
| Command | Description |
| ------- | ----------- |
| `git commit --amend` |Change last commit to an entirely new commit|
| `git commit --amend --no-edit` |Change last commit to an entirely new commit without changing commit message|




## Branching

| Command | Description |
| ------- | ----------- |
| `git branch` | List branches (the asterisk denotes the current branch) |
| `git branch -a` | List all branches (local and remote) |
| `git branch [branch name]` | Create a new branch |
| `git branch -d [branch name]` | Delete a branch |
| `git push origin --delete [branch name]` | Delete a remote branch |
| `git checkout -b [branch name]` | Create a new branch and switch to it |
| `git checkout -b [branch name] origin/[branch name]` | Clone a remote branch and switch to it |
| `git checkout [branch name]` | Switch to a branch |
| `git checkout -` | Switch to the branch last checked out |
| `git checkout -- [file-name.txt]` | Discard changes to a file |



## Merging

| Command | Description |
| ------- | ----------- |
| `git diff` | Preview all changes before staging |
| `git diff [source branch] [target branch]` | Preview changes before merging |
| `git merge [branch name]` | Merge a branch into the active branch ff-merge |
| `git merge [branch name] --no-ff` | Merge a branch into the active branch noff-merge |
| `git merge [branch name] -m "message"` | Merge a branch into the active branch 3way-merge |
| `git merge [source branch] [target branch]` | Merge a branch into a target branch |
| `git mergetool` | Merge conflict branches |


## Rebase 

| Command | Description |
| ------- | ----------- |
| `git rebase [branch_name]` | Preview changes before merging |


## Sharing & Updating Projects

| Command | Description |
| ------- | ----------- |
| `git push origin [branch name]` | Push a branch to your remote repository |
| `git push -u origin [branch name]` | Push changes to remote repository (and remember the branch) |
| `git push` | Push changes to remote repository (remembered branch) |
| `git push origin --delete [branch name]` | Delete a remote branch |
| `git pull` | Update local repository to the newest commit |
| `git pull origin [branch name]` | Pull changes from remote repository |
| `git remote add origin ssh://git@github.com/[username]/[repository-name].git` | Add a remote repository |
| `git remote set-url origin ssh://git@github.com/[username]/[repository-name].git` | Set a github repository's origin branch to SSH |
| `git remote set-url origin ssh://git@bitbucket.org/[username]/[repository-name].git` | Set a bitbucket repository's origin branch to SSH |

