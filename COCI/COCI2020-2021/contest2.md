# COCI2020-2021#2 做题记录

## T1 Crtanje

### 题目描述: 
​J 先生过去在 Logo 公司工作，他热爱画图，不过这些天他非常沮丧，因为他被辞退了。为了表示怀念，他决定绘制一条曲线来表示他曾经的公司在过去 $n$ 天的业绩。

​对于这 $n$ 天中的每一天，公司的业绩要么增长一个单位(用符号 `+`)来表示，要么减少一个单位(用符号 `-` 来表示)，或者保持不变(用符号 `=` 来表示)。我们把第一天公司的业绩定义为标准 0。

​J 先生会在一个巨大的无限字符矩阵中绘制这这一条曲线。矩阵的行表示公司的业绩，标准 0 上的第 $i$ 行，表示公司的收益为 $i$ 个单位。对于第 $i$ 天的业务变化，J 先生会在第 $i$ 列上绘制一些字符。这一列上每一行的字符遵循如下原则：

​如果第 $i$ 天公司的业务增长了，那么他就会在代表当天开始的时候公司业绩的对应行 $x$ 的第 $i$ 列设置为字符 `/`。

​如果第 $i$ 天公司的业务不变，则会在字符矩阵中把对应的位置设置为 `_`。如果第 $i$ 天的公司业务降低了，则会把字符矩阵的对应位置设置为 `\`。矩阵中的所有其他位置都是字符 `.`。

​你的任务是输出最小的，可以包含整个曲线的子矩阵。

### 输入描述:
​输入的第一行包括一个正整数 $n(1\leq n\leq 100)$，表示记录的天数。

​输入的第二行是一个长度为 $n$ 的由字符 `+` , `-` 和 `=` 组成的字符串，表示在给定的时间中，共色的业绩变化情况。

### 输出描述:
​输出包含了业绩曲线的最小子矩阵。
### 分析:

​水题一道，只需要挨着一个一个地去找这个字符应该画在哪儿就可以了。

​时间复杂度为 $O(n^2)$(主要是输出复杂度)。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1010
#define ywhin cin
#define ywhout cout
using namespace std;
char ans[N][N],cha[N];van n,max_,min_=1e18;
int main()
{
//	ifstream ywhin("crtanje.in");
//	ofstream ywhout("crtanje.out");
	van n;ywhin>>n;
	for (int i=1;i<=n;i++) ywhin>>cha[i];
	for (van i=1,now=500;i<=n;i++)
	{
		if (cha[i]=='=')
		{
			if (cha[i-1]=='-') now--;
			ans[now][i]='_';
			max_=max(max_,now);
			min_=min(min_,now);
		}
		else if (cha[i]=='-')
		{
			if (cha[i-1]!='-') now++;
			max_=max(max_,now);
			min_=min(min_,now);
			ans[now++][i]='\\';
		}
		else if (cha[i]=='+')
		{
			if (cha[i-1]=='-') now--;
			max_=max(max_,now);
			min_=min(min_,now);
			ans[now--][i]='/';
		}
	}//以y=500为基准线来查找这个曲线究竟长啥样
	for (int i=min_;i<=max_;i++)
	{
		for (int j=1;j<=n;j++) ywhout<<(ans[i][j]==0?'.':ans[i][j]);
		ywhout<<endl;
	}//输出矩阵
	return 0;
}
```



## T2 Odasiljaci
### 题目描述:
​这是 Sean 最后一次在银幕上出演 James Bond(007)。

​他的任务是让 $n$ 座坐落在广阔沙漠中的基站连接成一个网络。这个沙漠可以用一个二维坐标系来表示。007可以决定所有基站(基站的信号强度必须是统一的)的信号强度，信号强度可以用一个非负整数 $r$ 来表示。一个基站的覆盖范围包括以基站坐标为圆心的半径为 $r$ 内的所有区域。如果两个基站存在公共覆盖点 $c$。我们就称这两个基站之间是可以通信的。如果基站 A 和基站 B，基站 B 和基站 C 可以通信。那么我们称基站 A 和基站 C 之间也可以建立通信。

​007需要让任意两个基站之间都可以通信。鉴于军情6处经费有限，而越强的信号需要的经费也就越高。你需要帮007求出最小的可以让所有基站互相通信的信号强度 $r$。

### 输入描述:
​输入的第一行是一个正整数 $n(1\leq n\leq 1000)$，表示基站的数目。

