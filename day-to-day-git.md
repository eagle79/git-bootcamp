# Day-to-day git
Let's dig into day-to-day interaction with git. For now, we'll ignore branching and remotes and concentrate just on 
making changes.

If you haven't already, create a local repository (`~/repos/my-repo` below) using `git init` as discussed in the 
[Getting started](./getting-started.md) section.

## Understanding file states
Git defines three primary states/areas for file changes to reside:
* **Working directory** - These are the files you directly interact with. After checking out (or creating a repository),
  these files will be located in the repository folder (all other states are stored inside the hidden `.git/` folder).
  You work with these files in the normal manner (text editor, IDE, etc.): creating, modifying, renaming, and deleting
  as necessary. Changing a file or directory here will cause it to move into the "modified" state.
* **The index (staging area)** - Provides a "staging area" called the "index" for assembling a desired change (though
  technically the term "index" is more correct, most users call this the "staging area" or just "staging"). The staging
  area is a planning tool for commits. As you modify files for a given change, you can add/remove those modifications 
  to/from the index (this is called "staging" the changes/files). Or if you are working on multiple (non-overlapping)
  changes, you can stage all the files for one change and commit that change only.
* **The commmit** - Official record of changes. Once created, a commit cannot be modified[^1].

## Checking repository status
TODO

## What changed?
TODO

## Staging changes
TODO

## Committing changes
TODO

## Undoing local mistakes
TODO

[^1]: While technically true that commit _objects_ cannot be modified, there _are_ techniques for "rewriting history". 
    These techniques do not, however modify the commit object, but create _new_ commit objects and change how they
    are linked in the graph.
---
[Home](./README.md) | [Previous:]() | [Next:]()