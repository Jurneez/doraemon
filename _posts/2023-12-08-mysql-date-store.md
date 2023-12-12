---
layout: post
title: "mysql 数据存储"
category: mysql 
---

## 1. 数据存储物理位置查询
以本机mysql为例，进入mysql，执行sql：
<pre>
mysql> show variables like 'datadir';
+---------------+--------------------------+
| Variable_name | Value                    |
+---------------+--------------------------+
| datadir       | /opt/homebrew/var/mysql/ |
+---------------+--------------------------+
1 row in set (0.00 sec)
</pre>

## 2. innodb存储引擎
innodb引擎用`idb`数据文件存储数据。

比如，test数据库中有people和people_age两个表，其存储方式如下：
<pre>
╭─user@xxx in /opt/homebrew/var/mysql on stable ✔
╰$ cd test
╭─user@xxx in /opt/homebrew/var/mysql/test on stable ✔
╰$ tree
.
├── people.ibd
└── people_age.ibd

1 directory, 2 files
</pre>

### 如何查询idb文件
执行命令：`ibd2sdi --dump-file people.txt people.ibd`

会生成一个people.txt文件，打开people.txt文件即可查看具体存储信息。

