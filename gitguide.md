# Git Guide

## Cloning a repo

This clones a repo to your current working directory:

```sh
git clone git@github.com:ORGANIZATION/REPONAME.git
```

## Keeping main up-to-date and creating a branch

It's a good idea to update your local main before creating a feature branch so your work starts from the latest code.

Option A — update local main and then create a branch:

```sh
# Make sure you're on main
git checkout main        # or: git switch main

# Update it from origin
git pull --rebase origin main   # or: git fetch origin && git merge origin/main

# Create and switch to a new branch
git switch -c NEW_BRANCH_NAME   # or: git checkout -b NEW_BRANCH_NAME
```

Notes:
- `git pull --rebase` is often preferred to avoid unnecessary merge commits; follow your team's convention.
- If you use older Git or prefer, `git checkout -b NEW_BRANCH_NAME` still works.

## Observing the status

See what files are changed and what is staged/untracked:

```sh
git status
# or for a compact view
git status -s
```

Important: untracked (new) files are not included by a commit until you stage them with `git add`.

## Adding files

Stage a new or modified file so it will be included in the next commit:

```sh
# Stage a specific file
git add foobar.txt

# Stage all changes (new/modified/deleted)
git add -A
# or
git add .
```

## Commit changes

- `-a` automatically stages changes to already tracked files (modified/deleted), but does NOT add new, untracked files.
- To include new files you must `git add` them first.

Examples:

```sh
# Good: stage what you want, then commit with a message
git add -A
git commit -m "A concise description of what you changed"

# Commit tracked changes only (no new files)
git commit -a -m "Update tracked files"
```

Commit message tip: keep the subject line short (50-72 chars), and explain why you changed things if needed in a longer body.

## Pushing your branch to GitHub

Push your branch to the remote and set the upstream so later `git push` knows where to go:

```sh
git push -u origin NEW_BRANCH_NAME
```

After that you can simply `git push` to send more commits.

## Updating an open PR

If you've already opened a PR and want to add changes:
- Make commits on the same branch.
- Push them: `git push` (or if necessary `git push --force-with-lease` after a rebase — see below).
The PR will update automatically with new commits on the branch.

## Discarding local changes to a file

There are newer commands and older ones. Examples:

```sh
# Old-style (still works on many Git versions) — be explicit with `--` to avoid ambiguity:
git checkout -- path/to/file

# Preferred modern way (since Git 2.23):
git restore path/to/file
# Or to restore from HEAD explicitly:
git restore --source=HEAD path/to/file
```

Use these only when you intend to discard local unstaged changes.

## Rebasing (keeping your branch on top of main)

Rebasing takes your branch's commits and re-applies them onto the tip of another branch (commonly main), yielding a linear history.

Typical workflow:

```sh
# Ensure remote refs are up-to-date
git fetch origin

# Update local main (optional: based on team policy)
git checkout main
git pull --rebase origin main   # or: git fetch origin && git merge origin/main

# Switch back to your branch and rebase onto main
git checkout your-branch
git rebase main          # or: git rebase origin/main
```

If there are conflicts, Git will stop and let you resolve them. After resolving a conflict in a file:
```sh
git add path/to/conflicted-file
git rebase --continue
```
To abort the rebase and return to the previous state:
```sh
git rebase --abort
```

Important notes:
- Rebasing rewrites history. Avoid rebasing branches that have been pushed and are being used/shared by others.
- After rebasing a branch that was already pushed, you must force-push. Prefer:
```sh
git push --force-with-lease
```
instead of `--force` to reduce the risk of overwriting others' work.

## Example scenario

- `main` has: A -> B -> C
- Your branch (created from B) has: D -> E

Someone pushes F to main: now
```
main: A -> B -> C -> F
your-branch: A -> B -> D -> E
```

Rebase your branch onto the updated main:
```sh
git fetch origin
git checkout your-branch
git rebase origin/main
```
After rebasing:
```
main: A -> B -> C -> F
your-branch: A -> B -> C -> F -> D' -> E'
```
The commits D and E become D' and E' (rewritten). Push using `--force-with-lease`.

## Short tips / cheatsheet

- Create branch: git switch -c my-branch
- Stage changes: git add -A
- Commit: git commit -m "message"
- Push new branch: git push -u origin my-branch
- Update local main: git fetch origin && git merge origin/main  (or git pull --rebase origin main)
- Rebase: git rebase origin/main; resolve conflicts; git rebase --continue
- Force-push safely: git push --force-with-lease

## Final notes

- Follow your team's branching and merge/rebase conventions.
- Use `--force-with-lease` instead of `--force` when pushing rebased branches.
- Prefer the newer commands git switch and git restore for clarity, but older commands (git checkout) still work on many systems.