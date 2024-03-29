
![](attachments/执行K%20次后的最大数_image_0.png)


其实很快就可以想到使用优先队列，只是不知道该怎么用。

```
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        priority_queue<int> q(nums.begin(), nums.end());
        long long ans = 0;
        for (int _ = 0; _ < k; ++_) {
            int x = q.top();
            q.pop();
            ans += x;
            q.push((x + 2) / 3);
        }
        return ans;
    }
};
```

我们可以学到：
1.   优先队列的用法。
priority_queue<type,vector<type>,comp>Q;
第一个参数type是数据类型
第二个是容器类型，STL默认容器是vector
第三comp个是比较函数，如果没有此参数，默认大根堆，即降序排列

也可以是
priority_queue<int> q(nums.begin(), nums.end());

2.
想上取整，因为除数是3，我们给每个数字除之前加上2，就可以实现向上取整。  有余数的都会加一，没有余数的还是原来的数。
