### 跳跃游戏 VII

[题目链接](https://leetcode-cn.com/problems/jump-game-vii/)

给你一个下标从 0 开始的二进制字符串 s 和两个整数 minJump 和 maxJump 。一开始，你在下标 0 处，且该位置的值一定为 '0' 。当同时满足如下条件时，你可以从下标 i 移动到下标 j 处：

 + i + minJump <= j <= min(i + maxJump, s.length - 1) 且

 + s[j] == '0'.

  如果你可以到达 s 的下标 s.length - 1 处，请你返回 true ，否则返回 false 。

 

示例 1：

```
输入：s = "011010", minJump = 2, maxJump = 3
输出：true
解释：
第一步，从下标 0 移动到下标 3 。
第二步，从下标 3 移动到下标 5 。
```


示例 2：

```
输入：s = "01101110", minJump = 2, maxJump = 3
输出：false
```



**思路**

```
递推
从可以到达的位置f[i](f[i]=1表示可以到达，=0表示不能到达)往前递推，f[i]是从[i-a,i-b]区间中的某一位置跳过来的，因此，若能跳过去，则[i-a,i-b]中至少存在一个位置可以到达，即f[k]=1.k在[i-a,i-b]范围内。
因此可以想到在区间[i-a,i-b]的所有f[i]之和至少为1,联想到用前缀和的方法来做。
具体见代码。
```

```c++
class Solution {
public:
    bool canReach(string s, int minJump, int maxJump) {
        int n = s.size();
        int f[n+1],S[n+1];
        memset(f,0,sizeof(f));
        memset(S,0,sizeof(S));
        f[1] = 1;
        S[1] = 1;
        for(int i=2;i<=n;i++){
            if(s[i-1]=='0'&&(i-minJump)>=1){
                int l = max(1,i-maxJump),r = i-minJump;
                if(S[r]>S[l-1]) f[i] = 1;
            }
            S[i] = S[i-1]+f[i];
        }
        return f[n];
    }
};
```



