---
title: LeetCodeTop100
date: 2023-07-26
tags: ['星海']
categories: 星海
id: LeetCodeTop100
---

# 力扣TOP100

# 目录

- [哈希](#section.2)<a name="context.2"> </a>
- [双指针](#section.3)<a name="context.3"> </a>
- [滑动窗口](#section.4)<a name="context.4"> </a>
- [子串](#section.5)<a name="context.5"> </a>
- [数组](#section.6)<a name="context.6"> </a>
- [矩阵](#section.7)<a name="context.7"> </a>
- [链表](#section.8)<a name="context.8"> </a>
------------------------------------------------

<!-- more -->

[LeetCode 热题 100 - 学习计划 - 力扣（LeetCode）全球极客挚爱的技术成长平台](https://leetcode.cn/studyplan/top-100-liked/)



## [哈希](#context.2)<a name="section.2"> </a>

[1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

输入：nums = [3,2,4], target = 6
输出：[1,2]

输入：nums = [3,3], target = 6
输出：[0,1]

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
只会存在一个有效答案
*/
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ret;
        if (!nums.empty())
        {
            unordered_map<int, int> um;
            for (int i = 0; i < nums.size(); ++i) {
                auto it = um.find(target - nums[i]);
                if (it != um.end())
                {
                    ret = {it->second, i};
                    break;
                }

                um[nums[i]] = i;
            }
        }

        return ret;
    }
};

```



[49. 字母异位词分组 - 力扣（LeetCode）](https://leetcode.cn/problems/group-anagrams/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
字母异位词 是由重新排列源单词的所有字母得到的一个新单词。

输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]

输入: strs = [""]
输出: [[""]]

输入: strs = ["a"]
输出: [["a"]]

1 <= strs.length <= 104
0 <= strs[i].length <= 100
strs[i] 仅包含小写字母
*/
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> ret;
        if (!strs.empty())
        {
            unordered_map<string, vector<string>> um;
            for (auto str: strs)
            {
                string s(26, '0');
                for (auto c: str)
                    ++s[c - 'a'];
                
                um[s].emplace_back(str);
            }
            
            for (const auto& it: um)
                ret.emplace_back(it.second);
        }

        return ret;
    }
};
```



[128. 最长连续序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-consecutive-sequence/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
请你设计并实现时间复杂度为 O(n) 的算法解决此问题。

输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。

输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9

0 <= nums.length <= 105
-109 <= nums[i] <= 109
*/

class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int ret = 0;
        if (!nums.empty())
        {
            unordered_set<int> us(nums.begin(), nums.end());
            for (auto num: nums)
            {
                if (!us.count(num - 1))
                {
                    int cur = num;
                    int size = 1;
                    while (us.count(++cur))
                        ++size;

                    ret = max(ret, size);
                }
            }
        }

        return ret;
    }
};
```





## [双指针](#context.3)<a name="section.3"> </a>

[283. 移动零 - 力扣（LeetCode）](https://leetcode.cn/problems/move-zeroes/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
请注意 ，必须在不复制数组的情况下原地对数组进行操作。

输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]

输入: nums = [0]
输出: [0]

1 <= nums.length <= 104
-231 <= nums[i] <= 231 - 1
*/
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        if (nums.size() > 1)
        {
            int fast = 0;
            int slow = 0;
            for (; fast < nums.size(); ++fast)
            {
                if (nums[fast] != 0)
                    swap(nums[slow++], nums[fast]);
            }
        }
    }
};
```



[11. 盛最多水的容器 - 力扣（LeetCode）](https://leetcode.cn/problems/container-with-most-water/?envType=study-plan-v2&envId=top-100-liked)

![image-20230808221705531](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230808221705531.png)

```c++
/**
给定一个长度为 n 的整数数组 height 。有 n 条垂线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i]) 。
找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
返回容器可以储存的最大水量。
说明：你不能倾斜容器。

输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

输入：height = [1,1]
输出：1

n == height.length
2 <= n <= 105
0 <= height[i] <= 104
*/
class Solution {
public:
    int maxArea(vector<int>& height) {
        int ret = 0;
        if (height.size() > 1)
        {
            int left = 0;
            int right = height.size() - 1;
            int len;
            while (left < right)
            {
                len = right - left;
                ret = height[left] > height[right]
                      ? max(ret, len * height[right--])
                      : max(ret, len * height[left++]);
            }
        }

        return ret;
    }
};
```



[15. 3Sum - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请
你返回所有和为 0 且不重复的三元组。
注意：答案中不可以包含重复的三元组。

输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。

输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。

输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。

3 <= nums.length <= 3000
-105 <= nums[i] <= 105
*/
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ret;
        if (nums.size() > 2)
        {
            sort(nums.begin(), nums.end());
            for (int s = 0, e = nums.size() - 2; s < e; ++s)
            {
                if (nums[s] > 0)
                    break;
                else if (s > 0 && nums[s] == nums[s - 1])
                    ;
                else
                {
                    int left = s + 1;
                    int right = nums.size() - 1;
                    int sum;
                    while (left < right)
                    {
                        sum = nums[s] + nums[left] + nums[right];
                        if (sum > 0)
                            --right;
                        else if (sum < 0)
                            ++left;
                        else
                        {
                            ret.emplace_back(vector<int>{nums[s], nums[left], nums[right]});
                            while (++left < right && nums[left] == nums[left - 1]);
                            while (left < --right && nums[right] == nums[right + 1]);
                        }
                    }
                }
            }
        }

        return ret;
    }
};
```



[42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/?envType=study-plan-v2&envId=top-100-liked)

![image-20230808221849426](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230808221849426.png)

```c++
/**
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 

输入：height = [4,2,0,3,2,5]
输出：9

n == height.length
1 <= n <= 2 * 104
0 <= height[i] <= 105
*/
class Solution {
public:
    int trap(vector<int>& height) {
        int ret = 0;
        if (height.size() > 1)
        {
            int left = 0, hl = 0;
            int right = height.size() - 1, hr = 0;
            while (left < right)
            {
                hl = max(hl, height[left]);
                hr = max(hr, height[right]);
                ret += height[left] > height[right] ? hr - height[right--] : hl - height[left++];
            }
        }

        return ret;
    }
};
```





## [滑动窗口](#context.4)<a name="section.4"> </a>

[3. 无重复字符的最长子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-substring-without-repeating-characters/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
     
0 <= s.length <= 5 * 104
s 由英文字母、数字、符号和空格组成
*/
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ret = 0;
        if (!s.empty())
        {
            int fast = 0;
            int slow = 0;
            vector<int> arr(128, 0);
            for (; fast < s.size(); ++fast)
            {
                slow = max(slow, arr[s[fast]]);
                arr[s[fast]] = fast + 1;
                ret = max(ret, fast - slow + 1);
            }
        }

        return ret;
    }
};
```



[438. 找到字符串中所有字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。
异位词 指由相同字母重排列形成的字符串（包括相同的字符串）。

输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。

1 <= s.length, p.length <= 3 * 104
s 和 p 仅包含小写字母
*/
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> ret;
        vector<int> tar(26, 0);
        for (auto c: p)
            ++tar[c - 'a'];

        int fast = 0;
        int slow = 0;
        for (; fast < s.size(); ++fast)
        {
            --tar[s[fast] - 'a'];
            while (tar[s[fast] - 'a'] < 0)
                ++tar[s[slow++] - 'a'];

            if (fast - slow + 1 == p.size())
                ret.emplace_back(slow);
        }

        return ret;
    }
};
```



## [子串](#context.5)<a name="section.5"> </a>

[560. 和为 K 的子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的连续子数组的个数 。

输入：nums = [1,1,1], k = 2
输出：2

输入：nums = [1,2,3], k = 3
输出：2

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107
*/
/* 前缀 + 哈希 */
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int ret = 0;
        if (!nums.empty())
        {
            unordered_map<int, int> um;
            int pre = 0;
            um[0] = 1;
            for (auto n: nums)
            {
                pre += n;
                if (um.count(pre - k))
                    ret += um[pre - k];

                ++um[pre];
            }
        }

        return ret;
    }
};
```



[239. 滑动窗口最大值 - 力扣（LeetCode）](https://leetcode.cn/problems/sliding-window-maximum/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
返回 滑动窗口中的最大值 。

输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 
输入：nums = [1], k = 1
输出：[1]

1 <= nums.length <= 105
-104 <= nums[i] <= 104
1 <= k <= nums.length
*/
///< Method 1: 采用堆结构 priority_queue
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ret;
        if (!nums.empty())
        {
            priority_queue<pair<int, int>> pq;
            for (int i = 0; i < k; ++i) {
                pq.emplace(nums[i], i);
            }

            ret.emplace_back(pq.top().first);
            for (int i = k; i < nums.size(); ++i)
            {
                pq.emplace(nums[i], i);
                while (pq.top().second <= i - k)
                    pq.pop();

                ret.emplace_back(pq.top().first);
            }
        }
        
        return ret;
    }
};
/*
时间复杂度：O(nlog⁡n)，其中 n 是数组 nums 的长度。在最坏情况下，数组 nums 中的元素单调递增，那么最终优先队列中包含了所有元素，没有元素被移除。由于将一个元素放入优先队列的时间复杂度为 O(log⁡n)，因此总时间复杂度为 O(nlog⁡n)。

空间复杂度：O(n)，即为优先队列需要使用的空间。这里所有的空间复杂度分析都不考虑返回的答案需要的 O(n) 空间，只计算额外的空间使用。
*/

///< Method 2: 单调栈 deque
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ret;
        if (!nums.empty())
        {
            deque<int> dq;
            for (int i = 0; i < k; ++i) {
                while (!dq.empty() && nums[i] >= nums[dq.back()])
                    dq.pop_back();

                dq.emplace_back(i);
            }

            ret.emplace_back(nums[dq.front()]);
            for (int i = k; i < nums.size(); ++i)
            {
                while (!dq.empty() && nums[i] >= nums[dq.back()])
                    dq.pop_back();

                dq.emplace_back(i);
                while (!dq.empty() && dq.front() <= i - k)
                    dq.pop_front();
                
                ret.emplace_back(nums[dq.front()]);
            }
        }

        return ret;
    }
};
/*
时间复杂度：O(n)，其中 n 是数组 nums 的长度。每一个下标恰好被放入队列一次，并且最多被弹出队列一次，因此时间复杂度为 O(n)。
空间复杂度：O(k)，与方法一不同的是，在方法二中我们使用的数据结构是双向的，因此「不断从队首弹出元素」保证了队列中最多不会有超过 k+1 个元素，因此队列使用的空间为 O(k)
*/
```



[76. 最小覆盖子串 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-window-substring/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。

注意：
对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。
如果 s 中存在这样的子串，我们保证它是唯一的答案。

输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。

输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。

输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。

m == s.length
n == t.length
1 <= m, n <= 105
s 和 t 由英文字母组成
*/
///< Method 1: 滑动窗口
class Solution {
public:
    string minWindow(string s, string t) {
        string ret;
        if (s.size() >= t.size())
        {
            int need = 0;
            unordered_map<char, int> um;
            for (auto c: t)
            {
                --um[c];
                ++need;
            }

            int slow = 0, fast, start;
            int min_size = INT_MAX;
            deque<int> dq;
            while (slow < s.size() && !um.count(s[slow])) ++slow;
            for (fast = slow; fast < s.size(); ++fast)
            {
                if (um.count(s[fast]))
                {
                    need -= ++um[s[fast]] <= 0;
                    dq.emplace_back(fast);
                    while (need == 0)
                    {
                        if (fast - slow + 1 < min_size)
                        {
                            start = slow;
                            min_size = fast - slow + 1;
                        }

                        if (--um[s[slow]] < 0)
                            ++need;

                        dq.pop_front();
                        slow = dq.front();
                    }
                }
            }

            if (min_size != INT_MAX)
                ret = s.substr(start, min_size);
        }

        return ret;
    }
};
```



## [数组](#context.6)<a name="section.6"> </a>

[53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
子数组 是数组中的一个连续部分。

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。

输入：nums = [1]
输出：1

输入：nums = [5,4,-1,7,8]
输出：23

1 <= nums.length <= 105
-104 <= nums[i] <= 104
*/
///< Method 1: 动规（无后效性）
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int ret = 0;
        if (!nums.empty())
        {
            ret = nums[0];
            int pre = 0;
            for (auto num: nums) {
                pre = max(pre + num, num);
                ret = max(ret, pre);
            }
        }

        return ret;
    }
};
```



[56. 合并区间 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-intervals/description/?envType=study-plan-v2&envId=top-100-liked)

[189. 轮转数组 - 力扣（LeetCode）](https://leetcode.cn/problems/rotate-array/?envType=study-plan-v2&envId=top-100-liked)

[238. 除自身以外数组的乘积 - 力扣（LeetCode）](https://leetcode.cn/problems/product-of-array-except-self/description/?envType=study-plan-v2&envId=top-100-liked)

[41. 缺失的第一个正数 - 力扣（LeetCode）](https://leetcode.cn/problems/first-missing-positive/description/)

```c++
/**
给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。
请你实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案。

输入：nums = [1,2,0]
输出：3

输入：nums = [3,4,-1,1]
输出：2

输入：nums = [7,8,9,11,12]
输出：1

1 <= nums.length <= 5 * 105
-231 <= nums[i] <= 231 - 1
*/
///< Method 1: 在元素组上哈希
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int ret = 0;
        if (!nums.empty())
        {
            ret = nums.size() + 1;
            for (int i = 0; i < nums.size(); ++i) {
                if (nums[i] <= 0)
                    nums[i] = nums.size() + 1;
            }

            for (int i = 0; i < nums.size(); ++i) {
                int num = abs(nums[i]);
                if (num <= nums.size()) ///< num [1, N]
                    nums[num - 1] = -abs(nums[num - 1]);
            }


            for (int i = 0; i < nums.size(); ++i) {
                if (nums[i] > 0)
                {
                    ret = i + 1;
                    break;
                }
            }
        }

        return ret;
    }
};
```



## [矩阵](#context.7)<a name="section.7"> </a>

[73. 矩阵置零 - 力扣（LeetCode）](https://leetcode.cn/problems/set-matrix-zeroes/description/?envType=study-plan-v2&envId=top-100-liked)

[54. 螺旋矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]

m == matrix.length
n == matrix[i].length
1 <= m, n <= 10
-100 <= matrix[i][j] <= 100
*/
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> ret;
        if (!matrix.empty())
        {
            int r = matrix[0].size() - 1;
            int d = matrix.size() - 1;
            int l = 0;
            int u = 0;
            while (true)
            {
                for (int i = l; i <= r; ++i) ret.emplace_back(matrix[u][i]);
                if (++u > d)
                    break;

                for (int i = u; i <= d; ++i) ret.emplace_back(matrix[i][r]);
                if (--r < l)
                    break;

                for (int i = r; i >= l; --i) ret.emplace_back(matrix[d][i]);
                if (--d < u)
                    break;

                for (int i = d; i >= u; --i) ret.emplace_back(matrix[i][l]);
                if (++l > r)
                    break;
            }
        }

        return ret;
    }
};
```



