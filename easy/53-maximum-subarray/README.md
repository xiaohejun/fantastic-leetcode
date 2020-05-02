# [53. Maximum Subarray](https://leetcode-cn.com/problems/maximum-subarray/)

## 题目大意

求一个数组中的最大子数组和

输入：[-2,1,-3,4,-1,2,1,-5,4]

输出：6

解释：[4,-1,2,1] has the largest sum = 6.

## 题解

### 思路1

#### 思路描述

考虑动态规划(Dynamic programming,以下简称dp)的做法

1.定义状态

dp[i]定义为以第i个元素结尾的子数组和的最大值

2.动态转移方程

显然，根据定义的状态，要想计算dp[i]，有两种情况

- 以第i个元素作为开始，也作为结束，那么dp[i] = nums[i]
- 把第i个元素接在第i-1个元素后面，那么dp[i] = dp[i-1] + nums[i]

我们想要求得的是最大值，所以dp[i]的结果应该是上述两种情况的最大值

3.答案

通过上面的分析，不难知道答案就是max{dp[i] | 0 <= i < n}，其中n表示数组的大小

#### 代码

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 获取nums数组的大小      
        int n = nums.size();
        // 定义dp数组，大小是n
        // dp[i]表示以第i个元素结尾的最大子数组和
        int dp[n];
        // 根据定义显然dp[0] = nums[0]
        dp[0] = nums[0];
        // 答案是max{dp[i] | 0 <= i < n}
        int ans = nums[0];
        for(int i = 1; i < n; ++i){
            // 根据dp状态的定义
            // 以第i个元素结尾的最大子数组和等于下面两种情况子数组和的最大值
            // 以第i个元素开始作为结束的子数组，也就是只有第i个元素自己
            // 把第i个元素接在第i-1个元素结尾的上面
            dp[i] = dp[i-1]+nums[i] > nums[i] ? dp[i-1]+nums[i] : nums[i];  
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
/*
Time complexity: O(n)
Location Complexity: O(n)
*/
```



### 思路2

#### 思路描述：

该思路是`思路1`的空间上的优化，我们注意到其实没必要用一个大小是n的数组来记录dp的状态，所以可以把空间复杂度优化到O(1)

#### 代码：

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 获取nums数组的大小      
        int n = nums.size();
        // 定义dp数组，大小是n
        // dp表示当前计算的dp值，也就是dp[i]，lst表示上一个dp值，也就是dp[i-1]
        int dp, lst;
        // 根据定义显然dp[0] = nums[0]
        dp = lst = nums[0];
        // 答案是max{dp[i] | 0 <= i < n}
        int ans = nums[0];
        for(int i = 1; i < n; ++i){
            // 根据dp状态的定义
            // 以第i个元素结尾的最大子数组和等于下面两种情况子数组和的最大值
            // 以第i个元素开始作为结束的子数组，也就是只有第i个元素自己
            // 把第i个元素接在第i-1个元素结尾的上面
            dp = lst + nums[i] > nums[i] ? lst + nums[i] : nums[i];  
            ans = max(ans, dp);
            lst = dp;
        }
        return ans;
    }
};
/*
Time complexity: O(n)
Location Complexity: O(1)
*/
```



