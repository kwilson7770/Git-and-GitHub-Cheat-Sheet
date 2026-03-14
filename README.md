# Git & GitHub Cheat Sheet

`<>` indicates a variable that must be filled in

`[]` indicates an optional parameter

---

# Git Concepts Primer

Git tracks changes to files using three primary states:

* Working tree
* Staging area
* Repository history

These represent the lifecycle of changes as they move from editing to permanent history.

---

# Working Tree

The working tree (also called the "working directory" or "working files") contains the files currently checked out from Git that you are actively editing.

Changes made here are not tracked in the next commit until they are staged.

When switching branches or commits, Git updates the working tree to match the files stored in the checked-out commit.

---

# Staging Area

The staging area, also known as the index, stores the exact snapshot of files that will become the next commit.

Files are added to the staging area using:

```bash
git add
```

Common synonyms developers use:

* staged files
* indexed files
* added files

Files may also be untracked, meaning Git has never been told to track them. Once added with `git add`, they become tracked files.

---

# Repository History

When you run:

```bash
git commit
```

Git saves the contents of the staging area as a commit object in the repository history.

Other names for this include:

* commit history
* project history

Commits are immutable snapshots referencing the state of all tracked files at that moment.

---

# General Workflow

The lifecycle of a file typically follows this progression:

```text
Working directory > staged > committed
```

---

# Branches

Git branches are movable pointers to commits.

The currently active branch is referenced by a special pointer called HEAD, which represents the commit currently checked out in the working tree.

Branches allow developers to work on features independently without affecting the main codebase.

Example common branches:

* main or master – stable production code
* develop – integration branch
* feature/name – feature work
* hotfix/name – urgent fixes

---

# Git Object Model (Simplified)

Internally, Git stores data as four types of objects:

| Object | Purpose                 |
| ------ | ----------------------- |
| Blob   | File contents           |
| Tree   | Directory structure     |
| Commit | Snapshot of repository  |
| Tag    | Named pointer to commit |

A commit stores:

* pointer to a tree
* parent commit(s)
* author information
* commit message
* timestamp

Because of this design, Git can:

* create branches instantly
* store history efficiently
* merge changes reliably.

---

# GitHub Concepts Primer

GitHub is a remote repository hosting platform built around Git. A remote repository is simply a Git repository stored on another system. Remotes allow developers to collaborate and synchronize their work.

The default remote created when cloning a repository is usually named:

```text
origin
```

Developers:

* push commits to share their work
* pull or fetch commits to receive updates from others

---

# GitHub Features

GitHub adds collaboration tools on top of Git:

| Feature       | Purpose                                                         |
| ------------- | --------------------------------------------------------------- |
| Issues        | Bug tracking and task management                                |
| Pull Requests | Proposed changes that can be reviewed                           |
| Forks         | Personal copies of repositories                                 |
| Actions       | Continuous Integration/Continuous Deployment (CI/CD) automation |

---

# GitHub Authentication

GitHub authentication can use:

* HTTPS with tokens
* SSH with keys

SSH authentication avoids repeated credential prompts.

---

# Terminology Comparison

| Official Term      | Common synonyms                  | Basic definition                                 |
| ------------------ | -------------------------------- | ------------------------------------------------ |
| Working tree       | working directory, working files | Files in your local directory that can be edited |
| Staging area       | index                            | Stores files staged for the next commit          |
| Repository history | commit history, project history  | Contains history of all commits                  |

---

# Initial Setup

Configure Git identity and preferences.

```bash
# set the author name used when creating commits (can also be a username)
git config --global user.name "<firstname lastname>"

# set the author email used when creating commits
git config --global user.email "<email>"

# enable automatic command line coloring
git config --global color.ui auto

# create a global alias for a command
git config --global alias.lg "log --oneline --graph --decorate --all"
# Running:

git lg

# is equivalent to:
git log --oneline --graph --decorate --all
```

---

# Creating or Obtaining a Repository

```bash
# initialize an existing directory as a Git repository (creates the .git folder as well as the necessary files for git to work)
git init

# initialize an existing directory as a Git repository, but instead of the "master" branch being the default, change it to main
git init -b main

# Clones an existing repository from a hosted location including its full history.
git clone <url>
```

---

# Removing a Repository

Permanently delete the .git directory on your local system which removes all Git tracking