[48. 旋转图像 - 力扣（LeetCode）](https://leetcode.cn/problems/rotate-image/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]

输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

n == matrix.length == matrix[i].length
1 <= n <= 20
-1000 <= matrix[i][j] <= 1000
*/
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        if (!matrix.empty())
        {
            int u = 0, d = matrix.size() - 1;
            int l = 0, r = matrix[0].size() - 1;

            while (true)
            {
                for (int i = l, j = u; i < r; ++i, ++j)
                    swap(matrix[u][i], matrix[j][r]);

                for (int i = l, j = r; i < r; ++i, --j)
                    swap(matrix[u][i], matrix[d][j]);

                for (int i = l, j = d; i < r; ++i, --j)
                    swap(matrix[u][i], matrix[j][l]);

                ++u;
                --d;
                ++l;
                --r;

                if (l >= r)
                    break;
            }
        }
    }
};
```



[240. 搜索二维矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/search-a-2d-matrix-ii/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：
每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true

输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false

m == matrix.length
n == matrix[i].length
1 <= n, m <= 300
-109 <= matrix[i][j] <= 109
每行的所有元素从左到右升序排列
每列的所有元素从上到下升序排列
-109 <= target <= 109
*/
///< Method 1: 矩阵向左旋转45度，转换为BST树(二叉搜索树)，左树小，右树大
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        bool ret = false;
        if (!matrix.empty() && matrix[0][0] <= target)
        {
            int col = matrix[0].size() - 1;  ///< 列
            int row = 0;  ///< 行
            while (col >= 0 && row < matrix.size())
            {
                if (matrix[row][col] < target)
                    ++row;
                else if (matrix[row][col] > target)
                    --col;
                else
                {
                    ret = true;
                    break;
                }
            }
        }

        return ret;
    }
};
```



