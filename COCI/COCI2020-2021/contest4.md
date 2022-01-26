# COCI2020-2021#4 做题记录

## T1 Pizza

### 题目描述:

​在一整天的漫长工作之后，M先生决定晚饭吃一顿披萨来犒劳一下自己。在桌上堆积如山的文件内，他找到了附近一家披萨餐厅的一张传单。 

​餐厅内提供 $m$ 种不同的披萨，每种披萨有不同的浇料，这些浇料用正整数编号来表示，第𝑖种披萨有 $k_i$ 种浇料，编号分别为 $b_{i,1},b_{i,2},...,b_{i,k_i}$。 

​M先生对于吃东西时非常挑剔的，在所有的这些浇料中，有 $n$ 种浇料是M先生完全不想接触的，他们的编号分别是 $a_1,a_2,...,a_n$。因此，他希望点的披萨是那些完全不包括这 $n$ 种浇料的披萨，你的任务是帮他找出，**有多少种披萨是M先生可以点的**。

### 输入描述:

​输入的第一行最开始是一个正整数 $n(1\leq n\leq 100)$。接下来有 $n$ 个互不相同的整数 $a_i(1\leq a_i\leq 100)$，表示M先生不喜欢的浇料的编号。

​接下来的一行包括一个正整数 $m(1\leq m\leq 100)$，表示披萨的种类数目。 

​接下来的 $m$ 行，每行描述一种披萨，最开始是一个整数 $k_i(1\leq k_i\leq 100)$，表示浇料的种类， 接下来是 $k_i$ 个互不相同的整数 $b_{i,j}(1\leq b_{i,j}\leq 100)$，表示披萨浇料的编号。

### 输出描述:

​输出M先生可以点的披萨的数目。

### 分析:

​水题一道，只需要在输入每一块披萨需要用到的浇料同时比较这种浇料是不是M先生喜欢的就行了。

​时间复杂度为 $O(nm^2)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 110
using namespace std;
van n,m,a[N],ans;
int main()
{
	cin>>n;
	for (int i=1;i<=n;i++) cin>>a[i];
	cin>>m;
	for (int i=1;i<=m;i++)
	{
		van sum;bool ok=true;
		cin>>sum;
		for (int j=1;j<=sum;j++)
		{
			van tmp;
			cin>>tmp;
			for (int k=1;k<=n;k++) if (a[k]==tmp) ok=false;
		}
		ans+=ok;
	}
	cout<<ans;
	return 0;
}
```



## T2 Vepar

### 题目描述:

​给定两个正整数区间 $\{a,a+1,..,b\}$ 以及 $\{c,c+1,...,d\}$ 。请你判断 $c(c+1)...d$ 是否被 $a(a+1)...b$ 整除。

### 输入描述:

​输入的第一行包括一个正整数 $t(1\leq t\leq 10)$，表示独立的数据的组数。 

​接下来的 $t$ 行，每行四个正整数 $a_i,b_i,c_i,d_i(1\leq a_i\leq b_i\leq 10^7,1\leq c_i\leq d_i\leq 10^7)$。

### 输出描述:

​输出 $t$ 行，对于第 $i$ 组数据，如果可以整除，请输出 `DA`，否则输出 `NE`。

### 分析:

#### 45Pts:

​只需要一个个挨着顺序去找这个连乘的结果所能拆分出来的质因数就行了。

​然后将比较每一个能拆分出来的质因数。若 $c(c+1)...d$ 的质因数 $i$ 的个数小于 $a(a+1)...b$。

​时间复杂度为 $O(tn\log_2n)$。

#### 100Pts:

​例题：阶乘分解

​$c(c+1)...d$ 等价于 $\frac{d!}{(c-1)!}$，$a(a+1)...b$ 等价于 $\frac{b!}{(a-1)!}$，对于 $c(c+1)...d$ 的质因数，只需要算出 $d!$ 的质因数然后减去 $(c-1)!$ 的质因数即可。对于 $a(a+1)...b$ 同理。

​接下来的比较与45Pts的做法相同。

​时间复杂度为 $O(tn)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 10001000
using namespace std;
van t,a,b,c,d;
bool prime[N];
van A[1000100],B[1000100],C[1000100],D[1000100];
vector<van> p;
void init()
{
	for (van i=2;i<=1e7;i++) if (!prime[i]) for (van j=i*i;j<=1e7;j+=i) prime[j]=true;
	for (van i=2;i<=1e7;i++) if (!prime[i]) p.push_back(i);
}//埃式筛法求出1-10^7里的质因数
void calc(van res[N],van n)
{
	for (int i=0;i<p.size();i++)
	{
		van base=1;
		if (p[i]>n) break;
		for (int j=1;j<=log(n)/log(p[i]);j++)
		{
			base*=p[i];
			res[i]+=n/base;
		}
	}
}//计算n!的质因数及个数
int main()
{
	init();
	cin>>t;
	while (t--)
	{
		cin>>a>>b>>c>>d;
		memset(A,0,sizeof A);
		memset(B,0,sizeof B);
		memset(C,0,sizeof C);
		memset(D,0,sizeof D);//清空质因数数组
		calc(A,a-1);calc(B,b);calc(C,c-1);calc(D,d);//计算四个阶乘的质因数
		bool ok=true;
		for (int i=0;i<p.size();i++)
			if (D[i]-C[i]<B[i]-A[i]) ok=false;//检查能否被整除
		cout<<(ok?"DA":"NE")<<endl;
	}
	return 0;
}

```



