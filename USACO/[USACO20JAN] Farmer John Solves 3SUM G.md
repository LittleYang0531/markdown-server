[Pixiv: 77171026]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/77171026_p0.jpg'

## [USACO20JAN] Farmer John Solves 3SUM G 做题记录

### 题目描述:

​草原牧民小约翰自称在算法设计上取得了重要突破，他声称发现了一个 3sum 问题的近似线性时间的算法。传统的 3sum 算法通常是这样的一个形式：给定一个整数数组 $s_1,s_2,s_3,..,s_m$ 计算满足 $s_i+s_j+s_k=0$ 的**无序**三元组 $(i,j,k)$ 的数量。

​为了测试小约翰是否真的完成了这样一个算法，通辽可汗决定进行测试。可汗给了小约翰一个大小为 $N$ 的数组 $A$。除此之外，可汗还会给出 $Q$ 个询问，每一个询问由两个数 $a_i$ 和 $b_i$ 组成，表示询问区间 $A[a_i,a_{i+1},...,b_i]$ 中 3sum 问题的答案。

### 输入描述:

​输入的第一行包含两个正整数 $N,Q(1\leq N\leq 5000,1\leq Q\leq 10^5)$，分别表示数组的大小以及询问的数量。 

​输入的第二行包括 $N$ 个用空格分隔开的整数，表示数组 $A(-10^6\leq A_i\leq 10^6)$。 接下来的 $Q$ 行，每行包含两个用空格隔开的整数 $a_i,b_i$，表示一个询问。

### 输出描述:

​输出包含 $Q$ 行，第 $i$ 行包括一个整数，表示第 $i$ 个询问的结果。

### 测试样例:

#### 【样例1输入】

```
7 3
2 0 -1 1 -2 3 3
1 5
2 4
1 7
```

#### 【样例1输出】

```
2
1
4
```

#### 【样例1解释】

​对于第一个询问，满足条件的三元组为 $(A_1,A_2,A_5),(A_2,A_3,A_4)$。

### 数据范围与提示:

| 子任务 | 分数 |      特殊性质      |
| :----: | :--: | :----------------: |
|   1    |  30  | $1\leq N\leq 500$  |
|   2    |  30  | $1\leq N\leq 2000$ |
|   3    |  40  |     无特殊限制     |

### 题目分析:

​分析题目，由于 $A_i+A_j+A_k=0$，因此 $A_i+A_j=-A_k$。所以如果我们能够用桶来维护 $A_i+A_j$ 的个数，把 $A_k$ 取个反就可以计算出包含 $A_k$ 的符合题意的值。由于不能有重复的，因此我们只能查找在位置 $k$ 之前的所有的 $A_i+A_j$ 的个数即可。

​至于如何查询，先将所有的查询以左端点来排个序，然后设定一个指针，指向当前能够查询的左端点，然后移动指针的时候只需要枚举当前指向的 $A$ 数组的值能够形成的 $A_i+A_j$ 的值，并将桶里面的值减一。最后查询时只需要查询右端点的答案减去左端点的答案就行了。

​具体思路有点小复杂，不太好讲清楚，请读者自行阅读代码理解。

​时间复杂度为 $O(N^2+Q\log N)$。

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define ywhin cin
#define ywhout cout
using namespace std;
bool ppppp;
const van N=5e3+10,Q=1e5+10;
const van A=6e6+10;
struct que {
	van l,r;
	van id;
}query[Q];//保存查询信息
van a[N],n,q;
van sum[A];
van c[N],res[N];
van ans[Q];
bool pppppp;
bool cmp(que a,que b) {
	return a.l<b.l;
}
van lowbit(van x) {
	return x&-x;
}
void AddTree(van x,van num) {
	while (x<=n) {
		c[x]+=num;
		x+=lowbit(x);
	}
}
van QueryTree(van x) {
	van res=0;
	while (x) {
		res+=c[x];
		x-=lowbit(x);
	}
	return res;
}//树状数组维护前缀和
int main() {
	ywhin>>n>>q;
	for (int i=1;i<=n;i++) {
		ywhin>>a[i];
	}
	for (int i=1;i<=n;i++) {
		van tmp=-a[i]+3e6;
		res[i]=sum[tmp];
		AddTree(i,res[i]);
		for (int j=1;j<i;j++) {
			van tmp=a[i]+a[j]+3e6;
			sum[tmp]++;
		}
	}//计算Ai与Aj的和并添加到桶里
	for (int qq=1;qq<=q;qq++) {
		van l,r;
		ywhin>>l>>r;
		query[qq].l=l,query[qq].r=r;
		query[qq].id=qq;
	}
	sort(query+1,query+q+1,cmp);//对查询进行排序
	van nowpt=1;
	memset(sum,0,sizeof sum);
	for (int qq=1;qq<=q;qq++) {
		van backup=nowpt;
		while (nowpt<query[qq].l) {
			for (int i=nowpt+1;i<=n;i++) {
				van tmp=-a[i]+3e6;
				AddTree(i,sum[tmp]);
				tmp=a[i]+a[nowpt]+3e6;
				sum[tmp]--;
			}
			for (int i=nowpt+1;i<=n;i++) {
				van tmp=a[i]+a[nowpt]+3e6;
				sum[tmp]++;
			}
			nowpt++;
		}//移动指针
		ans[query[qq].id]=QueryTree(query[qq].r)-QueryTree(query[qq].l-1);//查询结果
	}
	for (int i=1;i<=q;i++) {
		ywhout<<ans[i]<<endl;
	}
}
```

