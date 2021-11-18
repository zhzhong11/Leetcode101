# Leetcode101

[TOC]

## 一、贪心

## 455、分发饼干

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

