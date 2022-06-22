# Git Introduction

What is Git? <br>
Free and open source version control system.

What is Version Control? <br>
The management of changes to documents, computer programs, large websites, and other collections of information.

What is Github? <br>
A website to host your repositories online.

# Terms

Directory -> Folder <br>
Terminal or Command Line -> Interface for Text Commands <br>
CLI -> Command Line Interface <br>
Code Editor -> Word Processor for Writing Code <br>
Repository -> Project, or the folder/place where your project is <br>

# Concepts
## Git Workflows
Work on Stuff -> Add Changes -> Commit <br>
Here, 'Work on Stuff' refers to making new files, editing files, deleting files  etc., 'Add Changes' refers to grouping specific changes together, in preparation  of committing, 'Commit' refers to committing everything that was previously added. This workflow involves two basic commands: `git add`, `git command`.<br>

Working Directory -> Staging Area -> Repository <br>
Here, every change we made will be reflected on the files in Working Directory.  However, unless we use `git add`, these changes will not be staged, i.e. enter  into the Staging Area. And furthermore, unless they are committed by using  command  `git commit`, they will be received into Repository. Every change that  is received in Repository exists in the form of Blobs, Trees, Commits Objects  inside folder of '.git', this enables us to retrieve any previously change.<br>

Note: Please try to keep each commit focused on a single thing.


# Commands

## Configuring Git

``` shell
# print current user name and email
git config user.name
git config user.email

# set the global user name and email
git config --global user.name "example"
git config --global user.email example@gmail.com

# configure the editor
git config --global core.editor "code --wait"
```

## Very Basics

Warning: Do not initiate a repo inside of a repo. Before running 'git init',  use 'git status' to verify that you are not currently inside a repo.<br>

If you happen to end up with nested repos, you can sort it out by deleting one of a doted folders (i.e. folder of .git)

Rule: make a folder for a project and then initialize git in that folder.

```shell
# Initialize the repo in the top-level folder containing your project, 
# creating a new git repository.
git init


# Print information on the current status of a git repository and its contents.
git status 

# Add specific files to the staging area
git add file1 file2 
git add . # stage all changes at once

# Commit with whatever changes have been staged
git commit -m "my message" 

# Note: commit message should summarize the changes included in that commit.

# If using VS code as the default editor, this will lead to edit message in VS code.
git commit 

# --amend will open the default editor, two reasons: redo the commit message, or add forgotten changes.
git commit --amend

# The following command do the git add and git commit all at once.
git add -A && git commit -m "Your Message"

# The following command do the git add and git commit all at once, provided that no new file was created.
git commit -am "You Message" 

# Print the log of the commits
git log
git log --abbrev-commit 
git log --oneline
```

## Ignore Certain Files
`.gitignore` creates a file called .gitignore in the root of a repository. 
Inside the file, we can write patterns to tell Git which files & folders to 
ignore.

```shell
# It's conventional to put .gitignore in the root of your project directory.
touch .gitignore 

# write in the .gitignore the files you want to ignore (see examples below), through editing in default editor.

.DS_Store
secrets.txt
mode_modules/
*.log

# .DS_Store will ignore files named .DS_Store
# folderName/ will ingore an entire directory
# *.log will ignore any files with the .log extension
```

## Create/Switch Branches
Note: Before switching between branches, add/commit changes or stash them. <br>
```shell
git branch # the * indicates the branch you are currently on
git branch <branch-name> # create a new branch based upon the current HEAD
git switch <branch-name> # switch to a branch
git checkout <branch-name> # switch to a branch
git switch -c <branch-name> # create a new branch and switch
git checkout -b <branch-name> # create a new branch and switch

git branch -d <branch-name> # delete a branch
git branch --delete <branch-name> # delete a branch
git branch -D <branch-name> # delete a branch by force: --delete --force

# When in list mode, show sha1 and commit subject line for each head, 
# along with relationship to upstream branch (if any)
git branch -v 


git -m <new-name> # move/rename the current branch
git --move <new-name> # same for -m
git --M <new-name> # shortcut for --move --force
```

