---
layout: post
title: "Emacs+git: Adding git identity to the emacs frame title"
date: 2014-01-16 22:25:30 -0500
comments: true
categories: [emacs, git, integration, bashrc, init.el]
---
I am a heavy emacs  person, so, when I started using Git, I really  wanted to see what Git
branch I am  working on, and in what  repository. To make this happen, you  will need both
<tt>.bashrc</tt> code as well as <tt>~/.emacs.d/init.el</tt> code.

First, the <tt>~/.bashrc</tt> code for setting up the environment variables:

``` sh bashrc code for setting up environment variables
PROMPT_COMMAND="export GIT_REPO_NAME=\$(git remote -v 2> /dev/null | grep \"origin.*fetch\" | awk '{print \$2}' | sed 's,https://github.com/.*/\(.*\).git,\1,g'); export GIT_BRANCH=\$(git branch 2> /dev/null | grep \"^*\" | awk '{
print \$2}')"
```

What the above lines give you is that they set up the environment variables <tt>GIT_BRANCH</tt> and <tt>GIT_REPO_NAME</tt>. You can use them in your <tt>~/.emacs.d/init.el</tt> like so:

``` lisp init.el code for setting up emacs frame title
;;; Rest of init.el code
(setq-default
  frame-title-format
  (concat 
   "%f"
   (if (getenv "GIT_BRANCH")
       (concat
	" in [ " (getenv "GIT_BRANCH") "/" (getenv "GIT_REPO_NAME") " ]"
	)
     )
   )
  )

;;; All the other goodies in init.el
```

There is one caveat, though. The frame title does not change as the working buffer changes
from one directory to another. I have to  figure out a way to constantly refresh the frame
title to reflect the current buffer's git identity/status.