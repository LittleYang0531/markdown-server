# COCI2020-2021#5 做题记录

## T1 Sifra

### 题目描述:

​骑士 $Borna$ 正在试图破译一封来自他的敌人的秘密信件。这封信件由一个由小写字母和数字符号组成， $Borna$ 需要破译出来的密码就是字符串内不同的数字个数。(连续的数字字符看成一个数字，不可拆开) 

​输入保证字符串中的每一个数字都没有前导零。你的任务就是求出不同的数字的个数。

### 输入描述:

​第一行包含一个字符串，长度在1到100之间，字符串由小写字母和数字字符组成。输入保 证所有的数字最多只由三个数字字符组成。

### 输出描述:

​输出字符串内不同的数字数量。

### 分析:

​水题一道，从左往右扫一遍字符串，当扫到了某一个数字的时候，在循环内部继续扫下去，直到扫不到数字了为止，并将外层循环的 $i$ 赋值为当前的位置，至于计算这个数字是多少，只需要按照快读的思路计算就行了。

​时间复杂度为 $O(s.size())$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1010
#define ywhin cin
#define ywhout cout
using namespace std;
bool used[N];van an;
int main()
{
//	ifstream ywhin("sifra.in");
//	ofstream ywhout("sifra.out");
	string x;
	ywhin>>x;
	for (int i=0;i<x.size();i++)
	{
		van ans=0;
		while (x[i]>='0'&&x[i]<='9') ans*=10,ans+=x[i]-'0',i++;
		if (ans!=0) used[ans]=1,i--;
	}
	for (int i=0;i<=1000;i++) if (used[i]) an++;
	ywhout<<ans<<endl;
	return 0;
}
```



## T2 Po

### 题目描述:

​你会获得一个长度为 $n$ 的序列 $a_i$，这个序列是由一个初始序列经过若干变换得到的。初始序列是一个长度为 $n$ 的全0序列。可能出现的变换规则如下：

​单个变换的效果为选择一个区间 [$l$, $r$]，将 $a_l,a_{l+1},...,a_r$ 的值加上一个正整数 $x$。任意两次变换的区间只能**互不相交或者一个区间包含另一个区间**。 

​你的问题是，对于给定的序列 $a_i$，这个序列至少是进行了多少次变换得到的。

### 输入描述:

​输入的第一行是一个整数 $n(1\leq n\leq 10^5)$，表示序列的长度。

​输入的第二行包含𝑛个非负整数 $a_i(0\leq a_i\leq 10^9)$。表示经过若干次变换得到的序列。

### 输出描述:

​输出一个整数$m$，表示要得到这个区间的最少的变换次数。

### 分析:

​分析题意，首先这个题加上的这个正整数 $x$ 只能是个正整数，这意味着我们在每次变换时只能变换到当前区间最小的一个值，如果变换到的不是最小的一个值，那么就再也变换不到当前区间的最小值了。

​考虑通过分治来解决此问题。每次变换时查询当前区间的最小值，并根据这个最小值在当前区间所出现的位置来将当前区间拆分为若干个子区间继续进行分治，并且将 $ans$ 的值增加1，直到 $l<r$ 就可以退出该递归函数了。

​注意，当区间的最小值为0时， $ans$ 的值不需要增加1，因为此时不需要进行一次变换。

​时间复杂度为 $O(n\log_2^2n)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 100010
#define ywhin cin
#define ywhout cout
using namespace std;
van n,a[N],tmp[N];
van tree[N*4],ans;
vector<van> place[N];
void build(van p,van l,van r)
{
	if (l==r){tree[p]=a[l];return;}
	van mid=l+r>>1;
	build(p*2,l,mid);build(p*2+1,mid+1,r);
	tree[p]=min(tree[p*2],tree[p*2+1]);
}//构建线段树
van query(van p,van l,van r,van L,van R)
{
	if (L<=l&&r<=R) return tree[p];
	van mid=l+r>>1;
	van x=1e18;
	if (L<=mid) x=min(x,query(p*2,l,mid,L,R));
	if (R>mid) x=min(x,query(p*2+1,mid+1,r,L,R));
	return x;
}//在线段树内查询最小值
void solve(van l,van r)
{
	if (l>r) return;
	van min_=query(1,1,n,l,r);//获取[l,r]的最小值
	if (tmp[min_]!=0) ans++;//注意:当最小值为0时是不需要ans++的
	van ll=0,rr=place[min_].size()-1,lll;
	while (ll<=rr)
	{
		van mid=ll+rr>>1;
		if (place[min_][mid]>=l) lll=mid,rr=mid-1;
		else ll=mid+1;
	}//二分查找最小值所在的位置id最小的位置
	ll=0,rr=place[min_].size()-1;van rrr;
	while (ll<=rr)
	{
		van mid=ll+rr>>1;
		if (place[min_][mid]<=r) rrr=mid,ll=mid+1;
		else rr=mid-1;
	}//二分查找最小值所在的位置id最大的位置
	solve(l,place[min_][lll]-1);//解决[l,a_l-1]的区间
	for (int i=lll;i<rrr;i++) solve(place[min_][i]+1,place[min_][i+1]-1);//解决[a_i+1,a_{i+1}-1]所在的区间
	solve(place[min_][rrr]+1,r);//解决[a_r+1,r]的区间
}//分治解决问题
int main()
{
//	ifstream ywhin("po.in");
//	ofstream ywhout("po.out");
	ywhin>>n;
	for (int i=1;i<=n;i++) ywhin>>a[i];
	memcpy(tmp,a,sizeof tmp);
	sort(tmp+1,tmp+n+1);
	van cnt=unique(tmp+1,tmp+n+1)-tmp-1;
	for (int i=1;i<=n;i++) a[i]=lower_bound(tmp+1,tmp+cnt+1,a[i])-tmp;//离散化
	for (int i=1;i<=n;i++) place[a[i]].push_back(i);//记录某一个离散化后的数会出现在哪些位置
	build(1,1,n);
	solve(1,n);
	ywhout<<ans<<endl;
	return 0;
}
```



