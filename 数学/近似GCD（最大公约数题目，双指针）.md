
![](attachments/近似GCD（最大公约数题目，双指针）_image_0.png)

我一开始是要不断的求GCD 循环的求，但其实这样好傻。区间内公约数是g那么这一段都会是g的倍数。我们只要看这段区间内不是g的倍数的个数。

双指针代码：
```
#include <bits/stdc++.h>
using namespace std;
const int N = 2e5 + 10;
long long a[N];
int n, g;

long long ans = 0;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);

	cin >> n >> g;
	for (int i = 0; i < n; i++)
	{
		cin >> a[i];
	}
	int count = 0;
	
	int j = 0;
	int last = -1;
	for(int i = 0; i < n; i++){
		while(j < n &&(a[j]%g == 0 || last < i)){
			if(a[j] % g != 0)
				last = j;
			j++;
		}
		if(j > i +1)
			ans += (j - i -1);
	}
	cout << ans << "\n";
}
```