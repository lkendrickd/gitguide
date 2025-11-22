# Git Guide

### Cloning a Repo

This just clones a repo to the current working directory

```sh
git clone git@github.com:ORGANIZATION/REPONAME.git
```

### Checkout a branch

You usually are on main or master on clone. You want to checkout a branch to make updates.
You COULD just change main and push it up but it is sinful to do so. Others working on other branches would not know or have the updates you made.

ALWAYS ALWAYS PULL MAIN this will pull any changes that you might not have since other people make PRs.  This pulls and makes your local main match what is in github.

```sh
git pull origin main
```

THEN create your branch while main is checked out.  The branch will then switch off the main branch and onto your new one.

```sh
git checkout -b NEW_BRANCH_NAME
```

Then you can make your changes.

### Observing the Status

This is good to see what files are changed. If it says untracked a simple commit will still add the changes.  If you add a **NEW** file you will need to git add it.

```sh
git status
```

### Adding Files

If you created new files git add them.  This allows git to track the files.

```sh
git add foobar.txt
```

### Commit Changes

```sh
git commit -a -m "A nice commit message explaining what you did"
```

### Push the Changes to the Upstream Repository

This pushes your branch up to github so you can make the PR or Pull Request

```sh
git push origin NEW_BRANCH_NAME
```

### Additional Changes

If you already have the PR submitted you can still make additional changes. Just make the changes git add or git commit and then git push origin BRANCH_NAME.  Those changes get **ADDED** to the open PR.


### Rebasing

When you rebase, Git takes the commits from your branch and re-applies them on top of the latest commits from the target branch (e.g., main).
This also keeps the commit history clean. it creates a linear history.

### Why Rebase is Useful in Teams:
- **Avoids Merge Commits:** Keeps the history clean and linear, which is easier to read.
- **Keeps Your Branch Up-to-Date:** Ensures your branch has the latest changes from the main branch.
- **Reduces Conflicts:** Resolving conflicts during a rebase ensures your branch is compatible with the latest changes.

### Important Notes:
- **Rewriting History:** Rebasing rewrites commit history, so avoid rebasing public/shared branches.
- **Force Push Required:** After rebasing, you need to force push (`git push --force`) because the commit history has changed.

### Example Scenario:
Imagine this scenario:
- `main` branch has commits: `A -> B -> C`
- Your branch has commits: `D -> E` (based on `B`)

If someone adds a new commit `F` to `main`, your branch is now behind:
```
main: A -> B -> C -> F
your-branch: A -> B -> D -> E
```

To rebase your branch onto `main`:
```bash
git checkout your-branch
git rebase main
```

After rebasing, your branch will look like this:
```
main: A -> B -> C -> F
your-branch: A -> B -> C -> F -> D' -> E'
```
Git re-applies your commits (`D` and `E`) on top of `F`, creating new commits (`D'` and `E'`).