## [链表](#context.8)<a name="section.8"> </a>

[160. 相交链表 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

listA 中节点数目为 m
listB 中节点数目为 n
1 <= m, n <= 3 * 104
1 <= Node.val <= 105
0 <= skipA <= m
0 <= skipB <= n
如果 listA 和 listB 没有交点，intersectVal 为 0
如果 listA 和 listB 有交点，intersectVal == listA[skipA] == listB[skipB]
*/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
///< Method 1: 双指针，两条路各走一遍
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *ret = nullptr;
        if (headA && headB)
        {
            ListNode *pa = headA, *pb = headB;
            while (pa != pb)
            {
                pa = pa == nullptr ? headB : pa->next;
                pb = pb == nullptr ? headA : pb->next;
            }

            ret = pa;
        }

        return ret;
    }
};
```



[206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
*/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
///< Method 1: 三指针赋值 pre cur next
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *ret = nullptr, *next;
        while  (head != nullptr)
        {
            next = head->next;
            head->next = ret;
            ret = head;
            head = next;
        }

        return ret;
    }
};
```



[234. 回文链表 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-linked-list/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。

链表中节点数目在范围[1, 105] 内
0 <= Node.val <= 9

进阶：你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？
*/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
///< Method 1: 存vector转用双指针法
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        bool ret = false;
        if (head != nullptr)
        {
            vector<int> arr;
            do
            {
                arr.emplace_back(head->val);
                head = head->next;
            } while (head != nullptr);
            
            int l = 0;
            int r = arr.size() - 1;
            while (l < r && arr[l] == arr[r])
            {
                ++l;
                --r;
            }
            
            ret = l >= r;
        }

        return ret;
    }
};