```bash
rm -rf .git
```

This does not delete your files, only the Git history.

---

# Inspecting Repository State

```bash
# show the state of the working tree and staging area, including untracked files. This mentions which branch is checked out and any modified files that haven't been staged.
git status

# list files currently tracked in the staging area
git ls-files

# list files currently untracked in the working tree
git ls-files -o
```

---

# Staging Files

```bash
# stage a file for the next commit. This also tracks something if previously untracked.
git add <file>

# stage files using wildcard expansion
git add <*.md>

# stage all files in a directory
git add .
git add <path/to/folder>
# Warning: Be cautious as this may include unintended files.
```

---

# Ignoring Files

Create a `.gitignore` file:

```text
logs/
*.notes
pattern*/
```

System-wide ignore rules:

```bash
git config --global core.excludesfile <file>
```

---

# Creating Commits

```bash
# saves the staging area as a new commit snapshot
git commit -m "<descriptive message>"

# commit all modified tracked files but does not include new untracked files.
git commit -am "<descriptive message>"
```

## Commit Message Guidelines

A good commit message should:

* briefly describe the change
* explain the reason if not obvious
* reference related issues when applicable

Example:

```text
Fix authentication timeout bug

The login session expired too early due to incorrect token duration.
Closes #42
```

---

# GitHub Issue Keywords

Commit messages containing issue numbers link to the issue.

Example:

```bash
git commit -m "Fix UI bug (#7)"
```

Keywords that close issues automatically:

```text
close
closes
closed
fix
fixes
fixed
resolve
resolves
resolved
```

---

# Viewing Changes

```bash
# show difference of working tree files and staging area files
git diff

# show difference of staging area files and last committed files
git diff --staged

# show difference of working tree files and last committed files
git diff HEAD
```

---

# Undoing Local Changes

```bash
# Overwrites the file in your working tree with the version in the staging area (or latest commit if not staged), discarding local modifications. Permanent data loss may occur if changes were not committed.
git restore <file>

# Same thing as above, but restore a file from a specific commit
git restore --source <commit> <file>

# remove a file from the staging area leaving the working tree alone
git restore --staged <file>

# exact same as git restore (see above)
git reset --staged <file>

# unstage all files leaving the working tree alone
git reset HEAD

# discard all working directory and staged changes. Permanent data loss may occur if changes were not committed.
git reset --hard HEAD

# preview what would be removed (recommended first)
git clean -n

# preview only ignored files that would be removed
git clean -ndX

# remove untracked files from the working directory. Warning: This permanently deletes untracked files.
git clean -f

# remove untracked directories as well. Warning: This permanently deletes untracked directories and files.
git clean -fd
```

---

# Temporary Work (Stash)

```bash
# save staged and working tree changes for later
git stash

# stash only specific files
git stash push <file>

# stash including untracked files
git stash -u

# create a named stash message
git stash push -m "<message>"

# list stashes
git stash list

# show the contents of a stash
git stash show -p

# restore latest stash and remove it from stack
git stash pop

# delete most recent stash
git stash drop
```

---

# Tags

Tags label specific commits, commonly used for releases.

```bash
# create lightweight tag that points to the current commit with no other associated metadata
git tag <simpleTagNameWithoutSpaces>

# create annotated tag that points to the current commit and stores more information (name, email, date, an optional tagging message, and an optional GPG signature) 
git tag -a <simpleTagNameWithoutSpaces> [-m "<message>"]
# Annotated tags are recommended for releases.

# list tags
git tag

# delete tag
git tag -d <tagName>

# force overwrite tag
git tag -f <tagName> [<commit>]
# Note: ideally you should create a new tag as updating the tag locally (and remotely) will require everyone to forcefully overwrite their existing tag as well (e.g. v1.0.1 instead of fixing the v1.0 tag)
```

---

# Branching

```bash
# list all branches
git branch

# show the current branch name
git branch --show-current

# create a new branch at the current commit
git branch <branch>

# delete a branch
git branch -d <branch>

# switch to another branch or checkout a specific commit
git checkout <branch/commit>

# create and switch to the new branch
git checkout -b <branch>

# switch branches (modern alternative to checkout)
git switch <branch>

# create and switch to new branch
git switch -c <branch>
```

---

# Merging

```bash
# Combines the history of another branch with the current branch.
git merge <branch>
```

