# Getting started
With the conceptual stuff out of the way, let's get our digital hands dirty. This section will focus on getting a 
local repository created for testing, doing a little basic configuration, and examining the result.

> [!NOTE]
> This guide assumes you store your repositories in a folder named `~/repos` (under Linux). In Windows, this might 
> be `C:\Users\<your-id>\repos`. Wherever you plan to store them, replace the directory names in the commands throughout
> this guide to match.

## Creating a local repository
There are two ways you can get a local repository on your machine. Create a new one or clone an existing one.

### Create a repository from scratch
You can create a repository from scratch using the `git init` command:
```bash
# You can initialize from inside the intended directory
$ mkdir -p ~/repos/my-repo
$ cd ~/repos/my-repo
$ git init

# or you can pass a directory path to the command
$ git init ~/repos/my-repo
$ cd ~/repos/my-repo
```
This will:
* Initialize the repository's hidden `.git` folder
* Create an initial (empty) default branch[^1] (typically `main` or `master`)

> [!IMPORTANT]
> Let's do this now. Open a terminal, and create a local test repository named `my-repo` using `git init`.

> [!NOTE]
> When initializing a repository, the repository directory does not have to be empty. Git will initialize it without 
> touching your files (assuming you don't already have a `.git` folder in the target directory).

### Clone an existing repository
You can use the command `git clone` to clone an existing repository. The `git clone` command takes one required argument
(a URL) and one optional argument (target directory, just as in `git init`). Git URLs can take one of several flavors:
* **HTTP** - a standard HTTP or HTTPS URL (e.g. `https://github.com/eagle79/git-bootcamp.git`)
  ```bash
  $ git clone https://server.com/path/to/my-repo ~/repos/my-repo
  ```
* **SSH** - one of a number of standard SSH connection formats (e.g. `git@github.com:eagle79/git-bootcamp.git`)
  ```bash
  $ git clone user@server.com:path/to/my-repo.git ~/repos/my-repo
  $ git clone ssh://user@server.com/path/to/my-repo.git ~/repos/my-repo
  ```
* **Local (filesystem)** - a local file path (e.g. `/path/to/repo.git`)
  ```bash
  $ git clone ~/repos/my-repo ~/repos/my-repo-copy
  ```
> [!NOTE]
> In practice, this URL (along with authentication configuration) will typically be provided to you. This guide does not
> go into detail about how to set up authentication or how to get the necessary URL.

This will:
* Download a copy of the commit history and object database (the `./.git` folder).[^2]
* Set up a default remote pointer named `origin` (we will discuss this more in [Branching out](./branching.md)).
* Check out the default branch (typically `main` or `master`) into your working directory.

We will look at this again in the [Collaborating](./collaborating.md) section.

## Repository & global configuration
The most important thing to configure for Git and any repository is commit identity information. This was mentioned on
the [home page](./README.md) of this guide, but we present it again here.

To set identity information globally (recommended):
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "you@example.com"
```

To override global configuration for the repository you are currently in:
```bash
$ git config user.name "Your Name"
$ git config user.email "you@example.com"
```

> [!IMPORTANT]
> Go ahead and set these values (either globally or locally in your `my-repo` repository).

There are a lot of configuration options that this guide does not explore. To list available configuration information,
you can execute:
```bash
$ git config --list
```

## Peeking under the hood
For the morbidly curious, take a look at the files and directories in `~/repos/my-repo/.git`. These files _are_ your
repository. A few highlights:
* `HEAD` - a text file containing a pointer to your current working context (usually the branch you're working on).
* `config` - configuration file (INI-style) containing repository-local settings.
* `index` - a binary file representing the staging area (more on this later).
* `objects/` - the content-addressable database; this directory contains the repository's content blobs, trees, commits,
   and tag objects.[^2]
* `refs/` - this directory is the reference store; it contains pointers to tracking branches, tags, and remotes.

---
[Home](./README.md) | [Previous: The Git mindset](./git-mindset.md) | [Next: Making changes](./making-changes.md)

Copyright © 2026 Aaron Craven.

[^1]: Technically an "unborn initial branch". This is explained in more detail later on.
[^2]: Technically a normal clone fetches all history reachable from the remote's advertised refs (active branches, tags,
    etc.). In practice, this is all content a user needs for most purposes.
