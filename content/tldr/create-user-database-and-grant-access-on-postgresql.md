+++
author = "Alcadramin"
title = "TL;DR; Creating User, Database and Grant Access on PostgreSQL"
description = "There are a number of ways to grant access to PostgreSQL. This article will describe the basic ways and come up with an example."
tags = "PostgreSQL"
series = "TL;DR;"
categories = "TL;DR;"
specification = "TL;DR;"
keywords = ["postgresql", "create table", "create user"]
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