## T3 Megenta

### 题目描述:

​小P和小M在玩一个游戏，游戏的内容是这样的。 

​有一棵 $n$ 个节点的树，树上的 $𝑛−1$ 条边由三种标记，分别为 $𝑚𝑎𝑔𝑒𝑛𝑡𝑎,𝑝𝑙𝑎𝑣𝑎,c𝑟𝑣𝑒𝑛𝑎$ 。游戏开始的时候，小P的棋子位于节点 $a$ ，小M的棋子位于节点 $b$ 。游戏开始后，小P先行动，他需要选择一条边，从节点 $a$ 沿着边走到另一端，要求小M的棋子另一端并且边的标记为 $𝑝𝑙𝑎𝑣𝑎$ 或者 $𝑚𝑎𝑔𝑒𝑛𝑡𝑎$ 。随后由小M操作，同样沿着一条边走，要求小P的棋子不在另一端，边的标记为 $𝑐𝑟𝑣𝑒𝑛𝑎$ 或 $𝑚𝑎𝑔𝑒𝑛𝑡𝑎$ 。二人轮流操作，首先无法操作的人失败。 

​现在，我们假设二人都是绝对理性和聪明的，请问谁会获得胜利。

### 输入描述:

​输入的第一行包括一个正整数 $n(1\leq n\leq 10^5)$ ，表示树上的节点的数目。

​输入的第二行包括两个正整数 $a,b(1\leq a,b\leq n,a\not=b)$ ，表示最开始小P和小M的棋子所在的 位置。 

​接下来的 $n-1$ 行，每行的格式如下" $x\ y\ signal$ "。表示边 $<x,y>$ 上的标记为 $signal$。$signal\in\{magenta,crvena,plava\}$。

### 输出描述:

