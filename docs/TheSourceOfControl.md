[Back to main](readme.md)

[TOC]

# A collapsible section with markdown
<details>
  <summary>Click to expand!</summary>
  
  ## Heading
  1. A numbered
  2. list
     * With some
     * Sub bullets
</details>

# The Source of Control
The term "Source control" is a general term for keeping track of a history of changes to the files in a project.  Most of the time, the files are all source code but images and other arbitrary files can be included.  For the most part, source control software is optimized to handle text files so keeping random binary files to a minimum is suggested.  Some other names for this type of software are,

- Source Control Management, SCM
- Version Control System/Software, VCS
- Revision Control

## Why?
Even when working alone on a project, there will likely be times when you are trying to get one feature working and find a critical bug.  Now you are stuck with a partially modified project and a bug that needs fixed before the new feature can be completed.  With source control, the new feature would be on a branch allowing you to switch back to the master branch, create a new branch from there to handle the bug fix, fix the bug, and merge the bug fix branch back to master.  Then, you can go back to the new feature branch and import the new changes.
When working as part of a team, this is especially useful as each person could be working on different features and have their own branches.  When one person is done, they submit a pull request and move on to the next feature or bug.  When the pull request is approved, it is merged to the master branch and anyone with open branches should pull the recent changes.

## Terminology
There is a pile of terms that are commonly used by most VCS.  The particular behavior of some of these changes slightly depending on the VCS but for the most part, the changes aren't 180 degrees.  You won't run into a case where one VCS defines True as False.

Repository
:    The repository is what holds all of the information for a project - every commit, comment, and any metadata associated with the repository.  Generally, there will be a hidden folder in the project's root folder that stores this data.  Usually, there's no reason for anyone to dig into that folder as making changes can easily break the repository.  Unless you have a backup or can somehow repair what broke, then all of the project's history is gone.  A repository is not a backup.

Fork
:    Let's say there's a project you like but has some quirks you don't.  You want to improve on that project but don't want to start from scratch.  You can copy the project at its current version and start making changes to your local repository.  When you make commits, they only get added to your local repository and the source of the fork does not see them.  Making the assumption you have a GitHub account, there's a handy `fork` link on repo pages that will put a copy of the repo into your account.  This preserves the parent/child relation making it easier for you to submit changes if you wanted to.  Also allows GitHub and others to see how many times (and by who) a repo was forked.  Kind of like a popularity count.

Clone
:    Downloading a copy of a repository to a local system is called cloning.  Where a fork is copying another repo to your online account, cloning doesn't create a copy of that repo on your account.  You can clone any public repo as many times as you feel like.

Up/Down Stream
:    Upstream would be closer to the authoritative main source.  In the fork example, the project you forked would be upstream.  If someone forked your project, they would be downstream.  If the upstream project makes some changes you'd like to have, you'd need to import them by pulling.

Branch
:    Branches are meant to isolate changes from other branches in the same repository.  Depending on the SCM, there can be some "special" branches that have some built-in meaning.  In general, there will be a master or default branch that would be the main branch.  There aren't any hard-set rules for branch names or how they are used.  There are plenty of conventions which behave as rules depending on the source control software being used.  Branches are used to separate units of work like a bugfix or single feature.  The naming scheme and how they are used mostly depends on the team doing the work.  The last project I worked on used abbreviated ticket names as the branches and had a set of procedures around default, develop, hotfix, and feature branches.
Continuing the fork example, now that you have your own version of the repo, you can start making changes.  But, in order to make it easier to get ahold of the last working version (master branch), you create a feature branch in which to do the new work.

Branch: Master
:    Generally, there would be a master or default branch which contains the absolute most up-to-date form of the code.  In most cases, the qualifier, "*functional*" is added to mean you'd be able to pick any commit on that branch and the software should work.  No incomplete features should be committed to the master branch.  Bug fix or feature branches can be started from this branch and merged back in only when they are complete.

