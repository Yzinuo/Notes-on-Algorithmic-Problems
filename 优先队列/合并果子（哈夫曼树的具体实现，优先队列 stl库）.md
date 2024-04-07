
![](attachments/合并果子（哈夫曼树的具体实现，优先队列%20stl库）_image_0.png)


明显是哈夫曼树的解决方法，那我们应该如何实现哈夫曼树呢？用优先队列的小顶堆方法，队列中第一个总是最小的。

```
priority_queue<int, vector<int>, greater<int> >q;//小顶堆 priority_queue<int,vector<int>,greater<int>>q;
priority_queue<int, vector<int>, less<int> >q;//大顶堆
```


具体实现代码：
```
#include<bits/stdc++.h>
using namespace std;

const int N = 2e5+10;
priority_queue<int, vector<int>, greater<int> >q;

int main()
{	
	int n,x;
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>x;
		q.push(x);
	}
	int ans =0;
 	
	for(int i=0;i<n-1;i++)
	{
		int x1=q.top(); q.pop();
		int x2=q.top(); q.pop();
		ans +=x1+x2;
		q.push(x1+x2);
	}
	cout<<ans;
	return 0;
 } 
```
