---
layout: post
title:  "Various PHP related messages"
date:   2009-10-06 14:17:00 -0600
categories: blog
tags: October 2009 KU SOECS
---
ntwdblib.dll: This application has failed to start because ntwdblib.dll was not found. Re-install the application may fix this problem

Resolved:
Comment out the following lines from php.ini

``` bash
[PHP_MSSQL]
extension=php_mssql.dll
[PHP_PDO_MSSQL]
extension=php_pdo_mssql.dll
```

OCI.dll: This application has failed to start because OCI.dll was not found. Re-install the application may fix this problem

Resolved:
Comment out the following lines from php.ini

``` bash
[PHP_OCI8]
extension=php_oci8.dll
[PHP_PDO_OCI]
extension=php_pdo_oci.dll
[PHP_PDO_OCI8]
extension=php_pdo_oci8.dll
```

LIBPQ.dll: This application has failed to start because LIBPQ.dll was not found. Re-install the application may fix this problem

Resolved:
Comment out the following lines from php.ini

``` bash
[PHP_PDO_PGSQL]
extension=php_pdo_pgsql.dll
[PHP_PGSQL]
extension=php_pgsql.dll
```

sqlite3.dll: This application has failed to start because sqlite3.dll was not found. Re-install the application may fix this problem

Resolved:
Comment out the following lines from php.ini

``` bash
[PHP_PDO_SQLITE_EXTERNAL]
extension=php_pdo_sqlite_external.dll
```

aspell-15.dll: This application has failed to start because aspell-15.dll was not found. Re-install the application may fix this problem

Resolved:
Comment out the following lines from php.ini

``` bash
[PHP_PSPELL]
extension=php_pspell.dll
```

libcs.dll: This application has failed to start because libcs.dll was not found. Re-install the application may fix this problem

Resolved:
Comment out the following lines from php.ini

``` bash
[PHP_SYBASE_CT]
extension=php_sybase_ct.dll
```
