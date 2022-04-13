+++
author = "Alcadramin"
title = "TL;DR; Creating User, Database and Grant Access on PostgreSQL"
description = "TL;DR; Creating User, Database and Grant Access on PostgreSQL"
tags = ["PostgreSQL"]
series = ["TL;DR;"]
categories = ["TL;DR;"]
+++

Here is a simple snippet for creating user, database and grant access to that table on PostgreSQL.

> Valid as of 2022-04-12 - PostgreSQL 14.1.

```bash
$ sudo su postgres
$ psql
postgres=# CREATE DATABASE db;
postgres=# CREATE USER user WITH ENCRYPTED PASSWORD 'super-secret';
postgres=# GRANT ALL PRIVILEGES ON DATABASE db TO user;
```
