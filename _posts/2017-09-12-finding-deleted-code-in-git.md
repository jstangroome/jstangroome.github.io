---
permalink: http://blog.stangroome.com/2017/09/12/finding-deleted-code-in-git/
title: Finding deleted code in git
description: None
date: 2017-09-12 11:38:44 -0000
last_modified_at: 2017-09-12 11:38:44 -0000
publish: true
pin: false
categories:
- Uncategorized
tags: []
---
Recently, [Matt Hilton](https://twitter.com/mjhilton_) blogged about [Source Control Antipatterns](https://www.mjhilton.net/2017/09/06/source-control-antipatterns/) which included the practice of commenting code instead of deleting the code. As wholeheartedly as I agree with deleting code, I know that a popular objection is that deleted code is harder to find. While it might be harder than your favourite editor's Find In Files feature, it is important to know how to use the tools central to your development workflow. For my work, and seemingly the majority of projects today, git is the version control tool of choice. So I'm sharing some git commands here that I have found useful for locating deleted code. I'm using the [Varnish Cache repository](https://github.com/varnishcache/varnish-cache) for my examples if you want to try them yourself. If you know some text from the code that was deleted, you can find the commit where it was deleted. In this example I'm looking for when the C structure named `smu` was deleted.
  
        $ git log -G "struct +smu" --oneline
    
    766dee0 Drop long broken umem code

If you know the name of a file that was deleted, but aren't sure which directory the file was in, you can find the commit when the file was deleted with:
  
        $ git log --oneline -- **/storage_umem.c
    
    766dee0 Drop long broken umem code
    b07c34f include cleanup - found by FlexeLint
    75615a6 When I grow up, I want to learn to program in C

  If you're not even sure what the deleted file was named but just want to see recent commits with deleted files, you can use:
  
        $ git log --diff-filter=D --summary
    
    commit f4faa6e3c431d6ccf581f5683af56008e4d4be10
    Author: Federico G. Schwindt <fgsch@lodoss.net>
    Date: Fri Mar 10 18:59:14 2017 +0000
    
    Fold r00936.vtc into vcc_action.c tests
    
    delete mode 100644 bin/varnishtest/tests/r00936.vtc

There is a lot more you can do with [git log](https://git-scm.com/docs/git-log) than just find deleted code but hopefully these examples are a useful start.
