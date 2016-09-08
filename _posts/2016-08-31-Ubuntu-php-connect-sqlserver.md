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

## 使用链接服务器

如果链接sqlserver后，使用了链接服务器就需要设置参数，否则会提示错误

**Error: Heterogeneous queries require the ANSI_NULLS and ANSI_WARNINGS options to be set for the connection**

在每次执行前需要设置以下参数。

```
SET ANSI_NULLS ON ;
SET ANSI_WARNINGS ON;

```
如果需要在codeigniter中使用，需要修改mssql_driver.php,修改 function db_connect(),增加执行的sql语句

```

$this->simple_query("SET ANSI_NULLS ON ;SET ANSI_WARNINGS ON;");
$query = $this->query('SELECT CASE WHEN (@@OPTIONS | 256) = @@OPTIONS THEN 1 ELSE 0 END AS qi');

```

## 最后

如果你使用docker,可以使用[https://github.com/ngineered/nginx-php-fpm](https://github.com/ngineered/nginx-php-fpm)，
在dockerfile中增加 **php5-mssql** 就可以正常使用了！！！