## T3 Hop

### 题目描述:

​池塘内有𝑛朵睡莲，第𝑖朵睡莲上有一个正整数 $x_i$。序列 $(x_i)_{1\leq i\leq n}$ **严格单调递增**。 

​每一组数对 $(a,b)$，$(1\leq a<b\leq n)$，属于**三只青蛙**中的一只。一只青蛙可以从睡莲 $i$ 跳到睡莲 $j(j>i)$，当且仅当数对 $(i,j)$属于它，并且**$x_i|x_j$(即$x_i$整除$x_j$)**。

 给青蛙分配数对使得没有任何青蛙可以连续跳跃超过三次。

### 输入描述:

​输入的第一行包括一个正整数 $n(1\leq n\leq 1000)$，表示睡莲的数量。

​输入的第二行包括 $n$ 个正整数 $x_i(1\leq x_i\leq 10^{18})$，表示睡莲上的数字。

### 输出描述:

​输出 $n-1$ 行，在第 $i$ 行内，输出 $i$ 个数字，每一个数字为 $\{1,2,3\}$ 中的一个，第 $i$ 行的第 $j$ 个数字 表示边 $(j,i+1)$ 的归属。

### 分析:

#### 1-3Pts:

​rand一下就行了。能得多少分全靠运气了。

#### 100Pts:

​由于 $(x_i)_{1\leq i\leq n}$ 且 $x_i|x_j$，则 $x_i<x_j$，因此 $x_j$ 最少都会是 $x_i$ 的两倍。由于每只青蛙最多只能连续跳三次，因此它连续跳三次之后到达的数最小都会是当前这个数的 $2^3$ 倍。接下来我们将整个区块按照数的highbit值分为4部分，每一部分的数的范围分别为 $[1,2^{16}-1],[2^{16},2^{32}-1],[2^{32},2^{48}-1],[2^{48},2^{64}-1]$，再将已经分好的4个部分里又分成4个小部分，这16个部分里的数又分别为 $[1,2^4-1],[2^4,2^8-1],...$，以此类推。因为 $10^{18}\approx 2^{64}$ 且 $10^{18}<2^{64}$，因此我们可以让第一只青蛙只能分别在这16个小部分里面去跳，不能跳出某一小部分到另外的部分去了，这样第一只青蛙无论怎么跳最多只能跳三次。接下来我们又让第二只青蛙只在这4个大部分里去跳，但又只能在这某一个部分里面去跳，又不能从某一个小部分又跳到这一个小部分，最后我们再将剩下的没有跳过的边给第三只青蛙。这样我们就保证了一只青蛙只能连续跳3次。整个过程的模拟如下图所示：

![image-20210819193143586](./T3-分析-100Pts-1.jpg)

