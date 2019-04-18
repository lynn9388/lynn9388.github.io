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

- **`git status`**
- `git init`
- `git clone LINK`

### Review a Repository's History

- `git log`
  - `--oneline`
  - `--graph`
  - `--all`
  - `-p` or `--patch`
  - `-w`
  - `--stat`
  - `--decorate`
- `git show SHA`

### Add Commits to a Repository

- `git add FILE1 FILE2`
  - `git add .`
- `git commit`
  - `git commit -m MESSAGE`
- `git diff`

  ```bash
  Working Directory    <----+--------+-------+
          |                 |        |       |
          |              diff HEAD   |       |
          V                 |        |       |
      "git add"             |        |       |
          |                 |        |     diff
          |                 |        |       |
          V                 |        |       |
        Index    <----+-----|--------|-------+
          |           |     |        |
          |   diff --cached |        |
          V           |     |        |
    "git commit"      |     |        |
          |           |     |        |
          |           |     |        |
          V           |     |        |
        HEAD     <----+-----+        |
          |                          |
          |                       diff HEAD^
          V                          |
  previous "git commit"              |
          |                          |
          |                          |
          V                          |
        HEAD^          <-------------+
  ```

### Good Commit Messages

- Explain **what** the commit does in the first line
- Explain **why** in the body
- No **how**

### Tagging, Branching and Merging

- `git tag`
  - `git tag TAG`
  - `git tag -a TAG` or `git tag --annotate TAG`
  - `git tag -d TAG`
- `git branch`
  - `git branch BRANCH`
  - `git branch -d BRANCH`
  - `git checkout BRANCH/TAG/SHA`
  - `git checkout -b BRANCH`
- `git merge BRANCH/TAG/SHA`
- Merge Conflicts

### Undoing Changes

- Relative Commit References
  - `^` indicates the parent commit
  - `~` indicates the first parent commit
- `git commit --amend`
- `git revert SHA`
- `git reset SHA`
  - `--mixed`
  - `--soft`
  - `---hard`
  - [Video](https://s3.cn-north-1.amazonaws.com.cn/u-vid-hd/UN7ki2G2yKc.mp4)

## Git Collaboration

### Working With Remotes

![Remote Repository](https://s3.cn-north-1.amazonaws.com.cn/u-img/fbbde604-5978-4814-b3ab-872d82dcfa30)

- `git remote`
  - `-v`
  - `add REMOTE URL`
  - `rename OLD NEW`
  - `remove REMOTE`
- `git branch -u REMOTE/BRANCH`
- `git push REMOTE BRANCH`
- `git fetch REMOTE BRANCH`
- `git pull REMOTE BRANCH` = `git fetch` + `git merge`

### Working On Another Developer's Repository

- Fork repository
- `git short log`
  - `-s`
  - `-n`
  - `--author=NAME`
  - `--grep=REGEX`

### Staying In Sync With A Remote Repository

- Pull request
- Stay in sync with source project
  - origin
  - upstream
- `git rebase -i BASE`

## References

[GitHub & Collaboration](https://www.udacity.com/course/github-collaboration--ud456)