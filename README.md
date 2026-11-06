# Git bootcamp
An introduction to the basics of Git. (Packaged in a Git repository!)

## Introduction
This tutorial is designed for experienced developers who are new to Git. While we'll discuss command syntax, the focus
is on the way Git works to track changes in a flexible manner. 

## Setup
Before getting started, please ensure the system you're using has Git installed and configured. This guide will show
examples using POSIX-style syntax, but most commands (especially Git commands) will be similar on Windows. This guide
does not detail how to ensure the necessary tools are installed.

Verify and configure Git using the following commands:
```bash
# Check git installation
$ git --version
# You should see something like "git version 2.56.0.4"

# Set your "commit" identity globally
$ git config --global user.name "Your Name"
$ git config --global user.email "you@example.com"

# When in a repository, you can also use repository-specific values by omitting --global
$ git config user.name "Your Name"
$ git config user.email "you@example.com"
```

## Contents
||Section|Description|
|---:|:---|:---|
|1.|[Versioning through the ages](./history.md)|A _very_ brief history of version control.|
|2.|[The Git mindset](./git-mindset.md)|How Git differs from previous version control systems.|
|3.|[Getting started](./getting-started.md)|How to clone or create a repository to work with.|
|4.|[Day-to-day Git](./day-to-day-git.md)|Making and committing changes.|
|5.|[Branching out](./branching.md)|Parallel work with branches.|
|6.|[Collaborating](./local-and-remote.md)|Sharing work - local vs. remote|
|A1.|[Appendix: Terminology](./terminology.md)|Common terms|

---
## License
This educational content is licensed under a 
[Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

You are free to copy, distribute, translate, adapt, or build upon this material for any purpose (including 
commercially), provided you:
- **Provide attribution** - to the original author (Aaron Craven) and link to the license.
- **Indicate changes** - if you modified or built upon the material.
- **ShareAlike:** - release any derivative work under the same (CC BY-SA 4.0) license.
