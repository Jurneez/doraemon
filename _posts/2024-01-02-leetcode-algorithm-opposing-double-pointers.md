---
layout: post
title: "算法之相向双指针"
category: leetcode_algorithm 
---

## 1. 描述
定义两个指针，分别从链表的头和尾一起向中间遍历。

## 2. 使用场景
- 两数之和
- 翻转类型
- 分割类型

## 3. 经典案例
### 3.1 两数之和 II - 输入有序数组（167）
[leetcode链接](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

####  解题说明
有序数组，必有且仅有一对 两数相加等于target。采用相向双指针，定义两个指针，left_index指向最小数，right_index指向最大数，使 最大数+最小数：
1. 如果大于target，缩小最大数
2. 如果小于target，增大最小数
3. 如此循环，直到 等于target。

#### 代码实现
以下为go语言代码实现：
<pre>
func twoSum(numbers []int, target int) []int {
	left, right := 0, len(numbers)-1
	result := make([]int, 0)
	for {
		if left == right {
			break
		}
		if numbers[left]+numbers[right] > target {
			right--
		} else if numbers[left]+numbers[right] < target {
			left++
		} else {
			break
		}
	}
	result = append(result, left+1)
	result = append(result, right+1)
	return result
}
</pre>

#### 时间/空间复杂度
时间复杂度：O(n)
空间复杂度：O(1)