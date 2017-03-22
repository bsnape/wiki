# What did I learn today

# 22/2/17

* AWS `r` instance class - for memory intensive applications e.g. Elasticsearch
* Elasticsearch heap size should be set to ~50% of _available_ memory.
  * Giving elasticsearch too much memory is a bad thing.
* Snapshotting a `standard` (old, magnetic disk) EBS volume takes ages.
  * 300GB took ~3 hours
* Elasticsearch cluster health.
  * curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'

# 23/2/17

* EBS volumes
  * Persistent storage
* EBS snapshots
  * A backup of the EBS volume.
  * Each one is incremental - only the changed blocks since the last snapshot are saved.
  * New EBS volumes can be created from snapshots.
* How EBS volumes are mounted.
  * EBS volumes are tagged e.g. `Service = influx`
  * Puppet then mounts the relevant volume using a python script: `attachandmount.py`.
* Diagnosing low disk space
  * `df -h`
  * `du -hs <dir>`
  * `ls -laShr <dir>`

# Fri 24/2/17

* Although EBS volumes are stored incrementally, you can safely delete old ones!

# Monday 27/2/17

* Mounting EBS volumes using AWS tags (ITV-specific).
* Physical/virtual/logical volumes - needs more work.
* Ansible - no agents, code is run centrally and connects via SSH.
* Routing tables.
  * A set of rules (_routing entries_) used to determine where data packets traveling over an IP network will be directed.
  * Each packet contains information about its origin and destination.
  * When a packet is received, a network device examines the packet and matches it to the routing table entry providing the best match for its destination.
  * The table then provides the device with instructions for sending the packet to the next _hop_ on its route across the network.
  * The fewer entries in a routing table, the faster this decision process.
    * This helped lead to classless addressing (CIDR - Classless Inter Domain Routing).
* POSIX - Portable Operating System Interface.
  * A family of standards specified by the IEEE for maintaining compatibility amongst Unix-y operating systems.
  * This means that applications will (probably) work for OSX, Solaris, Linux (distro dependent) etc.
* What do `search` and `domain` mean in the `/etc/resolv.conf`?
  * http://unix.stackexchange.com/questions/128091/no-domain-defined-in-etc-resolv-conf

# Tuesday 28/2/17

* Let's Encrypt
  * Lets you create Domain Validated (DV) certificates but not Organisation Validation (OV) or Extended Validation (EV).
  * Wildcard certificates are also unsupported.
  * Certificates expire after 90 days.
  * Certificates are fully trusted by major browsers thanks to the CA IdenTrust who signed LE's intermediate certificates.
* When running Java applications, `Xms` means starting memory allocation and `Xmx` means max memory allocation.
  * It is usually best to set these two values as the same e.g. Elasticsearch, Solr.
* You can monitor the Elasticsearch heap size easily using Sensu.
  * e.g. warn on 80% full, critical on 90% full.
  * If the heap becomes full, garbage collection will kick in.

# Wednesday 1/3/17

* Configuring an email provider (e.g. Zoho) to use your own domain name (e.g. admin@bensnape.com).
  * You must verify your domain ownership with a CNAME (Canonical name) or TXT (Text) record pointing to Zoho.
    * CNAME specifies that a domain name is an alias for another domain (the "canonical" one).
    * TXT provides the ability to associate some arbitrary, unformatted text with a host. Commonly used for SPF records or DKIM.
  * You then must configure MX (Mail Exchanger) records pointing to the 3rd party mail server (e.g. Zoho) for emails to that domain.
  * For email verification you need to set up DKIM and SPF records to verify your ownership of the domain.
    * They are both used - independently of each other - to prevent spoofing.
    * DKIM (DomainKeys Identified Mail) embeds information within the email.
    * SPF (Sender Policy Framework) lets the recipient server verify the sending domain.
    * Some mail servers require just DKIM or SPF, or both.

# Thursday 2/3/17

* Correctly sizing the Java heap (for Elasticsearch).
  *
* DNS SOA (Start of Authority) record.
  * Every domain must have a SOA record at the point where the parent domain delegates to it.
  * Example: `ns1.dnsimple.com admin.dnsimple.com 1428493971 86400 7200 604800 300`.
    * `ns1.dnsimple.com` - the primary nameserver
    * `admin.dnsimple.com` - the responsible party for the domain
    * `1428493971` - timestamp of the last update
    * `86400` - seconds before the zone should be refreshed
    * `7200` - seconds before a failed refresh should be retried
    * `604800` - upper seconds limit before a zone is considered no longer authoritative.
    * `300` - negative result TTL
* Logstash
  * To prevent the logstash server (indexing instance) from being flooded, you can use a message broker as a buffer.
    * e.g. Redis, Kafka, RabbitMQ
    * https://www.elastic.co/guide/en/logstash/current/deploying-and-scaling.html#deploying-message-queueing
    * (ITV use) Redis for its speed, ease of use, and low resource requirements.


# Monday 6/3/17

* Redis
  * All data stored in memory.
  * Has a useful CLI - `redis-cli` (installed with Redis)
  * `brew install redis`
  * `redis-cli -h <redis-address>`
  * Get all information - `INFO`
  * Get list of databases - `INFO keyspace`
  * List keys - `KEYS *`
    * `KEYS` is dangerous because Redis will be unavailable to do other operations while it serves it.
    * Use [SCAN](https://redis.io/commands/scan) instead.
  * Switch database - `SELECT <index-starting-at-0>`

# Tuesday 7/3/17

* System journal (`journalctl`)
  * `-u <unit>` to view logs for a particular unit
  * `-x` to add message explanations where available (?)

# Wednesday 8/3/17

* File system types
  * XFS (CentOS 7 default) - cannot be shrunk?
  * ext4
* LVM - Logical Volume Manager
  * Physical Volume (PV) - this can be created on a whole physical disk (think `/dev/sda`) or a Linux partition.
  * Volume Group (VG) - this is made up of at least one or more physical volumes.
  * Logical Volume (LV) - this is sometimes referred to as the partition, it sits within a volume group and has a file system written to it.
  * File system - a filesystem (e.g. XFS, ext4) will be written on the logical volume.
* Shrinking an LVM partition
  * You must first decrease the filesystem within to avoid data corruption.
  * Shrinking an LV will give you more room in the VG.
    * You could then extend another LV within that VG to use the space.

# Monday 13/3/17

* EBS
  * Snapshots that were used to create volumes can safely be deleted.
* DMS (Database Migration Service)
  * Lets you migrate databases as one-offs or ongoing replications.
  * A replication instance is created.
  * You then configure source and target endpoints.
  * A task is then configured to facilitate the replication between the DBs.

todo
file handles
file descriptors
ulimit
routing tables
umask
