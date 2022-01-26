[Pixiv: 80193800]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/80193800_p0.png'

## [USACO19OPEN] I Would Walk 500 Miles G 做题记录

### 题目描述:

​在艾泽拉斯，有一群小精灵，他们一共有 $N$ 个个体，编号为 $1...N$。现在，大祭司需要把他们分成非空的 $K$ 组去执行任务。然而，小精灵们总是不听话的，具体来说，属于不同两组之间的小精灵距离越近，他们就越可能在执行任务的时候逃跑见面。

​属于不同两组的小精灵 $x,y$ 之间的距离为 $(2019201913x+2019201949y)\ mod\ 2019201997$。 大祭司希望能够得到一种分组方案，使得**不在同一组的小精灵之间的最短距离最长。**

​你的任务就是帮大祭司求出这个最大的最短距离。

### 输入描述:

​输入只有一行，包括两个正整数 $N$ 和 $K(N\leq 7500,2\leq K\leq N)$，用空格分隔。含义如题面所示。

### 输出描述:

​输出最大的不同组精灵的最小距离。

### 测试样例:

#### 【样例1输入】

```
3 2
```

#### 【样例1输出】

```
2019201769
```

#### 【样例1解释】

​在样例中，小精灵1和小精灵2之间的距离为2019201817，小精灵2和小精灵3之间的距离为2019201685，小精灵1和小精灵3之间的距离为2019201769。所以将小精灵1单独分为一组，小精灵2和3分为一组。$ans=min(2019201817,2019201769)=2019201769$。这是我们能够获得的最优解。

### 数据范围与提示:

| 子任务 | 分数 |   特殊性质   |
| :----: | :--: | :----------: |
|   1    |  30  | $N\leq 1000$ |
|   2    |  30  |    $K=2$     |
|   3    |  40  |  无特殊性质  |

### 题目分析:

​一道数学题(从下面的代码就可以看出来)。

​一个很简单的思路: 每次将两个距离最小的精灵给安排到一起，直到只有 $K$ 个小组后距离最小且在不同小组的讲个精灵之间的距离就是本题的答案。

​我们首先先将题目中的式子简化一下: 

​$(2019201913x+2019201949y)\ mod\ 2019201997$

​$=(2019201997x-84x+2019201997y-48y)\ mod\ 2019201997$

​$=(-84x-48y)\ mod\ 2019201997$

​$=2019201997-(84x+48y)\ mod\ 2019201997$

​$\because x,y\leq 7500$

​$\therefore 84x+48y<2019201997$

​$\therefore$ 原式 $=2019201997-84x-48y$

​根据以上式子，容易看出当 $x=N-1,y=N$ 时，原式的值一定是最小的，因此我们就把编号为 $N$ 和 $N-1$ 的精灵给合并在一起了。

​接下来继续考虑其他的精灵，容易看出当 $x=N-2,y=N$ 时，上述式子的结果是在所有满足条件的结果中最小的，因此我们就把编号为 $N$，$N-1$ 和 $N-2$ 的精灵给合并在了一起。

​那么当已经将 $i,i+1,...,N-1,N$ 的精灵合并在一起了的时候，同样不难看出，当 $x=i-1,y=N$ 时，上述式子的结果是在所有满足条件的结果中最小的。

​综上，最终的结果就是当 $x=K-1,y=N$ 时上述式子的值。 

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define ywhin cin
#define ywhout cout
using namespace std;
bool ppppp;
const van N=7510;
//ifstream ywhin("walk.in");
//ofstream ywhout("walk.out");
van n,k;
bool pppppp;
int main() {
	ywhin>>n>>k;
	ywhout<<2019201997LL-84*(k-1)-48*n<<endl;
}
```
