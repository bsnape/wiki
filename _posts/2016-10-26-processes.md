---
layout: post
title:  "Processes"
date:   2016-10-26 12:39:30 +0100
categories: processes
---
# Jobs and Processes

Lots of processes run:

```
$ ps aux | wc

249    3601  110605
```

Users that launch commands from a shell environment are called jobs.

For example, start the following job which essentially copies nothing to nowhere:

```
$ dd if=/dev/zero of=/dev/null
```

N.B.
> /dev/zero is a special file in Unix-like operating systems that provides as many null characters (ASCII NUL, 0x00) as are read from it. One of the typical uses is to provide a character stream for initializing data storage.

> /dev/null redirects the command standard output to the null device, which is a special device which discards the information written to it.

You can see that the foreground job occupies the terminal because it is busy.

We can move the job to the background by first sending the `SUSP` character with `CTRL-Z`.

```
$ dd if=/dev/zero of=/dev/null
^Z
[1]+  Stopped                 dd if=/dev/zero of=/dev/null
```

We can then move the job to the background with `bg`.

```
$ bg
[1]+ dd if=/dev/zero of=/dev/null &
```

You can view the currently running jobs by running `jobs`.

```
$ jobs
[1]+  Running                 dd if=/dev/zero of=/dev/null &
```

Jobs are always related to the current shell environment. If you type `jobs` you
will only see jobs related to the current user in the current shell environment.

Processes are the total amount of tasks running in the system no matter how they
were started.

# Managing shell jobs

Every command that you start from a shell is considered a shell job (e.g. `ls`).

Many jobs run and finish quickly.

Long running jobs can be moved to the background as we saw above.

But you can also move jobs from the background to the foreground.

```
$ sleep 600
^Z
[2]+  Stopped                 sleep 600

$ bg
[2]+ sleep 600 &

$ jobs
[1]-  Running                 dd if=/dev/zero of=/dev/null &
[2]+  Running                 sleep 600 &

$ fg 2
sleep 600
```

You can also explicitly run a job in the background by including ampersand (`&`)
at the end.

```
$ sleep 600 &
[1] 80257

$ jobs
[1]+  Running                 sleep 600 &
```

# Getting process information with `ps`

To view all processes and jobs, run `ps`.

A user will only see his processes, unless he is root.

```
# ps aux|head
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0  19360  1528 ?        Ss   14:46   0:02 /sbin/init
root         2  0.0  0.0      0     0 ?        S    14:46   0:00 [kthreadd]
root         3  0.1  0.0      0     0 ?        S    14:46   0:17 [migration/0]
root         4  0.0  0.0      0     0 ?        S    14:46   0:00 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S    14:46   0:00 [stopper/0]
root         6  0.0  0.0      0     0 ?        S    14:46   0:09 [watchdog/0]
root         7  0.0  0.0      0     0 ?        S    14:46   0:06 [migration/1]
root         8  0.0  0.0      0     0 ?        S    14:46   0:00 [stopper/1]
root         9  0.0  0.0      0     0 ?        S    14:46   0:00 [ksoftirqd/1]
```

- `USER` is the user who owns the process
- `PID` is the process ID
- `%CPU` is the percentage of the system CPU the process is using
- `$MEM` is the percentage of the system memory the process is using
- `VSZ` (Virtual Memory Size) is the amount of memory that the process can access
- `RSS` (Resident Set Size) is the amount of memory that has been allocated in RAM
- `TTY` is the terminal that the process is running on (`?` denotes background)
- `STATUS` is the current [status](#process_state_codes) of the process
- `START` is when the process started
- `TIME` is how long the process has been running
- `COMMAND` is the associated command

See [this](http://stackoverflow.com/questions/7880784/what-is-rss-and-vsz-in-linux-memory-management)
to learn more about VSZ and RSS.

To view the relationship between parent and child processes, use `ps fax`

```
# ps fax
```

## Process State Codes

Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display to describe the state of a process:
```
D    uninterruptible sleep (usually IO)
R    running or runnable (on run queue)
S    interruptible sleep (waiting for an event to complete)
T    stopped, either by a job control signal or because it is being traced.
W    paging (not valid since the 2.6.xx kernel)
X    dead (should never be seen)
Z    defunct ("zombie") process, terminated but not reaped by its parent.
```

For BSD formats and when the stat keyword is used, additional characters may be displayed:
```
<    high-priority (not nice to other users)
N    low-priority (nice to other users)
L    has pages locked into memory (for real-time and custom IO)
s    is a session leader
l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
+    is in the foreground process group.
```

# Useful commands

## `ps`

### Useful flags

* `aux`
* `ef`
*

```
ps fax
```

view parent and child processes.
