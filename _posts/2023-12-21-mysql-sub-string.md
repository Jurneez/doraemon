---
layout: post
title: "mysql 中字符串截取的函数"
category: mysql 
---
### 常用字符串截取函数
1. left(str, length) ：从左开始截取指定长度
2. right(str, length) ：从右开始截取指定长度
3. substring(str, pos)：从第pos位开始截取之后的字符串，包含pos位
4. substring(str, pos, length): 从第pos位开始截取指定长度的字符串
5. substring_index(str, delim, count)：截取第count个关键字delim前后的数据

### 具体使用场景
1. 截取`https://www.runoob.com/html/html-tutorial.html`中的协议类型
> SELECT substring_index('https://www.runoob.com/html/html-tutorial.html', ":",1) -- 截取第一个:之前的内容
2. 截取`https://www.runoob.com/html/html-tutorial.html`中的页面文件后缀
> SELECT substring_index('https://www.runoob.com/html/html-tutorial.html', ".",-1) -- 截取倒数第一个.之后的内容