​接下来的 $n$ 行，每行两个正整数 $x,y(1\leq x,y\leq 10^9)$，表示第 $i$ 个基站的坐标。 

### 输出描述:
​输出最小的信号强度 $r$。采用实数比较模式，与标准答案的差距小于 $10^{-6}$ 即为正确。
### 分析:

​首先计算每两个点之间所需要的 $r_{min}$，排序之后挨个判断继续加上这条边后能否使得任意两个基站之间都可以通信，并且记录能使得上述条件成立的 $r$ 的最小值。至于判断上述条件如何成立，只需要用一个并查集来维护即可。

​时间复杂度为 $O(2n^2\log_2n)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1010
#define ywhin cin
#define ywhout cout
using namespace std;
struct node
{
	double num;
	van a,b;
}line[N*N];
van n;double x[N],y[N],ans;van cnt;
van f[N];
van FindFather(van x)
{
	if (x==f[x]) return x;
	return f[x]=FindFather(f[x]);
}//并查集
bool cmp(node a,node b)
{
	return a.num<b.num;
}
int main()
{
//	ifstream ywhin("odasiljaci.in");
//	ofstream ywhout("odasiljaci.out");
	ywhin>>n;
	for (int i=1;i<=n;i++) ywhin>>x[i]>>y[i];
	for (int i=1;i<=n;i++)
	{
		for (int j=1;j<i;j++)
		{
			double dis=(x[i]-x[j])*(x[i]-x[j])+(y[i]-y[j])*(y[i]-y[j]);
			dis=sqrt(dis);
			dis/=2;
			line[++cnt].num=dis;
			line[cnt].a=i,line[cnt].b=j;
		}
	}//计算任意两个点之间的r的最小值
	for (int i=1;i<=n;i++) f[i]=i;
	sort(line+1,line+cnt+1,cmp);//按照每两个点之间的r的最小值排序
	for (int i=1;i<=cnt;i++)
	{
		if (FindFather(line[i].a)!=FindFather(line[i].b))
		{
			f[FindFather(line[i].a)]=f[FindFather(line[i].b)];
			ans=line[i].num;
		}
	}//并查集查找答案
	ywhout<<fixed<<setprecision(7)<<ans<<endl;
	return 0;
}
```



## T3 Euklid 
### 题目描述:
​小 E 是一个对数学非常感兴趣的孩子，一天，他突发奇想发明了一个函数 R。
$$
R(a,b)=
\begin{cases}
R(b,a), a<b\\\\
R(\frac{a}{b},b), a\geq b>1\\\\
a, a\geq b=1
\end{cases}
$$
​你的任务是，对于给定的正整数 **$g$** 和 **$h$**。找到一对正整数(a,b)，使得 **$a$ 和 $b$ 的最大公约数为 $g$，同时 $R(a,b)=h$**。
### 输入描述:
​输入的第一行包含一个正整数 $t(1\leq t\leq 40)$。表示测试数据的数目。

​接下来的 $t$ 行，每行两个正整数 $g$ 和 $h(h\geq 2)$，含义见题目描述。 

### 输出描述:

​总共输出 $t$ 行，对于第 $i$ 行，输出两个正整数 $a_i,b_i$。满足 $\gcd(a_i,b_i)=g_i$ 且 $R(a_i,b_i)=h_i$。

​输出的数字大小不能超过 $10^{18}$，保证这样的解总是存在的。如果由多组解，输出任意一组。

### 分析:

#### 4Pts:

​当 $g=h$ 时，特解 $a=b=g$ 能恰好使得 $\gcd(a,b)=g$ 且 $R(a,b)=g=h$。

#### 12Pts:

​在完成Subtask1的基础上考虑如何解决Subtask2。

​不难发现，当 $h=2$ 时，特解 $a=5\times g,b=2\times g$ 能恰好使得 $\gcd(a,b)=g$ 且 $R(a,b)=2$。

​注意：$a=5\times g,b=2\times g$ 并不是该Subtask的唯一解。

#### 20Pts:

​在完成Subtask1和Subtask2的基础上考虑如何解决Subtask3。

​不难发现，当 $g=h^2$ 时，特解 $a=h^3,b=h^2$ 能恰好使得 $\gcd(a,b)=h^2=g$ 且 $R(a,b)=h$。

​注意：$a=h^3,b=h^2$ 并不是该Subtask的唯一解

#### 100Pts:

​在完成前3个Subtask的基础上考虑如何解决此题。

​由于 $h=R(h,1)=R(1,h)=R(b,h)$，则计算 $k$ 次 $\lfloor\frac{b}{h}\rfloor$ 的最终结果一定为1。

​考虑暴力 $b$ 的值，由于 $\gcd(a,b)=g$，则 $b=yg$，暴力枚举 $yg$ 是否符合上述条件即可。

​对于计算 $a$ 的值，由于 $\gcd(a,b)=g$，则可以令 $a=xg$，由于 $R(a,b)=R(xg,yg)=R(yg,h)=R(h,1)$，即 $\lfloor\frac{x}{y}\rfloor=h$，那么我们可以令 $x=hy+1$，这样既满足 $R(a,b)=h$，也能使得 $\gcd(a,b)=g$。

​时间复杂度为 $O(\sum_{i=1}^t x_i)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 100010
#define ywhin cin
#define ywhout cout
using namespace std;
van t;
int main()
{
//	ifstream ywhin("euklid.in");
//	ofstream ywhout("euklid.out");
	ywhin>>t;
	while(t--)
	{
		van g,h;
		ywhin>>g>>h;
		if (g==h) ywhout<<g<<" "<<g<<endl;//Subtask1
		else if (g==h*h) ywhout<<h*h*h<<" "<<h*h<<endl;//Subtask3
		else if (h==2) ywhout<<5*g<<" "<<2*g<<endl;//Subtask2
		else
		{
			van now=2*g;bool ok=false;
			while (!ok)
			{
				van tmp=now;
				while(tmp>1) tmp/=h;
				if (tmp==1)
				{
					ywhout<<now*h+g<<" "<<now<<endl;
					ok=true;
				}
				now+=g;
			}//暴力枚举b的值是否符合条件
		}
	}
	return 0;
}
```



