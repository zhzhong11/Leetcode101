# Leetcode101

[TOC]

## 一、贪心

## 455、分发饼干

把大于某个孩子饥饿度中的最小的饼干给这个孩子。

``` c++
int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(), g.end());
    sort(s.begin(), s.end());
    int num_child = g.size(), num_biscuit = s.size(), res = 0, i = 0, j = 0;
    while(i < num_child && j < num_biscuit) {
        if(g[i] <= s[j]) res ++, i ++;
        j ++;
    }
    return res;
}
```



## 135、分发糖果

把所有孩子的糖果数初始化为1；
先从左往右遍历一遍，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的
糖果数加1；再从右往左遍历一遍，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数
不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加1。

```c++
int candy(vector<int>& ratings) {
    int num = ratings.size(), res = 0;
    vector<int> candy(num, 1);
    for(int i = 0; i < num - 1; i ++ )
        if(ratings[i+1] > ratings[i]) candy[i+1] = candy[i] + 1;
    for(int i = num - 1; i > 0; i -- )
        if(ratings[i-1] > ratings[i] && candy[i-1] <= candy[i]) candy[i-1] = candy[i] + 1;
    for(auto i : candy) res += i;
    return res;
}
```