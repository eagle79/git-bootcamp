# Git tools
Throughout this guide, we've focused on interacting with Git via the command line. This was intentional. The best way
to understand Git is by knowing what commands are being run for each action. But that's hardly the end of the story.

Because of its widespread use and open nature, there are countless tools for working with Git repositories. Most IDEs
have built-in Git support, which can provide easy integration into your coding workflow. And there are stand-alone Git 
clients as well, providing support ranging from straightforward visualization and stage/commit workflows to AI-powered
coding assistance.

One of the best things about Git is that you can use any number of these tools (or none at all) on the same repository.
While different Git clients have different features, they're all working with the same underlying repository structure.
So if you prefer to handle code editing in [VSCode](https://code.visualstudio.com), researching historical changes in 
[Git Extensions](https://gitextensions.github.io/), and merging branches in 
[SourceTree](https://www.sourcetreeapp.com/), go for it! The world is your oyster! 

## Clients and stand-alone tools
Here are a few (non-IDE) tools to explore:

| Tool | Description | Platforms | License |
| ---: | :--- | :---: | :---: |
| [Fork](https://git-fork.com/) | Fast client known for clean interface and side-by-side diffs | ![Mac](./resources/os-icons/apple.png) ![Windows](./resources/os-icons/windows.png) | One-time purchase (free evaluation) |
| [GitExtensions](https://gitextensions.github.io/) | Feature-rich standalone client with Windows Explorer integration | ![Windows](./resources/os-icons/windows.png) | Open Source: GPL3 |
| [GitHub Desktop](https://github.com/apps/desktop) | Streamlined Git client with seamless GitHub integration | ![Mac](./resources/os-icons/apple.png) ![Windows](./resources/os-icons/windows.png) | Open Source: MIT |
| [GitKraken](https://www.gitkraken.com/) | Feature-rich client with highly visual interface and seamless collaboration tools | ![Linux](./resources/os-icons/linux.png) ![Mac](./resources/os-icons/apple.png) ![Windows](./resources/os-icons/windows.png) | Free tier (personal) + annual sub. (commercial/premium) |
| [SourceTree](https://www.sourcetreeapp.com/) | Feature-rich client with git-flow and Mercurial support with a simple interface | ![Mac](./resources/os-icons/apple.png) ![Windows](./resources/os-icons/windows.png) | Freeware |
| [Sublime Merge](https://www.sublimemerge.com/) | Built by the makers of Sublime Text. Includes high-performance rendering and syntax-highlighted diff tooling | ![Linux](./resources/os-icons/linux.png) ![Mac](./resources/os-icons/apple.png) ![Windows](./resources/os-icons/windows.png) | Nagware |
| [TortoiseGit](https://tortoisegit.org/) | Direct Windows Explorer integration | ![Windows](./resources/os-icons/windows.png) | Open Source: GPL2 |

## Git hosting platforms
While the most widely known Git hosting platform is [GitHub](https://github.com), there are many other tools. While not
truly a Git hosting platform, even Git itself has its own (rarely used) tool called Gitweb (a dated read-only web 
view of a repository). These platforms can offer a range of value-added features for Git, including Continuous 
Integration & Deployment (CI/CD) tools, issue tracking, project management, granular permissions management, and more. 
But probably the most important features are related to code review and collaboration workflows. At the core of these 
features is the concept of "pull requests".

### Pull/merge requests and branch protection
This feature is so important it deserves its own section! 

Pull requests grew out of the Linux kernel project, where Linus Torvalds needed a way for developers to propose changes
without giving them write access to the main repository. He wrote a command line script for developers that would 
automatically generate content for an email request with details (from their repository) on proposed changes. 
Maintainers could take the email and (if they approved it), literally `git pull` the changes from the proposer's 
repository URL.

In 2010, GitHub released a new version of its existing web-based code review interface based on the original process. 
Instead of emails, pull requests became an element of the hosted repository, providing a web page where contributors 
and maintainers could discuss the changes, examine them, and ultimately accept them.

While most systems continue to use this naming, [GitLab](https://gitlab.com) chose to name it "Merge Request" to reflect
the action being taken (as opposed to the commands being invoked). Whatever you call it, PRs are a major part of today's
development landscape and a core feature of most Git hosting platforms.

### Notable platforms
There are a number of notable Git hosting platforms available, with varying levels of features (and cost).

Here are a few:
|Platform|Description|License|
|---:|:---|:---:|
|[Bitbucket](https://bitbucket.org/product/)|Full-featured, cloud platform by Atlassian with extensive CI/CD tooling.|Subscription|
|[Forgejo](https://forgejo.org/)|Light-weight self-hosted platform with less extensive CI/CD tooling. Forgejo is a hard-fork of Gitea.|Open Source (GPL3+)|
|[Gitea](https://about.gitea.com/)|Light-weight platform with less extensive CI/CD tooling. Can be self-hosted or cloud-hosted.|Open Source (MIT) with premium-tier paid support|
|[GitHub](https://github.com/)|The heavyweight leader in the Git hosting platform space. Now owned by Microsoft. Extensive CI/CD tooling.|Freemium, with multiple premium tiers|
|[GitLab](https://about.gitlab.com/)|A heavy competitor to the commercial providers, GitLab has both a fully free base and an extensive paid tier.|Hybrid: CE - Open Source (MIT), EE - GitLab Enterprise Edition license|

---
[Home](./README.md) | [Previous: Collaborating](./collaborating.md) | Next

Copyright © 2026 Aaron Craven.
* <a target="_blank" href="https://icons8.com/icon/22989/microsoft">Microsoft</a> icon by <a target="_blank" href="https://icons8.com">Icons8</a>
* <a target="_blank" href="https://icons8.com/icon/17843/apple-inc">Apple Inc</a> icon by <a target="_blank" href="https://icons8.com">Icons8</a>
* <a target="_blank" href="https://icons8.com/icon/17842/linux">Tux</a> icon by <a target="_blank" href="https://icons8.com">Icons8</a>