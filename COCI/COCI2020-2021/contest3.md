[Pixiv: 92260993]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/92260993_p0.png'

# COCI2020-2021#3 做题记录

## T1 Knjige

### 题目描述:

​		小 A 同学的家里有两个书桌，其中的一个桌子上摆着一摞书，另一个书桌则是空的。小 A 同学是一个强迫症，他总是希望书籍按照自己的厚度堆放。具体的说，最厚的书应该放在最下面， 次厚的书应该放在倒数第二本的位置，以此类推。可惜，现在堆放着书的书桌并没有按照这个规则来摆放。小 A 同学决定进行整理。每一次，小 A 同学会执行如下两个操作中的一个：

​		1.将一本书从一摞书的顶部拿到自己的某一只手上，前提是这只手上之前没有别的书。 

​		2.将自己某一只手上的书放到某个书桌上那一摞书的顶部。

​		小 A 虽然热爱运动，身体强健，但是并不擅长于整理书籍，因此，他希望你能够给他一个操作序列，根据这个操作序列的顺序执行，就可以将所有的书籍整理到**左侧书桌**上，且书籍按照**从厚到薄的顺序从下往上堆叠。**

### 输入描述:

​		输入的第一行包括一个正整数 $n(1\leq n\leq 100)$，表示书籍的数量。 

​		输入的第二行是一个长度为 $n$ 的数组 $d$，$d_i(1\leq d_i\leq 1000)$ 表示最开始左侧书桌上的那一摞书从上往下数第 $i$ 本书的厚度。

### 输出描述:

​		输出的第一行是一个整数 $k(0\leq k\leq 10^5)$，表示你给出的答案操作序列的长度。 

​		接下来的 $k$ 行每一行是一个格式为: `INSTRUCTION HAND SHELF` 的指令，各个参数意义如下: 

​		1.`INSTRUCTION` 是下列单词中的一个: `UZMI` (表示从某一个书架顶拿书)，`STAVI`(将一本 书放在某一个书架上) 

​		2.`HAND` 只能是字符 `L` 和 `D` 中的一个，`L `表示左手，`D` 表示右手。

​		3.`SHELF` 也只能是字符 `L` 和 `D` 中的一个，`L` 表示左侧书架，`D` 表示右侧书架。

​		例如:`UZMI L L` 表示的意思就是从左侧书架顶上取出一本书拿在左手上。你不需要最小化你的操作序列长度 $k$。不过操作序列的长度不能超过 $10^5$。可以保证存在满足这个条件的操作序列。

### 分析:

​		由于此题没要求最小化操作序列长度 $k$，因此只需要随便输出可行方案即可。

​		最简单的一个思路就是现将所有的书籍全部转移到右书桌上，再在右书桌中依次寻找剩下的书本中最大的一个并将其放置左书桌上。

​		至于如何操作，我们可以先将某一本当前状态下最厚的书以上的所有书籍全部通过某一只手放到另一个书架上，再将当前这本书拿起来，并用另外一只手将刚刚我们所拿到另外一个书架上的书全部拿回来，再将手上的这本书放到另外一个书架上。

​		对于 $k\leq 10^5$ 的限制，由于每次转移时放到另外一个书架上的书的本数不会超过 $n$，因此每次放书所要执行的操作次数不会超过 $4n$。总操作次数不会超过 $4n^2\leq 4\times 10^4$。

