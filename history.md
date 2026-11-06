# Versioning through the ages
Let's start at the beginning. In 1459, the first known second edition of _Psalterium Benedictinum_ was published by... 
Okay, maybe not _that_ far back...

Generally speaking, version control techniques for source code can be grouped into three basic paradigms.

## Local version control (1970s-1980s)
Source libraries were mostly kept on a single machine. Programmers would manually make copies as backups before
modifying the "official" source or would maintain "patch" files that described the differences between versions. This
culminated in tools like **SCCS (Source Code Control System)** and **RCS (Revision Control System)** that stored a
base file alongside a series of "deltas" that could be traced through to produce any version.

## Centralized version control (1990s-2000s)
Source files and history were stored on centralized servers. Developers would check out a file from the server, edit it
and commit (or "check in") the changes they made back to the server. These systems also popularized the concept of 
branching (divergent parallel development on the same codebase), but the process was often tedious and resolving 
conflicts when merging a branch back in could be difficult.

The primary tools of this era were **CVS (Concurrent Versions System)** and its improved successor 
**SVN (Subversion)**. A proliferation of commercial products were also created during this timeframe, including 
**VSS (Microsoft Visual SourceSafe)** and **Rational ClearCase (IBM)**. Many of the commercial products in this range 
used reserved checkout systems (where a developer would flag a file they were editing, preventing any other developer
from editing it until they checked it back in.)

## Distributed version control (2000s-present)
Keeping source code on a centralized server limited offline development. While many systems could work with the local
copies of files already on the machine, more complex operations (notably branching) required a connection to the server.

In 2000, the proprietary **BitKeeper** source control system was released, pioneering a peer-to-peer model where 
developers have a full clone of the entire source control repository on their machine, which they can fully work with. 
This system was used to manage Linux kernel development until the relationship broke down in 2005. **Git** was developed
as a free alternative and rapidly became the de facto choice for source control.

## Other notable systems
* **Mercurial** - a contemporary of Git also inspired by **BitKeeper**. It is still widely used (notably by Meta) and is
  conceptually very similar to **Git**.
* **Microsoft Team Foundation Version Control** - Enterprise source control tightly integrated with project management,
  issue tracking, and build pipelines in Visual Studio.
* **Perforce** - Version control optimized to handle giant codebases and binary assets. Dominant in game development
  where large amounts of binary data are common.
* **AccuRev** - A hybrid between centralized and distributed version control, AccuRev is notable for creating a unique
  "stream-based" architecture which treated branch-like development with CI/CD pipelines (e.g. 
  _Dev --> Testing --> Release_).

---
[Home](./README.md) | Previous | [Next: The Git mindset](./git-mindset.md)

Copyright © 2026 Aaron Craven.
