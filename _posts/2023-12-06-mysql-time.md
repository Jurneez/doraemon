---
layout: post
title: "mysql 时间格式之间的转换"
category: mysql 
---

##### 获取当前时间：
<pre>
SELECT now(); -- 2023-12-06 15:39:28
</pre>

##### 获取当前（秒级别）时间戳：
<pre>
SELECT unix_timestamp(now()); -- 1701848434
</pre>

##### （秒级别）时间戳转时间：
<pre>
SELECT from_unixtime(1701848081); -- 2023-12-06 15:34:41
SELECT from_unixtime(1701848081,'%Y-%m-%d'); -- 2023-12-06
</pre>

##### 时间转（秒级别时间戳）：
<pre>
SELECT unix_timestamp("2023-12-06"); -- 1701792000 , 2023-12-06 00:00:00
</pre>
