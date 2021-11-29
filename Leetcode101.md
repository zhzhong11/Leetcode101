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

## 605、种花问题

在不打破规则的情况下尽可能地多种花，注意边界情况的处理。

```c++
bool canPlaceFlowers(vector<int>& flowerbed, int n) {
    int len = flowerbed.size(), num = 0, i = 0;
    while(i < len) {
        if(flowerbed[i] == 1) i += 2;
        else if(i == len - 1 || flowerbed[i+1] == 0) num ++, i += 2;
        else i += 3;
    }

    return n <= num;
}
```

## 452、用最少数量的箭引爆气球

将区间按照右区间升序排列，题目与 435.无重叠区间 的解法非常相似，初始取第一个右区间为第一支箭，设定初始射箭数为1，从第二个区间开始遍历，当区间的左区间小于当前第一支箭时，说明这支箭同样也可以贯穿这个区间的气球，则这支箭还可以继续飞，所以继续遍历下一个区间，当当前区间的左区间大于当前在飞的箭时，则说明当前的这支箭已经不能射穿这个区间的气球了，则取这个区间气球的右区间作为下一支箭，继续射出飞行，用n记录总共射出的箭数，最后返回即可。

```c++
static bool cmp(const vector<int> &a, const vector<int> &b) {
	return a[1] < b[1];
}

int findMinArrowShots(vector<vector<int>>& points) {
    if(points.size() == 1) return 1;
    sort(points.begin(), points.end(), cmp);
    int n = 1, tmp = points[0][1];
    for(int i = 0; i < points.size() - 1; i ++ ) {
        if(points[i+1][0] <= tmp) {
            continue;
        }
        else {
            n ++;
            tmp = points[i+1][1];
        }
    }
    return n;
}
```

## 763、划分字母区间

在遍历的过程中相当于是要找每一个字母的边界，**如果找到之前遍历过的所有字母的最远边界，说明这个边界就是分割点了**。此时前面出现过所有字母，最远也就到这个边界了。

可以分为如下两步：

- 统计每一个字符最后出现的位置
- 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点

```C++
vector<int> partitionLabels(string s) {
    map<char, int> hash;
    vector<int> res;
    for(int i = 0; i < s.size(); i ++ ) hash[s[i]] = i;
    //for(auto i : hash) cout << i.first << i.second << endl;
    int start = 0, end = 0;
    for(int i = 0; i < s.size(); i ++ ) {
        end = max(end, hash[s[i]]);
        if(i == end) {
            res.push_back(end - start + 1);
            start = end + 1;
        }
    }
    return res;
```

## 122、买卖股票的最佳时机II

买卖股票系列用动态规划做，这题简单可以贪心。

```C++
int maxProfit(vector<int>& prices) {
    if(prices.empty()) return 0;
    int res = 0;
    for(int i = 0; i < prices.size() - 1; i ++) {
        res += max(prices[i+1] - prices[i], 0);
    }
    return res; 
}
```

## 406、根据身高重建队列

先按身高降序排列，优先安放身高高的人，如果身高相同，先放位次低的。这样所有人的相对位次不用改变

```c++
static bool cmp(const vector<int> &a, const vector<int> &b) {
    if(a[0] == b[0]) return a[1] < b[1]; 
    return a[0] > b[0];
}
vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    //先按身高降序排列，优先安放身高高的人，如果身高相同，先放位次低的
    sort(people.begin(), people.end(), cmp);
    vector<vector<int>> res;
    int place;
    for(int i = 0; i < people.size(); i ++ ) {
        place = people[i][1];
        res.insert(res.begin() + place, people[i]);
    }
    return res;
}
```

## 665、非递减数列

本题是要维持一个非递减的数列，所以遇到递减的情况时（nums[i] > nums[i + 1]），要么将前面的元素缩小，要么将后面的元素放大。

但是本题唯一的易错点就在这，

如果将nums[i]缩小，可能会导致其无法融入前面已经遍历过的非递减子数列；
如果将nums[i + 1]放大，可能会导致其后续的继续出现递减；
所以要采取贪心的策略，在遍历时，每次需要看连续的三个元素，也就是瞻前顾后，遵循以下两个原则：

需要尽可能不放大nums[i + 1]，这样会让后续非递减更困难；
如果缩小nums[i]，但不破坏前面的子序列的非递减性；
算法步骤：

遍历数组，如果遇到递减：
还能修改：
修改方案1：将nums[i]缩小至nums[i + 1]；
修改方案2：将nums[i + 1]放大至nums[i]；
不能修改了：直接返回false；

```C++
bool checkPossibility(vector<int>& nums) {
    if(nums.size() <= 2) return true;
    int cnt = 1; //修改次数
    if(nums[0] > nums[1]) cnt --;
    for(int i = 1; i < nums.size() - 1; i ++ ) {
        if(nums[i] > nums[i+1]) {
            if(cnt > 0) {
                if(nums[i+1] >= nums[i-1]) nums[i] = nums[i-1];
                else nums[i+1] = nums[i];
                cnt --;
            }
            else return false;
        }
    }
    return true;
}
```



## 二、双指针

两个指针指向同一数组

遍历方向相同且不相交：滑动窗口

遍历方向相反：搜索（数组往往是排序好的）