​		时间复杂度为 $O(n^2)$

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1010
#define ywhin cin
#define ywhout cout
using namespace std;
van n,d[N],tmp[N],cnt;bool used[N];
struct res 
{
	string op;
	char hand,shelf;
}ans[N*N];
void Output(bool get,bool left_hand,bool left_shelf) {
	cnt++;
	if (get) ans[cnt].op="UZMI";
	else ans[cnt].op="STAVI";
	if (left_hand) ans[cnt].hand='L';
	else ans[cnt].hand='D';
	if (left_shelf) ans[cnt].shelf='L';
	else ans[cnt].shelf='D';
}//添加输出
int main() {
	ywhin>>n;
	for (int i=1;i<=n;i++) {
		ywhin>>d[i];
	}
	memcpy(tmp,d,sizeof tmp);
	sort(tmp+1,tmp+n+1);
	for (int i=1;i<=n;i++) {
		Output(1,1,1);
		Output(0,1,0);
	}//将所有书放到右书架上
    for (int i=n;i>=1;i--) {
		van ct=n;
		while(ct>=1) {
			if (!used[ct]&&d[ct]==tmp[i]) break;
			ct--;
		}//查找最厚的书的上面有多少本书
		used[ct]=1;
		for (int j=n;j>ct;j--) if (!used[j]) {
			Output(1,1,0);
			Output(0,1,1);
		}//将其上面的书全部放到另一个书架上
		Output(1,1,0);//拿起这本书
		for (int j=n;j>ct;j--) if (!used[j]) {
			Output(1,0,1);
			Output(0,0,0);
		}//将刚刚拿走的书通过另一只手拿回来
		Output(0,1,1);//将这本书放下去
	}
	ywhout<<cnt<<endl;
	for (int i=1;i<=cnt;i++) {
		ywhout<<ans[i].op<<" "<<ans[i].hand<<" "<<ans[i].shelf<<endl;
	}
	return 0;
}
```



## T2 Vlak

### 题目描述:

​		Nina 和 Emilija 正在玩一个特殊的游戏。这个游戏是在一张最开始为空白的纸上进行的。在每一个人的行动回合内，这个人会在这张纸上当前的单词后面加入一个字母。她们会轮流行动， 而 Nina 先手行动。 

​		操作者必须保证这样一个条件：在添加完一个字符后，整张纸上的单词必须是操作人最喜欢的歌曲的一个单词的前缀。如果不满足条件，进行这个操作的人就输了。

​		你的问题是，如果两个人都采取最优策略，那么谁会获得最后的胜利。

### 输入描述:

​		输入的第一行是一个正整数 $n$，表示 Nina 最喜欢的歌曲内的单词数目。

​		接下来的 $n$ 行，每行包含一个字符串，即 Nina 最喜欢的歌曲内的一个单词。

​		接下来的一行是一个正整数 $n$，表示 Emilija 最喜欢的歌曲的单词数目。 

​		接下来的 $m$ 行，每行包含一个字符串，即 Emilija 最喜欢的歌曲内的一个单词。

### 输出描述:

​		输出获胜者的名字，即 Nina 或 Emilija。

### 分析:

​		很明显的一个 Trie 加博弈论。

​		由于所有字母的总长度小于等于 $2\times 10^5$，因此我们可以将所有的字母全部扔进 Trie 树里，并标记某个单词究竟是谁的。

​		最后一遍 DFS 扫描 Trie 树来判断最后究竟是谁获胜即可。

​		时间复杂度为 $O(\sum_{i=1}^n s_i.size()+\sum_{i=1}^ms_i.size())$ (实际上就是字符串的总长度)。

### 代码:

```C++
#include<bits/stdc++.h>
using namespace std;
int n,m,cnt;
struct Trie {
	int son[26];
	bool w[2];
} a[200009];
string s;
void add(string t,bool p) {
	int id=0;
	for(int i=0; i<t.size(); i++) {
		if(a[id].son[t[i]-'a']==0)
			a[id].son[t[i]-'a']=++cnt;
		id=a[id].son[t[i]-'a'];
		a[id].w[p]=1;
	}
}
bool dfs(int x,bool r) {
	Trie now=a[x];
	for(int i=0; i<26; i++) {
		int v=now.son[i];
		if(v==0) continue;
		if(a[v].w[r]&&dfs(v,!r)==r) return r;
	}
	return (!r);
}
int main() {
	scanf("%d",&n);
	for(int i=1; i<=n; i++) {
		cin>>s;
		add(s,1);
	}
	scanf("%d",&m);
	for(int i=1; i<=m; i++) {
		cin>>s;
		add(s,0);
	}
	if(dfs(0,1)) cout<<"Nina";
	else cout<<"Emilija";
	return 0;
}
```



## T3 Sateliti

### 题目描述:

​		一个科学团队正在使用科学望远镜观测土星的卫星。为此，科学家们必须对不同卫星的照片进行分类。这个任务相当困难，因为从不同角度上看过去，卫星的面貌是不一样的。 

​		卫星图片可以被看成是一个 $n\times m$ 的字符矩阵，矩阵上由两种字符 `*` 和 `.` 构成。我们说两个图片表示的是同一个卫星，表示通过一些**行列平移**，两张图片会变得完全相同。 

​		为了区分不同的卫星，科学家每需要找到每一个卫星的**字典序最小**的字符矩阵。注意，当我们比较两个字符矩阵的字典序的时候，你可以理解为比较两个字符矩阵**按行拼接**起来的字符串的字典序。你的任务是对于一个给定的字符矩阵，找出它经过行列平移后字典序最小的形式。

### 输入描述:

​		输入的第一行包含两个正整数 $n,m(q\leq n,m\leq 1000)$。表示字符矩阵的大小。

​		接下来的 $n$ 行，每行一个长度为 $m$ 的字符串，字符串仅由 `*` 和 `.`构成。

### 输出描述:

​		输出 $n$ 行，每行 $m$ 个字符，表示我们需要找到的字典序最小的字符矩阵。

### 分析:

​		由于涉及到比较字典序，考虑使用二维哈希算法。

​		设 $hash(c_{i,j})=p^i\times q^j\times c_{i,j}$，则一个二维矩阵的哈希值可以用这个矩阵中的每一个字符的哈希值之和来表示。至于如何求出某一矩阵中每一个字符的哈希值之和，可以考虑使用二维前缀和。设 $sum_{i,j}$ 为以 $(1,1)$ 为起始点的长为 $i$，宽为 $j$ 的矩阵中所有字符的哈希值之和。则一个起点为 $(x,y)$ 且长为 $l$，宽为 $w$ 的矩阵哈希为 $sum_{x+l-1,y+j-1}-sum_{x-1,y+j-1}-sum_{x+i-1,y-1}+sum_{x-1,y-1}$。

​		接下来两个二分查找就可以确定两个矩阵的最长公共前缀。先将两个矩阵移动到相同的位置，将整个矩阵乘上 $p^i\times q^j$ 即可。再比较两个矩阵最多有多少行是相等的，最后比较两个矩阵的下一行有多上个字符是相等的。

​		执行完以上操作之后，比较下一位的字符，更小的代表该矩阵的字典序最小。

​		时间复杂度为 $O(n^2\log_2 n)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 3010
#define ywhin cin
#define ywhout cout
using namespace std;
const van p=131,q=133;
char ch[N][N];
van n,m,sx=1,sy=1;
unsigned van hash[N][N],sum[N][N];
unsigned van power(van a,van b) {
	unsigned van ans=1,base=a;
	while (b>0){
		if (b%2==1) ans*=base;
		base*=base,b>>=1; 
	}
	return ans;
}//计算乘方
unsigned van HashCalc(van i,van j) {
	return power(p,i)*power(q,j)*ch[i][j];
}//计算单点哈希
void init() {
	for (int i=1;i<=n*2;i++) {
		for (int j=1;j<=m*2;j++) {
			sum[i][j]=sum[i][j-1]+sum[i-1][j]-sum[i-1][j-1]+hash[i][j];
		}
	}
}//计算二维前缀和
unsigned van Calc_Matrix(van sx,van sy,van height,van width) {
	van ex=sx+height-1,ey=sy+width-1;sx--,sy--;
	return sum[ex][ey]-sum[sx][ey]-sum[ex][sy]+sum[sx][sy];
}//查询矩阵哈希
//ifstream ywhin("sateliti.in");
//ofstream ywhout("sateliti.out");
int main() {
	ywhin>>n>>m;
	for (int i=1;i<=n;i++) {
		for (int j=1;j<=m;j++) {
			ywhin>>ch[i][j];
			ch[i][j+m]=ch[i+n][j]=ch[i+n][j+m]=ch[i][j];
		}
	}//开个四倍数组
	for (int i=1;i<=n*2;i++) {
		for (int j=1;j<=m*2;j++) {
//			cout<<power(2,2)<<endl;
			hash[i][j]=HashCalc(i,j);
		}
	}//计算单点哈希
	init();
	for (int i=1;i<=n;i++) {
		for (int j=1;j<=m;j++) {
			if (i!=1||j!=1) {
				unsigned van base1=power(p,sx-i)*power(q,sy-j),
							 base2=power(p,i-sx)*power(q,j-sy);//移位
				van l=1,r=n,tmp=0;
				while(l<=r) {
					van mid=l+r>>1;
					unsigned van matrix1=Calc_Matrix(i,j,mid,m)*base1,
								 matrix2=Calc_Matrix(sx,sy,mid,m)*base2;
					if (matrix1==matrix2) {
						tmp=mid;l=mid+1;
					}
					else r=mid-1;
				}//计算最多有多少行是相等的
//				cout<<i<<" "<<j<<" "<<sx<<" "<<sy<<" "<<tmp<<endl;
				l=1,r=m;van tmp2=0;
				while (l<=r) {
					van mid=l+r>>1;
					unsigned van matrix1=Calc_Matrix(i+tmp,j,1,mid)*base1,
								 matrix2=Calc_Matrix(sx+tmp,sy,1,mid)*base2;
					if (matrix1==matrix2) {
						tmp2=mid,l=mid+1;
					}
					else r=mid-1;
				}//计算下一行有多少个相等的连续的字符
				if (ch[i+tmp][j+tmp2]<ch[sx+tmp][sy+tmp2]) sx=i,sy=j;//比较上述结果的下一位的字符
//				cout<<i<<" "<<j<<" "<<sx<<" "<<sy<<endl;
			}
		}
	}
	for (int i=1;i<=n;i++) {
		for (int j=1;j<=m;j++) {
			ywhout<<ch[i+sx-1][j+sy-1];
		}
		ywhout<<endl;
	}
	return 0;
}
```



