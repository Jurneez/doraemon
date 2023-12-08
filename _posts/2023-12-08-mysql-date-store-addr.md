---
layout: post
title: "mysql 数据存储物理位置查询"
category: mysql 
---

以本机mysql为例，进入mysql，查询sql：
<pre>
mysql> show variables like 'datadir';
+---------------+--------------------------+
| Variable_name | Value                    |
+---------------+--------------------------+
| datadir       | /opt/homebrew/var/mysql/ |
+---------------+--------------------------+
1 row in set (0.00 sec)
</pre>
