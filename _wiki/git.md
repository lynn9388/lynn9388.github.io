---
title: Git
---

## Config

- [Mac/Linux Setup](https://classroom.udacity.com/courses/ud123/lessons/1b369991-f1ca-4d6a-ba8f-e8318d76322f/concepts/63a6f935-dea7-43c2-aaa3-61deea5070c8)
- [Windows Setup](https://classroom.udacity.com/courses/ud123/lessons/1b369991-f1ca-4d6a-ba8f-e8318d76322f/concepts/8a5af628-7a18-49cf-bbc8-02691762f862)

## Terminology

- Working Directory
- Staging Area / Staging Index / Index

  ![Area](https://git-scm.com/book/en/v2/images/areas.png)

## Basic Commands

### Create or Clone a Repository

- Check the repository's status
  - **`git status`**
- Initial a local repository
  - `git init NAME`
- Clone a remote repository to local (default branch: *master*, default remote name: *origin*)
  - `git clone URL`

### Review a Repository's History

- List all commits
  - `git log`
    - `--oneline`
    - `--graph`
    - `--all`
    - `-p` or `--patch`
    - `--stat`
- List specific commit
  - `git show SHA`

### Add Commits to a Repository

- Add
  - Add files to staging area
    - `git add FILE1 FILE2`
  - Add all changed files to staging area
    - `git add .`
- Commit
  - Record changes in staging area to the repository
    - `git commit`
  - Use the given message as the commit message
    - `git commit -m MESSAGE`
- Diff
  - `git diff`

```bash
Working Directory      <------+-------+-------+
        |                     |       |       |
        |                     |       |       |
        V                     |       |       |
    "git add"                 |       |     diff
        |                     |       |       |
        |                     |       |       |
        V                     |       |       |
      Index      <----+-------|-------|-------+
        |             |   diff HEAD   |
        |             |       |       |
        V             |       |       |
  "git commit"  diff --cached |   diff HEAD^
        |             |       |       |
        |             |       |       |
        V             |       |       |
      HEAD       <----+-------+       |
        |                             |
        |                             |
        V                             |
previous "git commit"                 |
        |                             |
        |                             |
        V                             |
      HEAD^            <--------------+
```

### Good Commit Messages

- Explain **what** the commit does in the first line
- Explain **why** in the body
- No **how**

### Tag, Branch and Merge

- Tag
  - Create tag
    - `git tag TAG`
  - Create tag with annotation
    - `git tag -a TAG` or `git tag --annotate TAG`
  - Delete tag
    - `git tag -d TAG`
- Branch
  - Create branch
    - `git branch BRANCH`
  - Delete branch
    - `git branch -d BRANCH`
  - Switch branch or restore workign tree files
    - `git checkout BRANCH/TAG/SHA`
  - Create a branch and switch to it
    - `git checkout -b BRANCH`
- Merge
  - `git merge BRANCH/TAG/SHA`

### Undoing Changes

- Relative Commit References
  - `^` indicates the parent commit
  - `~` indicates the first parent commit
- Replace the tip of the current branch by creating a new commit
  - `git commit --amend`
- Revert a existing commit
  - `git revert SHA`
- Reset current HEAD to the specified state
  - `git reset SHA`
    - `--mixed`
    - `--soft`
    - `---hard`
    - [Video](https://s3.cn-north-1.amazonaws.com.cn/u-vid-hd/UN7ki2G2yKc.mp4)

## Git Collaboration

### Working With Remotes

![Remote Repository](https://s3.cn-north-1.amazonaws.com.cn/u-img/fbbde604-5978-4814-b3ab-872d82dcfa30)

- Remote
  - `git remote`
    - `-v`
    - `add REMOTE URL`
    - `rename OLD NEW`
    - `remove REMOTE`
- Update remote refs along with associated objects
  - `git push`
- Download objects and refs from another repository
  - `git fetch`
- Fetch from and integrate with another repository or a local branch
  - `git pull` = `git fetch` + `git merge`

### Working On Another Developer's Repository

- Fork repository
- Summarize 'git log' output
  - `git shortlog`
    - `-s`
    - `-n`
    - `--author=NAME`
    - `--grep=REGEX`

### Staying In Sync With A Remote Repository

- Pull request
- Stay in sync with source project
  - origin
  - upstream
- Reapply commits on top of another base tip
  - `git rebase -i BASE`

## References

[GitHub & Collaboration](https://www.udacity.com/course/github-collaboration--ud456)