​时间复杂度为 $O(n^2)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1010
using namespace std;
van n,a[N];
van belong[N][N];
van highbit(van x)
{
	van tmp=x,ans=0,now=0;
	while (x)
	{
		if(x%2) ans=now;
		now++,x/=2;
	}
	return ans;
}//计算高位highbit
int main()
{
	cin>>n;
	for (int i=1;i<=n;i++) cin>>a[i];
	for (int i=1;i<=n;i++)
		for (int j=i+1;j<=n;j++)
		{
			if (a[j]%a[i]!=0) belong[i][j]=1;
			else
			{
				if (highbit(a[i])/4==highbit(a[j])/4) belong[i][j]=1;
				else if (highbit(a[i])/16==highbit(a[j])/16) belong[i][j]=2;
				else belong[i][j]=3;
			}
		}//输入并计算点对i,j究竟应该给哪只青蛙
	for (int i=1;i<=n;i++)
	{
		for (int j=1;j<i;j++) cout<<belong[j][i]<<" ";
		cout<<endl;
	}//输出结果
	return 0;
}
```



## T4 Janjetina

### 题目描述:

​在长期的封锁终于结束之后，M先生决定开始一场盛大的旅行享受风景，以及最重要的美食。 M先生计划可能旅行的城市一共有 $n$ 座，这 $n$ 座城市由 $n-1$ 条道路连接起来，每条道路连接两个城市，并且任意两个点之间都有路径可以联通。 所有连接两个城市的路上都有若干餐厅，他们的美味程度可以用一个数字 $w_i$ 来表示。M先生计划的旅行会从一个城市 $x$ 出发，沿着最短路到另一个城市 $y$ 结束。我们假定每一条连接两个城市的长度都记为一个单位长度。旅行过程中，**M先生会在美味程度最高的一条道路上享用美食**。 对于一次经过 $l$ 个单位长度距离，享受的美食美味程度为 $w$ 的一次旅行，如果 $w-l\geq k$，M先生就会对这次旅行感到满意。你的任务是求出会令M先生感到满意的城市对的数目。

### 输入描述:

​输入的第一行包含两个正整数 $n$ 和 $k(1\leq n,k\leq 10^5)$，表示城市的数目以及满意的阈值。 

​接下来的 $n-1$ 行，每行三个正整数 $x,y,w(1\leq x,y\leq n,x\not=y,1\leq w\leq 10^5)$，这表示有 一条连接城市 $x$ 和城市 $y$ 的道路，道路上的餐厅美味程度为$w$。

### 输出描述:

​输出不同的可以令M先生感到满意的城市对。定义两个城市对 $(x_1,y_1)$，$(x_2,y_2)$ 不同，这两个数对至少满足 $x_1\not=x_2$ 以及 $y_1\not=y_2$ 中的一个。

### 分析:

#### 15Pts:

​此档数据范围 $n\leq 1000$，枚举两个点再通过LCA求出 $w$ 与 $l$ 的值与 $k$ 比较，若符合条件则 $ans$ 加上1。

​时间复杂度为 $O(n^2\log_2n)$。

#### 50Pts:

​在完成Subtask1的基础上考虑如何解决Subtask2。

​由于在Subtask2中一定会形成一条链，考虑采用线性的方法解决这个Subtask。我们先将整幅图分为两个区间，计算左区间的某一个点与右区间的某一个点符合条件的情况有多少种。再将左区间分为两个区间，在这两个区间内继续执行上面的操作……以此类推，我们就可以通过分治来解决这个Subtask。当然，在每次都平分这个区间的情况下，能最小化时间复杂度。

​时间复杂度为 $O(n\log_2n)$。

#### 100Pts:

​在完成Subtask2的基础上继续考虑如何解决此问题。

​首先，Subtask2中使用到了分治的方法解决线性的问题，那么我们可以考虑在一棵树上使用分治来解决Subtask3，即点分治。

​剩下的只需要将点分治的模板套上去就可以解决此问题了。

​点分治的参考链接：[【算法学习】点分治 - 粉兔 - 博客园 (cnblogs.com)](https://www.cnblogs.com/PinkRabbit/p/8593080.html)

​时间复杂度为 $O(nlog_2^2n)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 100010
using namespace std;
vector<pair<van,van> > g[N];
van size[N],n,k,deep[N],cnt;bool used[N];
pair<van,van> wei[N];van c[N],ans,center,mx;
void FindSize(van root,van f,van s)
{
	size[root]=1;van num=0;
	for (int i=0;i<g[root].size();i++) if (g[root][i].first!=f&&!used[g[root][i].first])
	{
		FindSize(g[root][i].first,root,s);
		size[root]+=size[g[root][i].first];
		if (num<size[g[root][i].first]) num=size[g[root][i].first];
	}
	if (num<s-size[root]) num=s-size[root];
	if (num<mx) mx=num,center=root;
}//查找以root为根节点的树的大小，并且计算这棵树的重心
void FindWeight(van root,van weight,van f)
{
	wei[++cnt]=make_pair(weight,deep[root]);
	for (int i=0;i<g[root].size();i++) if (g[root][i].first!=f&&!used[g[root][i].first])
	{
		deep[g[root][i].first]=deep[root]+1;
		FindWeight(g[root][i].first,max(weight,g[root][i].second),root);
	}
}//计算从root到其子节点的w值
van lowbit(van x){return x&-x;}
void add(van x,van num)
{
	while (x<N)//不能是n，因为w值有可能会大于n
	{
		c[x]+=num;
		x+=lowbit(x);
	}
}
van query(van x)
{
	van ans=0;
	while (x>0)
	{
		ans+=c[x];
		x-=lowbit(x);
	}
	return ans;
}//树状数组快速计算答案
van CalcAnswer(van root,van weight,van d)
{
	cnt=0;deep[root]=d;van tmp=0;
	FindWeight(root,weight,0);
	sort(wei+1,wei+cnt+1);
	for (int i=1;i<=cnt;i++)
	{
		tmp+=query(wei[i].first-k-wei[i].second+1);
		add(wei[i].second+1,1);
	}
	for (int i=1;i<=cnt;i++) add(wei[i].second+1,-1);
	return tmp;
}//计算以root为根节点的答案
void Division(van root)
{
	ans+=CalcAnswer(root,0,0);
	used[root]=1;
	for (int i=0;i<g[root].size();i++) if (!used[g[root][i].first]) ans-=CalcAnswer(g[root][i].first,g[root][i].second,1);//容斥原理减去CalcAnswer(root,0,0)加多了的
	for (int i=0;i<g[root].size();i++) if (!used[g[root][i].first])//到其子树下继续执行点分治
	{
		mx=1e18;
		FindSize(g[root][i].first,root,size[g[root][i].first]);//重新计算树的重心与树的大小
		Division(center);
	}
};
int main()
{
	cin>>n>>k;
	for (int i=1;i<n;i++)
	{
		van f,s,w;
		scanf("%lld%lld%lld",&f,&s,&w);
		g[f].push_back(make_pair(s,w));
		g[s].push_back(make_pair(f,w));//建边
	}
	mx=1e18;FindSize(1,0,n);//查找树的大小以及树的重心
	memset(used,0,sizeof used);
	Division(center);//点分治
	cout<<ans*2<<endl;//由于每两个点只计算了一次，因此最后答案需要乘以2
}
```



