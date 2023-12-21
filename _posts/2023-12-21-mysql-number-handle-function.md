---
layout: post
title: "mysql 中常用的处理数据的函数"
category: mysql 
---

#### 1. FLOOR(X) 向下整数

>select FLOOR(1.9); -- 1
>
>select FLOOR(2.1); -- 2

#### 2. CEIL(X)、CEILING(X) 向上取整

>select CEIL(1.9); -- 2
>
>select CEIL(2.1); -- 3

#### 3. ROUND(X) 四舍五入取整

>select ROUND(1.9); -- 2
>
>select ROUND(2.1); -- 2

#### 4. TRUNCATE(X,D) 数据截断：保留D位小数

>select TRUNCATE(1.9126343, 3); -- 1.912
>
>select TRUNCATE(2.1, 3); -- 2.1

#### 5. ROUND(X,D) 四舍五入：保留D位小数

>select ROUND(1.9126343, 3); -- 1.913
>
>select ROUND(2.1, 3); -- 2.1


