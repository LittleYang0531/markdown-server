[Pixiv: 85035742]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/85035742_p0.png'

## [USACO19FEB] Painting the Barn G 做题记录

### 题目描述:

​F 先生需要粉刷一面墙壁。我们将这面墙壁抽象成一个二维的 $X-Y$ 平面。每一次 F 先生粉刷墙壁都是在墙壁上的一个与 X 轴 Y 轴平行的矩形范围内进行粉刷的。F 先生已经进行了 $N$ 次墙壁粉刷。

​F 先生并不希望只涂一层就敷衍了事，经过他的科学论证，总共被粉刷了 $K$ 次的墙壁效果是最好的。不过他现在的油漆不是很够了，最多只能再进行两次粉刷，**并且这两次粉刷的矩形不能相交**(可以选择只粉刷一个矩形或不粉刷)。F 先生希望最后被粉刷了 $K$ 次的墙壁面积最大，你能帮帮他吗？

### 输入描述:

​输入的第一行是两个被空格隔开的正整数 $N,K(1\leq N,K\leq 10^5)$。

​接下来的 $N$ 行，每行四个整数 $x_1,y_1,x_2,y_2(0\leq x_1,y_1,x_2,y_2\leq 200)$ 描述一个矩形，其左下角坐标为 $(x_1,y_1)$，右上角坐标为 $(x_2,y_2)$。保证所有矩形面积为正，不会退化到线段或者点。

​新粉刷的矩形与输入一致，左下角和右上角坐标必须为整数，坐标在 0~200 之间，面积必须为正。

### 输出描述:

​输出恰被粉刷了 $K$ 次的区域的最大面积。

### 测试样例:

#### 【样例1输入】

```
3 2 
1 1 4 4
3 3 7 6
2 2 8 7
```

#### 【样例1输出】

```
26
```

### 数据范围与提示:

| 子任务 | 分数 |          特殊性质          |
| :----: | :--: | :------------------------: |
|   1    |  20  |           $N=1$            |
|   2    |  30  | $K>1$ 且所有坐标小于等于50 |
|   3    |  50  |         无特殊性质         |

### 题目分析:

​一道 DP 的问题。

​我们将 $f_{i,j,k}$ 定义为粉刷以上边界为 $l:y=j$，下边界为 $l:y=k$，右边界为 $l:x=i$，且左边界未定义的矩形面积时对答案造成的最大的贡献**变化**。

​将 $g_{i,j,k}$ 定义为粉刷以上边界为 $l:y=j$，下边界为 $l:y=k$，左边界为 $l:x=i$，且右边界未定义的矩形面积时对答案造成的最大贡献**变化**。

​将 $f'_i$ 定义为粉刷以右边界为 $l:x=i$ 且其他边界都未定义的矩形面积时对答案造成的最大贡献**变化**。

​将 $g'_i$ 定义为粉刷以左边界为 $l:x=i$ 且其他边界都未定义的矩形面积时对答案造成的最大贡献**变化**。

​将 $ans_{i,j,k}$ 定义为粉刷以上边界为 $l:y=j$，下边界为 $l:y=k$，左右边界都为 $l:x=i$ 的矩形面积时对答案造成的贡献变化。

​则 $f_{i,j,k}=max(f_{i-1,j,k}+ans_{i,j,k},ans_{i,j,k}),g_{i,j,k}=max(g_{i+1,j,k}+ans_{i,j,k},ans_{i,j,k})$。

​对于 $f'_i,g'_i$ 的值，只需要将所有 $f_{i,j,k}$ 或 $g_{i,j,k}$ 的值取个最大值就行了。

​设 $ans1$ 为最初一次都不粉刷的答案，$ans2$ 为只粉刷一次时的最大答案，$ans3$ 为粉刷两次时的最大答案。

​则 $ans2=\max_{i=1}^{n}max(f_i',g_i'),ans3=\max_{i=1,j=1}^{i<j\leq N}(f_i'+g_i')+ans1$。

​因此最终结果是 $max(asn1,ans2,ans3)$。

​最后再将整个矩形翻转再做一遍并与上述最终结果取最大值即为最终答案。

