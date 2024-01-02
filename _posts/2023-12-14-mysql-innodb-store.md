---
layout: post
title: "InnoDB引擎存储逻辑详解"
category: mysql 
---

日常工作中很多业务表结构都是用InnoDB存储的，了解其存储逻辑对后期学习索引会有很大帮助
## InnoDB引擎数据存储框架
innodb引擎存储逻辑如图：
![Alt text](https://github.com/Jurneez/doraemon/tree/main/_screenshots/2023-12-14-mysql-innodb-store.png){:.ioda}
其存储单位层级关系为：数据库——〉表空间——〉段——〉区——〉页——〉行

### 一、表空间
表空间是innodb逻辑结构的最高层，其所有数据都存放在表空间中。
表空间又分为：
    1. 系统表空间（共享表空间）
    2. 表文件表空间
    3. 撤销表空间
    4. 临时表空间
#### 1.1 系统表空间（共享表空间）
##### 存储内容
1. 存储innodb相关对象的元数
2. 双写缓冲（doublewrite buffer）
3. 数据、双写缓冲（doublewrite buffer）
4. undo日志（undo logs）
5. 用户在该表空间创建的表和索引
6. 等等

##### 存储位置
系统表文件默认存储在ibdata1数据文件中。
执行sql命令：`show variables like 'datadir';`查询数据存储的物理位置。ibdata1数据文件就在该目录下。
可以通过设置`innodb_data_home_dir`和`innodb_data_file_path`选项控制系统表空间数据文件的路径、大小、名称。
比如：
设置以下命令：
<pre>
innodb_data_home_dir不做设置
innodb_data_file_path=ibdata1:1024M;ibdata2:1024M:autoextend
</pre>
此时会在`datadir`路径下生成1024M大小的ibdata1和1024M大小的ibdata2

设置以下命令：
<pre>
innodb_data_home_dir=/tmp
innodb_data_file_path=ibdata1:1024M;ibdata2:1024M:autoextend
</pre>
此时生成的数据文件会位于`/tmp`路径下

如果需要为每个数据文件设置单独的路径，执行下面设置
<pre>
innodb_data_home_dir不做设置
innodb_data_file_path=/t1/ibdata1:1024M;/t2/ibdata2:1024M:autoextend
</pre>
此时，ibdata1和ibdata2分别位于`/t1` 和 `/t2`路径下。

#### 1.2 表文件表空间
默认情况下，用户创建的表和索引会存储在系统表结构的ibdata1数据文件中，当开启`innodb_file_per_table`字段时，引擎就会为每张表开一个单独的表空间，此时表空间为表文件表空间。
##### 存储位置
`datadir`路径下为每一个数据库创建一个文件夹，文件夹下为每一个表创建一个`.ibd`数据文件.

### 二、段
上面说的表空间由一个或多个段组成。
段又分为：数据段、索引段、回滚段；数据段存储B+树的叶子结点，索引段存储B+树的非叶子结点。