​如果小P会获胜，输出" $Paula$ ",如果小M获胜，输出" $Marin$ "。如果平局，输出" $Magenta$ "。

### 分析:

#### 30Pts:

​由于每条路对于每个人都是可以走的，因此只需要判断两个人之间的距离就行了。

#### 100Pts:

​令当轮到自己行动前与对方的距离为偶数的一方killer，另一方为loser，则killer只会赢或平局，毕竟其走不到对方的棋子上(当killer根本无法行走时，要注意判断)。

​搜索killer能走到的路径，并在这条路径上的每一条父节点扩展他还能到达的子节点标记为1，再搜索loser能否到达这些标记为1的点，即loser能否成功逃脱，若loser能成功逃脱，则为平局，否则loser输。

​分类讨论:

​1.若先手无处可走，后手赢。

​2.若后手无处可走，先手赢。

​3.若loser能够成功逃脱，平局。

​4.否则killer赢。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 100010
#define pb push_back
#define mp make_pair
using namespace std;
van n,a,b,fa[N],dis[N],len;
vector<pair<van,van> > graph[N];
bool used1[N],used2[N];
van cnt1,cnt2,killer,loser;
void PutError(string info)
{
	cout<<info;
	exit(0);
}//错误输出
void A(){printf("Paula");exit(0);}//先手赢
void B(){printf("Marin");exit(0);}//后手赢
void C(){printf("Magenta");exit(0);}//平局
void DFS1(van u,van f)
{
	fa[u]=f;
	for (int i=0;i<graph[u].size();i++) if (graph[u][i].first!=f) DFS1(graph[u][i].first,u);
}//查找每个子节点的父亲节点
void DFS2(van u,van f)
{
	cnt1++;
	for (int i=0;i<graph[u].size();i++) if (graph[u][i].first!=f&&graph[u][i].first!=b&&graph[u][i].second!=a) DFS2(graph[u][i].first,u);
}//查找a能走的点
void DFS3(van u,van f)
{
	cnt2++;
	for (int i=0;i<graph[u].size();i++) if (graph[u][i].first!=f&&graph[u][i].second!=b) DFS3(graph[u][i].first,u);
}//查找b能走的点
void DFS4(van u,van f,van step)
{
	dis[u]=step;cnt1++;used1[u]=1;
	for (int i=0;i<graph[u].size();i++) if (graph[u][i].first!=f&&graph[u][i].second!=killer) DFS4(graph[u][i].first,u,step+1);
}//搜索killer能走到的点
void DFS5(van u,van f,van step)
{
	if (used2[u]) C();
	for (int i=0;i<graph[u].size();i++) if (graph[u][i].first!=f&&graph[u][i].second!=loser&&step+1-(loser==a)<dis[graph[u][i].first]) DFS5(graph[u][i].first,u,step+1);
}//搜索loser能能否逃脱
int main()
{
	scanf("%lld\n%lld%lld",&n,&a,&b);
	for (int i=1;i<n;i++)
	{
		van f,s;string w;
		scanf("%lld%lld",&f,&s);cin>>w;
		van weight=112700;
		if (w[0]=='m') weight=0;
		else if (w[0]=='p') weight=b;
		else if (w[0]=='c') weight=a;
		else PutError("Input Error!");
		graph[f].pb(mp(s,weight));
		graph[s].pb(mp(f,weight));
	}//加边
	DFS1(1,0);
	for (int i=a;i;i=fa[i])
	{
		used1[i]=1;
		dis[fa[i]]=dis[i]+1;
	}
	if (used1[b]) len=dis[b];
	else for (int i=b;i;i=fa[i])
	{
		if (used1[fa[i]])
		{
			len=dis[i]+dis[fa[i]]+1;
			break;
		}
		dis[fa[i]]=dis[i]+1;
	}
	killer=len%2?b:a,loser=len%2?a:b;//查找killer与loser
	for (int i=1;i<=n;i++) used1[i]=0,dis[i]=1e18;
	DFS2(a,0);if (cnt1==1) B();
	DFS3(b,0);if (cnt2==1) A();
	DFS4(killer,0,0);
	for (int i=1;i<=n;i++) if (!used1[i])
	{
		for (int j=0;j<graph[i].size();j++) if (!used1[graph[i][j].first]&&graph[i][j].second!=loser)
		{
			used2[i]=1;
			break;
		}
	}
	DFS5(loser,0,0);
	if (killer==a) A();else B();//loser无法逃脱
	return 0;
}
```



## T4 Planine

### 题目描述 :

​勇者小𝑍要跨越精灵山脉寻找遗落的精灵秘宝。精灵山脉可以用一个长为奇数的坐标序列表示，如果我们将这些点的坐标按照 $x$ 坐标排序，除去第一个点和最后一个点之外的奇数编号的点的 $y$ 坐标表示**山谷**的高度，偶数编号的点表示**山顶**的坐标。显然，山谷的高度一定比旁边的两个山顶的高度低。 

​勇者小𝑍惧怕黑暗，好在精灵山脉内有着可以照明的精灵，这些精灵固定在高度为$h$的地方， 一个精灵可以照亮一座山谷，当且仅当**该精灵与这座山谷的连线不会与山脉相交**。

​你的任务是求出，最少需要多少只精灵的帮助才能够照亮所有山谷，让小𝑍穿越精灵山脉。

### 输入描述:

​输入的第一行包括两个正整数 $n(3\leq n\leq 10^6)$，$n$ 是一个奇数和正整数 $h(1\leq h\leq 106)$ 分别描述精灵山脉以及照明精灵的高度。 

​接下来的𝑛行，第𝑖行包括两个整数 $x_i$ 和 $y_i(-10^6\leq x_i\leq 10^6,0\leq y_i<h)$，表示描述山脉的第 𝑖个点的坐标。 

​输入保证 $x_1<x_2<...<x_n$，并且 $y_1<y_2,y_2>y_3,y_3<y_4...y_{n-1}>y_n$。

### 输出描述:

​输出最少的，照亮所有山谷所需要的照明精灵的数目。

### 分析:

#### 20Pts:

​由于这一档的山顶的高度都相同，因此我们只需要通过映射一只精灵能够照亮某一个山谷所需要放置的范围，然后用一个贪心就可以通过此题。

​贪心策略:

​设第$i$个山谷映射后的范围为$[l_i,r_i]$，已经安放的最后一只精灵的坐标为$(pos,h)$，$pos=-\infty$ ，按照每个范围的左端点 $l_i$ 从小到大排序。

​依次考虑每个区间，若 $l_i>pos$，则在 $r_i$ 的位置上新增一只精灵。否则就将最后的那一只精灵从 $pos$ 放置到 $\min(pos,r_i)$。

#### 50Pts:

​此档数据量 $n\leqslant 2000$，在 $n^2$ 的时间复杂度能够通过此题，因此在执行贪心操作之前使用 $n^2$ 的时间复杂度找到不会被挡住的山顶的编号以及映射到的位置即可。

#### 100Pts:

​在某相邻两个山顶中间连一条线(如下图所示)，在这条线所在的直线上方的所有山谷与上方的山顶的连线都会比与下方山顶的连线更优，因此其右上方的山谷会将下方山顶给踢掉，最后形成的会是一个上凸壳，从左往右以及从右往左各扫一遍之后就能找到对于每个山谷所映射的位置，然后根据贪心策略解决此问题即可。

![image-20210817203120771](./T4-分析-100Pts-1.jpg)

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1000100
#define ywhin cin
#define ywhout cout
using namespace std;
struct node
{
	van x,y;
}pts[N];//存储输入的点
struct node2
{
	double l,r;
}area[N];//存储山谷所映射到h的区间
van p[N],l=1,r=0;
van L[N],R[N];
van n,h,cnt,ans;
double calc(node a,node b)
{
	double dertax=b.x-a.x,dertay=b.y-a.y;
	return dertay/dertax;
}//计算斜率
bool cmp(node2 a,node2 b)
{
	if (a.l==b.l) return a.r<b.r;
	return a.l<b.l;
}//比较函数
int main()
{
 	scanf("%lld %lld",&n,&h);
	for (int i=1;i<=n;i++) scanf("%lld %lld",&pts[i].x,&pts[i].y);
	for (int i=2;i<=n-1;i++)//正着扫一遍
	{
		if (!(i%2))
		{
			while (l<r&&calc(pts[p[r-1]],pts[p[r]])<=calc(pts[p[r]],pts[i])) r--;//注意:这里不加等于号会TLE 
			p[++r]=i;
			while (r==l+1&&pts[p[l]].y<pts[p[r]].y) l++;
		}//山顶踢山顶(维护凸壳)
		else
		{
			double min_k=1e18,now=-1;
			for (int j=l;j<=r;j++)
			    if (now==-1||calc(pts[p[j]],pts[i])<min_k) min_k=calc(pts[p[j]],pts[i]),now=p[j];
			L[i]=now;
		}//查找对于当前山谷来说最高的左边的山顶的编号
	}
	l=1,r=0;
	for (int i=n-1;i>=2;i--)//反着扫一遍
	{
		if (!(i%2))
		{
			while (l<r&&calc(pts[p[r]],pts[p[r-1]])>=calc(pts[i],pts[p[r]])) r--;//注意:这里不加等于号会TLE
			p[++r]=i;
			while (r==l+1&&pts[p[r]].y<pts[p[l]].y) l++;
		}//山顶踢山顶(维护凸壳)
		else
		{
			double min_k=1e18,now=-1;
			for (int j=l;j<=r;j++)
			    if (now==-1||calc(pts[p[j]],pts[i])>min_k) min_k=calc(pts[p[j]],pts[i]),now=p[j];
			R[i]=now;
		}//查找对于当前山谷来说最高的右边的山顶的编号
	}
	for (int i=3;i<=n-2;i+=2)
	{
		area[++cnt].l=(double)pts[i].x-(double)(h-pts[i].y)/(pts[L[i]].y-(double)pts[i].y)*(double)(pts[i].x-pts[L[i]].x);
		area[cnt].r=(double)pts[i].x+(double)(h-pts[i].y)/(pts[R[i]].y-(double)pts[i].y)*(double)(pts[R[i]].x-pts[i].x);
	}//计算当前山谷所映射到h的区间
	double pos=-1e9;
	sort(area+1,area+cnt+1,cmp);
	for (int i=1;i<=cnt;i++)
	{
		if (area[i].l>pos) ans++,pos=area[i].r;
		else pos=min(pos,area[i].r);
	}//贪心计算最小值
	cout<<ans<<endl;
	return 0;
}
```