If conflicts occur, they must be resolved before completing the merge.

---

# Rebasing

```bash
git rebase <branch>
```

Replays commits from the current branch onto another branch.

Before:

```text
A---B---C main
     \
      D---E feature
```

After rebase:

```text
A---B---C---D'---E'
```

Your commits are recreated, producing a linear history.

Warning: avoid rebasing commits already shared with others.

---

# Viewing History

```bash
# show commit history in detail
git log

# show both commit metadata and the actual code changes introduced in each commit.
git log -p

# Graph view of branches.
git log --oneline --graph --decorate --all

# show commits in branchA not in branchB
git log branchB..branchA

# show commits affecting a file (follows renames)
git log --follow <file>

# show commit details and diff
git show

# show the last commit and its changes
git show HEAD

# show details for specific commit
git show <hash>

# Shows which commit last modified each line of a file.
git blame <file>
```

---

# Finding the Commit that Introduced a Bug (git bisect)

`git bisect` performs a binary search through commit history to identify the commit that introduced a bug. Because it performs a binary search, it can locate a bug among hundreds of commits in only a handful of tests.

```bash
# start the bisect process
git bisect start

# mark the current commit as bad
git bisect bad

# mark a known good commit
git bisect good <commit>

# Git will check out a commit halfway between the good and bad commits. Next, test the code and mark the result with one of these:
git bisect good
git bisect bad

# finish the bisect session
git bisect reset
```

---

# Comparing Branches and Commits

```bash
# show differences between two branches
git diff branchB...branchA

# Example
git diff main...dev

# show changes between two commits
git diff <commit1>..<commit2>

# Example
git diff v1.0..v1.1
```

---

# GitHub Time-Based Comparisons

Git allows referencing commits relative to time.

```text
<branch,tag,commitHash>@{#.day[s].ago}
```

Example:

```text
master@{2.days.ago}
```

---

# Remote Repositories

```bash
# add remote repository
git remote add <alias> <url>

# GitHub ssh example
git remote add origin git@github.com:<username>/<projectName>.git

# GitHub https example
git remote add origin https://github.com/<username>/<projectName>.git

# change remote URL
git remote set-url origin git@github.com:<username>/<projectName>.git

# show configured remote repositories and their URLs
git remote -v
```

---

# Fetching and Pulling

```bash
# fetch down all the branches from that Git remote source. Does not perform any merging
git fetch <alias>

# merge a remote branch into your current branch to bring it up to date. May need to resolve conflicts
git merge <alias>/<branch>

# fetch and merge any commits from the remote branch. Equivalent to running git fetch followed by git merge of the upstream tracking branch.
git pull

# fetches the changes and then replays your commits on top of them, keeping the repository history cleaner
git pull --rebase
```

---

# Pushing Changes

```bash
# push commits
git push <alias> [<branch>]

# push all tags (which are excluded by default)
git push <alias> --tags

# push single tag
git push <alias> <tagname>

# safer force push (prevents overwriting others' work)
git push --force-with-lease
```

---

# GitHub SSH Authentication

1. Generate SSH key

```bash
ssh-keygen -t ed25519
```

2. Go to

```text
https://github.com/settings/keys
```

3. Click New SSH key

4. Paste the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

---

# Tracking File Moves or Deletes

```bash
# remove file and stage the deletion (preventing the need to delete the file on your system and then run git add -u)
git rm <file>

# move or rename file (preventing the need to run rename/move the file on your system and then add it back with git add and git add -u (or just git add -A if there is no reason to untrack files))
git mv <existing-path> <new-path>

# show commits with rename detection
git log --stat -M
```

---

# Rewriting History

```bash
# moves the current branch to a previous commit while keeping all changes in the staging area
git reset --soft <commit>

# clear working tree and staging area, moves your current branch back to a specific commit. This can result in permanent loss of data.
git reset --hard <commit>
```

# Git Debugging Tools

These commands help investigate repository history and identify when bugs were introduced.

---

## Search commit messages

```bash
git log --grep "<text>"
```

Example:

```bash
git log --grep "authentication"
```

---

## Find commits that added or removed code

```bash
# Exact match
git log -S "<text>"

# Regex
git log -G "<regex>"
```

Example:

```bash
git log -S "validateToken"
```

This shows commits where the specified code was added or removed.

