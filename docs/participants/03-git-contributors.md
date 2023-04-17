---
title: Git for contributors
---

You might be used to working with source control either alone or with others.
Working with a fork and making sure we can merge swiftly brings some additional challenges.
This page aims to help you out with the things you might get asked to do but may be outside your comfort zone.

Sit back, relax and maybe bring a brew.

!!! tip
If you're not comfortable using Git from the CLI, we advise using [GitKraken](https://www.gitkraken.com/invite/nQmDPR9D). GitKraken is a great GUI for version control and is very useful for beginners and advanced developers alike.

## I didn't stick to the conventional commit guidelines

Some projects have specific guidelines and conventions for commits and pull requests. Sometimes we make a mistake
and forget to follow those conventions.

### I only have one commit

To reword the last commit, we can use Git's `--amend` switch to add something to our latest commit (code, changes, rewording). Use the following commands to rephrase the last commit and get that change merged!

````console
$ Git commit --amend -m "feat: better-worded feature```
$ git push --force
````

### I added more than one commit

Suppose all of your commits need to go to `main` because it makes sense to treat these as atomic units. In that case, you can use Git's interactive rebase functionality to reword any commit between `main` and your `HEAD`. To start an interactive rebase, type:

```sh
git rebase -i main.
```

This will open your `$EDITOR`, and you can mark the commits you want to reword with `reword` (or `r`) rather than pick. Exiting that file will start the rebase and spawn your `$EDITOR` to alter the commit message for each commit you marked as reword.

Once done, use `git push --force` to bring the changes to the pull request.

!!! tip "For VSCode users."
The latest version of VSCode has a built-in GUI to help you select `reword` or any other action on a commit. Select the right ones and press **Start Rebase** to continue.

## My branch is out of date with the remote

This means the main branch of the project you are working on contains commits your branch does not (could be your `main` branch or the branch you created to work on). To fix this, we need to rebase (add the new commits of the project's main branch underneath your new commits), so the pull request can get merged.

The first thing to do is add the project codebase as a `remote` to your local git repository. By default, your fork is a standalone copy of the project with its own remote on GitHub that's not connected to the upstream project codebase. Forks and Pull Requests are a feature GitHub introduced on top of git functionality, so we need to mimic that situation ourselves.

Follow these steps:

1. Add the remote to your local git repository

   ```sh
   git remote add upstream git@github.com:<org>/project.git
   ```

   !!! important
   Make sure to replace the organisation or user and repo accordingly.

2. Update the remote repository (now called `upstream`):

   ```sh
   git fetch upstream
   ```

3. Rebase the branch onto `upstream/main`:

   ```sh
   # go to the feature branch
   git checkout my-feature
   # make a backup in case you mess up
   git branch tmp my-feature
   # rebase on the upstream main branch
   git rebase upstream/main
   ```

4. Push your changes and remove the backup branch:

   ```sh
   git push --force
   # delete back up
   git branch -D tmp
   ```

## I have messed something up

Sometimes, you mess up merges or rebases. Luckily, in Git it is relatively
straightforward to recover from such mistakes.

If you mess up during a rebase, you need to type the following command:

```sh
git rebase --abort
```

If you only noticed the mistake **after** the rebase

```sh
# reset branch back to the last saved point
git reset --hard tmp
```

If you forgot to make a backup branch during the rebase:

```sh
# look at the reflog of the branch
git reflog show my-feature

8630830 my-feature@{0}: commit: BUG: io: close file handles immediately
278dd2a my-feature@{1}: rebase finished: refs/heads/my-feature onto 11ee694744f2552d
26aa21a my-feature@{2}: commit: BUG: lib: make seek_gzip_factory not leak gzip obj
...

# reset the branch to where it was before the botched rebase
git reset --hard my-feature@{2}
```

If you didn't mess up, but there are merge conflicts, you need to resolve those. This can be one of the trickier things to get right. For a good description of how to do this, see this article on [merging conflicts](https://git-scm.com/book/en/Git-Branching-Basic-Branching-and-Merging#Basic-Merge-Conflicts) or this [tutorial](https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts).

## Delete a branch

Maybe your branch has been merged into `main`, and you no longer need it. To delete a branch, you need to:

```sh
git checkout main
# delete branch locally
git branch -D my-unwanted-branch
# delete branch on GitHub
git push origin --delete my-unwanted-branch
```

You might also want to check this StackOverflow answer on how to [delete local and remote branches](https://stackoverflow.com/questions/2003505/how-do-i-delete-a-git-branch-locally-and-remotely).

## Additional Git resources

- [GitHub help](https://help.github.com/) has an excellent series of how-to guides.
- [learn GitHub](https://help.github.com/) has an excellent series of tutorials
- The [pro git book](https://git-scm.com/book/) is a good in-depth book on Git.
- [Git ready](http://www.gitready.com/) - a nice series of tutorials
- [GitFlow etiquette](https://betterprogramming.pub/git-workflow-etiquette-f22d96b8b0b8) if you want to dive more into GitFlow and how to collaborate with others