## T5 Sjeckanje

### 题目描述:

​给定一个长度为 $n$ 的序列 $a_i$。我们定义一个序列的划分是将一个序列划分为若干个区间，这 些区间依次连接起来就可以还原整个序列。例如：区间[1 3]\[1 4][3 6]就是序列1 3 1 4 3 6的一个 合法划分。我们定义一个区间的权值为**区间内的最大值减去区间内的最小值。定义在这个区间划分下，序列的权值为各个区间的权值的和。**

​例如：在区间划分[1 3]\[1 4][3 6]的情况下，序列的权值为 $(3-1)+(4-1)+(6-3)=9$。 

​你的任务是：求出序列 $a_i$ 权值最大的区间划分所对应的序列权值。 

​需要支持 $q$ 次修改操作，修改操作形式如下:将下标区间[ $l,r$ ]中的 $a_i$ 加上 $x$ 。你需要在每一次修改操作之后，输出新的序列的权值最大的区间划分所对应的权值。

### 输入描述:

​输入的第一行包括两个整数 $n$ 和 $q(1\leq n,q\leq 2\times10^5)$，分别表示序列的长度已经更新操作的次数。 第二行包含 $n$ 个整数 $a_i(-10^8\leq a_i\leq 10^8)$，表示序列的初值 $a_i$。 接下来的 $q$ 行，每行包括三个整数 $l,r(1\leq l\leq r\leq n)$ 和 $x(-10^8\leq x\leq 10^8)$，描述一个修改操作。

