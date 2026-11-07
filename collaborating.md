# Collaborating
For most development work, we don't get to interact with code in complete isolation. Ultimately, we work in teams to 
create more complex software than a single person can create alone. Git has to have ways to support this with mutually
accessible resources. We do that with what we refer to as "the remote".

## What is a "remote"?
A remote is simply a named URL pointing to another copy of your repository.

Because Git is fully distributed, every copy of a repository has its own complete history, commit graph, and branches. 
A remote is not a live database connection; it is a destination alias saved in your repository config (usually named 
`origin`) that (in effect) points to a network-accessible repository you share with your team.

# Setting up a remote
So how do we set up a remote? There are two ways. We can add a remote to an existing repository, or it will be added
automatically when we clone an existing repository. The latter approach is by far the most common. Approach. But since
we've already got an existing test repository, we'll do things the hard way! 

### Populating an uninitialized remote from an existing repository
From here on, you will need a remote git server to work with that you have the ability to create repositories on. The 
easiest way to do this is with a service like [GitHub](https://github.com). This guide will not go through the steps 
needed to set up an account or set up authentication. It will, however, assume you're using GitHub.

Start by creating an _uninitialized_ repository on your service of choice. You can name it what you would like, but in
keeping with earlier work, this guide will assume the name `my-repo`. It is important that you do not _initialize_ your
repository, because we will be connecting our existing repository to it and pushing it up.

> [!NOTE]
> Don't worry about understanding all these commands. This is mostly to create a remote your local repository that is
> populated with the work we've done so far.

With that created, go into your local copy of the repository and make sure you're on `main` (or whatever your default
branch name is).:
```bash
$ git switch main
```
GitHub recommends you use `main` as your default branch name, so if it's something different, rename it:
```bash
$ git branch -M main
```
Now get retrieve the repository URL from the repository webpage and set it as the origin for your local repository:
```bash
$ git remote add origin git@github.com:johndoe/my-repo.git
```
Finally, push to the remote, setting it's "upstream" as you do:
```bash
$ git push -u origin main
Enumerating objects: 22, done.
Counting objects: 100% (22/22), done.
Delta compression using up to 24 threads
Compressing objects: 100% (15/15), done.
Writing objects: 100% (22/22), 1.90 KiB | 971.00 KiB/s, done.
Total 22 (delta 3), reused 9 (delta 1), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (3/3), done.
To github.com:johndoe/my-repo.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```
On GitHub, check to see that your repository was populated.

### Getting a remote by cloning
Now we'll do something a bit radical: Delete the folder containing your local repository (~/repos/my-repo).

Now, let's clone the repository from GitHub:
```bash
$ cd ~/repos
$ git clone git@github.com:johndoe/my-repo.git my-repo
Cloning into 'my-repo'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 22 (delta 3), reused 22 (delta 3), pack-reused 0 (from 0)
Receiving objects: 100% (22/22), done.
Resolving deltas: 100% (3/3), done.
```
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
From our local repository, we can instruct the remote to change its own branches. The primary interaction here is 
creating remote branches.

### "Publishing" a local branch to the remote
To create (often called "publish") a local branch to the remote, we create it locally, and then "push" it to the remote:
```bash
$ git switch -c newbranch
Switched to a new branch 'newbranch'
$ git push origin newbranch
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote:
remote: Create a pull request for 'newbranch' on GitHub by visiting:
remote:      https://github.com/johndoe/my-repo/pull/new/newbranch
remote:
To github.com:johndoe/my-repo.git
 * [new branch]      newbranch -> newbranch

# set the upstream target for the new branch
$ git branch -u origin newbranch
branch 'newbranch' set up to track 'origin/main'.
```
Before moving on, switch back to `main`
```bash
$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

### Other operations
Other operations, especially deleting and renaming remote branches can be performed, but must be handled with care, as
they can break connections on other developers' clones of the repository. This document does not go into detail for 
these operations.

## Synchronizing simple changes with the remote
While absolutely necessary for a team environment, dealing with a remote repository adds an additional layer of 
complexity. We now not only have our local branches, we also have remote branches that we need to keep in sync. 

### Checking in with the remote
An important thing to keep in mind is that your local repository is not in continuous communication with the remote.
Changes will be made (pushed) to the remote from time to time, and how you handle them depends heavily on your local
repository being made aware of them. Git provides the `git fetch` command for this purpose.

`git fetch` is designed to do just one thing: make sure your local repository has up-to-date copies of all necessary
information stored on the remote. It is entirely safe to run. It will not modify your local branches, your staging area,
or working directory. It simply makes Git aware of what's currently on the remote.

Let's illustrate this. On the GitHub site, you should be able to directly edit files in your repository. Modify any of
those files now (GitHub will actually update, stage, and commit your change directly in the remote repository).

Now on your local, check out your current status and commit log:
```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
$ git log --oneline
9fed0d5 (HEAD -> main, origin/main, origin/HEAD) Resolve merge conflicts
69fe000 Update bar.txt on main branch
...
b6f666a Initial commit
```
Our repository is blissfully unaware that we've modified a file on the remote. Let's help it out:
```bash
$ git fetch
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 971 bytes | 74.00 KiB/s, done.
From github.com:johndoe/my-repo
   9fed0d5..f3c26af  main       -> origin/main