## T4 Sjekira

### 题目描述:

​M 先生是一个程序员，不过，他赚够了钱，来到了一个小村子定居。现在，凛冬将至，M 先生不得不砍伐木材生火过冬。

​M 先生面前有一棵巨大的树。M 先生将这棵物理意义上的树抽象成了数据结构中的一棵树。树上的每一个节点存在一个坚硬程度 $t_i$。当 M 先生想要砍掉树上的一条边时，他的斧头的受损程度会增加 $t_x+t_y$。$x$ 和 $y$ 分别是砍去这条边后形成的两个子树内 $t_i$ 最大的节点。

​你的任务是求出一个砍伐方案，将这棵树的所有边全部砍掉，使得 M 先生的斧头受到的损伤程度最小。

### 输入描述:

​输入的第一行包含一个整数 $n$，表示树的大小，树的节点编号从 1 到 $n$。

​第二行包含一个 $n$ 个整数，第 $i$ 个整数即为 $t_i(1\leq t_i\leq 10^9)$。含义见题面。

​接下来的 $n-1$ 行，每一行由两个整数 $x$ 和 $y(1\leq x,y\leq n)$。表示这棵树上的边。

### 输出描述:

​输出一行，一个整数，即最小的斧头磨损程度。

### 分析:

#### 40Pts:

​此档数据范围 $n\leq 1000$，在 $O(n^2)$ 的时间复杂度下完全能够通过此题。不难发现，当从权值最大的节点开始砍直到权值最小的节点的最终的损伤程度一定不劣于随便砍后最终的损伤程度。

​最后从根节点开始扫就行了，找到以当前节点为根的子树中权值最大的节点并将其砍掉，再从这个权值最大的点的子树开始扫，直到所有节点都被砍掉即可。

​时间复杂度为 $O(n^2)$。

#### 100Pts:

​按照上述思路进行观察。若某一个节点被找到了的话，那么其所在的子树在上一轮被分割时的最大值一定是它。然后在分割时其又会对最终结果产生 $t_i\times g_i$ 的贡献(设 $g_i$ 为 $i$ 节点的没被砍掉的边的数量)。因此，除权值最大的节点外，其他节点对最终结果的贡献值为 $t_i\times (g_i+1)$。由于权值最大的节点是第一个被分割的，因此在此之前没有进行分割操作，其对最终结果的贡献应为 $t_i\times g_i$。这样我们就可以通过对节点的权值排序再去计算每个结点对最终结果的贡献。最终计算时的时间复杂度仅为 $O(n)$。

