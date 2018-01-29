# Using Github at Puppet

This document will help you adapt the fork-and-branch git workflow to working at Puppet. This approach favors command line usage over the Github webpage, by using some git aliases (that we will setup along the way) and a tool from Github called `hub` (that we will install as we go).

# Prerequisites

- A Github Login
- Github `hub` tool
- git

# Installation

# Git

## Windows

```
PS> choco install git
```

## OSX

```
$ brew install git
```

# hub

## Windows

```
PS> choco install hub
```

## OSX

```
$ brew install hub
```

## Setup

We're going to use the `hub` tool aliased to `git`, so we can call git at all times

# Workflow

At the core of this workflow, is the concept that the source of truth is *always* the puppetlabs repo. This means that the `origin` repo is *always* puppetlabs. This is important because it allows us to have a clear mental separation from the 'source of truth' and our fork.

This separation is also important because you are likely to have more than one remote configured, because you are likely going to have a branch for the work you are doing as well as a branch for reviewing other people's code.

We'll use the puppetlabs-dsc_lite repo as an example, where we will walk through making a fix.

## Step 1 Clone the repository

We'll use `git` (really `hub`) to clone the repo we want to work with.

```
> git clone puppetlabs/puppetlabs-dsc_lite
Cloning into 'puppetlabs-dsc_lite'...
remote: Counting objects: 20005, done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 20005 (delta 6), reused 10 (delta 3), pack-reused 19983
Receiving objects: 100% (20005/20005), 12.94 MiB | 6.71 MiB/s, done.
Resolving deltas: 100% (13504/13504), done.
> cd ~/src/puppetlabs/puppetlabs-dsc_lite
>
```

This works because `hub` takes 'puppetlabs/puppetlabs-dsc_lite' and converts it to 'https://github.com/puppetlabs/puppetlabs-dsc_lite' behind the scenes. No need to type out long urls on the command line.

Now that we have the puppetlabs/puppetlabs-dsc_lite repo we can look at the remotes

```
> git remote -v
origin	https://github.com/puppetlabs/puppetlabs-dsc.git (fetch)
origin	https://github.com/puppetlabs/puppetlabs-dsc.git (push)
```

At this point we see the `origin` remote is the puppetlabs remote. This will always be the case, as we regard the puppetlabs repo as the source of truth. Since this is the `fork-and-branch` workflow, we now must fork the repository. 

## Step 2 Fork the repository

> Note: Put your username instead of the username in the example here.

```
> git fetch jpogran
```

That's it! The `hub` tool knows what the `origin` of the repo we are working in, and knows how Github urls are made, so it knows to replace `puppetlabs` with `jpogran` and fork the https://github.com/puppetlabs/puppetlabs-dsc_lite to a new repo at https://github.com/jpogran/puppetlabs-dsc_lite. We can verify this by running the `git remote` command with the verbose switch.

```
> git remote -v
jpogran	https://github.com/jpogran/puppetlabs-dsc.git (fetch)
jpogran	https://github.com/jpogran/puppetlabs-dsc.git (push)
origin	https://github.com/puppetlabs/puppetlabs-dsc.git (fetch)
origin	https://github.com/puppetlabs/puppetlabs-dsc.git (push)
```

We now have a remote called `jpogran` (it will always be called the username of the fork). We will see a little bit later how this also helps us review work other contirbutors to the repo have. 

At this point we have a repo checked out and two remotes setup, one pointing to puppetlabs and one pointing to our fork. We've completed the `fork` part of the `fork-and-branch` workflow, let's now move onto the `branch`.

## Step 2 Create a branch to work in

The `fork-and-branch` workflow requires you to do all your work in git branches, which are merged into master when complete. At Puppet we require Pull Requests (PR) to merge branches into master, which have to have a PR review completed. First, let's create a branch:

```
> git checkout -b ticket/master/MODULES-1234-breaking-stuff
```

Here we used the `git checkout` command and passed the `-b` parameter. This tells git to create a branch called `ticket/master/MODULES-1234-breaking-stuff` and change the repo to that branch.

We'll simulate some work and then add it to a commit.

```
> touch awesome_fix.cs
> git add .
> git commit
# (MODULES-1234) Fixing stuff
#
# This commit fixes stuff by doing foo and bar to baz
```

By issuing `git commit` without any parameters, `git` opens up our default editor and asks us to add a commit message. The commit message is composed of two 'parts': a header and a body. The header always has 

> git push jpogran ticket/master/MODULES-1234-breaking-stuff
> git pull-request
> git pr show
```

# track a remote branch

```
> cd puppetlabs-dsc
> git checkout -t jpogran/ticket/master/MODULES-1234-breaking-stuff
```

# open github comparing two releases

```
> cd puppetlabs-sqlserver
> git compare 2.0.1..2.0.2
```