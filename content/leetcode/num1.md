---
title: "LeetCode-1. Two Sum（两数之和）C++实现"
date: 2019-08-24T11:52:21+08:00
type: posts
categories: [LeetCode,C++]
keywords: [LeetCode,算法,C++]
tags: [LeetCode,C++]
---

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
<!--more-->
**示例:**
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```
下面给出三种解法的C++代码
#### 1. 第一种解法：暴力解法
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size(); i++) {
            for(int j = i + 1; j < nums.size(); j++) {
                if (nums[i] + nums[j] == target) {
                    return vector<int> {i, j};
                }
            }
        }
    }
};
```

**复杂度分析**
- 时间复杂度：O(n^2)。
- 空间复杂度：O(1)。
#### 2. 第二种解法：两遍哈希表
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> num_map;
        
        for (int i = 0; i < nums.size(); i++){
            num_map[nums[i]] = i;
        }
        
        for (int i = 0; i < nums.size(); i++){
            auto it = num_map.find(target - nums[i]);
            
            // 此处注意不要遗漏去重判断，两者不能为同一个数
            if (it != num_map.end() && it->second != i){
                return vector<int>{ i, it->second };
            }
        }
        
        return vector<int>();
    }
};
```
**复杂度分析**
- 时间复杂度：O(n)。
- 空间复杂度：O(n)。
#### 3. 第三种解法：一遍哈希表
```cpp
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) {
		unordered_map<int, int> num_map;

		for (int i = 0; i < nums.size(); i++){
			auto it = num_map.find(target - nums[i]);

			// 此处不需要去重判断，按照题目意思，数组中不存在重复数字
			// 因此在一次遍历的情况下，不会出现重复数字
			if (it != num_map.end()){
				return vector<int>{ it->second, i };
			}
			num_map[nums[i]] = i;
		}

		return vector<int>();
	}
};
```
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {}
        
        for i, v in enumerate(nums):
            if target-v in d:
                return [d[target-v], i]
            d[v] = i
            
        return []
```
**复杂度分析**
- 时间复杂度：O(n)。
- 空间复杂度：O(n)。

**原题地址**
英文版：https://leetcode.com/problems/two-sum/
中文版：https://leetcode-cn.com/problems/two-sum/
