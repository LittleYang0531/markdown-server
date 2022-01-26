## [USACO20OPEN] Haircut G 做题记录

### 题目描述:

​wby 对于自己头发的打理极其重视，这一次，他决定取理发店仔细打理自己的头发。

​wby 有一排一共 $N$ 缕头发，第 $i$ 缕头发初始时长度为 $A_i$ 微米。wby 希望自己的头发在长度上单调不降，所以，他定义他的头发的“不良度”为逆序对的数量：满足 $i<j,A_i>A_j$ 的二元对 $(i,j)$ 的数量。

​对于每一个 $j=0,1,...,N-1$。wby 想知道如果他的所有长度大于 $j$ 的头发的长度均减少到 $j$ 时他的头发的不良度。

### 输入描述:

​输入的第一行包含一个正整数 $N(1\leq N\leq 10^5)$。 

​输入的第二行包含𝑁个用空格隔开的非负整数 $A_i(0\leq A_i\leq N)$。

### 输出描述:

​输入 $N$ 行，对于每一个 $j=0,1,...,N-1$，用一行输出 wby 头发的不良度。

### 测试样例:

#### 【样例1输入】

```
5
5 2 3 3 0
```

#### 【样例1输出】

```
0
4
4
5
7
```

#### 【样例1解释】

​$j=4$ 时，$A=[4,2,3,3,0]$，有7个逆序对。$j=3$时，$A=[3,2,3,3,0]$，有5个逆序对。

​$j=2,1$时，$A=[2,2,2,2,0]$，有4个逆序对。$j=0$时，**不存在逆序对**。

### 数据范围与提示:

| 子任务 | 分数 |   特殊性质   |
| :----: | :--: | :----------: |
|   1    |  40  | $N\leq 5000$ |
|   2    |  60  |  无特殊性质  |

### 题目分析:

​树状数组求逆序对数的典型例题。

​设包含第 $i$ 个数及其以前的任意一个数组成的逆序对数为 $res_i$，不难看出，当 $j\leq A_i$ 时，$res_i$ 的不能对答案造成任何贡献。

​设 $less_i$ 表示当 $j=i$ 时需要从答案中减去的逆序对的数量。

​因此我们可以记录对于每一个数 $A_i$ 不能对造成贡献的起始的 $j$ 的值 ( 其实就是 $A_i$ ) 并向 $less_{A_i}$ 加上 $res_i$ 的值，最后从后往前扫一遍，每次减去 $less_i$ 的值就是对于 $j=i$ 时的答案。

​时间复杂度为 $O(N\log N)$。

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define lowbit(x) x&-x
#define ywhin cin
#define ywhout cout
using namespace std;
bool ppppp;
const van N=101000;
van c[N],n;
van res[N],ans[N];
bool pppppp;
void add(van x,van num) {
	while (x<=n+1) {
		c[x]+=num;
		x+=lowbit(x);
	}
}
van query(van x) {
	van tmp=0;
	while (x) {
		tmp+=c[x];
		x-=lowbit(x);
	}
	return tmp;
}//树状数组
int main() {
	ywhin>>n;
	for (int i=1;i<=n;i++) {
		van tmp;
		ywhin>>tmp;
		res[tmp]+=query(n+1)-query(tmp+1);
		ans[n]+=query(n+1)-query(tmp+1);
		add(tmp+1,1);
	}//计算每次要减掉的值
	for (int i=n-1;i>=0;i--) {
		ans[i]=ans[i+1]-res[i];
	}//计算每一次的答案
	for (int i=0;i<n;i++) {
		ywhout<<ans[i]<<endl;
	}//输出
}
```