///< Method 2: 快慢指针 + 反转链表
class Solution {
private:
    ListNode *Reverse(ListNode *head)
    {
        // 已知非空，故不做判断
        ListNode *ret = nullptr, *next;
        while (head != nullptr)
        {
            next = head->next;
            head->next = ret;
            ret = head;
            head = next;
        }

        return ret;
    }

public:
    bool isPalindrome(ListNode* head) {
        bool ret = false;
        if (head != nullptr)
        {
            ListNode *fast = head;
            ListNode *slow = head;

            while (fast->next != nullptr && fast->next->next != nullptr)
            {
                fast = fast->next->next;
                slow = slow->next;
            }

            ListNode *temp = fast->next ? fast->next : fast;
            if (head->val == temp->val)
            {
                slow->next = Reverse(slow->next);
                while (temp != nullptr && head->val == temp->val)
                {
                    temp = temp->next;
                    head = head->next;
                }
                
                ret = temp == nullptr;
                slow->next = Reverse(slow->next);
            }
        }

        return ret;
    }
};
```



[141. 环形链表 - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给你一个链表的头节点 head ，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。
如果链表中存在环 ，则返回 true 。 否则，返回 false 。

进阶：你能用 O(1)（即，常量）内存解决此问题吗？
*/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
///< Method 1: 快慢指针
class Solution {
public:
    bool hasCycle(ListNode *head) {
        bool ret = false;
        if (head != nullptr)
        {
            ListNode *fast = head->next;
            ListNode *slow = head;
            while (fast != slow && fast)
            {
                fast = fast->next ? fast->next->next : fast->next;
                slow = slow->next;
            }

            ret = fast != nullptr;
        }

        return ret;
    }
};
```



