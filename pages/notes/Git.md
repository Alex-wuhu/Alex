---
title: Git Cheat Sheet
date: 2025-10-10
type: note
tags:
  - Git
  - CLI
---

A quick reference for the most common Git commands.

[[toc]]

## Configuration

To set up your Git identity.

| Command                                            | Description                      |
| -------------------------------------------------- | -------------------------------- |
| `git config --global user.name "Your Name"`        | Set your name for all commits.   |
| `git config --global user.email "you@example.com"` | Set your email for all commits.  |
| `git config --list`                                | List all Git settings.           |
| `git init`                                         | Initialize a new Git repository. |
| `git clone <url>`                                  | Clone an existing repository.    |

## Local Changes

Managing changes in your working directory.

| Command                        | Description                       |
| ------------------------------ | --------------------------------- |
| `git status`                   | Show the status of your files.    |
| `git add <file>`               | Stage a specific file.            |
| `git add .`                    | Stage all new and modified files. |
| `git diff`                     | Show unstaged changes.            |
| `git diff --staged`            | Show staged changes.              |
| `git commit -m "Your message"` | Commit staged changes.            |
| `git commit --amend`           | Modify the last commit.           |

## Branches

Working with branches.

| Command                        | Description                              |
| ------------------------------ | ---------------------------------------- |
| `git branch`                   | List all local branches.                 |
| `git branch <branch-name>`     | Create a new branch.                     |
| `git checkout <branch-name>`   | Switch to a branch.                      |
| `git checkout -b <new-branch>` | Create and switch to a new branch.       |
| `git merge <branch-name>`      | Merge a branch into your current branch. |
| `git branch -d <branch-name>`  | Delete a local branch.                   |

## Remote Repositories

Interacting with remote repositories like GitHub.

| Command                        | Description                              |
| ------------------------------ | ---------------------------------------- |
| `git remote -v`                | List all remote repositories.            |
| `git remote add <alias> <url>` | Add a new remote repository.             |
| `git fetch <remote>`           | Download changes from a remote.          |
| `git pull`                     | Fetch and merge changes from the remote. |
| `git push <remote> <branch>`   | Push your commits to a remote branch.    |

## History

Inspecting the commit history.

| Command                  | Description                                 |
| ------------------------ | ------------------------------------------- |
| `git log`                | Show the commit history.                    |
| `git log --oneline`      | Show a condensed commit history.            |
| `git show <commit-hash>` | Show details of a specific commit.          |
| `git blame <file>`       | Show who last modified each line of a file. |

## Undoing Changes

Reverting and resetting changes.

| Command                          | Description                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------------- |
| `git reset <file>`               | Unstage a file.                                                                       |
| `git reset --hard <commit-hash>` | **(Use with caution)** Reset to a specific commit, discarding all subsequent changes. |
| `git revert <commit-hash>`       | Create a new commit that undoes a previous commit.                                    |
| `git stash`                      | Temporarily save changes that are not ready to be committed.                          |
| `git stash pop`                  | Apply the most recently stashed changes.                                              |

## GitHub Release

Creating a release on GitHub using the `gh` CLI.

```bash
# Create a compressed archive
tar zcvf snp-release.tar.gz snp-release/

# Create a new release on GitHub
gh release create v5.19.0 snp-release.tar.gz --title "Release Title" --notes "Release notes"
```
