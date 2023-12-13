---
layout: post
title: "Golang之图片安装原比例插入excel"
category: mysql 
---

```golang
package main

import (
	"fmt"

	_ "image/gif"
	_ "image/jpeg"
	_ "image/png"

	"github.com/360EntSecGroup-Skylar/excelize"
)

func main() {
	f := excelize.NewFile()

	// 插入图片
	// 工作表名称、单元格坐标、图片地址、图片格式（偏移、缩放、打印设置）
	// 图片左上角位置 对齐 A2单元格左上角 位置，保持图片原比例写入。
	if err := f.AddPicture("Sheet1", "A2", "11.png", ""); err != nil {
		fmt.Printf("添加图片错误：%s \n ", err.Error())
	}

	if err := f.SaveAs("tx.xlsx"); err != nil {
		fmt.Printf("保存图片错误：%s \n", err.Error())
	}
}
```

以上代码会使图片保持安装原比例写入，进而遮盖其他单元格内容

其实现效果如下：
<img src="../_screenshots/2023-12-13-golang-picture-insert-excel-01-display.png" />

