# gitpro

A hands-on practice repository for learning Git and GitHub. It holds plain text files that are safe to edit, commit, branch, merge and rebase, together with notes on what each exercise showed.

Nothing here is application code. The value of the repository is its history: every branch, merge, conflict and pull request in it was made on purpose to see how Git behaves.

## Contents

- [What this project is about](#what-this-project-is-about)
- [Project structure](#project-structure)
- [Branches](#branches)
- [Exercises completed](#exercises-completed)
- [Lessons learned](#lessons-learned)
- [Command reference](#command-reference)
- [Further reading](#further-reading)
- [Author](#author)
- [Steps to perform](#steps-to-perform)
- [Summary](#summary)

## What this project is about

The project started on 10 November 2026 as two sample text files and grew over three days into a record of the core Git workflow. Each topic was practised on its own branch, pushed to GitHub and merged into `master` through a pull request.

The goals were to:

- practise the everyday commands until they were familiar: `status`, `add`, `commit`, `log`, `diff`;
- understand branching by actually making branches diverge and come back together;
- cause a merge conflict deliberately and resolve it by hand;
- learn what `stash` and `rebase` really do, including what happens when they are used wrongly;
- use the full GitHub flow: push a branch, open a pull request, merge it, pull the result;
- write down what was learned, so the repository doubles as a reference.

The sample files are written as short reference sheets. Each section in them was added as a separate commit, which is why the files read like notes and the history reads like a sequence of small steps.

## Project structure

```
gitpro/
├── README.md
├── .gitignore                     # ignores node_modules and .env
├── gitfol1/
│   ├── gitfl1.txt                 # basic commands, undoing, the three areas, commit tips
│   ├── gitfl2.txt                 # remotes, tags, merge types, history, .gitignore, rebase, stash
│   ├── bugfixes.txt               # bug fix practice file
│   ├── switch.txt                 # git switch practice
│   ├── rebase.txt                 # first rebase practice file
│   ├── rebase2.txt                # second rebase practice file
│   ├── rebase-notes.txt           # what the two rebase attempts showed
│   ├── merge-conflict-notes.txt   # merge conflicts explained, with the real example
│   └── git-extra-topics.txt       # 20 topics not covered in the other files
├── gitfol2/
│   ├── gitfl1.txt                 # git reset and git reflog
│   └── gitfl2.txt                 # git cherry-pick
├── gitfol3/
│   └── gitfl1.txt                 # remote workflow, fetch vs pull
└── gitfol4/
    ├── git-learning-guide.txt     # the full learning guide in one file
    └── git-references.txt         # links for every topic in the guide
```

Git tracks files, not folders. `gitfol2`, `gitfol3` and `gitfol4` were empty at the start and only appeared in the repository once each had a file in it.

## Branches

Every topic had its own branch. All of them have been merged into `master`.

| Branch | What was practised | Pull request |
| --- | --- | --- |
| `aishks14-diff-and-stash` | `git diff`, `git stash`, the blocked branch switch | #1 |
| `aishks14-switch-cmd-new-branch` | `git switch`, creating branches | #2 |
| `aishhks14-git-rebase` | first rebase attempt, which became a merge | #3 |
| `aishks14-git-rebase-2` | second rebase attempt, a real rebase | #4 |
| `aishks14-git-learning` | clean merges, the learning guide and references | #5 |
| `aishks14-merge-conflict` | a deliberate merge conflict | #6 |
| `aishks14-orange-mode` | first feature branch, extra topics file | #7 |

## Exercises completed

### 1. Setting up the repository

The repository was created with two sample files and a README, then a `.gitignore` was added so that `node_modules` and `.env` files are never committed. A `bugfixes.txt` file was added as a first extra commit.

| Commit | What it did |
| --- | --- |
| `3d82c11` | Initial commit: README and sample text files |
| `5afd458` | Added `.gitignore` |
| `75b1c67` | Added the bug fix file |

### 2. Viewing the commit graph

The history was viewed in the terminal and in VS Code, using the built-in Source Control graph and the Git Graph extension.

```bash
git log --graph --oneline --all --decorate
```

Watching the graph after every commit made it clear how branches fork from a commit and join again at a merge.

### 3. Branching and merging without conflicts

Three branches each added a new section to `gitfol1/gitfl2.txt`:

| Branch | Section added |
| --- | --- |
| `master` | Merge Types, Working with Remotes |
| `aishks14-git-learning` | Stashing Work (plus two sections in `gitfl1.txt`) |
| `aishks14-orange-mode` | Orange Mode |

Each section was placed in a part of the file that no other branch had touched. When `aishks14-git-learning` was merged into `master` (commit `ea039c1`), Git combined the changes by itself. This was a three-way merge with a merge commit and no conflict.

### 4. Creating and resolving a merge conflict

On purpose, two branches inserted different sections at exactly the same place in `gitfol1/gitfl2.txt`, right after "Inspecting History":

| Side | Commit | Section |
| --- | --- | --- |
| `master` | `4e19193` | Ignoring Files (.gitignore) |
| `aishks14-merge-conflict` | `85d17cb` | Rebasing Basics |

Merging stopped with:

```
CONFLICT (content): Merge conflict in gitfol1/gitfl2.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Both sections were kept, the `<<<<<<<`, `=======` and `>>>>>>>` markers were removed, and the merge was completed in commit `944b230`.

While removing the markers, the blank line and the dashed line above "Rebasing Basics" were deleted by mistake. Git accepted the result because it only checks that the markers are gone. The full write-up is in [gitfol1/merge-conflict-notes.txt](gitfol1/merge-conflict-notes.txt).

### 5. Comparing changes and stashing

On `aishks14-diff-and-stash`, uncommitted edits to `gitfol1/gitfl1.txt` were used to try `git diff` and `git stash`.

Two behaviours were observed when switching branches with uncommitted edits:

- Switching to a branch where the file was identical worked, and the edits came along.
- Switching to a branch where the file was different was refused:

```
error: Your local changes to the following files would be overwritten by checkout:
        gitfol1/gitfl1.txt
Please commit your changes or stash them before you switch branches.
```

The edits were then shelved with `git stash`, listed with `git stash list` and brought back with `git stash apply`. Applying a stash onto a file that already had uncommitted edits also failed until those edits were committed.

### 6. Rebasing

Rebase was attempted twice, and the difference between the two attempts was the lesson.

| | Attempt 1 | Attempt 2 |
| --- | --- | --- |
| Branch | `aishhks14-git-rebase` | `aishks14-git-rebase-2` |
| Command used | `git merge master` | `git rebase master` |
| Result | merge commit `39826d3`, history still forked | commit `63b6090` became `3d3a327` on top of `7a8916d` |
| History | not linear | linear |

The first attempt showed that `git merge master` and `git pull` are not rebases. The second showed that rebase writes new copies of commits, which is why the commit id changed. The full write-up is in [gitfol1/rebase-notes.txt](gitfol1/rebase-notes.txt).

### 7. Switching branches

On `aishks14-switch-cmd-new-branch`, `git switch` was used in place of the older `git checkout`: `git switch <branch>` to move, `git switch -c <branch>` to create, and `git switch -` to go back. The notes are in [gitfol1/switch.txt](gitfol1/switch.txt).

### 8. Working with GitHub and pull requests

The repository was pushed to GitHub. Each branch then followed the same cycle:

1. Push the branch with `git push -u origin <branch>`.
2. Bring the branch up to date with `git pull origin master`.
3. Open a pull request into `master` on GitHub.
4. Merge the pull request.
5. Update the local `master` with `git pull origin master`.

Seven pull requests, #1 to #7, were merged this way.

### 9. Writing the learning material

The last exercise was to write down everything learned, plus the topics not yet practised.

| File | Contents |
| --- | --- |
| [gitfol4/git-learning-guide.txt](gitfol4/git-learning-guide.txt) | 14 sections on topics practised here and 23 on topics not practised yet |
| [gitfol4/git-references.txt](gitfol4/git-references.txt) | official documentation, tutorials and a link for every section of the guide |
| [gitfol1/merge-conflict-notes.txt](gitfol1/merge-conflict-notes.txt) | how merges and conflicts work, with the example from exercise 4 |
| [gitfol1/rebase-notes.txt](gitfol1/rebase-notes.txt) | the two rebase attempts from exercise 6 |
| [gitfol1/git-extra-topics.txt](gitfol1/git-extra-topics.txt) | 20 further topics, such as partial staging, SSH keys and GitHub Actions |
| [gitfol2/gitfl1.txt](gitfol2/gitfl1.txt), [gitfol2/gitfl2.txt](gitfol2/gitfl2.txt) | practice sheets for reset, reflog and cherry-pick |
| [gitfol3/gitfl1.txt](gitfol3/gitfl1.txt) | practice sheet for the remote workflow |

## Lessons learned

- **Uncommitted edits belong to the working folder, not to a branch.** They follow you when you switch, unless the switch would overwrite them.
- **Edits in different places merge cleanly.** A conflict needs both branches to change the same lines, or to insert at the same position.
- **Git cannot check a conflict resolution.** It only checks that the markers are gone, so the merged text must be read again.
- **`git merge master` is not a rebase.** Only `git rebase master` gives a straight history.
- **Rebase creates new commits.** The ids change, so commits that are already pushed and shared should not be rebased.
- **A stash is not tied to a branch.** It can be applied anywhere, but not onto a file with uncommitted edits.
- **Empty folders are not tracked.** A folder appears in the repository only when it contains a file.
- **In PowerShell, `stash@{1}` needs quotes.** `git stash apply 1` is the simpler form.

## Command reference

| Command | What it does |
| --- | --- |
| `git status` | Show changed, staged and untracked files |
| `git add <file>` | Stage a file for the next commit |
| `git commit -m "msg"` | Save staged changes to history |
| `git log --graph --oneline --all` | Show every branch as a graph |
| `git diff` | Show unstaged changes line by line |
| `git diff --staged` | Show staged changes |
| `git switch <branch>` | Move to an existing branch |
| `git switch -c <branch>` | Create a branch and move to it |
| `git merge <branch>` | Merge a branch into the current one |
| `git merge --abort` | Cancel a merge that is in progress |
| `git rebase master` | Replay the current branch on top of `master` |
| `git stash` | Shelve uncommitted edits |
| `git stash list` | Show everything that is shelved |
| `git stash apply <n>` | Apply stash number `n` and keep it |
| `git stash pop` | Apply the latest stash and remove it |
| `git restore <file>` | Discard uncommitted edits in a file |
| `git revert <commit>` | Undo a commit with a new commit |
| `git push -u origin <branch>` | Upload a branch and track it |
| `git pull origin master` | Fetch and merge the remote `master` |
| `git branch -d <branch>` | Delete a merged branch |

## Further reading

- [gitfol4/git-learning-guide.txt](gitfol4/git-learning-guide.txt) for every topic in one place.
- [gitfol4/git-references.txt](gitfol4/git-references.txt) for links to the official manual and tutorials.
- [Pro Git](https://git-scm.com/book/en/v2), the free official book.
- [Learn Git Branching](https://learngitbranching.js.org), for visual practice.

## Author

Aishwarya Kr Singh

## Steps to perform

Follow these steps to repeat the exercises yourself. You need [Git](https://git-scm.com/downloads) installed and, for step 8, a GitHub account.

### Step 1. Get the repository

```bash
git clone https://github.com/aishks14/gitpro.git
cd gitpro
git log --graph --oneline --all --decorate
```

### Step 2. Make a basic commit

1. Edit a line under "Sample Content" in `gitfol1/gitfl1.txt`.
2. Look at the change, then commit it:

```bash
git status
git diff
git add gitfol1/gitfl1.txt
git commit -m "Update sample line in gitfl1"
git log --oneline
```

### Step 3. Branch and merge without a conflict

1. Create a branch and add a new section to `gitfol1/gitfl2.txt`:

```bash
git switch -c feature-clean
git add gitfol1/gitfl2.txt
git commit -m "Add a section on the feature branch"
```

2. Go back to `master`, add a different section in another part of the same file, and commit it.
3. Merge. Git combines both changes by itself:

```bash
git switch master
git merge feature-clean
git branch -d feature-clean
```

### Step 4. Cause and resolve a merge conflict

1. On a new branch, change `Note 3` in `gitfol1/gitfl2.txt` and commit:

```bash
git switch -c conflict-demo
git commit -am "Change Note 3 on the branch"
```

2. On `master`, change the same line in a different way and commit:

```bash
git switch master
git commit -am "Change Note 3 on master"
```

3. Merge. Git reports a conflict:

```bash
git merge conflict-demo
```

4. Open the file, keep the text you want and delete the `<<<<<<<`, `=======` and `>>>>>>>` lines.
5. Read the result again, then finish the merge:

```bash
git add gitfol1/gitfl2.txt
git commit
```

### Step 5. Compare and stash

1. Edit `gitfol1/gitfl1.txt` without committing.
2. Inspect the edit, shelve it and bring it back:

```bash
git diff
git stash
git stash list
git switch master
git switch -
git stash pop
```

### Step 6. Rebase a branch

1. Create a branch and commit a change on it:

```bash
git switch -c rebase-demo
git commit -am "Change on the rebase branch"
```

2. Commit a different change on `master`, so the branch is behind:

```bash
git switch master
git commit -am "Change on master"
```

3. Replay the branch on top of `master`, then fast-forward `master`:

```bash
git switch rebase-demo
git rebase master
git switch master
git merge rebase-demo
```

4. Check that the history is a straight line:

```bash
git log --graph --oneline
```

### Step 7. Undo a change

```bash
git restore <file>            # discard uncommitted edits
git restore --staged <file>   # unstage, keep the edits
git revert <commit>           # undo a commit with a new commit
```

### Step 8. Push and open a pull request

To push, use your own fork or repository as the remote.

```bash
git switch -c my-feature
git commit -am "Describe the change"
git push -u origin my-feature
```

1. On GitHub, open a pull request from `my-feature` into `master`.
2. Review it and merge it.
3. Update your local copy:

```bash
git switch master
git pull origin master
```

### Step 9. Go further

Work through the practice sheets for the topics not yet covered here:

- `gitfol2/gitfl1.txt` for `git reset` and `git reflog`
- `gitfol2/gitfl2.txt` for `git cherry-pick`
- Part B of `gitfol4/git-learning-guide.txt` for interactive rebase, bisect, worktrees and more

## Summary

`gitpro` is a practice repository that records a complete first pass through Git and GitHub. Over three days, nine exercises were carried out on seven topic branches and merged into `master` through seven pull requests.

| Area | What was done |
| --- | --- |
| Basics | repository setup, `.gitignore`, staging and committing, reading the graph |
| Branching | topic branches, `git switch`, clean three-way merges |
| Conflicts | one deliberate merge conflict, resolved by hand |
| Stash and diff | shelving edits, the blocked branch switch and its fix |
| Rebase | one attempt that turned out to be a merge, one real rebase |
| GitHub | pushing branches, seven pull requests, pulling `master` |
| Documentation | a learning guide, references and three sets of notes |

The main takeaways are that Git merges non-overlapping edits by itself, that conflicts are resolved by a person and must be re-read, and that rebase rewrites commits where merge preserves them. Reset, reflog, cherry-pick and interactive rebase have practice sheets in the repository and are the next topics to work through.