---

## Find commits affecting a function

```bash
git log -L :<functionName>:<file>
```

Example:

```bash
git log -L :authenticateUser:auth.js
```

---

## Show which branches contain a commit

```bash
git branch --contains <commit>
```

Remote branches:

```bash
git branch -r --contains <commit>
```

---

## Blame specific lines in a file

```bash
git blame -L <start>,<end> <file>
```

Example:

```bash
git blame -L 10,20 auth.js
```

---

# Recovering Lost Work

Git rarely deletes data immediately.

Use:

```bash
# Shows recent HEAD movements.
git reflog

# show dangling commits and objects
git fsck --lost-found
```

Recover commit:

```bash
git checkout <commit>
```

Recreate branch:

```bash
git checkout -b <branch> <commit>
```

---

# Discouraged Git Usage

## Unintuitive file restore

```bash
git checkout -- <file>
```

This is an older syntax for restoring a file from the last commit

Better:

```bash
git restore <file>
```

## Blind git pull

```bash
git pull
```

Fetches remote changes and merges them automatically.
This can create merge commits unexpectedly.

Better:

```bash
git pull --rebase
```

---

## Careless git reset --hard

Destroys uncommitted changes.

Always inspect first:

```bash
git status
git diff
```

---

## Rebasing shared branches

Avoid rewriting history others rely on.

---

## Dangerous force pushes

```bash
git push --force
```

Better:

```bash
git push --force-with-lease
```

Prevents overwriting others' work.

---

# Common Git Workflows

These workflows describe common development tasks and the typical sequence of Git commands used to accomplish them. Optional commands are included where they may help inspect or verify changes before continuing.

---

# Starting Work on an Existing Project

Clone a repository and move into the project directory.

```bash
git clone <repo-url>
cd <repo-folder>
```

Optional: verify repository status.

```bash
git status
git log --oneline --graph --decorate --all
```

---

# Starting Work on a New Feature

Create a new branch so changes are isolated from the main codebase.

```bash
git checkout -b <feature-branch>
```

---

# Saving Work (Making a Commit)

After editing files, stage and commit your work.

```bash
git add <file>
git commit -m "<descriptive message>"
```

Common shortcuts:

```bash
git add .
git commit -am "<descriptive message>"
```

Optional inspection before committing:

```bash
git status
git diff
git diff --staged
```

---

# Reviewing Project History

Inspect commits, changes, or repository state.

```bash
git log
git log --oneline --graph --decorate --all
git show <commit>
git diff
```

Review changes to a specific file:

```bash
git log --follow <file>
```

---

# Updating Your Branch With the Latest Remote Changes

## Option 1 (safe, but extra work)

```bash
# downloads the latest data from the server
git fetch origin
# shows commits pushed you don't have yet
git log HEAD..origin/main
# shows the actual code changes
git diff HEAD..origin/main
# bring the changes into your branch and handle all merge conflicts
git merge origin/main
```

---

## Option 2 (Fetch and rebase)

```bash
# This sets your local commits aside, downloads the new commits, then reapplies local commits on top of the updated branch history. If conflicts occur they must be resolved before continuing
git pull --rebase origin main
# If a conflict happens, you fix it and stage it to be "rebased" (otherwise your local changes will be discarded)
git add <file>
# tells Git you are finished resolving conflicts and the rebase is done. This prevents the "merge bubble" helping it look like you wrote your code after everyone else finished theirs.
git rebase --continue
```

## Option 3 (Fetch and rebase using the explicit commands)

```bash
# This explicitly fetches remote changes
git fetch origin
# and rebases your work on top of them.
git rebase origin/main
```

---

# Publishing Your Work to the Remote Repository

Push your branch to the remote repository.

```bash
git push origin <branch>
```

If the branch does not exist remotely yet, Git may suggest:

```bash
git push -u origin <branch>
```

After this, future pushes can be done with:

```bash
git push
```

---

# Updating a Fork

```bash
# Adds the original repo as a remote, which is aliased as "upstream" (only needs to be done once)
git remote add upstream <original-repo-url>
# Ensures you are on the right branch 
git checkout main
# Downloads the latest commits from the upstream repository's main branch, but does not modify your files yet.
git fetch upstream main
# Merges the upstream project's latest main branch into your current branch. Conflicts would be resolved at this stage.
git merge upstream/main
# Updates your repo (origin) with the merged changes
git push origin main
```