​时间复杂度为 $O(n\log_2 n)$(主要是排序时间复杂度)。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 100010
using namespace std;
van n,line[N],sum;vector<van> g[N];
struct node{van num,id;}ans[N];
bool cmp(node a,node b){return a.num>b.num;}
int main(){
	cin>>n;
	for (int i=1;i<=n;i++) cin>>ans[i].num,ans[i].id=i;
	for (int i=1;i<n;i++){
		van f,s;
		cin>>f>>s;
		g[f].push_back(s);
		g[s].push_back(f);//建边
		line[s]++,line[f]++;//计算某个点所连接的边的数量
	}
    sort(ans+1,ans+n+1,cmp);
	sum+=line[ans[1].id]*ans[1].num;
	for (int i=0;i<g[ans[1].id].size();i++) line[g[ans[1].id][i]]--;//特殊处理权值最大的点
	for (int i=2;i<=n;i++){
		sum+=(line[ans[i].id]+1)*ans[i].num;
		for (int j=0;j<g[ans[i].id].size();j++) line[g[ans[i].id][j]]--;
	}//处理其他的点
	cout<<sum<<endl;
	return 0;
}
```



## T5 Svjetlo

原文链接: [COCI 2020/2021 Svjetlo（树形DP）_ZSJZ_liuzian的博客-CSDN博客](https://blog.csdn.net/qq_39565901/article/details/110502442)

### 题目描述:

​小 F 家的一些灯关上了。现在，小 F 得去把所有的灯打开。小 F 家的灯结构比较特殊，灯泡和电线组成了一个**树结构**。

​对于一个灯泡，有一个唯一对应的开关。如果灯是关着的，那么按一下开关之后，灯就会打开，如果灯是打开的，那么按一下开关之后灯就会灭掉。

​小 F 按灯泡有一个奇怪的癖好，他会沿着一条**树上的路径**依次找到所有节点对应的开关然后按下开关。你的任务是寻找一条最短的路径，沿着这条路径按开关，会使得**所有的灯泡都被打开**。

​输入数据保证至少由一个灯泡在最初的时候是熄灭的。

### 输入描述:

​输入的第一行包括一个正整数 $n$。表示灯泡的数量，灯泡的编号从 1 到 $n$。

​输入的第二行是一个长度为 $n$ 的由`0`和`1`组成的字符串。如果字符串的第 $i$ 位为`0`，那就表示编号为 $i$ 的灯泡最开始是熄灭的。如果字符串的第 $i$ 位为`1`，就表示编号为 $i$ 的灯泡最开始是打开的。

​接下来的 $n-1$ 行，每行两个整数 $x$ 和 $y(1\leq x,y\leq n)$，表示树上的一条连接 $x$ 和 $y$ 的边。

### 输出描述:

​输出最小的使得所有灯泡打开的按开关路径，保证存在解，路径的长度定义为路径上点的数目。

### 分析:

​路径是可以重复的，简单的树形DP可能难以处理，考虑路径的拼接。

​设 $f_{i,j,k}$ 表示第 $i$ 个点的子树内（除了自己）的奇偶性已经满足，且子树内（包括自己）的路径端点数有 $j$ 个，第 $i$ 个点的奇偶性为 $k$ 的最短路径长度，其中 $j\in \{0,1,2\},k\in\{0,1\}$。

​转移的时候有很多种情况，但它们都是类似的，端点个数（状态第二维）的转移有：

​1、儿子子树内均为 0 个端点 –> 自己子树 0 个端点

​2、儿子子树内均为 0 个端点 + 自己作为某一个端点 –> 自己子树内 1 个端点

​3、儿子子树内均为 0 个端点 + 自己作为两个端点 –> 自己子树内 2 个端点

​4、一个儿子子树内 1 个端点 + 其他儿子子树内均为 0 个端点 –> 自己子树内 1 个端点	

​5、一个儿子子树内 1 个端点 + 其他儿子子树内均为 0 个端点 + 自己作为某个端点 –> 自己子树内 2 个端点

​6、一个儿子子树内 2 个端点 + 其他儿子子树内均为 0 个端点 –> 自己子树内 2 个端点

​7、两个儿子子树内各 1 个端点 + 其他儿子子树内均为 0 个端点 –> 自己子树内 2 个端点

​第二维的 $j$​ 可以理解为是伸出了多少个“头”，然后每个子树相连拼接上，再用剩下的“头”继续往上转移。0 和 2 都是两个“头”， 1 是一个“头”。

​儿子之间合并的时候要注意答案所求的是路径点的个数，所以不能把每个儿子所有“2”都延长 2 的长度到父亲，不然儿子之间相接时会算重，而应该少延长一个”头“，最后更新答案时再只加多 1。

​如何保证儿子子树内的奇偶性都满足条件？如果从儿子节点尚不满足奇偶性的点转移时，需要多加上 2 的长度，表示到了父亲再往下到儿子走一个来回。

​至于第三维是转移到当前节点的 0 还是 1，需要看转移上来的偶儿子（指 $j$ 为偶数的儿子）个数的奇偶性。

​还要注意，根节点剩下的两个“头”会相连，不仅答案会减 1 ，而且对他而言奇偶性还会再多变一次。

​这样就做完了吗？

​写到后面可能会很容易忽略的是，这样写会默认每个节点都至少被经过一次，而其实并不然，所以根节点设为任意一个奇偶性条件为 1 点，同时在枚举儿子转移时，若整个子树都已经满足了就直接跳过。

### 代码:

```C++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
#define N 500010
int last[N], nxt[N * 2], to[N * 2], len = 0;
int f[N][3][2], a[N], s[N];
void add(int x, int y) {
	to[++len] = y;
	nxt[len] = last[x];
	last[x] = len;
}//加边
void dfs(int k, int fa) {
	int s0 = 0, s1 = 1e9, s2 = 1e9, s3 = 1e9, s4 = 1e9, s5 = 1e9, s6 = 1e9, s7 = 1e9; 
	int t0, t1;
	if(!a[k]) s[k]++;
	for(int i = last[k]; i; i = nxt[i]) if(to[i] != fa) {
		int x = to[i];
		dfs(x, k);
		if(!s[to[i]]) continue;
		s[k] += s[to[i]];
		t0 = min(s7 + f[x][0][1] + 1, s6 + f[x][0][0] + 3), t1 = min(s6 + f[x][0][1] + 1, s7 + f[x][0][0] + 3);
		s6 = t0, s7 = t1;
		t0 = min(s2 + f[x][1][1], s3 + f[x][1][0] + 2), t1 = min(s3 + f[x][1][1], s2 + f[x][1][0] + 2);
		s6 = min(s6, t0), s7 = min(s7, t1);
		
		t0 = min(s5 + f[x][0][1] + 1, s4 + f[x][0][0] + 3), t1 = min(s4 + f[x][0][1] + 1, s5 + f[x][0][0] + 3);
		s4 = t0, s5 = t1;
		t0 = min(s1 + f[x][2][1] + 1, s0 + f[x][2][0] + 3), t1 = min(s0 + f[x][2][1] + 1, s1 + f[x][2][0] + 3);
		s4 = min(s4, t0), s5 = min(s5, t1);
		
		t0 = min(s3 + f[x][0][1] + 1, s2 + f[x][0][0] + 3), t1 = min(s2 + f[x][0][1] + 1, s3 + f[x][0][0] + 3);
		s2 = t0, s3 = t1;
		t0 = min(s0 + f[x][1][1], s1 + f[x][1][0] + 2), t1 = min(s1 + f[x][1][1], s0 + f[x][1][0] + 2);
		s2 = min(s2, t0), s3 = min(s3, t1);
		
		t0 = min(s1 + f[x][0][1] + 1, s0 + f[x][0][0] + 3), t1 = min(s0 + f[x][0][1] + 1, s1 + f[x][0][0] + 3);
		s0 = t0, s1 = t1;
	
	}//快乐状态转移方程
	f[k][0][a[k]] = s1 + 1;
	f[k][0][a[k] ^ 1] = s0 + 1;
	f[k][1][a[k]] = min(s3, s1) + 1;
	f[k][1][a[k] ^ 1] = min(s2, s0) + 1;
	f[k][2][a[k]] = min(min(s0 + 2, s5 + 1), min(s6 + 2, s2 + 2));
	f[k][2][a[k] ^ 1] = min(min(s1 + 2, s4 + 1), min(s7 + 2, s3 + 2));//赋值
}
int main() {
	int n, i, x, y;
	scanf("%d\n", &n);
	for(i = 1; i <= n; i++) {
		a[i] = getchar() - '0';
	}
	for(i = 1; i < n; i++) {
		scanf("%d%d", &x, &y);
		add(x, y), add(y, x);
	}//输入
	for(i = 1; i <= n; i++) if(!a[i]) break;//查找第一个没有打开的灯
	dfs(i, 0);
	printf("%d\n", f[i][2][0] - 1);
	fclose(stdin);
	fclose(stdout);
	return 0;
}
```

