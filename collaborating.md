# Collaborating
For most development work, we don't get to interact with code in complete isolation. Ultimately, we work in teams to 
create more complex software than a single person can create alone. Git allows for this by using a concept known as 
"the remote".

## What is a "remote"?
A remote is simply a named URL pointing to another repository (usually located on a different machine and accessed via
the network).

Because Git is fully distributed, every copy of a repository has its own complete history, commit graph, and branches.
A remote is not a live database connection; it is a destination alias saved in your repository config (usually named 
`origin`) that (in effect) points to a network-accessible repository you share with your team.

## Setting up a remote
So how do we set up a remote? There are two ways. We can add a remote to an existing repository, or it will be added
automatically when we clone an existing repository (the latter approach is by far the most common). But since we've 
already got an existing test repository, we'll do things the hard way! 

### Populating an uninitialized remote from an existing repository
> [!NOTE]
> From here on, you will need a remote git server to work with that you have the ability to create repositories on. The 
> easiest way to do this is with a service like [GitHub](https://github.com). This guide will not go through the steps 
> needed to set up an account or set up authentication. It will, however, assume you're using GitHub and SSH 
> authentication.
>
> Don't worry about understanding all these commands in this section right now. This isn't something you'll do often 
> (or possibly ever).

> [!IMPORTANT]
> Start by creating an _uninitialized_ repository on your service of choice. You can name it what you would like, but in
> keeping with earlier work, this guide will assume the name `my-repo`. It is important that you do not _initialize_ 
> your repository, because we will be connecting our existing repository to it and uploading our local history.
> 
> With that created, go into your local copy of the repository and make sure you're on `main` (or whatever your default
> branch name is).:
> ```bash
> $ git switch main
> ```
> GitHub recommends you use `main` as your default branch name, so if it's something different, rename it:
> ```bash
> $ git branch -M main
> ```
> Now retrieve the repository URL from the repository webpage and set it as the origin for your local repository:
> ```bash
> $ git remote add origin git@github.com:johndoe/my-repo.git
> ```
> Finally, push to the remote, setting the "upstream" for your local branch as you do:
> ```bash
> $ git push -u origin main
> Enumerating objects: 22, done.
> Counting objects: 100% (22/22), done.
> Delta compression using up to 24 threads
> Compressing objects: 100% (15/15), done.
> Writing objects: 100% (22/22), 1.90 KiB | 971.00 KiB/s, done.
> Total 22 (delta 3), reused 9 (delta 1), pack-reused 0 (from 0)
> remote: Resolving deltas: 100% (3/3), done.
> To github.com:johndoe/my-repo.git
>  * [new branch]      main -> main
> branch 'main' set up to track 'origin/main'.
> ```
> On GitHub, check to see that your repository was populated.

### Getting a remote by cloning
Now we'll do something a bit radical.

> [!IMPORTANT]
> Delete the folder containing your local repository (~/repos/my-repo).
> 
> Now, clone the repository from GitHub:
> ```bash
> $ cd ~/repos
> $ git clone git@github.com:johndoe/my-repo.git my-repo
> Cloning into 'my-repo'...
> remote: Enumerating objects: 22, done.
> remote: Counting objects: 100% (22/22), done.
> remote: Compressing objects: 100% (13/13), done.
> remote: Total 22 (delta 3), reused 22 (delta 3), pack-reused 0 (from 0)
> Receiving objects: 100% (22/22), done.
> Resolving deltas: 100% (3/3), done.
> ```

Git clones the repository and automatically sets the remote up for us.

Change directories into your repository and take a look:
```bash
# Make sure all the files are there
$ ls
...
# Make sure the commit history wasn't lost
$ git log --oneline --graph --all
...
# Take a look at git status
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```
We have some new information from git status. It now tells us our status with relation to the remote. 

## Managing remote branches
From our local repository, we can instruct the remote to change its own branches. The primary use case for this is 
creating new remote branches.

### "Publishing" a local branch to the remote
To create (often called "publish") a new local branch to the remote, we usually create it locally, and then "push" it to
the remote.