Branch: Develop and Feature
:    If you've ever seen the change log of most software, they don't update the version number for every single change.  A version change usually includes many changes.  The develop branch would be started from the master branch and then feature branches would be started from the develop.  Imagine having a list of new features but only a couple of developers.  One at a time per developer (or team), each feature would be started from the develop branch.  Work would be done on those feature branches.  Once a feature is complete, the changes would be merged from the feature to the develop branch.  If any other feature branches exist, they would need to pull changes from the develop branch.

Branch: Bugfix/Hotfix
:    This is a special case as this usually means customers are affected.  These branches can be made from the master branch and merged back into master once completed.  And tested, of course.

Pull
:    Pulling changes from upstream is done to get new changes from the source.  This doesn't always "just work".  There are plenty of times this will result in conflicting changes where your local copy has a changed file and the upstream changed the same file in such a way the VCS is unable to automatically figure out how to resolve it.

Commit
:    Saving changes in called commiting.  The changes can be across multiple files, including adding and removing files.  Some source control software even track changes to file properties such as executable permissions.  There are many schools of thought about how much change should be in a single commit.  My personal preference is to limit changes to a couple sentences of description.  As in, if you cannot describe what changed in a couple of sentences, then there's too much going on.  Of course, life doesn't always work out that way so I've made my share of large commits.  One way to look at it, if you need to dig through the history to figure out what changed and why it was done in a particular way, then small steps would probably be easier to follow along.
When making a commit, pretty much every source control software allows including a comment.  This is where those couple of sentence descriptions would go.  Since there is likely a branch that is meant for a single feature, there's no need to go too in-depth in the comments.  As said earlier, just summarize what changed and possibly why.  If you are working on a team, imagine if someone else needed to look through the history and be able to understand it.

Pull Request
:    So you had started a branch to work on a new feature, made many commits as work progressed, tested the crap out of it, and are now done.  If the project only has you as the sole developer, you'd likely skip to the merge.  For teams, the next step is a pull request.  This pacakges all of the changes on the branch into one large batch of changes along with a full description of those changes.  For a pull request to be approved, it leads to a ...

Code Review
:    Reviewing pull requests is sometimes called a "code review".  Fancy name for someone other than the pull request author to check out the code, test it, read through it, verify it does what was needed, verify it meets coding style standards, etc.  Depending on how the team works, reviewing the changes could be a formal process involving the original author and one or more team members.  Or it could just be a non-author developer.  Regardless, the PR could be accepted right away or have comments added.  If not accepted, the author would then need to address the issues and update the PR.  Sometimes, the PR could be ultimately rejected if the reason behind the branch changes enough or if the solution presented is completely off base.  Of course, the ultimate goal of a PR is to lead to a ...

Merge
:    The bringing of changes from one branch to another is "merging".  #TODO: left off here.

Merge Conflict
:    This is the common name for when a VCS is unable to merge changes to a file because a local change and a remote change happened too close to each other.  How the VCS displays the conflict depends on the VCS and any preferences you've set.  The two common ways to show conflicts are side-by-side and unified.  The side-by-side is when the old and new versions are shown side-by-side, generally with some coloring to highlight the changes and conflicts.  The unified method is more difficult to get used to as it only shows a few lines before and after the change/conflict.  The advantage of side-by-side is the relative ease of seeing what is happening.  The advantage of unified is that it doesn't take up nearly as much space to display.  Regardless of how it is shown, the software leaves it up to the user to resolve the conflict.  This is done by picking which change to accept or by manually editing the file.



Push
:    tbd

## Git, Mercurial, and CVS, Oh My!
Personally, I've not heard much from anything except git and mercurial lately.  That doesn't mean the others are crap or even unpopular, just that I'm unobservant.  The actual software used doesn't really have too large of an impact on what they do.  All of them maintain a history of changes.  
All of them allow team members to work on things in a safe manner without stepping on other people's toes.  Where they differ is in philosphy and fiddly details of how certain steps are taken.

    :::bash
    brew install sourcetree

[Back to main](readme.md)
