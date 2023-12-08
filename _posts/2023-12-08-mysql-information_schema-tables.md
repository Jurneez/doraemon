---
layout: post
title: "mysql information_schema.tables详解"
category: mysql 
---
information_schema.tables表中记录当前mysql中所有表的基本信息。

### 1. 表字段说明
<pre>
mysql> select * from TABLES where TABLE_SCHEMA = 'test' limit 1\G;
*************************** 1. row ***************************
  TABLE_CATALOG: def         -- 数据表登记目录
   TABLE_SCHEMA: test        -- 表所属的数据库名称
     TABLE_NAME: people      -- 表名称
     TABLE_TYPE: BASE TABLE  -- 表类型
         ENGINE: InnoDB      -- 表的存储引擎
        VERSION: 10          -- 版本，默认10
     ROW_FORMAT: Dynamic     -- 行格式，Compact|Dynamic|Fixed
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