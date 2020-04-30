Programming is often an exploratory exercise where, for example, one might not know what a refactor should look like until the driving feature is done. This leads to a messy commit history.

Git offers tools to create a clean history upfront or to fix it up later.

# **What does an ideal history look like**

- Every commit does exactly one thing.
- Every commit is buildable.
- Every commit message has a meaningful subject line
- Every commit message establishes context (what, why, and how)
- Commits are ordered to tell a story of how a feature was implemented.
- The commit graph is linear (and not [spaghetti](https://cdn-images-1.medium.com/max/800/1*e-tlWqLwbUd1UmZyC_KbGg.png)).

# **How to keep a clean history**

Git supports you with:

- Keeping history clean upfront
  - [The Index](https://hackernoon.com/understanding-git-index-4821a0765cf) (`git add <path>`, `git add -p <path>`)
  - [Stashing](https://git-scm.com/book/en/v1/Git-Tools-Stashing) (`git stash`, `git stash pop`)
  - Branches (`git checkout -b <name>`)
- Fixing it up later
  - Rebasing (`git rebase`,`git rebase -i`, `git pull --rebase`)
  - Cherry-picking (`git cherry-pick <hash>`)
  - Universal undo (`git reflog`)

# **Fixing: I need to add/remove/combine/split/re-order commits**

`git rebase -i` let's you edit your commit history in your `$EDITOR` of choice

Note: things get funky when merge commits are used. Another reason not to use them.

Just type:

```
git rebase -i master
```

And your commit history and instructions on editing it will pop up

```
pick 6e73d55c6 chore(nest): Baseline version of nipg.pl
pick 6aed59e08 chore(nest): Strip nipg down to essentials

# Rebase 1d9e1a08d..6aed59e08 onto 1d9e1a08d (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Once you've modified the "file", save and close and your history will be rewritten.

# Git Rebase**: Updating your branch to latest from `master`**

Coming from other SCM's, `git merge` sounds familiar and might be what you reach for by default. The thing to remember is that git's commits are not linear but a graph and `git merge` can easily lead to [complex commit graphs](https://cdn-images-1.medium.com/max/800/1*e-tlWqLwbUd1UmZyC_KbGg.png).

```
# - Fetch origin/master
# - Take existing commits and append them to origin/master
git pull --rebase origin master
```

In case you want to explore the difference, like when dealing with conflicts, it might be helpful to do this as separate commands:

```
git checkout master
git pull
git checkout - # `-` means "last branch"
git rebase master
```

# **Upfront: Partway through a change, realize you want to make a conflicting change**

[Stashing](https://git-scm.com/book/en/v1/Git-Tools-Stashing) is the go-to for quickly saving off your work to make another change

```
<change file>
git stash

<change file>
git add <file>
git commit

git stash pop
<resolve file>
<finish file>
git add <file>
git commit <file>
```

# **Upfront: Distinct changes in distinct sections of code**

When modifying one section of code, you might be reading another section and notice something you want to change, like a typo.

When the changes are in separate files

```
git add <file1>
git commit
git add <file2>
git commit
```

When the changes are in the same file, you can select which parts to stage.

```
# Interactive staging let's you choose which changed hunks you want to stage
#
# Sometimes a hunk is bigger than you want and you can split it.
git add -p <file>
git commit
git add <file>
git commit
```

**Tip:** Make staging files your habit rather than `git commit --all` to save yourself from having to fix things up later.
