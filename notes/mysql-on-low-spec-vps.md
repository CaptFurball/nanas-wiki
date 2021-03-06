---
title: MySQL on low spec VPS
description: 
published: true
date: 2020-07-27T00:21:08.151Z
tags: 
editor: undefined
dateCreated: 2020-07-05T04:59:30.297Z
---

In the attempt to setup two MySQL database docker containers on the cheapest DigitalOceans's VPS, I have found out that the MySQL container was crashing itself.

After identifying the issue, it is discovered that a single MySQL container uses ~300MB of RAM on idle.

## Why is that?

Although I am not entirely sure if this is the causation (correlation does not imply causation!), disabling the `Performance Schema` option seems to reduce the RAM usage significantly from ~300MB to just ~70MB.

> `Performance Schema` is a monitoring tool that comes with MySQL 8. Disabling it will also disable slow query logs as well as other useful tools for debugging MySQL. Please think twice before doing this.
{.is-info}

To disable it, modify `/etc/mysql/my.cnf` by adding the line below:

```
performance_schema=off
```

## Alternative

If the above option is not desirable, here are other options:

- Use only one DB container instance for all projects
- Host DB on dedicated DB instance
- Use other DB such as MongoDB or PostgreSQL (however I have not tried this)

## References

- https://mariadb.com/resources/blog/starting-mysql-on-low-memory-virtual-machines/
- https://lowendbox.com/blog/reducing-mysql-memory-usage-for-low-end-boxes/