---
layout: post
title: Bash Learning
date: 2017-02-22 19:41:00 +0800
categories:
- Tech
tags:
- Bash

---

## Bash Keywords

### export

`export` makes the variable available to sub-processes. —— [Defining a variable with or without export](http://stackoverflow.com/questions/1158091/defining-a-variable-with-or-without-export)

### &

If a command is terminated by the control operator `&`, the shell executes the command asynchronously in a subshell. —— [Bash manual](http://www.gnu.org/software/bash/manual/bashref.html#Lists)

### wait & sleep

`wait` waits for a process to finish; `sleep` sleeps for a certain amount of time.

## Bash Using Scenarios 

### get exit code of background process

To get the exit code from variety process exit codes, you could save the exit code to a temp file like this: `echo "BUILD_CLIENT_STATUS=false" > /tmp/build_client_status.sh`. Then, get those exit codes by excute `. /tmp/build_client_status.sh`.





