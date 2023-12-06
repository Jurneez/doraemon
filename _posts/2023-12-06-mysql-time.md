---
layout: post
category: mysql
---

获取当前时间：
```mysql
SELECT now(); -- 2023-12-06 15:39:28
```
获取当前（秒级别）时间戳
```mysql
SELECT unix_timestamp(now());; -- 1701848434
```