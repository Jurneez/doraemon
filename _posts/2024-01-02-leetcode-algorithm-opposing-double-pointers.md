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

### 3.2 三数之和 (15)
[leetcode链接](https://leetcode.cn/problems/3sum/)

#### 解题说明
数组无序，可重复，结果不确定。
题目中，求三个数之和为0的数组，可以假定其中一个数的负数形式为target，求其他两个数相加等于target。可采用相向双指针的方法。
解题过程：
1. 题目中输入的数组无序，所以采用相向双指针前，需要先对数组进行排序。使：i < j < k,num[i] < num[j] < num[k]
2. for循环设置每一个元素为target = -1 * (num[i]),只需要求剩下数据两数相加是否等于target即可。

#### 代码实现
以下为go语言代码实现：
<pre>
func threeSum(nums []int) [][]int {
	// 数组升序排序
	// i < j < k
	// nums[i] < num[j] < num[k]
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})

	res := make([][]int, 0)
	for i := 0; i < len(nums)-2; i++ {
        if i >0 && nums[i] == nums[i-1]{  // 连续重复的数据，不需要再次计算
            continue 
        }

		target := 0 - nums[i]
        if target < 0 { // 如果nums[i] > 0,那么，num[j] 和 nums[k] 一定也大于0，后续未计算到的数据也都是大于0的，所以直接break
            break
        }

        j, k := i+1, len(nums)-1
		for {
			if j == k {
				break
			}
			t := nums[j] + nums[k]
			if t > target {
				k--
			} else if t < target {
				j++
			} else {
				res = append(res, []int{nums[i],nums[j],nums[k]})
                j++ // 结果不止一个，查出一个符合条件的数据后，继续查找下一个符合条件的数组
                for j <k {
                    if nums[j] > nums[j-1] { // 连续重复的数据，不需要再次计算
                        break
                    }else{
                        j++
                    }
                }
                
			}
		}
	}

	return res
}
</pre>