## T5 Patkice II

### 题目描述:

​三只小鸭，再出发！这一次，海洋会更加汹涌。 

​海洋的情况可以用一张大小为 $r\times s$ 的海图表示，海图中的每一个格子会是`<>v^ox`中的一个 字符。其中`o`表示起点,`x`表示终点。字符`<>v^`表示洋流，表示如果小鸭子们到达对应的格子， 会被洋流推向一个相邻的格子`<`表示被洋流推向西侧的格子，`>`表示被洋流推向东侧的格子，`v`表示被洋流推向南侧的格子，`^`表示被洋流推向北侧的格子。`.`则表示一片平静的海洋，到达这种格子之后，小鸭子们就会结束他们的旅途。 

​小鸭子们最开始从`o`所在的位置出发，选择东西南北四个方向中的一个，到达相邻的格子， **随后，只能随着洋流移动**。很显然，不能保证小鸭子们一定可以到达目的地，即字符`x`所在的格 子。现在，你被赋予改变自然的能力，必须想办法让鸭子们可以从`o`出发到达`x`。你可以修改海洋中的任何一个格子所在的海域(将对应格子修改为`v^<>.`中的一个，不能修改`o`，`x`所在的格子)。

​你需要求出，最少需要修改多少个格子的海域，才能够让小鸭子们从`o`出发，到达`x`。

### 输入描述:

​输入的第一行包括两个整数 $r,s(3\leq r,s\leq 2000)$，表示海图的大小。 接下来的 $r$ 行，每行一个长为 $s$ 的字符串，字符集为`o<>v^.x`，表示当前的海洋情况。保证地图中只有一个`o`字符以及`x`字符，并且二者不会相邻。

