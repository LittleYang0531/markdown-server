## [USACO18DEC] Cowpatibility G 做题记录

### 题目描述:

​在幼儿园，协调同学们之间的关系时非常重要的，暑假正在幼儿园帮忙的 Emiya 正在为此烦恼，不过，他的烦恼马上就要被解决了。因为 Emiya 找到了处理孩子们关系的关键因素——冰激凌。

​幼儿园的每一位小朋友都列出了自己最喜欢的 5 种冰激凌，每种冰激凌可以用一个不超过 $10^6$ 的编号表示。对于两位小朋友，如果存在至少一种冰激凌，两位小朋友都喜欢，那么我们认为这两位小朋友是可以平衡相处的。

​你的任务时求出不能和谐相处的小朋友一共有多少对。

### 输入描述:

​输入的第一行包括一个正整数 $N(2\leq N\leq 5\times 10^4)$。

​接下来的 $N$ 行，每行包括 5 个互不相同的整数，表示一位小朋友最喜欢的五种冰激凌的编号。

### 输出描述:

​输出不能和谐相处的小朋友的对数。

### 测试样例:

#### 【样例1输入】

```
4
1 2 3 4 5
1 2 3 10 8
10 9 8 7 6
50 60 70 80 90
```

#### 【样例1输出】

```
4
```

#### 【样例1解释】

​在样例中，第四位小朋友不能与第一位，第二位，第三位小朋友和谐共处。第一位和第三位小朋友也不能和谐共处。

### 数据范围与提示

| 子任务 | 分数 |       特殊性质       |
| :----: | :--: | :------------------: |
|   1    |  20  |     $N\leq 1000$     |
|   2    |  30  | 冰激凌的编号不超过15 |
|   3    |  50  |      无特殊性质      |

### 题目分析:

​分析题意，其实就是让我们求出二元组 $(i,j)$，使得 $i$ 喜欢的冰激凌与 $j$ 喜欢的冰激凌没有交集的衣裳二元组的数量。

​考虑计算能和谐相处的小朋友的对数。设包含了冰激凌 $x$ 的小朋友的集合为 $A_x$，则能与 $i$ 和谐相处的小朋友为 $A_{like_{i,1}}\cup A_{like_{i,2}}\cup\ ...\ \cup A_{like_{i,5}}$。这玩意可以通过容斥原理计算。

​至于如何快速算出包含了某几个冰激凌的小朋友的数量，只需要通过C++的map就可以了。

​时间复杂度为 $O(5\times 2^5N\log N)$。

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define ywhin cin
#define ywhout cout
using namespace std;
bool ppppp;
const van N=101000;
const van X=20;
van n;
van like[N][6];
struct node {
	van x1=-1e9,x2=-1e9,x3=-1e9,x4=-1e9,x5=-1e9;
};
map<node,van> ma;
bool operator <(const node& a,const node& b) {
	if (a.x1!=b.x1) return a.x1<b.x1;
	if (a.x2!=b.x2) return a.x2<b.x2;
	if (a.x3!=b.x3) return a.x3<b.x3;
	if (a.x4!=b.x4) return a.x4<b.x4;
	return a.x5<b.x5;
}
bool pppppp;
van query(van x) {
	van ans=0;
	node a;
	for (int i1=1;i1<=5;i1++) {
		a.x1=like[x][i1];
		ans+=ma[a];
		for (int i2=i1+1;i2<=5;i2++) {
			a.x2=like[x][i2];
			ans-=ma[a];
			for (int i3=i2+1;i3<=5;i3++) {
				a.x3=like[x][i3];
				ans+=ma[a];
				for (int i4=i3+1;i4<=5;i4++) {
					a.x4=like[x][i4];
					ans-=ma[a];
					for (int i5=i4+1;i5<=5;i5++) {
						a.x5=like[x][i5];
						ans+=ma[a];
					}
					a.x5=-1e9;
				}
				a.x4=-1e9;
			}
			a.x3=-1e9;
		}
		a.x2=-1e9;
	}
	a.x1=-1e9;
	return ans;
}//容斥原理查询与编号为x的小朋友能和谐相处的小朋友的数量
void add(van x) {
	node a;
	for (int i1=1;i1<=5;i1++) {
		a.x1=like[x][i1];
		ma[a]++;
		for (int i2=i1+1;i2<=5;i2++) {
			a.x2=like[x][i2];
			ma[a]++;
			for (int i3=i2+1;i3<=5;i3++) {
				a.x3=like[x][i3];
				ma[a]++;
				for (int i4=i3+1;i4<=5;i4++) {
					a.x4=like[x][i4];
					ma[a]++;
					for (int i5=i4+1;i5<=5;i5++) {
						a.x5=like[x][i5];
						ma[a]++;
					}
					a.x5=-1e9;
				}
				a.x4=-1e9;
			}
			a.x3=-1e9;
		}
		a.x2=-1e9;
	}
	a.x1=-1e9;
}//将编号为i的小朋友加到桶里
void Accpeted() {
	van ans=n*(n-1)/2;
	for (int i=1;i<=n;i++) {
		ans-=query(i);//容斥原理计算答案
		add(i);//将当前小朋友加到桶里
	}
	ywhout<<ans;
}
int main() {
	ywhin>>n;
	for (int i=1;i<=n;i++) {
		for (int j=1;j<=5;j++) {
			ywhin>>like[i][j];
		}
		sort(like[i]+1,like[i]+6);
	}
	Accpeted();
}
```

