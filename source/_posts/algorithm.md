---
title: 算法(c++)
date: 2023-07-26
tags: ['星海']
categories: 星海
id: 算法(c++)
---

# 算法（c++）

# 目录
- [字符串](#section.1)<a name="context.1"> </a>
  - [KMP算法](#section.2)<a name="context.2"> </a>
  - [Manacher算法（马拉车）](#section.3)<a name="context.3"> </a>
  - [不重复最长子串](#section.4)<a name="context.4"> </a>
- [快排](#section.5)<a name="context.5"> </a>
- [回溯](#section.6)<a name="context.6"> </a>
- [递归法](#section.7)<a name="context.7"> </a>
- [贪心](#section.8)<a name="context.8"> </a>
------------------------------------------------

<!-- more -->

## [字符串](#context.1)<a name="section.1"> </a>

### [KMP算法](#context.2)<a name="section.2"> </a>

用于字符串匹配。给你两个字符串，寻找其中一个字符串是否包含另一个字符串，如果包含，返回包含的起始位置。



**求前缀表**

```c++
/* 求前缀表（最长公共子串） */
void getNext (int* next, const string& s){
    next[0] = 0;
    int j = 0;
    for(int i = 1;i < s.size(); i++){
        while(j > 0 && s[i] != s[j]) {
            j = next[j - 1];
        }
        if(s[i] == s[j]) {
            j++;
        }
        next[i] = j;
    }
}
```



相关例题：

[KMP 459. 重复的子字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/repeated-substring-pattern/description/)

```c++
/* 给定一个非空的字符串 s ，检查是否可以通过由它的一个子串重复多次构成。 */
/** 字符串 459 重复的子字符串 */
class Solution {
private:
    void GetNext(string &s, int *next)
    {
        next[0] = 0;
        int j = 0;  ///< j 为前缀末尾, i 为后缀末尾
        for (int i = 1; i < s.size(); ++i) {
            while (j > 0 && s[i] != s[j])
                j = next[j - 1];

            if (s[i] == s[j])
                ++j;

            next[i] = j;
        }
    }

public:
    bool repeatedSubstringPattern(string s) {
        bool ret = false;
        if (!s.empty())
        {
            int next[s.size()];
            GetNext(s, next);

            int len = s.size();
            ret = next[len - 1] != 0 && (len % (len - (next[len - 1]))) == 0;
        }

        return ret;
    }
};
```



[KMP 28. 找出字符串中第一个匹配项的下标 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

```C++
class Solution {
private:
    void Print(vector<int> &arr)
    {
        for (auto n: arr)
            cout << n << " ";
        cout << endl;
    }

public:
    int strStr(string haystack, string needle) {
        int ret = -1;
        if (!haystack.empty() && !needle.empty())
        {
            vector<int> next(needle.size(), 0);
            for (int i = 1, j = 0; i < needle.size(); ++i) {
                while (j > 0 && needle[i] != needle[j])
                    j = next[j - 1];

                if (needle[i] == needle[j])
                    ++j;

                next[i] = j;
            }

            for (int i = 0, j = 0; i < haystack.size(); ++i)
            {
                while (j > 0 && haystack[i] != needle[j])
                    j = next[j - 1];

                if (haystack[i] == needle[j] && ++j == needle.size())
                {
                    ret = i - j + 1;
                    break;
                }
            }
        }

        return ret;
    }
};

```



### [Manacher算法（马拉车）](#context.3)<a name="section.3"> </a>

一个高效匹配回文子串长度的算法，他可以在o(n)的时间复杂度中计算出一个字符串的回文子串的长度



[5. 最长回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-substring/description/)

```c++
/** 字符串 5 最长回文子串 马拉车算法 Manacher */
class Solution {
public:
    string longestPalindrome(string s) {
        string t = "*#";
        for (auto c: s)
        {
            t += c;
            t += "#";
        }

        t += "!";
        vector<int> p(t.size(), 0);  ///< p 用来记录各点的回文半径
        int r = 0;           ///< 某个回文串延伸到的最右边下标
        int mid = 0;         ///< r 所属回文串中心下标
        int max_mid = 0;     ///< 遍历过的最大回文串中心下标
        int max_rad = 0;     ///< 遍历过的最大回文半径
        for (int i = 1, end = t.size() - 1; i < end; ++i)
        {
            // 如果 i 还在区间内, 则此处半径为 i 关于 mid 对称点 j 处的半径(不超过左边界)
            p[i] = i < r ? min(p[2 * mid - i], r - i) : 1;  ///< 重要!!! 需理解!!!

            // 暴力计算回文串长度
            while (t[i - p[i]] == t[i + p[i]])
                ++p[i];

            // 当 i 处的回文字符串长度超过右边界 r 时  更新 mid 和 r
            if (i + p[i] > r) {
                mid = i;
                r = i + p[i];
            }

            // max_rad 记录最大的半径 max_mid 记录最大半径的位置
            if (p[i] > max_rad) {
                max_rad = p[i];
                max_mid = i;
            }
        }

        return s.substr((max_mid - max_rad) >> 1, max_rad - 1);  ///< 注意起始点需 / 2, 长度需 - 1
    }
};

```



[647. 回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindromic-substrings/description/)

```c++
/** 字符串 647 回文子串 */
class Solution {
public:
    int countSubstrings(string s) {
        int ret = 0;
        if (!s.empty())
        {
            string t = "*#";
            for (auto c: s)
            {
                t += c;
                t += "#";
            }

            t += "!";
            vector<int> p(t.size());
            int r = 0, mid = 0;
            for (int i = 1, e = t.size() - 1; i < e; ++i) {
                p[i] = i < r ? min(r - i, p[2 * mid - i]) : 1;

                while (t[i + p[i]] == t[i - p[i]]) ++p[i];

                if (i + p[i] > r)
                {
                    r = i + p[i];
                    mid = i;
                }

                ret += (p[i] >> 1);
            }
        }

        return ret;
    }

```





**总结马拉车**

步骤如下

1. `p[i] = i < r ? min(p[2 * mid - i], r - i) : 1;`
2. 暴力外扩
3. 外扩后判断**半径**是否超右边界，如果是，则更新右边界和中心点
4. 更新最大半径和最大半径的中心点



### [不重复最长子串](#context.4)<a name="section.4"> </a>

[3. 无重复字符的最长子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/)

```c++
/* 给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度 */
/** 字符串 3 无重复字符的最长子串 */
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ret = 0;
        if (!s.empty())
        {
            int fast = 0, slow = 0;
            vector<int> arr(128, 0);
            for (; fast < s.size(); ++fast) {
                slow = max(slow, arr[s[fast]]);
                arr[s[fast]] = fast + 1;
                ret = max(ret, fast - slow + 1);
            }
        }

        return ret;
    }
};
```







## [快排](#context.5)<a name="section.5"> </a>

[912. 排序数组 - 力扣（LeetCode）](https://leetcode.cn/problems/sort-an-array/)

```c++
class Solution {
public:
    bool Compare(int a, int b)
    {
        return a < b;
    }

    void RecurQS(vector<int> &nums, int start, int end)
    {
        int left, right, mid, temp;
        while (start < end)
        {
            left = start;
            right = end;

            mid = nums[rand() % (end - start + 1) + start];  // 随机效率比中位数高将近10倍
//            mid = nums[start + ((end - start) >> 1)];
            do
            {
                while (Compare(nums[left], mid))
                    ++left;
                while (Compare(mid, nums[right]))
                    --right;

                if (left < right)
                {
                    temp = nums[left];
                    nums[left] = nums[right];
                    nums[right] = temp;
                    ++left;
                    --right;
                }
                else if (left == right)
                {
                    ++left;
                    --right;
                    break;
                }
            } while (left <= right);

            if (start < right)
                RecurQS(nums, start, right);

            start = left;
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        if (nums.size() > 1)
        {
            RecurQS(nums, 0, nums.size() - 1);
        }

        return nums;
    }
};
```



## [回溯](#context.6)<a name="section.6"> </a>

```c++
/* 伪代码模板 */
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```





## [递归法](#context.7)<a name="section.7"> </a>

三部曲：

1. **确定递归函数的参数和返回值**

   因为我们要比较的是根节点的两个子树是否是相互翻转的，进而判断这个树是不是对称树，所以要比较的是两个树，参数自然也是左子树节点和右子树节点。

   返回值自然是bool类型。

   代码如下：

   ```c++
   bool compare(TreeNode* left, TreeNode* right)
   ```

   

2. **确定终止条件**
   要比较两个节点数值相不相同，首先要把两个节点为空的情况弄清楚！否则后面比较数值的时候就会操作空指针了。
   节点为空的情况有：（**注意我们比较的其实不是左孩子和右孩子，所以如下我称之为左节点右节点**）

   - 左节点为空，右节点不为空，不对称，return false
   - 左不为空，右为空，不对称 return false
   - 左右都为空，对称，返回true

   此时已经排除掉了节点为空的情况，那么剩下的就是左右节点不为空：

   - 左右都不为空，比较节点数值，不相同就return false

   此时左右节点不为空，且数值也不相同的情况我们也处理了。

   代码如下：

   ```c++
   if (left == NULL && right != NULL) return false;
   else if (left != NULL && right == NULL) return false;
   else if (left == NULL && right == NULL) return true;
   else if (left->val != right->val) return false; // 注意这里我没有使用else
   ```

   注意上面最后一种情况，我没有使用else，而是else if， 因为我们把以上情况都排除之后，剩下的就是 左右节点都不为空，且数值相同的情况。

3. 

4. **确定单层递归的逻辑**

   此时才进入单层递归的逻辑，单层递归的逻辑就是处理 左右节点都不为空，且数值相同的情况。

   - 比较二叉树外侧是否对称：传入的是左节点的左孩子，右节点的右孩子。
   - 比较内侧是否对称，传入左节点的右孩子，右节点的左孩子。
   - 如果左右都对称就返回true ，有一侧不对称就返回false 。

   代码如下：

   ```c++
   bool outside = compare(left->left, right->right);   // 左子树：左、 右子树：右
   bool inside = compare(left->right, right->left);    // 左子树：右、 右子树：左
   bool isSame = outside && inside;                    // 左子树：中、 右子树：中（逻辑处理）
   return isSame;
   ```

   

最后递归的C++整体代码如下：

```c++
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right) {
        // 首先排除空节点的情况
        if (left == NULL && right != NULL) return false;
        else if (left != NULL && right == NULL) return false;
        else if (left == NULL && right == NULL) return true;
        // 排除了空节点，再排除数值不相同的情况
        else if (left->val != right->val) return false;

        // 此时就是：左右节点都不为空，且数值相同的情况
        // 此时才做递归，做下一层的判断
        bool outside = compare(left->left, right->right);   // 左子树：左、 右子树：右
        bool inside = compare(left->right, right->left);    // 左子树：右、 右子树：左
        bool isSame = outside && inside;                    // 左子树：中、 右子树：中 （逻辑处理）
        return isSame;

    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        return compare(root->left, root->right);
    }
};
```



## [贪心](#context.8)<a name="section.8"> </a>

如果找到局部最优，然后推出整体最优，那么就是贪心

局部最优 → 总体最优



[代码随想录 (programmercarl.com)](https://www.programmercarl.com/0053.最大子序和.html)

```c++
/** 贪心 53 最大子数组和 */
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        ///< 1. 贪心
        int ret = 0;
        if (!nums.empty())
        {
            ret = INT32_MIN;
            int sum = 0;
            for (int i = 0; i < nums.size(); ++i) {
                sum += nums[i];
                ret = max(ret, sum);

                if (sum <= 0)
                    sum = 0;
            }

        ///< 2. 优化空间后的动态规划
//            ret = nums[0];
//            int pre = 0;
//            for (int i = 0; i < nums.size(); ++i) {
//                pre = max(pre + nums[i], nums[i]);
//                ret = max(ret, pre);
//            }

        ///< 3. 未优化空间的动态规划
//            vector<int> dp(nums.size(), 0);
//            dp[0] = nums[0];
//            ret = dp[0];
//            for (int i = 1; i < nums.size(); ++i) {
//                if (dp[i - 1] > 0)
//                    dp[i] = dp[i - 1] + nums[i];
//                else
//                    dp[i] = nums[i];
//
//                ret = max(ret, dp[i]);
//            }
        }

        return ret;
    }
};


```