$ git status
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```
### Incoming changes
Now our local knows that the remote has commits we don't have. It also knows that we can easily bring these commits in
easily. How? The same way as with a local branch: we merge!
```bash
$ git pull
Updating 9fed0d5..f3c26af
Fast-forward
 baz.txt | 1 +
 1 file changed, 1 insertion(+)
```
Wait. Didn't I say merge? 

Yes! `git pull` is shorthand for issuing `git fetch` followed by `git merge <upstream-name>`. Now our history should 
show the commit from GitHub.
```bash
$ git log --oneline
f3c26af (HEAD -> main, origin/main, origin/HEAD) Add a new line to baz.txt
...
b6f666a Initial commit
```

### Outgoing changes
So now, let's create our own change. Pick a file on your local and edit it. Stage the change and commit it.

Now check status:
```bash
$ git status
[main 0534596] Create local change
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```
Git now knows we have a local change that is not represented on the remote. If we want to be _really_ careful, we should
probably do a `git fetch` here to be sure, but since we own this repository, we can be rebels. Let's go ahead and move
our local change to the remote. This is called "pushing" to the remote:
```bash
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 24 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 340 bytes | 340.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To github.com:johndoe/my-repo.git
   f3c26af..0534596  main -> main
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

> [!NOTE]
> We were able to push to the remote without any arguments in the command because Git set the "upstream" (or tracking)
> branch for our local `main` branch when we cloned the repository. Without that, we would need to use 
> `git push origin main`. There are a variety of ways to set an upstream for a branch: 
> * During a pull or push operation, include the `--set-upstream` flag
> * On an existing branch, issue the command `git branch -u origin/<branch-name>`
> * Never have to deal with it again: `git config --global push.autoSetupRemote true`. Git will automatically 

## Handling divergent remote history
Since we're working in a disconnected environment, divergent changes are guaranteed to happen eventually. The remote is
quite literally another branch that someone else might be commiting to (it just happends to reside on a different 
machine). So the strategies for handling those changes are the same as locally, aside from the need to call `git fetch`
from time to time and the new commands `git pull` and `git push`.

But we do have strategies to minimize problems...

### The painful way
Try this:
* On your local, edit a file. Save, stage, and commit.
* On GitHub, edit the _same_ file and commit that change.

Now try to push your changes:
```bash
$ git push
To github.com:johndoe/my-repo.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:johndoe/my-repo.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
Well, _that_ didn't work.

Git recognized that the remote has changes we haven't seen and stops us from proceeding. It then recommends a 
`git pull`. Remember that's a `git fetch` followed by a `git merge`. And _that's_ the secret sauce...

### How to avoid conflicts with the remote
The last thing an developer wants to deal with when ready to release the change they've worked so hard on is a conflict.
But conflicts _will_ occur. The best way to handle them is to proactively watch for them and guard against them when 
synchronizing our changes to the remote. To do this, we can regularly pull in changes from the remote, dealing with 
incoming changes as they occur and when it's convenient for us. To do this, we start by regularly running `git fetch`
and `git status`.

If we fall behind the remote, `git status` will tell us:
```bash
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" if you want to integrate the remote branch with yours)

nothing to commit, working tree clean
```
Maybe this isn't a good time to deal with this. Maybe it is. It would be helpful to know the incoming changes. Here's a
few options:
```bash
# Do this first to ensure you have the most up-to-date information
$ git fetch

# Show all the commits we're missing as a compact list
$ git log HEAD..origin/main --oneline
94f9d03 (origin/main, origin/HEAD) Add remote line change to bar.txt

# Show all the files modified by commits we're missing
$ git diff --name-only HEAD origin/main
bar.txt

# Show patch level detail of all the changes in the commits we're missing
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
Let's bite the bullet and try to pull in the changes from the remote:
```bash
$ git pull
Auto-merging bar.txt
CONFLICT (content): Merge conflict in bar.txt
Automatic merge failed; fix conflicts and then commit the result.
```
If we looked at the output above, we expected this. This works the same as with a local merge. Resolve the conflict, and
commit it. Remember, we're merging the changes on the remote into our local. We'll still need to get the changes into
the remote. Let's check status and do that now.
```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
$ git push
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 24 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 707 bytes | 707.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To github.com:johndoe/my-repo.git
   94f9d03..99c13df  main -> main
```
> [!NOTE]
> Though possible, it's unusual to ever merge from a local branch to a _different_ remote branch. The usual pattern is 
> to check out _both_ branches to local copies, perform merges locally, then push the merged changes back up to the 
> remote.

### Beyond vanilla Git: pull requests
TODO

---
[Home](./README.md) | [Previous: Branching out](./branching.md) | Next

Copyright © 2026 Aaron Craven.
