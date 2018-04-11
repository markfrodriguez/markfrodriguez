+++
date = "2017-04-18T11:33:22-04:00"
description = ""
draft = false
slug = "mac-shell-env-vars"
tags = []
title = "Mac Terminal Shell Environment Variables Setup"
topics = []

+++

If not already there, create `.bash_profile` in `/Users/your_username`. Note, the preceding period in the filename.

Add the following content into the file. This is very specific for my environment / setup so adjust as necessary.

```
# configure multi-line prompt
PS1='
$PWD
===> '

#setup paths for Python
#PYTHONPATH=
#export PYTHONPATH

#setup path for Golang
GOPATH=~/workspace/goapps
export GOPATH

# specify preferred text editor
EDITOR=bbedit
export EDITOR

# general path munging
PATH=~/bin:/usr/local/bin:$GOPATH/bin:$PATH
export PATH

# The next line updates PATH for the Google Cloud SDK.
if [ -f ~/google-cloud-sdk/path.bash.inc ]; then source ~/google-cloud-sdk/path.bash.inc; fi

# The next line enables shell command completion for gcloud.
if [ -f ~/google-cloud-sdk/completion.bash.inc ]; then source ~/google-cloud-sdk/completion.bash.inc; fi

```



