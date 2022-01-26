[Pixiv: 83105924]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/83105924_p0.jpg'

## [USACO19JAN] Cow Poetry G 做题记录

### 题目描述:

​H 女士想要成为一名诗人，最近，她沉迷于研究伟人诗词，试图创造一些属于自己的诗歌。

​H 女士认识个诗词中可能会使用的单词。第 $i$ 个单词音节长度为 $s_i$，结尾韵部为 $c_i$。每一首诗由 $M$ 行组成，每一行必须由 $K$ 个音节构成。此外，还需要遵循一定的押韵模式，即某些行结尾的韵部必须相同。

​H 女士想要知道她可以写出多少符合条件的诗歌。

### 输入描述:

​输入的第一行包含三个正整数 $N,K,M(1\leq N\leq 5000,1\leq M\leq 10^5,1\leq K\leq 5000)$。分别表示 H 女士认识的单词数目，一首诗歌的行数以及每一行的音节数目。

​接下来的 $N$ 行，每行包括两个正整数 $s_i
(1\leq s_i\leq K)$ 和 $c_i(1\leq c_i\leq N)$，表示女士认识一个长度为 $s_i$ 个音节，结尾韵部为 $c_i$ 的单词。

​接下来的 $M$ 行，描述了一种押韵模式，每行包括一个大写字母，所有押韵模式为 $e_i$ 的行，其结尾单词的韵部必须相同，不要求不同的韵部模式结尾韵部不同。

### 输出描述:

​输出 H 女士可以写出的满足这些条件的不同的诗歌的数量，答案对 $10^9+7$ 取模。

### 测试样例:

#### 【样例1输入】

```
3 3 10
3 1
4 1
3 2
A
B
A
```

#### 【样例1输出】

```
960
```

#### 【样例1解释】

​在这个例子中，H 女士认识三个单词，前两个单词韵部相同。长度分别为3个音节和4个音节，最后一个单词长度为3个音节，不与其他单词押韵。她想写一个三行的诗，每一行包含10个音节，并且第一行和第三行结尾韵部相同。这样的诗一共有960首。下面是满足条件的一首(其中1,2,3分别代表第一个，第二个，第三个单词):121 123 321。

### 数据范围与提示:

| 子任务 | 分数 |       特殊性质       |
| :----: | :--: | :------------------: |
|   1    |  30  | $N\leq 50,M\leq 200$ |
|   2    |  30  |     $M\leq 5000$     |
|   3    |  40  |      无特殊性质      |

### 题目分析:

​分析题意，对于每一行的诗词，只对最后一个单词的韵部有要求，其他的单词可以随便放，组成一个长度为 $K$ 的诗词就行了。

​可以考虑做一个类似完全背包的问题，用 dp 推导出前面放了长度为 $i$ 的单词的总方案数 $f_i$。那么以 $j$ 单词结尾的诗句就一共有 $res_j=f_{K-s_j}$ 种。

​接下来就是考虑计算的问题。将所有对韵脚的要求排个序，统计出某一种韵脚的个数 $x$。对于某一种韵脚 $i$，他能组合的方案数有 $s_i=\sum_{j=1}^n res_j^{x_i}$ 种，最终的答案则是 $\prod_{i='A'}^{'Z'} s_i$。

​时间复杂度为 $O(NK)$。

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define ywhin cin
#define ywhout cout
using namespace std;
const van N=10010,M=100010,K=10010,mod=1e9+7;
//ifstream ywhin("poetry.in");
//ofstream ywhout("poetry.out");
van f[K],res[N];
van s[N],a[N];
bool appear[N];
van x[N];
van n,m,k;
van power(van a,van b) {
	van ans=1,base=a;
	while (b) {
		if (b%2) ans*=base,ans%=mod;
		base*=base,base%=mod,b/=2;
	}
	return ans%mod;
}//快速幂计算a的b次方模mod的值
int main() {
	ywhin>>n>>m>>k;
	for (int i=1;i<=n;i++) {
		ywhin>>s[i]>>a[i];
		appear[a[i]]=1;
	}
	f[0]=1;
	for (int i=0;i<=k;i++) {
		for (int j=1;j<=n;j++) {
			if (i>=s[j]) f[i]+=f[i-s[j]],f[i]%=mod;
		}
	}//dp推导f数组
	for (int i=1;i<=m;i++) {
		char tmp;
		ywhin>>tmp;
		x[tmp]++;
	}
	for (int i=1;i<=n;i++) {
		if (k>=s[i]) res[a[i]]+=f[k-s[i]],res[a[i]]%=mod;
	}
	van ans=1;
	for (int i='A';i<='Z';i++) {
		van tmp=0;
		if (x[i]==0) continue;
		for (int j=1;j<=n;j++) {
			if (appear[j]) {
				tmp+=power(res[j],x[i]);
				tmp%=mod;
			}
		}
		ans*=tmp;
		ans%=mod;	
	}//统计答案
	ywhout<<ans<<endl;
}
```