## Merge Branches
Principles of Merging:
- We merge branches, not specific commits;
- We always merge to the current HEAD branch.

```shell
git switch master
# Incorporate changes from named branch into the current branch, in this case it's to incorporate <brach> to master
git merge <branch>

# --no-ff means not fast forward even when it can
git merge --no-ff <branch>

```

Sometimes, there are conflicts in the files of the commits which we aimed to  merge. To resolve conflicts in merging, we take the following steps:
- Open up the file(s) with merge conflicts;
- Edit the file(s) to remove the conflicts. Decide which branch’s content you want to keep in each conflict. Or keep the content from both;
- Remove the conflict “markers” in the document;
- Add your changes and then make a commit!

Git Diff: to view changes between commits, branches, files, our working directory, and more!

``` shell

# Without additional options, 'git diff' lists all the changes in our working directory that are NOT staged for the next commit. That is, it compares staging area and working directory.
git diff

# 'git diff HEAD' lists all changes  (staged and unstaged) in the working tree since your last commit.
git diff HEAD 

git diff HEAD HEAD~1
git diff HEAD~1 # Compare file A as HEAD~1 and file B as HEAD

# Suppose 15db960 is the reference of HEAD's parent commit,
# compare HEAD with HEAD~1
git diff HEAD..15db960 
git diff 15db960 # the same: compare file A 15db960 with file B as HEAD

# List only staged changes, i.e. changes between the staging area and our last commit.
git diff --staged
git diff --cached

# Compare different files rather than different versions of the same file
git diff HEAD [filename]
git diff HEAD [filename-1] [filename-2]
git diff --staged [filename]

# Compare branches: list the changes between the tips of branch1 and branch2
git diff branch1..branch2
git diff branch1 branch2

# Compare commits:
git diff commit1..commit2
```

## Git Stash
Sometimes, I have not decided whether to commit my changes, but I need to switch to another branch. Changes uncommitted, if return to previous head, may come  along with me to the destination branch. That is, I have work that is  uncommitted on my current branch, if Git does not let me switch or I do not want  those changes to come with us when switching to another branch, use stash to  pause to save my changes without actually committing them.

Git provides an easy way of stashing the uncommitted changes so that we can  return to them later, without having to make unnecessary commits.`git stash`  helps you save changes that you are not yet ready to commit. You can stash  changes and then come back to them later. Running `git stash` will take all  uncommitted changes (staged and unstaged) and stash them, reverting the  changes in your working copy. `git stash pop` remove the most recent thing from  the stash.

```shell
# Save working directory (i.e. unstaged) and staged (but not committed) changes.
git stash 

# Pop the most recent changes out and re-apply them to your working copy.
git stash pop

# Apply whatever is stashed away, without removing it from the stash. It can be useful if you want to apply stashed changes to multiple branches.
git stash apply

# Stash multiple times
git stash # stash some things
git stash # stash other things
git stash # stash more things
git stash list

# Apply a specific stash using the ID
git stash apply stash@{2} 

git stash drop <stash-id> # Delete a particular stash
git stash clear # Delete all stashes

# Note: 99% of the time, we may just use git stash and git stash pop.
```

## Checkout, Restore, Reset and Revert
```shell
# git checkout yields a 'detached HEAD' state
git checkcout commit<commit-hash> 

git checkout HEAD~1 # refers to the commit before the HEAD
git chekcout HEAD~2 # refers to 2 commits before the HEAD

git checkout HEAD <file-name> 
git checkout -- <file-name> # discard changes since what HEAD pointed

git switch - # undo the operation of detached HEAD
```

HEAD usually refers to a branch NOT a specific commit. When we checkout a  particular commit, HEAD points at that commit rather than at the branch pointer. 

You have a couple options:

1. Stay in detached HEAD to examine the contents of the old commit. Poke around,  view the files, etc.
2. Leave and go back to wherever you were before - reattach the HEAD; (i.e. git  switch master; )
3. Create a new branch and switch to it. You can now make and save changes,  since HEAD is no longer detached.

