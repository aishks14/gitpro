# gitpro

A small practice repository for learning Git. It contains a few folders of plain text files that you can safely edit, commit, branch and merge without worrying about breaking anything.

## Project structure

```
gitpro/
├── README.md
├── gitfol1/
│   ├── gitfl1.txt   # basic commands: add, commit, status, log, diff
│   └── gitfl2.txt   # branching, merging and merge conflicts
├── gitfol2/         # empty, for further exercises
├── gitfol3/         # empty, for further exercises
└── gitfol4/         # empty, for further exercises
```

Git does not track empty folders. `gitfol2`, `gitfol3` and `gitfol4` will only appear in the repository once each has at least one file in it.

## Getting started

You need [Git](https://git-scm.com/downloads) installed. Then, from the project folder:

```bash
git init
git add .
git commit -m "Initial commit"
```

## Exercises

### 1. Basic workflow

Uses `gitfol1/gitfl1.txt`.

1. Edit one of the sample lines in the file.
2. See what changed:
   ```bash
   git status
   git diff
   ```
3. Save the change to history:
   ```bash
   git add gitfol1/gitfl1.txt
   git commit -m "Update sample line in gitfl1"
   git log --oneline
   ```

### 2. Branching and merging

Uses `gitfol1/gitfl2.txt`.

1. Create a branch and switch to it:
   ```bash
   git checkout -b feature-gitfl2
   ```
2. Edit a note in the file, then commit it:
   ```bash
   git add gitfol1/gitfl2.txt
   git commit -m "Update note on feature branch"
   ```
3. Merge it back and delete the branch:
   ```bash
   git checkout main
   git merge feature-gitfl2
   git branch -d feature-gitfl2
   ```

If your default branch is called `master`, use that name in place of `main`.

### 3. Merge conflict

1. On a new branch, change `Note 3` in `gitfl2.txt` and commit.
2. Switch back to `main`, change the same line differently and commit.
3. Run `git merge <branch-name>`. Git reports a conflict.
4. Open the file, keep the text you want, remove the `<<<<<<<`, `=======` and `>>>>>>>` markers, then:
   ```bash
   git add gitfol1/gitfl2.txt
   git commit
   ```

## Command reference

| Command | What it does |
| --- | --- |
| `git init` | Create a new repository in the current folder |
| `git status` | Show changed, staged and untracked files |
| `git add <file>` | Stage a file for the next commit |
| `git commit -m "msg"` | Save staged changes to history |
| `git diff` | Show unstaged changes line by line |
| `git log --oneline` | Show commit history, one line per commit |
| `git checkout -b <name>` | Create a branch and switch to it |
| `git merge <name>` | Merge a branch into the current one |
| `git branch -d <name>` | Delete a merged branch |

## Author

Aishwarya Kr Singh
