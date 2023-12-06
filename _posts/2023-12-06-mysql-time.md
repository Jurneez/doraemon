---
layout: post
category: mysql
---

获取当前时间：
<pre>
SELECT now(); -- 2023-12-06 15:39:28
</pre>
获取当前（秒级别）时间戳：
<pre>
SELECT unix_timestamp(now()); -- 1701848434
<pre>
（秒级别）时间戳转时间：
<pre>
SELECT FROM_UNIXTIME(1701848081); -- 2023-12-06 15:34:41
<pre>