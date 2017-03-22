
# Free memory

```
$ free -h
total       used       free     shared    buffers     cached
Mem:          7868       4264       3604          0        164       1289
-/+ buffers/cache:       2810       5057
Swap:            0          0          0
```

In Linux, "free" memory is generally a bad thing.

Ideally:

  - `free` memory should be close to 0
  - `used` memory should be close to `total`
  - `available`/`-/+ free` should have enough room (~20%)
  - `swap` `used` shouldn't change

Warning signs:

  - `available`/`-/+ free` approaches 0
  - `swap used` increases/fluctuates
  - `dmesg | grep oom-killer` shows the OOM killer at work

Linux borrows free memory for disk caching. This means when it reads something
from disk, it keeps it in memory to speed up subsequent reads.

If applications need more memory they take it back from the disk cache.

Disk caching only borrows the RAM that applications don't currently want.

# File/directory ownership

To change ownership of a file/directory:

```
$ chown user:group <file>
```

Use `-R` flag to recurse directories.

# File/directory permissions

```
$ chmod 0764 <file>
```

The octal corresponds to: special bit/user/group/other

The octal value is the combined value of:

  - `4` - read
  - `2` - write
  - `1` - execute
  - `0` - no permission

If you run `chmod +x <file>` it will make that file executable for all users.

# View file/directory permissions (in octal)

```
$ stat -c "%a %n" <file>

644 install.log
644 install.log.syslog
```

# Create a user

Create a user:

```
$ useradd <name>
```

This creates a user in a locked state.

We must create a password for that user:

```
$ passwd <name>
Changing password for user <name>.
```

Check it in `/etc/passwd`:

```
foo:x:1022:1022::/home/foo:/bin/bash
```

# Create a user without login permission

Point their bash to `/bin/false` or `/sbin/nologin` when creating the user.

```
$ useradd -s /sbin/nologin
```

Check it in `/etc/passwd`.

# Add an existing user to a groups

To add an existing to user to additional groups (without removing existing ones):

```
$ usermod -a -G <groups> <user>
```

# What is /etc/services ?

It maps port numbers (TCP/UDP) to running services.

```
$ cat /etc/services | less

tcpmux          1/tcp                           # TCP port service multiplexer
tcpmux          1/udp                           # TCP port service multiplexer
rje             5/tcp                           # Remote Job Entry
rje             5/udp                           # Remote Job Entry
echo            7/tcp
echo            7/udp
discard         9/tcp           sink null
discard         9/udp           sink null
systat          11/tcp          users
systat          11/udp          users
daytime         13/tcp
daytime         13/udp
qotd            17/tcp          quote
qotd            17/udp          quote
msp             18/tcp                          # message send protocol (historic)
msp             18/udp                          # message send protocol (historic)
chargen         19/tcp          ttytst source
chargen         19/udp          ttytst source
ftp-data        20/tcp
ftp-data        20/udp
# 21 is registered to ftp, but also used by fsp
ftp             21/tcp
ftp             21/udp          fsp fspd
ssh             22/tcp                          # The Secure Shell (SSH) Protocol
ssh             22/udp                          # The Secure Shell (SSH) Protocol
telnet          23/tcp
telnet          23/udp
```

# Load average

Use `uptime`/`top` to see load average details:

```
$ uptime

17:58:41 up 12:27,  1 user,  load average: 0.07, 0.05, 0.05
```

The numbers list the last 1/5/15 minutes of calculated load average.

The ideal load average is between 0 and 1 (for a single core system).

To view the number of cores:

```
$ nproc
2
```

To view details about each core:

```
$ cat /proc/cpuinfo
```

# Debug a network failure

Check TCP port is open - `telnet`:

```
$ telnet <host> <port>
```

Check network interfaces - `ifconfig`.

See if you can reach it (e.g. DNS issue) with `ping`:

```
$ ping <host>
```

See the path packets take to a destination using `traceroute`:

```
$ traceroute google.com
```

`netstat`

# Debug DNS issues

  1. Try to `ping` it (host and IP).
  2. Try `traceroute` to see if packets are being dropped.
  3. Check your configured nameservers - `/etc/resolv.conf`
  4. (Optional) check `dnsmasq` config - `/etc/dnsmasq.d/*`
  5. Test your nameservers with `nslookup`

```
$ nslookup -type=any <host> [<nameserver>]
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
Name:	vidispine.sit.phoenix.itv.com
Address: 10.205.80.163
Name:	vidispine.sit.phoenix.itv.com
Address: 10.205.81.33
```

# DNS record types

# DNS servers

BIND - Berkeley Internet Name Domain

# DNS forwarders