```shell
# This command is the same as git checkout HEAD <file-name>,
# this command undo changes, restore the file to the contents in the HEAD before last commit. If you have uncommitted changes in the file, they will be lost.
git restore <file-name>

# Restore the contents of the file to its state from the commit, which is prior to HEAD. You can use a particular commit hash as a source instead of HEAD~1
git restore --source HEAD~1 <file-name>

git restore --staged <file-name> # un-stage the file(s)
```

`git revert` is similar to `git reset` in that they both "undo" changes, but  they accomplish it in different ways:
- `git reset` actually moves the branch pointer backwards, eliminating commits.
- `git revert` instead creates a brand new commit which reverses/undos the  changes from a commit. Because it results in a new commit, you will be prompted  to enter a commit message.

Both `git reset` and `git revert` help us reverse changes, but there is a  significant  ifference when it comes to collaboration:
- If you want to reverse some commits that other people already have on their  machines, you should use revert.
- If you want to reverse commits that you haven't shared with others, use reset  and no one will ever know!

```shell
# Reset the repository back to a specific commit.
# This command removes the commits, but does not remove the changes, they are still in the working directory.
git reset <commit-hash> 

# Delete the commits after the specific commit and associated changes
git reset --hard <commit-hash>

git revert <commit-hash>
```
Note: Uncommitted changes to the files will be reflected in the working  directory, but will not be saved into the folder of object in .git folder. And the git status will remember the changes, i.e. whether they are staged or not.

## Git Clone
```shell
# To clone a remote repo, first make sure you are not inside of a repo when you clone.
git clone <url>
```

## Github Setup: SSH Config
```shell
# The codes below are Windows Version
# For Mac/Linux Version, find details at:
# https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

git config user.email
ls -al ~/.ssh # to see if existing SSH keys are present

# Generate a ssh key
ssh-keygen -t ed25519 -C "example@gmail.com" 

# Generate a new SSH key
# Enter file in which to save the key (/u//.ssh/id_ed25519)
# Enter passphrase (empty for no passphrase)

# Add ssh key to ssh-agent
# Start the ssh-agent in the background
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

clip < ~/.ssh/id_ed25519.pub
# Copies the contents of the id_ed25519.pub file to your clipboard
```

## Link Local Repo with Remote Repo
Option 1: Existing Repo <br>
If you already have an existing repo locally that you want to get on Github:
- Create a new repo on Github
- Connect your local repo (add a remote)
- Push up your changes to Github

Option 2: Start from Scratch <br>
If you haven't begun work on your local repo, you can:
- Create a brand new repo on Github
- Clone it down to your machine
- Do some work locally
- Push up your changes to Github

```shell
# Review remotes
git remote
git remote -v # verbose, for more info


# Add a new remote
git remote add <name> <url>

# When we clone a Github repo, the default remote name setup for us is called origin.
git remote add origin <url>
git branch -m Main # Move/rename a branch, together with its config and reflog.
git branch -M Main # -M is shortcut for --move --force

# Rename/delete remotes if needed: they are not commonly used
git remote rename <old> <new>
git remote remove <name>

git push <remote> <branch>
git push origin master # one example
git push origin empty # another example

# When the naming is different between local device and remote destination, push our local-branch up to a remote branch with a different name
git push <remote> <local-branch>:<remote-branch> 

# Set the upstream of the local master branch so that it tracks master branch on the origin repo.
git push -u origin master

# When the local branch and remote branch have the same name:
git push -u origin <branch> 

# When the local branch and remote branch do not have the same name:
git gush -u origin <local-branch>:<remote-branch>

# You can think of -u as a link connecting the local branch to a branch in Github.
# It sets the upstream of the local branch so that it tracks the remote branch (with the same name) on the origin repo.
git push -u origin master # one example

# After using -u for the first time, we no longer need to specify the branch name in order to push the branch
git push 


# List or delete (if used with -d) the remote-tracking branches. 
# The branches follow the pattern <remote>/<branch>, such as: origin/main, upstream/design
git branch -r

# Checkout the remote branch pointer when first cloned to local device.
git checkout origin/main
```