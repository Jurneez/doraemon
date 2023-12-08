---
layout: post
title: "mysql information_schema.tables详解"
category: mysql 
---

information_schema.tables表中记录当前mysql中所有表的基本信息。

### 表字段说明
<pre>
mysql> select * from TABLES where TABLE_SCHEMA = 'test' limit 1\G;
*************************** 1. row ***************************
  TABLE_CATALOG: def         -- 数据表登记目录
   TABLE_SCHEMA: test        -- 表所属的数据库名称
     TABLE_NAME: people      -- 表名称
     TABLE_TYPE: BASE TABLE  -- 表类型
         ENGINE: InnoDB      -- 表的存储引擎
        VERSION: 10          -- 版本，默认10
     ROW_FORMAT: Dynamic     -- 行格式
     TABLE_ROWS: 17          -- 表数据条数
 AVG_ROW_LENGTH: 963         -- 平均行长度 AVG_ROW_LENGTH = DATA_LENGTH / TABLE_ROWS
    DATA_LENGTH: 16384       -- 数据长度
MAX_DATA_LENGTH: 0           -- 最大数据长度
   INDEX_LENGTH: 0           -- 索引长度
      DATA_FREE: 0           -- 空间碎片
 AUTO_INCREMENT: NULL        -- 自增主键当前值
    CREATE_TIME: 2023-11-02 15:27:03 -- 表创建时间
    UPDATE_TIME: NULL                -- 表更新时间
     CHECK_TIME: NULL                -- 表检查时间
TABLE_COLLATION: utf8mb3_general_ci  -- 表字符校验编码集
       CHECKSUM: NULL                -- 校验和
 CREATE_OPTIONS:                     -- 创建选项
  TABLE_COMMENT: 人                  -- 表备注说明
1 row in set (0.00 sec)
</pre>

#### 1 Row Format行格式
Row Format行格式是指数据记录(或者称之为行)在磁盘中的物理存储方式。

有DEFAULT、FIXED、DYNAMIC、COMPRESSED、REDUNDANT（MySQL 5.0之前使用）、COMPACT（MySQL 5.0中被引入）等多种格式。

FIXED: 在mysql中， 若一张表里面不存在varchar、text以及其变形、blob以及其变形的字段的话，那么张这个表其实也叫静态表，即该表的row_format是fixed，就是说每条记录所占用的字节一样。其优点读取快，缺点浪费额外一部分空间。

DYNAMIC: 若一张表里面存在varchar、text以及其变形、blob以及其变形的字段的话，那么张这个表其实也叫动态表，即该表的row_format是dynamic，就是说每条记录所占用的字节是动态的。其优点节省空间，缺点增加读取的时间开销。

fixed--->dynamic: 这会导致CHAR变成VARCHAR

dynamic--->fixed: 这会导致VARCHAR变成CHAR