Dnsmasq (`dnsmasq`) allows you to set up forwarding rules for specific hostnames.

# TCP/IP stack (Internet protocol suite)

The Internet protocol suite model is a simpler model developed prior to the OSI model.

Four layers:

  - Application (highest)
    - DHCP, DNS, FTP, HTTP, IMAP, LDAP, NTP, SSL/TLS, SSH
  - Transport
    - TCP, UDP
  - Internet
    - IP, ICMP
  - Link
    - ARP, NDP, OSPF, Tunnels, MAC

# Open Systems Interconnection model (OSI model)

7.  Application layer
6.  Presentation layer
5.  Session layer
4.  Transport layer
3.  Network layer
2.  Data link layer
1.  Physical layer

# SSL vs TLS

SSL and TLS are cryptographic protocols that provide authentication and data encryption
between servers and applications over a network.

SSL 1 was never released publicly, SSLv2 (1995), SSLv3 (1996). The last SSL version (3.0) is now completely unsafe.

TLS was introduced in 1999 as the (incompatible) successor to SSL.
TLS 1.0 (1999), TLS 1.1 (2006) and TLS 1.2 (2008). TLS 1.3 is in draft.

They are protocols and completely independent of security certificates (often called SSL certificates).

# SHA vs MD5
https://www.google.co.uk/search?q=sha+vs+md5&oq=sha+vs+md5&aqs=chrome..69i57.2230j0j7&sourceid=chrome&ie=UTF-8

# View open connections

# What is a socket?

http://beej.us/guide/bgnet/output/html/singlepage/bgnet.html#theory

# SYN flood / SYN cookies

https://en.wikipedia.org/wiki/SYN_cookies

# File handle

https://www.google.co.uk/search?q=linux+filehandlers&oq=linux+filehandlers&aqs=chrome..69i57j0l5.9703j1j7&sourceid=chrome&ie=UTF-8#safe=active&q=file+handle+vs+file+descriptor

# File descriptor

# Disk IO

To monitor disk IO:

```
$ iotop -o

Total DISK READ :	0.00 B/s | Total DISK WRITE :      50.31 K/s
Actual DISK READ:	0.00 B/s | Actual DISK WRITE:      13.11 M/s
  TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND
25875 be/4 elastics    0.00 B/s   15.48 K/s  0.00 %  2.16 % java -Xms3782m -Xmx3782m -Djava.awt.headless=true -XX:+Use~work= -Des.default.path.conf=/etc/elasticsearch/es-logstash
25876 be/4 elastics    0.00 B/s   27.09 K/s  0.00 %  0.77 % java -Xms3782m -Xmx3782m -Djava.awt.headless=true -XX:+Use~work= -Des.default.path.conf=/etc/elasticsearch/es-logstash
25655 be/7 logstash    0.00 B/s    3.87 K/s  0.00 %  0.00 % java -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -Djava.awt.h~t -f /etc/logstash/conf.d -l /var/log/logstash/logstash.log
 4932 be/4 root        0.00 B/s    3.87 K/s  0.00 %  0.00 % dhclient -H phoenix-infraprd-aeuw1-elk-06a174de55a687ce3 -~nt/dhclient--eth0.lease -pf /var/run/dhclient-eth0.pid eth0
```

The `-o` flag displays only the processes actively using the disk.

To get stats for individual disks:

```
$ iostat

Linux 3.10.0-514.2.2.el7.x86_64 (phoenix-infraprd-aeuw1-elk-06a174de55a687ce3) 	02/21/2017 	_x86_64_	(2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
          15.31    0.27    4.30    1.14    0.03   78.96

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
xvda              0.91         6.27         9.69   18030135   27860685
xvdf             38.36        20.64      1129.84   59345375 3249222213
dm-0             40.93        20.61      1129.84   59281075 3249222213
```

Use the `-xd` flags to get extended information and a numerical value to refresh periodically (seconds):

```
iostat -xd 5

Linux 3.10.0-514.2.2.el7.x86_64 (phoenix-infraprd-aeuw1-elk-06a174de55a687ce3) 	02/21/2017 	_x86_64_	(2 CPU)

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
xvda              0.00     0.11    0.32    0.59     6.27     9.69    35.02     0.03   30.79   21.21   35.97   3.05   0.28
xvdf              0.00     2.57    0.50   37.86    20.64  1129.88    59.98     0.26    6.85   79.56    5.88   1.36   5.22
dm-0              0.00     0.00    0.50   40.43    20.61  1129.88    56.22     0.43   10.55   80.70    9.69   1.27   5.22
```

Here is an overview of the various columns:

https://linux.die.net/man/1/iostat