## T4 Selotejp

### 题目描述:

​		M 先生的手中有一个大小为 $n\times m$ 的棋盘，每一个棋盘的格子大小都是 $1\times 1$。不幸的是，有一些格子已经弄脏了，需要重新粉刷。

​		一次粉刷可以粉刷同一行或者同一列的某一些格子。但是 M 先生不允许粉刷到不需要粉刷的格子。你的任务是，求出最少需要粉刷多少次，才能够把棋盘上的脏格子全部粉刷一遍。

### 输入描述:

​		输入的第一行包含两个整数 $n,m(1\leq n\leq 1000,1\leq m\leq 10)$，表示棋盘的大小。 

​		接下来的 $n$ 行，每行一个长度为 $m$ 的字符串。只由字符 `.` 和 `#` 组成。字符 `.` 表示这个格子是干净的，字符 `#` 表示这个格子是脏的，需要粉刷。

### 输出描述:

​		输出一行，一个整数，最少粉刷次数。

### 分析:

#### 50Pts:

​		由于 $m\leq 10$，很自然而然地就会想到时间复杂度会出现一个 $2^m$。

​		考虑使用状压 DP，设 $f[x][k]$ 为搜索到 $x$ 行且当前行的状态为 $k$ 时需要的最小粉刷步数。

