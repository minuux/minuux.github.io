---
layout: post
title:  "Ubuntu php connect sqlserver"
date:   2016-08-31
categories: notes
---

## 安装freetds


```
apt-get install php5-sybase freetds-common tdsodbc libsybdb5

```

## 配置freetds
编辑 /etc/freetds/freetds.conf,

```
[global]
	tds version = 8.0
	client charset = UTF-8

```

## codeigniter

使用codeigniter连接的时候，也许需要修改下 **codeigniter/system/database/drivers/mssql/mssql_driver.php**中的_version函数
。
<https://github.com/bcit-ci/CodeIgniter/issues/4306>，如果不设置返回结果的类型（比如varchar），也许会报错。

```
A PHP Error was encountered

Severity: Warning

Message: mssql_query(): column 1 has unknown data type (98)

Filename: mssql/mssql_driver.php

Line Number: 178

```
调整函数为:

```
protected function _version()
	{
		return "SELECT left(cast(serverproperty('productversion') as varchar), 4) AS ver";
	}

```