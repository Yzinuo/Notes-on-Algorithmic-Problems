
![](attachments/奶酪问题(状态压缩DP）_image_0.png)

思路：状态压缩DP 状态DP：
简介：

![](attachments/奶酪问题(状态压缩DP）_image_1.png)
状态有二进制来表示，且这个二进制的十进制值来代码中用来存储此时的状态，因为二进制与十进制是映射关系，一一对应。

代码理解：
这里用题解的注释代码：
1<<(i-1)  表示仅仅占领了第i个奶酪的二进制情况。
k&（i<<(i-1)) 表示有没有进过第i个奶酪。
二进制的减法就是十进制的减法。
```
#include <cstdio>
#include <cstring> 
#include <cmath> 
#define min(a,b) (((a)<(b))?(a):(b)) 
//洛谷 P1433 吃奶酪 状压DP
double a[20][20];//预处理，从第i块到第j块的距离，使用两点之间距离公式 
double x[20],y[20];//每块奶酪的横、纵坐标
double F[18][34000];//状压DP数组 在第i个点上，走过的二进制状态的十进制表达为j时，最短的距离 
int N; 
double distance(int v,int w)//计算第v个和第w个奶酪之间的距离 
{
	return sqrt((x[v]-x[w])*(x[v]-x[w])+(y[v]-y[w])*(y[v]-y[w]));//两点间距离公式 
}
int main()
{
	int i,j,k;
	double ans;
	memset(F,127,sizeof(F));//这样可以给浮点数赋值无穷大 
	ans=F[0][0];
	scanf("%d",&N);
	for(i=1;i<=N;i++)
	{
		scanf("%lf%lf",&x[i],&y[i]);//数据读入 
	}
	x[0]=0;y[0]=0;
	for(i=0;i<=N;i++)
	{
		for(j=i+1;j<=N;j++)
		{
			a[i][j]=distance(i,j);//初始化距离数组 
			a[j][i]=a[i][j];
		}
	} 
	for(i=1;i<=N;i++)//初始化 
	{
		F[i][(1<<(i-1))]=a[0][i];//在i点上且只有经过i点时距离是原点到i点的距离 
	}
	for(k=1;k<(1<<N);k++)//枚举所有二进制的状态 
	{
		for(i=1;i<=N;i++)
		{
			if((k&(1<<(i-1)))==0)
				continue;//i的位置没被走过，所以不需要再继续计算了 
			for(j=1;j<=N;j++)
			{
				if(i==j)
					continue;//同一个点不需要再计算 
				if((k&(1<<(j-1)))==0)
					continue;//j的位置没走过  
				F[i][k]=min(F[i][k],F[j][k-(1<<(i-1))]+a[i][j]);
			} 
		} 
	} 
	for(i=1;i<=N;i++)
	{
		ans=min(ans,F[i][(1<<N)-1]);
	}
	printf("%.2f\n",ans);
}
```