### 输出描述:

​输出 $q$ 行，每行一个整数，表示每一次更新操作之后序列最大的可能的区间划分的权值。

### 分析:

#### 30pts:

​令 $dp_i$ 为对于前 $i$ 个数据的区间能取到的最大权值，则 $dp_{i+j}=dp_i+a_{max}-a_{min}$。

​而对于区间修改，for循环一个一个地加就行了，时间复杂度就只有 $O(qn)$。

​总时间复杂度 $O(qn^2)$。

#### 60pts:	

​最短路优化，将非单调区间拆分成若干个单调区间，使得最终答案一定不劣。

​令 $d_i$ 为 $a_i$ 与 $a_{i-1}$ 的差值，则 $d_i=a_i-a_{i-1}\ \&\ d_1=0$。

​若对于区间 $[l,r]$，使得 $d_{l+1},d_{l+2},...,d_r$ 的符号一定是一样的，则当前区间$[l,r]$所能提供的对答案的贡献为 $\sum_{l+1}^r|d_i|$。

​时间复杂度为 $O(qn)$。

#### 100pts:

​引入线段树进行维护，记录线段树节点 $T[x][0/1][0/1]$，表示线段树节点为$x$所代表的区间能取到的区间最值。

​后面的两个0/1分别表示左端点的 $d_i$ 取还是不取，以及右端点 $d_i$ 取还是不取。

