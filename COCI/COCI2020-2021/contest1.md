[pixiv: 92771524]: # 'https://cdn.jsdelivr.net/gh/LittleYang0531/image/blog/cover/92771524_p0_master1200.jpg'

# COCI2020-2021#1 做题记录

## T1 Patkice

### 题目描述:

​不是很久之前，在一个遥远的大陆上，住着三只橡皮鸭。在一个炎热的夏日，躺在沙滩上的鸭子们决定旅行到一座相邻的小岛(用一把老旧的黑色雨伞)。

​鸭子们是经验丰富的海洋探险家，在旅行之前，他们会检查当前海洋的海图。在海图上，鸭子们目前居住的岛屿被字符 `o` 标记，鸭子们可以朝东(E)南(S)西(W)北(N)中的任意方向开始他们的旅途。

​海洋中的洋流会像四个方向移动，在海图上，向东的洋流被标记为 `>`，向西的洋流被标记为 `<`，向北的洋流被标记为 `^`，向南的洋流被标记为 `v`。当鸭子们位于洋流中的某一个位置时，他们会移动到洋流所指向的下一个位置。这片海洋中的洋流比较特殊，它不会让鸭子们移动到海图之外的地方，同时，洋流也没有形成环路。

​平静的海洋在海图中被标记为 `.`。如果洋流带鸭子们到达了平静海域，或者时开始的岛屿，那么他们就无法继续他们的旅途了。他们想要到达的岛在图上被标记为 `x`。

​你的任务是帮鸭子们判断它们能否到达目的地。如果可以的话，他们需要选择哪一个方向。

​鉴于鸭子们并不喜欢海上漂泊的感觉，如果存在多个可以到达目的地的方向，你需要找到行动距离最短的方向。如果存在行动距离相同的方向，请按照方向的字典序输出字典序最小的方向。

### 输入描述:

​第一行包括两个整数 $r$ 和 $s(3\leq r,s\leq 100)$，表示海图的行数和列数。

​接下来的 $r$ 行，每一行包含 $s$ 个字符，字符集为 `o<>v^.x`，代表海洋的情况。图中保证只存在唯一的 `o` 和 `x`。保证字符 `o` 不会出现在第一行，第一列，最后一行，最后一列。

### 输出描述:

​如果鸭子们无法到达目的小岛，输出 `:(`。

​否则，请在第一行输出 `:)`。在第二行，输出它们出发的方向(N表示向北，E表示向东，W表示向西，S表示向南)。

### 分析:

​加强版:[Patkice II](../COCI2020-2021#4/main.md)。

​水题一道，只不过细节有一点小多。首先，现将小鸭子们移动到上下左右四个格子，然后再暴力模拟下鸭子最终会移动到哪个点，并判断其是否为终点。注意：三只小鸭可能会走回起点，如果忽略了起点可能会死循环然后超时。

​时间复杂度为 $O(n^2)$。

### 代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define N 1010
#define ywhin cin
#define ywhout cout
using namespace std;
van n,m;
char cha[N][N];
van len;char ans;
van DFS(van x,van y)
{
	switch(cha[x][y])
	{
		case '.':return -1e18;break;
		case 'o':return -1e18;break;//注意判断是否会回到起点
		case 'x':return 0;break;
		case '>':return DFS(x,y+1)+1;break;
		case '<':return DFS(x,y-1)+1;break;
		case 'v':return DFS(x+1,y)+1;break;
		case '^':return DFS(x-1,y)+1;break;
	}
}
int main()
{
//	ifstream ywhin("patkice.in");
//	ofstream ywhout("patkice.out");
	ywhin>>n>>m;ans='z',len=1e18;
	for (int i=1;i<=n;i++) for (int j=1;j<=m;j++) ywhin>>cha[i][j];
	van sx,sy;
	for (int i=1;i<=n;i++) for (int j=1;j<=m;j++) if (cha[i][j]=='o') sx=i,sy=j;
	van res=DFS(sx,sy+1);
	if (res>=0&&res<=len) ans=(len==res)?min(ans,'E'):'E',len=res;
	res=DFS(sx-1,sy);
	if (res>=0&&res<=len) ans=(len==res)?min(ans,'N'):'N',len=res;
	res=DFS(sx+1,sy);
	if (res>=0&&res<=len) ans=(len==res)?min(ans,'S'):'S',len=res;
	res=DFS(sx,sy-1);//注意移动方向不要写错了
	if (res>=0&&res<=len) ans=(len==res)?min(ans,'W'):'W',len=res;
	if (ans!='z') 
	{
		ywhout<<":)"<<endl;
		ywhout<<ans;
		return 0;
	}
	ywhout<<":(";
	return 0;
}
```



## T2 Bajka

### 题目描述:

​小 $𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 已经对图画书感到厌倦了，于是他决定开始阅读他的第一本童话书。不幸的是，$𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 经常会遇见一些让他感到害怕的词语。为了战胜恐惧，他会玩一个他自己发明的游戏。

​令我们的小主人公感到恐惧的词语是一个长度为 $𝑛$ 的小写字母序列。开始这个游戏时，$𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 会把他的手指放在这个字符序列的一个位置上，随后把这个位置上的字符写到一张纸上。接下来，他会执行下面的两个操作中的某一个，他会进行任意次这样的操作执行。

​1.将手指移动到现在指向的字符的左边一个字符或者右边一个字符(如果存在的话)。同时，$𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 也会把这个新的字符写在原来的最后一个字符后面。

​2.将手指移动到任意一个与现在指向字符相同的字符处。这一行动不会导致任何字母书写。

​移动手指从位置 $𝑥$ 到位置 $𝑦$ 需要花费的时间为 $|𝑥−𝑦|$。

​如果在游戏的最后，$𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 最喜欢的词语会出现在纸上，那么他就会克服他的恐惧。他想要尽快看完这本童话书，因此他希望你可以告诉他，最短需要多少时间，他才能够克服恐惧。

### 输入描述:

​输入的第一行包括正整数 $𝑛$ 和 $𝑚(1\leq 𝑛,𝑚\leq 3000)$（原题 $1\leq n,m\leq 300$，在此处只能获得70Pts）。

​输入的第二行包括 $𝑛$ 个小写字母，表示 $𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 害怕的单词。

​输入的第三行包括 $𝑚$ 个小写字母，表示 $𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 最喜欢的单词。

### 输出描述:

​输出 $𝐹𝑎𝑏𝑖𝑗𝑖𝑎𝑛$ 生成最喜欢的单词需要的最短时间，如果无法生成这个单词，那么输出 `−1`。

### 分析:

#### 30Pts:

​硬暴力。直接挨个去搜索就行了。

​时间复杂度为 $O(m)$。

#### 70Pts:

​设 $dp[i][j]$ 为已经找到 $i$ 个字符且当前所在的位置为 $j$ 所需要的最小步数。

​预处理某一个字母究竟在哪些地方出现过，然后再 DP 的过程中可以快速查找出下一步究竟可以到达哪一些位置，并且加上二者的位置之差就可以转移到下一个位置了。

​由于字符串有可能全部都是一样的，因此可能会导致单个状态转移的时间复杂度为 $O(n)$，因此，

​总时间复杂度为 $O(n^3)$，过 COCI 数据是没有问题了。

#### 100Pts:

​考虑如何降低时间复杂度。

​我们可以每次记录到某一个状态时每个符合条件的点的最小步数，然后从左扫一遍，记录到某一个点时的最小步数 $step$，并将符合条件的下一个点的步数赋值为 $step$，再从右向左扫一遍，与从左往右扫一遍获得的答案比较，保留小的，再去寻找下一个状态。

​时间复杂度为 $O(n^2)$

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 3010
#define ywhin cin
#define ywhout cout
using namespace std;
van f[N];
vector<van> where['z'+1];
char hate[N],like[N];
int main()
{
	memset(f,(1<<8)-1,sizeof f);
	van n,m;
	ywhin>>n>>m;
	for (int i=1;i<=n;i++) ywhin>>hate[i];
	for (int i=1;i<=m;i++) ywhin>>like[i];
	queue<pair<van,van> > q;
	for (int i=1;i<=n;i++) if (hate[i]==like[1]) q.push(make_pair(i,0));
	for (int i=2;i<=m;i++)
	{
		memset(f,(1<<6)-1,sizeof f);
		while (!q.empty())
		{
			pair<van,van> p=q.front();q.pop();
			van place=p.first,step=p.second;
			if (hate[place-1]==like[i]&&place>1) f[place-1]=min(f[place-1],step+1);
			if (hate[place+1]==like[i]&&place<n) f[place+1]=min(f[place+1],step+1);
		}//将上一轮的结果复制到f数组里
		van minans=1e18,pos=0;
		for (int j=1;j<=n;j++) if (hate[j]==like[i])
		{
			van dis=minans+j-pos;
			if (dis>f[j]) minans=f[j],pos=j;
			else if (dis<f[j]) f[j]=dis;
		}//从左往右扫一遍
		minans=1e18,pos=n+1;
		for (int j=n;j>=1;j--) if (hate[j]==like[i])
		{
			van dis=minans+pos-j;
			if (dis>f[j]) minans=f[j],pos=j;
			else if (dis<f[j]) f[j]=dis;
			if (f[j]<1e18) q.push(make_pair(j,f[j]));
		}//从右往左扫一遍
		if (q.empty())
		{
			ywhout<<-1<<endl;
			return 0;
		}//如果没有符合条件的点就可以输出-1了
	}
	van ans=1e18;
	while (!q.empty())
	{
		pair<van,van> p=q.front();q.pop();
		ans=min(ans,p.second);
	}
	ywhout<<ans;
	return 0;
}
```



## T3 Histogram

### 题目描述:

​有一个长度为 $N$ 的3D直方图。第 $i$ 个棱柱的宽度为 1 米，高度为 $a_i$，长度为 $b_i$。换句话说，其主视图(从正面看)是一个长方形序列，第 $i$ 个长方形高为 $a_i$，宽为 1。其俯视图(从上方看)是一个长方形序列，第 $i$ 个长方形高为 $b_i$，宽为 1。

​你的任务是找出这个直方图内可以放下的容积最大的棱柱。要求棱柱的棱与直方图的棱平行。

### 输入描述:

​输入的第一行包括一个正整数 $N(1\leq N\leq 2\times10^5)$。表示这个直方图的长度。 

​接下来的 $n$ 行，第 $i$ 行包括两个正整数 $a_i$ 和 $b_i(1\leq a_i,b_i\leq 10^6)$，含义见题面描述。

### 输出描述:

​输入最大内接棱柱的容积。

### 分析:

#### 20Pts:

​爆肝一遍就完事了。枚举区间的 $l$ 节点与区间长度，然后计算答案并且与  $ans$ 比较即可。

​时间复杂度为 $O(n^2)$ 。

#### 100Pts:

​考虑使用分治解决此题。

​将区间 $[l,r]$ 分为两部分。不难发现，$a$ 的最小值与 $b$ 的最小值的分布只会有两种情况。即 $a$ 的最小值在左边/右边与 $b$ 的最小值在左边/右边。

​分开讨论，当 $a,b$ 的最小值在同一边时，枚举 $a,b$ 的最小值并在另一边查找能走到的最远的地方就行了。

​当 $a,b$ 的最小值不在同一边时，设 $c_i=\min(a_{mid},a_{mid-1},...,a_{mid-i}),d_i=\min(b_{mid+1},b_{mid+2},...,b_{mid+i})$，以及 $f_j(i)=-i\times d_j+j\times d_j,i\leq j$。线段树维护 $[l,r]$ 这个区间里所有函数的凸包，最后查询这个线段树的值再乘上 $c_i$ 的值就行了。

​另一种同理。

​时间复杂度为 $O(n\log_2^2n)$。

### 代码:

代码链接:[[DS记录\]P7164 [COCI2020-2021#1] 3D Histogram - command_block 的博客 - 洛谷博客 (luogu.com.cn)](https://www.luogu.com.cn/blog/command-block/ds-ji-lu-p7164-coci2020-20211-3d-histogram)

```C++
#include<algorithm>
#include<cstdio>
#include<vector>
#define pb push_back
#define ll long long
#define MaxN 200500
using namespace std;
const int INF=1000000000;
struct Line{
  ll k,b;
  ll get(int x)
  {return k*x+b;}
  bool operator < (const Line &B) const
  {return k<B.k;}
};
bool chk(const Line &A,const Line &B,const Line &C){
  ll x1=A.k-B.k,y1=A.b-B.b
    ,x2=C.k-B.k,y2=C.b-B.b;
  return x1*y2-y1*x2>0; 
}
#define Hull vector<Line>
Line st[MaxN];
void max(const Hull &A,const Hull &B,Hull &C)
{
  merge(A.begin(),A.end(),B.begin(),B.end(),st);
  int n=A.size()+B.size();
  C.clear();
  for (int i=0;i<n;i++){
    while(C.size()>=2&&!chk(C[C.size()-2],C.back(),st[i]))C.pop_back();
    C.pb(st[i]);
  }
}
struct Node{Hull s;int p;};
ll get(Hull &s,int &p,int x){
  while(p+1<s.size()&&s[p+1].get(x)>s[p].get(x))p++;
  return s[p].get(x);
}
struct HullDS
{
  Node a[1<<18|500];
  Line sl[MaxN];
  int tl,tr;
  void build(int l,int r,int u)
  {
    a[u].s.clear();a[u].p=0;
    if (l==r){a[u].s.pb(sl[l]);return ;}
    int mid=(l+r)>>1;
    build(l,mid,u<<1);
    build(mid+1,r,u<<1|1);
    max(a[u<<1].s,a[u<<1|1].s,a[u].s);
  }
  int wfl,wfr,wfx;ll ret;
  void _qry(int l,int r,int u)
  {
    if (wfl<=l&&r<=wfr){
      ret=max(ret,get(a[u].s,a[u].p,wfx));
      return ;
    }int mid=(l+r)>>1;
    if (wfl<=mid)_qry(l,mid,u<<1);
    if (mid<wfr)_qry(mid+1,r,u<<1|1);
  }
  ll qry(int l,int r,int x){
    wfl=l;wfr=r;wfx=x;
    ret=0;_qry(tl,tr,1);
    return ret;
  }
}T0,T1,T01;
struct Data{int x0,x1;}s[MaxN];
int h0[MaxN],h1[MaxN];ll ans;
void solve(int l,int r)
{
  if (l==r)return ;
  int mid=(l+r)>>1;
  solve(l,mid);solve(mid+1,r);
  h1[mid]=h0[mid]=INF;
  T0.tl=T1.tl=T01.tl=mid+1;
  T0.tr=T1.tr=T01.tr=r;
  // (c0+c1)*min(t0,h0)*min(t1,h1)
  for (int i=mid+1;i<=r;i++){
    h0[i]=min(s[i].x0,h0[i-1]);
    h1[i]=min(s[i].x1,h1[i-1]);
    T0.sl[i]=(Line){h0[i],1ll*h0[i]*(i-mid)};
    //(c0+c1)*h0*t1 = t1*(c0*h0+c1*h0)
    T1.sl[i]=(Line){h1[i],1ll*h1[i]*(i-mid)};
    //(c0+c1)*t0*h1 = t0*(c0*h1+c1*h1)
    T01.sl[i]=(Line){1ll*h0[i]*h1[i],1ll*h0[i]*h1[i]*(i-mid)};
    //(c0+c1)*h0*h1 = c0*h0*h1+c1*h0*h1
  }
  T0.build(mid+1,r,1);
  T1.build(mid+1,r,1);
  T01.build(mid+1,r,1);
  for (int i=mid,t0=INF,t1=INF,p0=mid,p1=mid;i>=l;i--){
    t0=min(t0,s[i].x0);
    t1=min(t1,s[i].x1);
    while(p0<r&&h0[p0+1]>=t0)p0++;
    while(p1<r&&h1[p1+1]>=t1)p1++;
    ans=max(ans,1ll*(min(p0,p1)-i+1)*t0*t1);
    if (p0<p1)ans=max(ans,t1*T0.qry(p0+1,p1,mid-i+1));
    if (p1<p0)ans=max(ans,t0*T1.qry(p1+1,p0,mid-i+1));
    if (max(p0,p1)<r)ans=max(ans,T01.qry(max(p0,p1)+1,r,mid-i+1));
  }
}
int n;
int main()
{
  scanf("%d",&n);
  for (int i=1;i<=n;i++){
    scanf("%d%d",&s[i].x0,&s[i].x1);
    ans=max(ans,1ll*s[i].x0*s[i].x1);
  }solve(1,n);
  printf("%lld",ans);
  return 0;
}
```



## T4 Papricice

### 题目描述:

​在花园忙碌了一个早上之后。M 先生决定用自己种的干辣椒奖励一下自己。 

​M 先生有 $n$ 颗辣椒，这些辣椒由 $n-1$ 条线链接而成。任意两颗辣椒都可以通过若干条线连接起来。简单来说，这 $n$ 颗辣椒和这 $n-1$ 条线连接成了一棵树。 

​M 先生今天要吃三顿饭，因此他需要剪断两条线。获得三串小的辣椒。每一顿饭需要使用一串辣椒。

​显然，一顿饭不能太辣，因此他会选择一种分割方法，使得**辣椒最多的辣椒串和辣椒最少的辣椒串的数量差距最小。**你的任务就是求出这个最小的差距。

### 输入描述:

​输入的第一行包含一个整数 $n(1\leq n\leq 2\times10^5)$，表示辣椒的数量。辣椒从1到 $n$ 编号。 

​接下来的 $n-1$ 行，每行包括两个整数 $x$ 和 $y(1\leq x,y\leq n)$。表示由一条连接编号为 $x$ 的辣椒和编号为 $y$ 的辣椒的线。

### 输出描述:

​输出一行，一个整数，即最小的最大辣椒串和最小辣椒串的数量差距。

### 分析:

#### 50Pts:

​暴力枚举要删除的那两个点，然后计算这三个部分的点的数量并更新 $ans$ 的值就行了。

​时间复杂度为 $O(n^2)$。

#### 100Pts:

​假设我们已经切掉了点 $x$ 与其父亲节点所形成的的边，现在我们需要查找下一个需要删除的边究竟是哪一条。

​分成两种情况考虑：

​1.当另一条边在其祖先节点上，那么我们需要分出的另一个部分的节点数量最好为 $\frac{n-size[x]}{2}$。由于其祖先节点 $y$ 的 $size_y$ 包含了 $x$ 的子树下所有的节点，因此我们只需要找到 $x$ 的祖先节点中子树的节点数最靠近 $\frac{n+size[x]}{2}$ 的一个点并计算当前情况下的最终结果并将其与 $ans$ 比较即可。

​2.当另一条边在已经被访问过的子树上时，我们只需要找到已经访问过的节点中子树的节点数最接近 $\frac{n-size[x]}{2}$ 的一个点并计算当前情况下的最终结果并将其与 $ans$ 比较。

​至于如何查找，使用 multiset 自带的 lower_bound 函数即可。

​时间复杂度为 $O(n\log_2n )$。

### 代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define N 200010
#define ywhin cin
#define ywhout cout
using namespace std;
van n;vector<van> g[N];
bool used[N];van siz[N],ans=1e18;
multiset<van> father,son;
void init(van now)
{
//	cout<<now<<endl;
	used[now]=1,siz[now]=1;van v;
	for (int i=0;i<g[now].size();i++) if (!used[v=g[now][i]]) init(v),siz[now]+=siz[v];
}//计算子树中的节点数
void DFS(van now)
{
	used[now]=1;van v;
	multiset<van>::iterator it;van siz2,last,tmpans;
	if (!father.empty())
	{
		it=father.lower_bound((n-siz[now])/2+siz[now]);
		if (it!=father.end())
		{
			siz2=*it-siz[now];
			last=n-siz2-siz[now];
			tmpans=max(siz[now],max(siz2,last))-min(siz[now],min(siz2,last));
			ans=min(ans,tmpans);	
		}
		if (it!=father.begin())
		{
			it--;
			siz2=*it-siz[now];
			last=n-siz2-siz[now];
			tmpans=max(siz[now],max(siz2,last))-min(siz[now],min(siz2,last));
			ans=min(ans,tmpans);	
		}
	}//第一种情况
	if (!son.empty())
	{
		it=son.lower_bound((n-siz[now])/2);
		if (it!=son.end())
		{
			siz2=*it;
			last=n-siz2-siz[now];
			tmpans=max(siz[now],max(siz2,last))-min(siz[now],min(siz2,last));
			ans=min(ans,tmpans);	
		}
		if (it!=son.begin())
		{
			it--;
			siz2=*it;
			last=n-siz2-siz[now];
			tmpans=max(siz[now],max(siz2,last))-min(siz[now],min(siz2,last));
			ans=min(ans,tmpans);	
		}
	}//第二种情况
	if (now!=1) father.insert(siz[now]);//将自己加到祖先集合中
	for (int i=0;i<g[now].size();i++) if (!used[v=g[now][i]]) DFS(v);
	if (now!=1) father.erase(father.find(siz[now]));//将自己移除祖先集合
	if (now!=1) son.insert(siz[now]);//将自己移入已访问的节点的集合
}
int main()
{
	ywhin>>n;
	for (int i=1;i<n;i++)
	{
		van f,s;
		scanf("%lld %lld",&f,&s);
		g[f].push_back(s);
		g[s].push_back(f);
	}//建图
	init(1);
	memset(used,0,sizeof used);
	DFS(1);
	ywhout<<ans<<endl;
	return 0;
}
```



## T5 Tenis

### 题目描述:

​米尔科先生是一个网球的狂热粉丝。不久之后，一个重要的锦标赛就要开始了，这场锦标赛一共有 $n$ 位参赛选手。米尔科先生花费了多年时间研究参赛者们并收集了大量的数据。它将他们在三种不同场地上的实力调查清楚。并按照每一个不同的场地给运动员们进行了一个排序，排序为一的表示在这一场地上，这一运动员是最强的。排序最后的最弱。

​在这次的锦标赛中，每两位选手都会正好交手一次。因此一共会有 $\frac{n(n-1)}{2}$ 场比赛。一场网球赛是不会有平局的，**在这个场地上实力排序更强的选手会获得胜利。**主办方对此心知肚明，因此他们决定将每场比赛安排在胜者水平最高的场地，也就是说排名最靠前的场地上。如果存在两个场地胜者排名相同(例如：运动员 A 和 B 进行比赛，A 在场地 1 上会获胜，B 在场地 2 上会获胜，但是二者在对应的场地上拥有相同的排名)，他们就会选择败方排名最好的场地，如果败方排名相同，那么他们就会选择编号最小的场地。 

​你的任务是算出锦标赛的结果，包括：不同场地上的比赛场数，每一位运动员的获胜次数。

### 输入描述:

​输入的第一行包括一个整数 $n(1\leq n\leq 10^5)$，表示运动员的数量，运动员们从 1 到 $n$ 编号。 

​接下来的 3 行输入，每一行是一个长度为 $n$ 的序列，这个序列是 $1$ ~ $n$ 的一个排列，表示在这个场地上选手的实力排序，第一个输入的编号就是这个场地上实力最强的选手编号，以此类推。

### 输出描述:

​输出的第一行，包括三个整数，分别表示在场地 1,2,3 上举办的比赛的数量。 

​输出的第二行包括 $n$ 个整数，分别表示编号从 1 到 $n$ 的每一位选手的获胜场次。

### 分析:

​我们将比赛分成两类：第一种是严格的胜利，即赢家的获胜位置严格小于输家的获胜位置。第二种是非严格的胜利，这意味着球员在球场上的获胜位置是相等的。

​现在问题转化为在一个 3 列的网格上统计。从左往右扫一遍，对于这一列的每个球员是第一次出现，那么我们统计 $(i+1,n)$ 的答案；否则直接跳过。具体我们要维护 $cnt[8][3]$ 表示在状态 $mask$ 的第 $i$ 行排的最小且球场编号最小的球员的位置。

​现在我们计算非严格胜利。枚举 $i$ , $j$ 行 $(i<j)$, 如果 $x$, $y$ 都不等于 0 $(x\not=y)$, 我们可以枚举 $k\in [0,2]$ , 使得在输家位置最靠前的情况下场地编号最小，就可以比较出 $x$ , $y$ 的结果。

​常数大概有 $2^3\times 3^2=72$。时间复杂度为 $O(72\times n)$。

### 代码:

```C++
#include<bits/stdc++.h>
#define van long long
#define N 100010
#define ywhin cin
#define ywhout cout
using namespace std;
van garbage[3][N],n;
van que[N][3],cnt[8][3];
van ans1[N],ans2[N];
van in[N];
void add(van w,van d)
{
	for(int i=1;i<=7;i++) 
	{
		van min_=-1;
		for (int j=0;j<3;j++) if ((i>>j&1)&&(min_==-1||que[w][j]<que[w][min_])) min_=j;
		cnt[i][min_]+=d;
	}
}//维护cnt
int main()
{
	ywhin>>n;
	for (int i=0;i<3;i++) for (int j=1;j<=n;j++) ywhin>>garbage[i][j];
	for (int i=0;i<3;i++) for (int j=1;j<=n;j++) que[garbage[i][j]][i]=j;
	for (int i=1;i<=n;i++) add(i,1);
	for (int i=1;i<=n;i++) 
	{
		for (int j=0;j<3;j++) 
			if (in[garbage[j][i]]) garbage[j][i]=0;
			else
			{
				add(garbage[j][i],-1);
				for (int k=j;k<3;k++) if (garbage[k][i]==garbage[j][i]) in[garbage[j][i]]|=1<<k;
			}
		for (int j=0;j<3;j++) if (garbage[j][i])
		{
			van haha=garbage[j][i];
			for (int k=0;k<3;k++)
			{
				ans1[k]+=cnt[in[haha]][k];
				ans2[haha]+=cnt[in[haha]][k];
			}
			for (int k=j+1;k<3;k++) if (garbage[k][i])
			{
				van ha=garbage[k][i];
				pair<van,van> min_=make_pair(n+1,n+1);
				for (int l=0;l<3;l++) 
				{
					if (que[haha][l]==i) min_=min(min_,make_pair(que[ha][l],(van)l));
					if (que[ha][l]==i) min_=min(min_,make_pair(que[haha][l],(van)l));
				}
				ans1[min_.second]++;
				ans2[que[haha][min_.second]==i?haha:ha]++;
			}
		}
	}
	ywhout<<ans1[0]<<" "<<ans1[1]<<" "<<ans1[2]<<endl;
	for (int i=1;i<=n;i++) ywhout<<ans2[i]<<" ";
	ywhout<<endl;
	return 0;
}
```