​		枚举上一次的状态与当前行的状态，判断可行性后再进行步数的转移。

​		时间复杂度为 $O(n\times 2^{2m})$ (据说优化一下还可以拿到满分)。

#### 100Pts:

​		在完成 Subtask1 的基础上继续完成此题。考虑使用轮廓线 DP，记录 $f[i][j][k]$ 为枚举到第 $i$ 行 $j$ 列且前 $m$ 个格子状态为 $k$ 时需要用到的最小步数。

​		设 $ch[i][j]$ 为输入数组，$(ii,jj)$ 为所要转移到的下一个点的左边。分两种情况进行状态转移：

​		1.当 `ch[i][j]='.'` 时，`f[i][j][k]` 可以直接转移到 `f[ii][jj][k>>1]`。

​		2.当 `ch[i][j]='#'` 时，`f[i][j][k]` 可以转移到 `f[ii][jj][(k>>1)+(1<<m-1)]`，且当 `k&(1<<(m-1))&&j>1` 不成立时步数需要加一。在此状态下，`f[i][j][k]` 还可以转移到 `f[ii][jj][k>>1]`，且当 `((k&1)==0&&i>1&&ch[i-1][j]=='#')` 不成立时步数需要加一。

​		最后输出 `f[n+1][1]` 状态下的最大值即可。