​修改时只需要修改所需要修改的区间的左端点以及右端点然后更新到线段树上即可。

​时间复杂度为 $O(qlog_2n)。$

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 1000100
using namespace std;
van n,a[N],q,d[N];
van t[N*4][2][2];//线段树
void update(van p,van num)
{
	for (int i=0;i<2;i++) for (int j=0;j<2;j++)
	{
		t[p][i][j]=-1e18;
		for (int k=0;k<2;k++) for (int l=0;l<2;l++) if (l!=1||k!=1||d[num]*d[num+1]>=0) t[p][i][j]=max(t[p][i][j],t[p*2][i][k]+t[p*2+1][l][j]);
	}
}//更新线段树p节点上的值
void add(van p,van l,van r,van where)
{
	if (l==r)
	{
//		cout<<"add "<<l<<" "<<d[l]<<endl;
		t[p][1][1]=abs(d[l]);
		return;
	}
	van mid=l+r>>1;
	if (where<=mid) add(p*2,l,mid,where);
	if (where>mid) add(p*2+1,mid+1,r,where);
	update(p,mid);
}//更新线段树(单点修改)
van query(van x)
{
	return max(t[x][0][0],max(t[x][0][1],max(t[x][1][0],t[x][1][1])));
}//查询线段树上x节点的最大值
int main()
{
//	ifstream ywhin("sjeckanje.in");
//	ofstream ywhout("sjeckanje.out");
	#define ywhin cin
	#define ywhout cout
	ywhin>>n>>q;
	for (int i=1;i<=n;i++) ywhin>>a[i];
	for (int i=1;i<=n;i++) d[i]=i==1?0:a[i]-a[i-1],add(1,1,n,i);//差分并插入线段树上
	while (q--)
	{
		van l,r,k;
		ywhin>>l>>r>>k;
		if (l!=1)
		{
			d[l]+=k;
			add(1,1,n,l);
		}//添加左端点
		if (r!=n)
		{
			d[r+1]-=k;
			add(1,1,n,r+1);
		}//添加右端点
		cout<<query(1)<<endl;//查询[1,n]的区间最值
	}
	return 0;
}
```