​时间复杂度为 $O(N^3)$

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define ywhin cin
#define ywhout cout
using namespace std;
const van N=210;
van n,k;
van sum[N][N];
van kl0[N][N],kl1[N][N];//kl0:被覆盖了K层的点，kl1:被覆盖了K-1层的点
van f[N][N][N],g[N][N][N];
van fyp[N],gyp[N];
van ans;
van GetKL0(van x,van y,van l,van h) {
	van ex=x+l-1,ey=y+h-1;x--,y--;
	return (ex>=0&&ey>=0?kl0[ex][ey]:0)+(x>=0&&y>=0?kl0[x][y]:0)-(ex>=0&&y>=0?kl0[ex][y]:0)-(x>=0&&ey>=0?kl0[x][ey]:0);
}
van GetKL1(van x,van y,van l,van h) {
	van ex=x+l-1,ey=y+h-1;x--,y--;
	return (ex>=0&&ey>=0?kl1[ex][ey]:0)+(x>=0&&y>=0?kl1[x][y]:0)-(ex>=0&&y>=0?kl1[ex][y]:0)-(x>=0&&ey>=0?kl1[x][ey]:0);
}
van GetRes(van x,van y,van l,van h) {
	return GetKL1(x,y,l,h)-GetKL0(x,y,l,h);
}//计算粉刷以(x,y)左下角的点且长为l，宽为h的矩形面积时对答案的贡献变化
void solve() {
	memset(kl0,0,sizeof kl0);
	memset(kl1,0,sizeof kl1);
	memset(f,0,sizeof f);
	memset(g,0,sizeof g);
	memset(fyp,0,sizeof fyp);
	memset(gyp,0,sizeof gyp);//重置数组
	for (int i=0;i<=n;i++) {
		fyp[i]=gyp[i]=-1e18;
	} 
	for (int i=0;i<=n;i++) {
		for (int j=0;j<=n;j++) {
			if (sum[i][j]==k) kl0[i][j]++;
			if (sum[i][j]==k-1) kl1[i][j]++;
		}
	}//计算可能会对答案造成贡献的点
	for (int i=0;i<=n;i++) {
		for (int j=0;j<=n;j++) {
			kl0[i][j]+=(j>0?kl0[i][j-1]:0)+(i>0?kl0[i-1][j]:0)-(i>0&&j>0?kl0[i-1][j-1]:0);
			kl1[i][j]+=(j>0?kl1[i][j-1]:0)+(i>0?kl1[i-1][j]:0)-(i>0&&j>0?kl1[i-1][j-1]:0);
		}
	}//二位前缀和
	ans=max(ans,kl0[n][n]);
	for (int l=0;l<=n;l++) {
		for (int r=l+1;r<=n;r++) {
			g[0][l][r]=GetRes(0,l,1,r-l+1);
		}
	}//计算g(0,j,k)
	for (int i=1;i<=n;i++) {
		for (int l=0;l<=n;l++) {
			for (int r=l+1;r<=n;r++) {
				g[i][l][r]=max(g[i-1][l][r]+GetRes(i,l,1,r-l+1),GetRes(i,l,1,r-l+1));
				gyp[i]=max(gyp[i],g[i][l][r]);
			}
		}
	}//计算g(i,j,k)以及g'(i)
	for (int l=0;l<=n;l++) {
		for (int r=l+1;r<=n;r++) {
			f[n][l][r]=GetRes(n,l,1,r-l+1);
		}
	}//计算f(n,j,k)
	for (int i=n-1;i>=0;i--) {
		for (int l=0;l<=n;l++) {
			for (int r=l+1;r<=n;r++) {
				f[i][l][r]=max(f[i+1][l][r]+GetRes(i,l,1,r-l+1),GetRes(i,l,1,r-l+1));
				fyp[i]=max(fyp[i],f[i][l][r]);
			}
		}
	}//计算f(i,j,k)以及f'(i)
	for (int i=0;i<=n;i++) ans=max(ans,max(kl0[n][n]+fyp[i],kl0[n][n]+gyp[i]));
	for (int i=1;i<=n;i++) {
		for (int j=n-1;j>i;j--) {
			ans=max(ans,kl0[n][n]+gyp[i]+fyp[j]);
		}
	}//计算答案最大值
}
int main() {
	ywhin>>n>>k;
	memset(sum,0,sizeof sum);
	for (int i=1;i<=n;i++) {
		van x1,y1,x2,y2;
		ywhin>>x1>>y1>>x2>>y2;
		sum[x1][y1]++,sum[x2][y2]++;
		sum[x1][y2]--,sum[x2][y1]--;
	}
	n=199;
	for (int i=0;i<=n;i++) {
		for (int j=0;j<=n;j++){
			sum[i][j]+=(j>0?sum[i][j-1]:0)+(i>0?sum[i-1][j]:0)-(i>0&&j>0?sum[i-1][j-1]:0);
		}
	}//计算某个点被覆盖的层数
	solve();
	for (int i=0;i<=n;i++) {
		for (int j=0;j<i;j++) {
			swap(sum[i][j],sum[j][i]);
		}
	}
	solve();
	ywhout<<ans<<endl;
}
```