[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
不允许修改 链表。

进阶：你是否可以使用 O(1) 空间解决此题？
*/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
///< Method 1: 哈希表
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *ret = nullptr;
        if (head != nullptr)
        {
            unordered_set<ListNode *> us;
            do
            {
                if (!us.count(head))
                {
                    us.emplace(head);
                    head = head->next;
                }
                else
                {
                    ret = head;
                    break;
                }
            } while (head);
        }

        return ret;
    }
};

///< Method 2: 快慢指针
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *ret = nullptr;
        ListNode *fast = head;
        ListNode *slow = head;
        while (fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;

            if (fast == slow)
            {
                while (head != fast)
                {
                    head = head->next;
                    fast = fast->next;
                }

                ret = head;
                break;
            }
        }

        return ret;
    }
};
```



[21. 合并两个有序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-sorted-lists/description/?envType=study-plan-v2&envId=top-100-liked)

```c++
/**
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

两个链表的节点数目范围是 [0, 50]
-100 <= Node.val <= 100
l1 和 l2 均按 非递减顺序 排列
*/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
///< Method 1: 暴力(略)
///< Method 2: 循环迭代 + 智能指针
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        unique_ptr<ListNode> ptr(new ListNode());
        ListNode *pre = ptr.get();
        while (list1 && list2)
        {
            if (list1->val < list2->val)
            {
                pre->next = list1;
                list1 = list1->next;
            }
            else
            {
                pre->next = list2;
                list2 = list2->next;
            }

            pre = pre->next;
        }
        
        pre->next = list1 ? list1 : list2;

        return ptr->next;
    }
};
```

