---
layout: post
title:  "Databases"
date:   2016-07-29 07:39:30 +0100
categories: databases postgres
---

# Postgres

https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2

To get help on a command, prepend `\h` to it e.g.

```
$ psql -h db-infraprd.awseuwest1.infraprd.deirdre.itvcloud.zone -U monitor -c '\h CREATE ROLE'
Password for user monitor:
Command:     CREATE ROLE
Description: define a new database role
Syntax:
CREATE ROLE name [ [ WITH ] option [ ... ] ]

where option can be:

      SUPERUSER | NOSUPERUSER
    | CREATEDB | NOCREATEDB
    | CREATEROLE | NOCREATEROLE
    | CREATEUSER | NOCREATEUSER
    | INHERIT | NOINHERIT
    | LOGIN | NOLOGIN
    | REPLICATION | NOREPLICATION
    | BYPASSRLS | NOBYPASSRLS
    | CONNECTION LIMIT connlimit
    | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'
    | VALID UNTIL 'timestamp'
    | IN ROLE role_name [, ...]
    | IN GROUP role_name [, ...]
    | ROLE role_name [, ...]
    | ADMIN role_name [, ...]
    | USER role_name [, ...]
    | SYSID uid
```

## Permissions

Permissions are handled through roles.

To get a list of roles use `\du`:

```
$ psql -h db-infraprd.awseuwest1.infraprd.deirdre.itvcloud.zone -U monitor -c '\du'
Password for user monitor:
                                           List of roles
   Role name   |                   Attributes                   |            Member of
---------------+------------------------------------------------+----------------------------------
 monitor       | Create role, Create DB                        +| {rds_superuser,vidispine,portal}
               | Password valid until infinity                  |
 portal        |                                                | {}
 rds_superuser | Cannot login                                   | {}
 rdsadmin      | Superuser, Create role, Create DB, Replication+| {}
               | Password valid until infinity                  |
 rdsrepladmin  | No inheritance, Cannot login, Replication      | {}
 vidispine     |                                                | {}
 workflow      | Create DB                                      | {monitor}
```

### Create role

Use `CREATE ROLE <role>`

### Delete role

Use `DROP ROLE <role>`



## View database privileges

```
bensnape@C02KW512F6T6:~ $ psql -h db-infraprd.awseuwest1.infraprd.deirdre.itvcloud.zone -U monitor -c '\l'
Password for user monitor:
                                  List of databases
   Name    |   Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+-----------+----------+-------------+-------------+-----------------------
 monitor   | monitor   | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 portal    | portal    | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 postgres  | monitor   | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 rdsadmin  | rdsadmin  | UTF8     | en_US.UTF-8 | en_US.UTF-8 | rdsadmin=CTc/rdsadmin
 template0 | rdsadmin  | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/rdsadmin          +
           |           |          |             |             | rdsadmin=CTc/rdsadmin
 template1 | monitor   | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/monitor           +
           |           |          |             |             | monitor=CTc/monitor
 vidispine | vidispine | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
(7 rows)
```

## Show database tables

Use `\dt`. Make sure you specify the database whose tables you wish to see:

```
$ psql -h db-infraprd.awseuwest1.infraprd.deirdre.itvcloud.zone -U monitor -c '\dt' -d portal
Password for user monitor:
                                List of relations
 Schema |                         Name                          | Type  | Owner
--------+-------------------------------------------------------+-------+--------
 public | AnywhereKonnekt_anywhereconfig                        | table | portal
 public | AnywhereKonnekt_anywhereconfig_groups_allowed         | table | portal
 public | accesscontrol_accesscontrolentry                      | table | portal
 public | accesscontrol_accesscontrollist                       | table | portal
 public | activedirectory_activedirectorysettings               | table | portal
 public | activedirectory_activedirectorysettings_default_roles | table | portal
 public | archive_framework_aggregatearchivejob                 | table | portal
 public | archive_framework_archivejob                          | table | portal
 public | audittool_action                                      | table | portal
 ```

Or you can use:


```
SELECT
 *
FROM
 pg_catalog.pg_tables
WHERE
 schemaname != 'pg_catalog'
AND schemaname != 'information_schema';
```