​		时间复杂度为 $O(nm\times 2^m)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1010
#define ywhin cin
#define ywhout cout
using namespace std;
van f[N][11][1<<11];
char ch[N][11];
van n,m;
int main(){
	memset(f,(1<<6)-1,sizeof f);
	ywhin>>n>>m;
	for (int i=1;i<=n;i++) {
		for (int j=1;j<=m;j++) {
			ywhin>>ch[i][j];
		}
	}
	f[1][1][0]=0;
	for (int i=1;i<=n;i++) {
		for (int j=1;j<=m;j++) {
			for (int k=0;k<(1<<m);k++) {
				van ii=i,jj=j+1;
				if (jj>m) jj-=m,ii++;//下一个点的坐标
				if (ch[i][j]=='.') {
					f[ii][jj][k>>1]=min(f[ii][jj][k>>1],f[i][j][k]);
				}//第一种情况
				else {
					f[ii][jj][(k>>1)+(1<<(m-1))]=min(f[ii][jj][(k>>1)+(1<<(m-1))],f[i][j][k]+!(k&(1<<(m-1))&&j>1));
					f[ii][jj][k>>1]=min(f[ii][jj][k>>1],f[i][j][k]+!((k&1)==0&&i>1&&ch[i-1][j]=='#'));
				}//第二种情况
			}
		}
	}
	van ans=1e18;
	for (int i=0;i<(1<<m);i++) ans=min(ans,f[n+1][1][i]);//查找最终状态下的最大值
	ywhout<<ans<<endl;
	return 0;
}
```



## T5 Specijacija

### 题目描述:

​		给定一个长度为 $n$ 的正整数序列 $a_1,a_1,...,a_n$。其中 $\frac{i(i-1)}{2}<a_i\leq \frac{i(i+1)}{2}$ 。 

​		这个序列会生成一个包含 $\frac{(n+1)(n+2)}{2}$ 个节点的树。树高为 $n+1$。比如，序列 (1,2,6) 就会生成 下面这棵树： 

![image-20210827145513885](./T5-题目描述-1.jpg)

​		树的第 $i$ 层包含了节点 $\frac{i(i-1)}{2}+1,...,\frac{i(i+1)}{2}$ 。在这一层中，只有节点 $a_i$ 由两个孩子，其他的节点都只有一个孩子。

​		你的问题是回答 $q$ 个询问，每一个询问会询问你节点 $x,y$ 的最近公共祖先。

### 输入描述:

​		输入的第一行包括三个正整数 $n,q,t(1\leq n,q\leq 2\times 10^5,t\in {0,1})$。分别表示生成树的序列的长度，查询的数目以及是否强制在线。 

​		输入的第二行是一个长度为 $n$ 的数组 $a_i(\frac{i(i-1)}{2}<a_i\frac{i(i+1)}{2})$。这就是生成树的数组。接下来的 $n$ 行，每行两个正整数 $\tilde{x},\tilde{y}(1\leq \tilde{x_i},\tilde{y_i}\leq \frac{i(i+1)}{2})$。这两个数会决定最终询问的 $x,y$。 我们令 $z_i$ 表示第 $i$ 个询问操作的答案，并令 $z_0=0$。$x_i$ 和 $y_i$ 可以用如下方法求出:
$$
x_i=((\tilde{x_i}-1+t\times z_{i-1})\ mod\ \frac{(n+1)
(n+2)}{2})+1\\
y_i=((\tilde{y_i}-1+t\times z_{i-1})\ mod\ \frac{(n+1)(n+2)}{2})+1
$$
​		$mod$ 表示整数取模运算。需要注意的是，如果 $t=0$，那么就有 $x_i=\tilde{x_i}$ 而 $y_i=\tilde{y_i}$。

### 输出描述:

​		输出 $q$ 行，每行一个正整数，即 $x,y$ 的最近公共祖先的编号。

### 分析:

#### 20Pts:

​		直接建出一棵树然后跑个 LCA 就行了。

​		时间复杂度为 $O(q\log_2n)$，但 $n\leq 1000$。

#### 100Pts:

​		注意：此方法惨遭卡常，在运气好的情况下可以拿到满分。

​		由于需要支持在线查询，且 $n\leq 2\times 10^5$，直接建树空间一定会炸，考虑使用可持久化线段树来建树。

​		我们需要建两颗主席树，一颗用来维护建到当前深度时还有哪些点没有被使用过，一颗用来维护应该接在哪一个节点上。

​		每一次输入一个 $a[i]$ 时，计算 $a[i]$ 所在的行 $x$ 与列 $y$，在第一颗主席树中二分查询第 $y$ 个没有被使用的点 $k$，将这个点加入我们新建的一个节点 $node[cnt]$ 中。并将其与第二颗主席树上第 $k$ 与 $k+1$ 个点连边，将第一颗主席树的第 $k+1$ 个节点赋值为 0。

​		接着在节点数组 $node$ 上跑一遍正常的 DFS，计算出其树状结构下的深度与它上升 $2^0$ 的深度后的节点，再通过递推计算出某一个节点上升 $2^z$ 的深度后的节点。

​		接下来的查询操作，先将两个数 $x$ 与 $y$ 解密出来，计算出 $x$ 与 $y$ 所在的行 $f_x,f_y$ 与列 $g_x,g_y$，再在第二颗主席树中查询当其建树深度为 $f_x,f_y$ 时对应的节点 $x',y'$。并从这个节点开始在所有 $node$ 节点所构成的树上进行正常的 LCA。若发现这两个在同一条链上，则直接输出行数较低的那一个，否则输出 $x',y'$ 的最近公共祖先的真实编号。

​		时间复杂度为 $O(n\log_2^2n)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
using namespace std;
const van k=21,N=4e5+5;
struct node {
	van ls,rs;
	van num;
};
van up[N][k],n,a[N],RealNum[N<<2],cnt;
van root[N],cnt2,root2[N],cnt3;
node tree[N<<5],tree2[N<<5];
vector<van> g[N];van deep[N],q,t;
van Calc(van x) {
	return x*(x-1)/2;
}
pair<van,van> GetPos(van num) {
	van l=1,r=n+1,ans;
	while (l<=r) {
		van mid=l+r>>1;
		if (Calc(mid)<num) ans=mid,l=mid+1;
		else r=mid-1;
	}
	return make_pair(ans,num-Calc(ans));
}//由某一个数获取到其所在的行与列
van BuildTree(van l,van r) {
	van now=++cnt2;
	if (l==r) {
		++cnt;RealNum[cnt]=Calc(n+1)+l;
		tree[now].num=cnt;
		return now;
	}
	van mid=l+r>>1;
	tree[now].ls=BuildTree(l,mid);
	tree[now].rs=BuildTree(mid+1,r);
	return now;
}//建立第一颗主席树(及上文所提到的第二颗主席树)
van ChangeTree(van p,van l,van r,van where,van num) {
	van now=++cnt2;
	tree[now]=tree[p];
	if (l==r) {
		++cnt;RealNum[cnt]=num;
		tree[now].num=cnt;
		return now;
	}
	van mid=l+r>>1;
	if (where<=mid) tree[now].ls=ChangeTree(tree[p].ls,l,mid,where,num);
	else tree[now].rs=ChangeTree(tree[p].rs,mid+1,r,where,num);
	return now;
}//单点修改主席树上某一节点的值
van QueryTree(van p,van l,van r,van num) {
	if (l==r) return tree[p].num;
	van mid=l+r>>1;
	if (num<=mid) return QueryTree(tree[p].ls,l,mid,num);
	else return QueryTree(tree[p].rs,mid+1,r,num);
}//单点查询主席树上某一节点的值
van BuildTree2(van l,van r) {
	van now=++cnt3;
	if (l==r) {
		tree2[now].num=1;
		return now;
	}
	van mid=l+r>>1;
	tree2[now].ls=BuildTree2(l,mid);
	tree2[now].rs=BuildTree2(mid+1,r);
	tree2[now].num=tree2[tree2[now].ls].num+tree2[tree2[now].rs].num;
	return now;
}//建立第二颗主席树(及上文所提到的第一颗主席树)
van ChangeTree2(van p,van l,van r,van where,van num) {
	van now=++cnt3;
	tree2[now]=tree2[p];
	if (l==r) {
		tree2[now].num=num;
		return now;
	}
	van mid=l+r>>1;
	if (where<=mid) tree2[now].ls=ChangeTree2(tree2[p].ls,l,mid,where,num);
	else tree2[now].rs=ChangeTree2(tree2[p].rs,mid+1,r,where,num);
	tree2[now].num=tree2[tree2[now].ls].num+tree2[tree2[now].rs].num;
	return now;
}//单点修改第二颗主席树的值
van QueryTree2(van p,van l,van r,van L,van R) {
	if (L<=l&&r<=R) {
		return tree2[p].num;
	}
	van mid=l+r>>1,sum=0;
	if (L<=mid) sum+=QueryTree2(tree2[p].ls,l,mid,L,R);
	if (R>mid) sum+=QueryTree2(tree2[p].rs,mid+1,r,L,R);
	return sum;
}//区间查询第二颗主席树上某一些节点的值的和
void AddLine(van father,van son) {
	g[father].push_back(son);
	g[son].push_back(father);
}//新建一条边
void initDFS(van now,van f) {
	for (int i=0;i<g[now].size();i++) {
		if (g[now][i]!=f) {
			deep[g[now][i]]=deep[now]+1;
			up[g[now][i]][0]=now;
			initDFS(g[now][i],now);
		}
	}
}//预处理节点深度与up[i][0]数组
void initLCA() {
	deep[cnt]=1;
	initDFS(cnt,0);
	for (int j=1;j<k;j++) {
		for (int i=1;i<=cnt;i++) {
			up[i][j]=up[up[i][j-1]][j-1];
		}
	}
}//递推处理up数组
van GetDeep(van num) {
	return GetPos(num).first;
}
van LCA(van a,van b,van a1,van b1) {
	if (deep[a1]<deep[b1]) swap(a1,b1),swap(a,b);
	for (int i=k-1;i>=0;i--) {
		if (deep[up[a1][i]]>=deep[b1]) {
			a1=up[a1][i];
		}
	}
	if (a1==b1) {
		return (GetDeep(a)<GetDeep(b)?a:b);
	}
	for (int i=k-1;i>=0;i--) {
		if (up[a1][i]!=up[b1][i]) {
			a1=up[a1][i],b1=up[b1][i];
		}
	}
	return RealNum[up[a1][0]];
} //正常的LCA(但直接返回最终结果)
inline van read() {
    van x=0,f=1;
    char ch=getchar();
    while(ch<'0'||ch>'9'){
        if(ch=='-')
            f=-1;
        ch=getchar();
    }
    while(ch>='0'&&ch<='9'){
        x=(x<<1)+(x<<3)+(ch^48);
        ch=getchar();
    }
    return x*f;
}//快读
inline void write(van x) {
    char F[200];
    van tmp=x>0?x:-x ;
    if(x<0)putchar('-') ;
    van cnt=0 ;
    while(tmp>0)
    {
    	F[cnt++]=tmp%10+'0';
        tmp/=10;
    }
    while(cnt>0)putchar(F[--cnt]) ;
    putchar('\n');
}//快输
int main() {
	n=read(),q=read(),t=read();
	for (int i=1;i<=n;i++) {
		a[i]=read();
	}
	root[n+1]=BuildTree(1,n+1);
	root2[n+1]=BuildTree2(1,n+1);//建树
	for (int i=n;i>=1;i--) {
		pair<van,van> pos=GetPos(a[i]);
		van l=1,r=n+1,tmp1,tmp2;
		while (l<=r) {
			van mid=l+r>>1;
			if (QueryTree2(root2[i+1],1,n+1,1,mid)>=pos.second) {
				tmp1=mid;r=mid-1;
			} 
			else l=mid+1;
		}//查找应该接在哪一列
		l=1,r=n+1;
		while (l<=r) {
			van mid=l+r>>1;
			if (QueryTree2(root2[i+1],1,n+1,1,mid)>=pos.second+1) {
				tmp2=mid,r=mid-1;
			}
			else l=mid+1;
		}//查找下一个有值的列
		van son1=QueryTree(root[i+1],1,n+1,tmp1),
			son2=QueryTree(root[i+1],1,n+1,tmp2);
		root[i]=ChangeTree(root[i+1],1,n+1,tmp1,a[i]);
		van now=QueryTree(root[i],1,n+1,tmp1);
		AddLine(now,son1);AddLine(now,son2);//建边
		root2[i]=ChangeTree2(root2[i+1],1,n+1,tmp1,1);
		root2[i]=ChangeTree2(root2[i],1,n+1,tmp2,0);//修改第二颗主席树某一节点的边权
	} 
	initLCA();
	van z=0;
	while (q--) {
		van x,y;
		x=read(),y=read();
		if (t) {
			x=(x-1+t*z)%Calc(n+2)+1;
			y=(y-1+t*z)%Calc(n+2)+1;
		}//解密x&y
		pair<van,van> posx=GetPos(x),posy=GetPos(y);
		van l=1,r=n+1,tmp1,tmp2;
		while (l<=r) {
			van mid=l+r>>1;
			if (QueryTree2(root2[posx.first],1,n+1,1,mid)>=posx.second) {
				tmp1=mid,r=mid-1;
			}
			else l=mid+1;
		}
		l=1,r=n+1;
		while (l<=r) {
			van mid=l+r>>1;
			if (QueryTree2(root2[posy.first],1,n+1,1,mid)>=posy.second) {
				tmp2=mid,r=mid-1;
			}
			else l=mid+1;
		}//二分查询x&y节点在某一状态下所在的列
		van x1=QueryTree(root[posx.first],1,n+1,tmp1),
			y1=QueryTree(root[posy.first],1,n+1,tmp2);
		z=LCA(x,y,x1,y1);
		write(z);
	}
}
```

