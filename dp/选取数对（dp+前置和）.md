
![](attachments/选取数对（dp+前置和）_image_0.png)


审题： 首先是序列求和 ， 首先想到前缀和算法。
每个数对都要m单位长。

dp代码：

  其中 x数组存储的是 在前面 长度为i的区间内，得到了k段数对，这k段数对的最大值。
```
#include<bits/stdc++.h>
using namespace std; 

const int N = 5e3+10;

int f[N];
int	num[N];
int x[N][N];

int main()
{
	int n,m,k;
	cin>> n >> m>> k;
	
	for(int i =1 ;i <= n ; i++)
	{
		cin >> f[i];
		num[i] = num[i-1] + f[i];
	}
	
	for(int i =1;i <= n ;i++)
	{
	
		for(int j = 1; j<=k ; j++)
		{
		
			x[i][j] = x[i-1][j];			
			if( i>=m )  x[i][j] = max(x[i][j],x[i-m][j-1] + num[i] - num[i - m]);

		}
	}
	cout << x[n][k]<<"\n";
	
}
```

难点：

![](attachments/选取数对（dp+前置和）_image_1.png)

首先第一段：如果前i个区间内 不足以分出m长度数对，那我就不选这个元素，就是x[i-1][j];
如果可以分出，那我们就要考虑选不选当前这个元素  至于为什么选择x[i-m][j-1]这个区间呢 我认为是因为 数对和数对