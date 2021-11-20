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



## 435、无重叠区间

在选择要保留区间时，区间的结尾十分重要：选择的区间结尾越小，余留给其它区间的空间
就越大，就越能保留更多的区间。因此，采取的贪心策略为，优先保留结尾小且不相交的区
间。

```c++
static bool cmp(const vector<int> &a, const vector<int> &b) {
    //const引用比值传递效率高
    return a[1] < b[1];
}

int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if(intervals.empty()) return 0;
    sort(intervals.begin(), intervals.end(), cmp);
    int res = 0;
    vector<int> tmp = intervals[0];
    for(int i = 1; i < intervals.size(); i ++ ) {
        if(tmp[1] > intervals[i][0]) {
            res ++;
            continue;
        }
        else tmp = intervals[i];
    }
    return res;
}
```

