---
title: Git version control
subtitle: Linux

# Summary for listings and search engines
summary: 

# Link this post with a project
projects: []

# Date published
date: '2022-06-04T00:00:00Z'

# Date updated
lastmod: '2022-06-04T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'featured.jpg'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin

tags:
  - Academic

categories:
  - programming languages
---


# Git


Git is a free and open source distributed version control system designed to handle projects of any size, from small to very large, quickly and efficiently.

Git is easy to learn, takes up little space, and has lightning-fast performance. It outperforms SCM tools like Subversion, CVS, Perforce, and ClearCase with features like cheap local branching, convenient staging areas, and multiple workflows.


## Branch and merge


(This is what gives Git an advantage over other version control systems)

The feature of Git that really sets it apart from almost all other SCMs is its branching model.

Git allows and encourages multiple local branches, which can be completely independent of each other. Creating, merging and deleting these development lines takes seconds.

This means you can do things like:

Seamless context switching. Create a branch to try out the idea, commit a few times, go back to where you branched from, apply a patch, go back to where you're experimenting and merge it.
Role-Based Codelines. Have a branch that always contains only what goes into production, another where you merge work into for testing, and a few smaller branches for day-to-day work.
Feature-based workflow. Create new branches for every new feature you work on so you can easily switch between them, then delete each branch when that feature is merged into your mainline.
Disposable Experiments. Create a branch for experimentation, realize that it won't work, and just delete it by abandoning the work and no one else will see it (even if you've pushed other branches in the meantime).
branches

Notably, when pushing to a remote repository, you don't have to push all your branches. You can share just one of your branches, a few of them, or all of them. This generally allows people to try out new ideas without having to worry about what they need to plan, how and when they are going to combine them or share them with others.

There are ways to do this with other systems, but the job is much more difficult and error prone. Git makes this process incredibly easy and changes the way most developers work when they learn it.
