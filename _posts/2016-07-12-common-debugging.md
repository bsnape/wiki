---
layout: post
title:  "Common Debugging"
date:   2016-07-12
categories: debugging
---
## Topics
- low disk space
- disks mounted/unmounted
- running processes
- open/closed ports
- DNS (nameservers)
- yum stuff


### Low disk space

The usual candidates for low disk space are `/tmp`, `/var` and `/home` so make
sure you check those first using `du` (display disk usage statistics). E.g.:

```
$ du -hs /tmp
4.0K	/tmp
```

Useful flags:

- `-h` - human readable (i.e. not in bytes)
- `-s` - summary


If you delete a large log file for example, the disk might not be freed up until you close the filehandlers

```
[root@deirdre-qa-vidispine-i46905acb ~]# lsof|grep deleted
less       1027     isakj    4r      REG              202,1 26089639936     665811 /var/log/vidispine/server.log (deleted)
java       3565  logstash   30r      REG              202,1 26089639936     665811 /var/log/vidispine/server.log (deleted)
```

Search for deleted things in `/proc` that have been deleted but are still open:

```
find /proc/*/fd -ls | grep  '(deleted)'
```

truncate with:

```
: > /proc/3565/fd/30
```

### yum stuff

- List installed repos `yum repolist`
- Yum directory `/etc/yum.repos.d/`
- logs to `/var/log/yum.log`
- search for rpm `yum list <rpm>`


# RPM stuff

Find out what RPM provides a file:

```
rpm -q --whatprovides <filename>
```

or

```
yum whatprovides <pattern>
```


`netstat -pant`
