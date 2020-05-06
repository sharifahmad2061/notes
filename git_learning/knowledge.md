# Git

- Git has 3 internal state management mechanisms, which are:
  - The working Directory
  - The Staging Index
  - The Commit Tree (HEAD)
- sometimes these mechanisms are called `Git's Three Trees 🌳🌳🌳`. They are not however traditional tree datastructures, rather `node and pointer-based data structures`, that git uses to track a timeline of edits.

## The Working Directory

- This tree is in sync with the local filesystem and is representative of the immediate changes made to content in files and directories.
- `git status` can be used to show changes made to the working directory. They are displayed in red with `modified` prefix.

```sh
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        "modified:   git_learning/knowledge.md"

no changes added to commit (use "git add" and/or "git commit -a")
```

## The Staging Index

- This tree is tracking working directory changes, that have been promoted with `git add` to be stored in the next commit.
- This tree is a `complex internal caching mechanism`.
- To view the state of the staging index, we use command `git ls-files`, ls-files is a debug utility to inspect the state of staging index.

```sh
git ls-files -s
```

- Here we have executed the `git ls-files` command with the `-s` option. without this flag, ls-files just gives the names of the files in the staging index. with `-s` option, ls-files gives additional metadata of the files in the staging index. This additional metadata includes `mode bits`, `object name` and `stage number`. `Object Name` is standard `git object SHA1 hash`.
- This `SHA1 hash` is hash of the content of the files.
- The `commit history` stores its own SHA hashes for identifying `pointers` to `commits and refs`.
- staging index hashes are used for tracking down version of the files in the staging index.
- The `git status` command output displays changes between the commit history and the staging index.

## The Commit Tree

- The `git commit` command adds changes to a `permanent snapshot` that lives in the `commit history`. This snapshot also includes the `state of the staging index` at the time of the commit.

## git reset

- `git reset` is similar in behavior to `git checkout`, where git checkout solely operates on `HEAD ref pointer`, git reset changes both the `HEAD and the current branch ref pointers`.
- `git checkout commit-hash` will bring the repo to a `detached HEAD` state.
- the ref pointer modification updates the `commit tree`.
- the command-line arguments `--soft`, `--mixed` and `--hard` to `git reset`, direct git how to **modify** `staging index and working directory trees`.

## references

- [atlassian git reset manual](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset)