> [!IMPORTANT]
> Do this now:
> ```bash
> $ git switch -c newbranch
> Switched to a new branch 'newbranch'
>
> $ git push origin newbranch
> Total 0 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
> remote:
> remote: Create a pull request for 'newbranch' on GitHub by visiting:
> remote:      https://github.com/johndoe/my-repo/pull/new/newbranch
> remote:
> To github.com:johndoe/my-repo.git
>  * [new branch]      newbranch -> newbranch
> 
> # set the upstream target for the new branch
> $ git branch --set-upstream-to=origin/newbranch
> branch 'newbranch' set up to track 'origin/newbranch'.
> ```
>
> Before moving on, switch back to `main`
> ```bash
> $ git switch main
> Switched to branch 'main'
> Your branch is up to date with 'origin/main'.
> ```

### Switching to a branch on the remote
Other users will be creating branches as well, and we may need to be able to interact with them. For that, we need to
somehow find out the name of a remote branch. We can use `git branch -r` (lists remote branch names) to see all 
remote branch names (if you don't see the expected branch, you might need to run `git fetch`). Once you know the name, 
`git switch <branchname>` works in the same manner. If Git doesn't find  the branch locally, it automatically looks for
it in the remote branches, then sets up a new local branch that tracks the remote branch.

> [!NOTE]
> If you have a local branch already with the same name as a remote branch, things can get messy very quickly. Branch
> naming standards go a long way to avoiding this. One very simple way to ensure it never happens is to use a "prefix"
> in local-only branch names. For example `private/<branch-name>` or `local/<branch-name>`. Such branches should
> generally never be pushed to the remote (unless renamed according to your team's branch naming standards).

### Other operations
Other operations, especially deleting and renaming remote branches can be performed, but must be handled with care, as
they can break connections on other developers' clones of the repository. This document does not cover these operations.

## Synchronizing simple changes with the remote
While absolutely necessary for a team environment, dealing with a remote repository adds an additional layer of 
complexity. We now not only have our local branches, we also have remote branches that we need to keep in sync. 

### Checking in with the remote
An important thing to keep in mind is that your local repository is not in continuous communication with the remote.
Changes will be made (pushed) to the remote by other users from time to time, and how you handle them depends heavily 
on your local repository being made aware of them. Git provides the `git fetch` command for this purpose.

`git fetch` is designed to do just one thing: make sure your local repository has up-to-date copies of all necessary
information stored on the remote. It is safe to run at any time: it will not modify your local branches, your
staging area, or your working directory[^1]. It simply makes Git aware of 
what's currently on the remote and downloads everything you might need to work with it.

Let's illustrate this. On the GitHub site, you should be able to _directly_ edit files in your repository using the 
webpage. 

> [!IMPORTANT]
> Modify any of those files now (GitHub will actually update, stage, and commit your change directly in the remote
> repository).
> 
> Now on your local, look at the current status and commit log (our log is getting a little long, so I've removed a 
> number of lines in the output for brevity.):
> ```bash
> $ git status
> On branch main
> Your branch is up to date with 'origin/main'.
> 
> nothing to commit, working tree clean
>
> $ git log --oneline
> 9fed0d5 (HEAD -> main, origin/main, origin/HEAD) Resolve merge conflicts
> 69fe000 Update bar.txt on main branch
> ...
> b6f666a Initial commit
> ```

Our repository is blissfully unaware that we've modified a file on the remote. 

> [!IMPORTANT]
> Let's help it out by running `git fetch`:
> ```bash
> $ git fetch
> remote: Enumerating objects: 5, done.
> remote: Counting objects: 100% (5/5), done.
> remote: Compressing objects: 100% (2/2), done.
> remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
> Unpacking objects: 100% (3/3), 971 bytes | 74.00 KiB/s, done.
> From github.com:johndoe/my-repo
>    9fed0d5..f3c26af  main       -> origin/main
> 
> $ git status
> On branch main
> Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
>   (use "git pull" to update your local branch)
> 
> nothing to commit, working tree clean
> ```

### Incoming changes
> [!IMPORTANT]
> Execute the following command to ensure your default settings for this repository match the expected behavior. If you
> don't set this, you may get an error asking you to "specify how to reconcile" divergent branches. Rebasing (a 
> powerful tool for bringing in changes by "rewriting" local history) is not explained in this guide.
> ```bash
> $ git config pull.rebase false
> ```

Now our local knows that the remote has commits we don't have (`Your branch is behind 'origin/main' by 1 commit`). It 
also knows that we can bring these commits in easily (`and can be fast-forwarded.`). How do we bring the changes in? 
The same way as with a local branch: we merge!

> [!IMPORTANT]
> Execute a `git pull`:
> ```bash
> $ git pull
> Updating 9fed0d5..f3c26af
> Fast-forward
>  baz.txt | 1 +
>  1 file changed, 1 insertion(+)
> ```
Wait. But I said "merge" not "pull". What gives? 

`git pull` is shorthand for issuing `git fetch` followed by `git merge <upstream-name>` and is the standard command when
retrieving changes on a remote into a local branch. Now our history should show the commit from GitHub.
```bash
$ git log --oneline
f3c26af (HEAD -> main, origin/main, origin/HEAD) Add a new line to baz.txt
... 
b6f666a Initial commit
```

### Outgoing changes
So now, let's create our own change. 

> [!IMPORTANT]
> Pick a file on your local and edit it. Stage the change and commit it.

Now check status:
```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Git now knows we have a local change that is not represented on the remote. 

> [!IMPORTANT]
> Move your local change to the remote using the `git push` command. If you had multiple commits in your local that
> weren't in the remote, `git push` would move them all:
> ```bash
> $ #Do a fetch just to be sure there's nothing unexpected
> $ git fetch
>
> $ git push
> Enumerating objects: 5, done.
> Counting objects: 100% (5/5), done.
> Delta compression using up to 24 threads
> Compressing objects: 100% (2/2), done.
> Writing objects: 100% (3/3), 340 bytes | 340.00 KiB/s, done.
> Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
> To github.com:johndoe/my-repo.git
>    f3c26af..0534596  main -> main
>
> $ git status
> On branch main
> Your branch is up to date with 'origin/main'.
> 
> nothing to commit, working tree clean
> ```

> [!NOTE]
> We were able to push to the remote without any arguments in the command because Git set the "upstream" (or tracking)
> branch for our local `main` branch when we cloned the repository. Without that, we would need to use 
> `git push origin main`. There are a variety of ways to set an upstream for a branch if it's not already set: 
> * During a pull or push operation, include the `--set-upstream` flag
> * On an existing branch, issue the command `git branch --set-upstream-to=origin/<branchname>`
> * Never have to deal with it again: `git config --global push.autoSetupRemote true`. Git will automatically set the
>   upstream for every branch on the first "push" operation (including the first "push" of a local branch to create its
>   remote).

`git push` is a single, unified operation that performs the following steps as an _atomic_ operation[^2]:
* Upload all commits (and associated objects) on the current branch that do not yet exist on the remote.
* Update references on the remote (notably the branch pointer) to point to the latest commit.

> [!NOTE]
> Unlike `git pull` (which is fundamentally a merge operation), `git push` does not perform complex merges on the 
> remote. It will only add commits and move branch pointers. 
> 
> This is intentional: You can never push a change to the remote that would overwrite someone else's work 
> _unintentionally_ (because your local branch is "behind" the remote branch)[^3]. Git will _reject_ such a push 
> outright. You _must_ update your local with the changes on the remote first and reconcile any conflicts. Only then 
> can you push the results of that reconciliation to the remote. We'll look at that next.

## Handling divergent remote history
Since we're working in a disconnected environment, divergent changes are guaranteed to happen eventually. The remote is
quite literally another branch that someone else might be committing to (it just happens to reside on a different 
machine). So the strategies for handling those changes are very similar to those for handling diverging histories on the
local. 

In short, once aware of incoming changes (`git fetch` / `git status`), we merge the incoming changes locally 
(`git pull` and (if not a fast-forward merge) `git commit`) and then push the resulting (merged and reconciled) changes
back to the remote (`git push`).

But we do have strategies to avoid this being a nasty surprise we have to deal with at an inopportune moment...

### The painful way
Before doing this the "better" way, let's look at what it looks like if we _don't_.

> [!IMPORTANT]
> Try this:
> * On your local, edit a file. Save, stage, and commit.
> * On GitHub, edit the _same_ file and commit that change.
> 
> Now try to just go ahead and push your changes:
> ```bash
> $ git push
> To github.com:johndoe/my-repo.git
>  ! [rejected]        main -> main (fetch first)
> error: failed to push some refs to 'github.com:johndoe/my-repo.git'
> hint: Updates were rejected because the remote contains work that you do not
> hint: have locally. This is usually caused by another repository pushing to
> hint: the same ref. If you want to integrate the remote changes, use
> hint: 'git pull' before pushing again.
> hint: See the 'Note about fast-forwards' in 'git push --help' for details.
> ```
Well, _that_ didn't work.

Git recognized that the remote has changes we haven't seen and stops us from proceeding. It then recommends a 
`git pull`. Remember: that's a `git fetch` followed by a `git merge`. And _that's_ the secret sauce...

### The better way (or how to avoid conflicts with the remote)
The last thing a developer wants to deal with when ready to release the change they've worked so hard on is a conflict.
But conflicts _will_ occur. The best way to handle them is to proactively watch for them and incorporate them before
synchronizing our changes to the remote. To do this, we can regularly pull in changes from the remote when it's 
convenient for us. To do this, we start by regularly running `git fetch` and `git status`. These commands just make sure
we're aware of changes without forcing us to immediately deal with them.

If we fall behind the remote, `git status` will tell us:
```bash
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" if you want to integrate the remote branch with yours)

nothing to commit, working tree clean
```

Maybe this isn't a good time to deal with this. Maybe it is. For now, that choice is left up to you as a developer.

It might be helpful to know more about the incoming changes. Here's a few options:
```bash
# Do this first to ensure you have the most up-to-date information about the remote
$ git fetch

# Show all the commits we're missing as a compact list
$ git log HEAD..origin/main --oneline
94f9d03 (origin/main, origin/HEAD) Add remote line change to bar.txt

# Show all the files modified by commits we're missing
$ git diff --name-only HEAD origin/main
bar.txt

# Show extended details of all the changes in the commits we're missing
# (complete with commit comments and file-by-file diffs)
$ git log HEAD..origin/main -p
commit 94f9d03a2bfe26eb311613073e2195bda8e26ad5 (origin/main, origin/HEAD)
Author: John Doe <jdoe@outlook.com>
Date:   Fri Nov 6 20:28:57 2026 -0500

    Add remote line change to bar.txt

diff --git a/bar.txt b/bar.txt
index a30a5a5..92b0281 100644
--- a/bar.txt
+++ b/bar.txt
@@ -1,2 +1,3 @@
 test modification on main and edits branches
 modification 2
+remote line change
```

> [!IMPORTANT] 
> Let's bite the bullet and pull in the changes from the remote:
> ```bash
> $ git pull
> Auto-merging bar.txt
> CONFLICT (content): Merge conflict in bar.txt
> Automatic merge failed; fix conflicts and then commit the result.
> ```

If we looked at the output above, we might have expected this. We knew we edited the first line in the file and we could
see that the remote updates changed the same file (the command with extended details even told us which line). We'll 
have to resolve the conflict. 

Again we do this locally. 

> [!IMPORTANT] 
> Resolve the conflict by editing the file, then stage and commit it. 

With the commit, we've merged the changes on the _remote_ branch into our local. But we still need to get the changes
of the merge into the remote. 

> [!IMPORTANT] 
> Check status and push to the remote:
> ```bash
> $ git status
> On branch main
> Your branch is ahead of 'origin/main' by 2 commits.
>   (use "git push" to publish your local commits)
> 
> nothing to commit, working tree clean
>
> $ git push
> Enumerating objects: 10, done.
> Counting objects: 100% (10/10), done.
> Delta compression using up to 24 threads
> Compressing objects: 100% (5/5), done.
> Writing objects: 100% (6/6), 707 bytes | 707.00 KiB/s, done.
> Total 6 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
> To github.com:johndoe/my-repo.git
>    94f9d03..99c13df  main -> main
> ```

> [!NOTE]
> Though possible, it's unusual to ever merge from a local branch to a _different_ remote branch. The usual pattern is 
> to check out _both_ branches to local copies, perform merges locally, then push the merged changes back up to the 
> remote.

---
[Home](./README.md) | [Previous: Branching out](./branching.md) | Next

Copyright © 2026 Aaron Craven.

[^1]: This is not to say that the command has no effect. It updates remote-tracking refs, which are stored locally.
[^2]: `git push` is atomic for each "ref" that is pushed independently. If you push multiple branches at the same time 
    (e.g. `git push origin main newbranch otherbranch`), each branch will be separately pushed in a separate atomic 
    operation unless the `--atomic` flag is provided (and the server supports it).
[^3]: While true, `git push` can be used with the `--force` flag to tell Git to update the state of the remote to the 
    exact state of the local repository no matter what. A slightly safer option (`--force-with-lease`) does the same 
    thing but automatically aborts if the actual state of the remote has changed since the last fetch. Both approaches 
    are dangerous and are used operations this guide does not cover (rebasing, squash commits, amending commits, etc.)