---

# Resolving Merge Conflicts

When two commits modify the same lines, Git requires manual resolution.

Typical process:

```bash
git merge <branch>
```

Resolve conflicts in the affected files, then stage the fixes:

```bash
git add <file>
git commit
```

If the conflict occurred during a rebase:

```bash
git add <file>
git rebase --continue
```

---

# Undoing Changes Before They Are Committed

Discard changes to a file and revert it to version in latest commit:

```bash
git restore <file>
```

Unstage a file:

```bash
git reset --staged <file>
```

Discard all local changes:

```bash
git reset --hard HEAD
```

---

# Temporarily Saving Work (Stashing)

Store unfinished changes to switch branches safely.

```bash
git stash
```

View stashes:

```bash
git stash list
```

Restore the latest stash:

```bash
git stash pop
```

---

# Switching Branches Safely

Before switching branches, ensure your working directory is clean.

```bash
git status
git stash
git checkout <branch>
```

Restore work afterward if needed:

```bash
git stash pop
```

---

# Deleting a Completed Feature Branch

After merging a feature branch, remove it locally.

```bash
git branch -d <branch>
```

If the branch also exists remotely:

```bash
git push origin --delete <branch>
```

---

# Creating a Release Tag

Mark a release point in history.

```bash
git tag -a v1.0 -m "Release version 1.0"
git push origin v1.0
```

---

# Fixing a Mistake in the Last Commit

If you forgot to add a file:

```bash
git add <file>
git commit --amend
```

---

# Recovering From a Bad Commit

Move the branch back to a previous commit.

Keep changes staged:

```bash
git reset --soft <commit>
```

Discard all changes:

```bash
git reset --hard <commit>
```

---

# Combining Multiple Commits

Example of combining the last 5 commits into a single commit with a new commit message

```bash
# This moves HEAD back 5 commits but keeps all changes staged.
git reset --soft HEAD~5
# Then create a new commit 
git commit -m "<new commit message that covers all 5 commits>"
```

Squashing Commits

```bash
# start the interactive rebase
git rebase -i HEAD~5

# In the editor, keep the first commit as "pick" and change the others to "squash"
# This combines commits while allowing you to edit the final commit message.
```

---

# Mental Model Summary

Typical development loop:

```text
update > branch > edit > stage > commit > push
```

Example git workflow:

```bash
git pull --rebase
git checkout -b <branch>
# edit files
git add .
git commit -m "<message>"
git push origin <branch>
```

---

# 10 Very Common Git Commands

1. `git status` - Provides a snapshot of the current working directory, showing which files are modified, staged, or untracked. This is crucial for understanding the current state before taking further action.
2. `git add` - Stages changes for the next commit.
3. `git commit` - Records the staged changes as a new snapshot in the project's history with a descriptive message. These commits are saved locally.
4. `git push` - Uploads local commits to a remote repository (like GitHub or Bitbucket), sharing your progress with the team and backing up your work.
5. `git pull` - Fetches updates from the remote repository and automatically merges them into your current local branch, keeping your project in sync with the team's changes.
6. `git fetch` - Downloads the latest commits, branches, and updates from the remote repository without modifying your working directory. This allows you to review incoming changes before merging them into your local branch.
7. `git branch` - Manages branches by creating new ones, listing all existing branches, or deleting old ones. This is essential for working on features or bug fixes in isolation.
8. `git checkout` / `git switch` - Switches between branches or specific commits. The newer git switch command offers a clearer and safer way to change branches.
9. `git merge` - Integrates changes from a specified branch into the current active branch. This combines parallel lines of development back into the main codebase.
10. `git log` - Displays the commit history for the current branch, showing details such as commit messages, authors, timestamps, and commit IDs. This helps you track project changes and understand how the codebase evolved over time.

---

# Git Mental Model

When solving problems, experienced developers typically think about Git in this order:

1. Where am I?

```bash
git status
git branch
```

2. What changed?

```bash
git diff
git log
```

3. What do I want to save?

```bash
git add
git commit
```

4. How does my work relate to others?

```bash
git fetch
git merge
git rebase
```

5. How do I share it?

```bash
git push
```

Thinking about Git in this order often simplifies debugging and workflow decisions.