### 输出描述:

​输出的第一行包括一个正整数 $k$，表示最少需要修改的海域的数目。 

​接下来的 $r$ 行，每行一个长度为 $s$ 的字符串，描述修改之后海域的形态，描述方法与输入的形式相同。如果存在多种可能的答案，输出任意一种。

### 分析:

​我们可以通过 01BFS 来解决此问题，将当前漩涡所指向的下一个节点与当前节点所形成的边的权值设置为 0，将其他三个方向的边设置为 1，并使用一个 deque 容器来解决这个 01BFS 问题。最后在使用一个 DFS 来查找需要修改的点并将合法方案输出。

### 代码:

```C++
#include<bits/stdc++.h>
#define van int
#define N 2010
using namespace std;
struct node
{
	van x,y;
	van step;
};
char cha[N][N];
van n,m,VanGo[4][2]={{1,0},{0,1},{-1,0},{0,-1}};
bool used[N][N];van f[N][N];
deque<node> q;
van sx,sy,ex,ey;
pair<van,van> GetDirByChar(char x)
{
	if (x=='^') return make_pair(-1,0);
	if (x=='v') return make_pair(1,0);
	if (x=='<') return make_pair(0,-1);
	if (x=='>') return make_pair(0,1);
}//根据漩涡的方向来推断要修改的坐标
char GetCharByDir(van x,van y)
{
	if (x==1&&y==0) return 'v';
	if (x==-1&&y==0) return '^';
	if (x==0&&y==1) return '>';
	if (x==0&&y==-1) return '<';
}//根据修改的坐标来推断漩涡的方向
bool in(van x,van y)
{
	return x>0&&x<=n&&y>0&&y<=m;
}//判断(x,y)是否在图内
void DFS(van x,van y)
{
	used[x][y]=1;
	if (x==ex&&y==ey)
	{
		for(int i=1;i<=n;i++)
		{
			for (int j=1;j<=m;j++) cout<<cha[i][j];
			cout<<endl;
		}
		exit(0);
	}//查找到了合法的图
	char tmp=cha[x][y];
	for (int i=0;i<4;i++)
	{
		van xx=x+VanGo[i][0],yy=y+VanGo[i][1];
		if (!in(xx,yy)||used[xx][yy]) continue;
		van w=GetCharByDir(VanGo[i][0],VanGo[i][1])==tmp||tmp=='o'||tmp=='x'?0:1;//计算边的权值
		if (f[xx][yy]==f[x][y]+w)//(xx,yy)有可能是(x,y)的下一个点
		{
			if (cha[x][y]!='o') cha[x][y]=GetCharByDir(VanGo[i][0],VanGo[i][1]);
			DFS(xx,yy);
		}
	}
	cha[x][y]=tmp;//回溯
}
int main()
{
	srand(time(0));
	cin>>n>>m;
	for (int i=1;i<=n;i++)
		for (int j=1;j<=m;j++) cin>>cha[i][j];//输入地图
	for (int i=1;i<=n;i++) for (int j=1;j<=m;j++)
	{
		if (cha[i][j]=='o') sx=i,sy=j;
		if (cha[i][j]=='x') ex=i,ey=j;
	}//判断起点与终点
	q.push_back((node){sx,sy,0});
	while (!q.empty())
	{
		node now=q.front();q.pop_front();
		if (used[now.x][now.y]) continue;
		used[now.x][now.y]=1;f[now.x][now.y]=now.step;
		if (now.x==ex&&now.y==ey) break;
		char tmp=cha[now.x][now.y];
		for (int i=0;i<4;i++)
		{
			node new_elem=now;
			new_elem.x+=VanGo[i][0],new_elem.y+=VanGo[i][1];
			if (!in(new_elem.x,new_elem.y)) continue;
			new_elem.step+=GetCharByDir(VanGo[i][0],VanGo[i][1])==tmp||tmp=='o'||tmp=='x'?0:1;
			if (new_elem.step-now.step==1) q.push_back(new_elem);
			else q.push_front(new_elem);
		}
	}//01BFS解决边权为0或1的最短路问题
	cout<<f[ex][ey]<<endl;//输出(ex,ey)的最小权值即修改次数
	memset(used,0,sizeof used);
	DFS(sx,sy);//DFS查找修改后的图
	return 0;
}
```

