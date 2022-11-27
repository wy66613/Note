# 一、基础算法

## 1、排序

### 插入排序

#### 算法实现

```c++
void InsertSort(int* arr,int n)
{
    int j,p;
    int tmp;
    for(p=1;p<n;p++)
        for(j=p;j>0&&arr[j-1]>arr[j];j--)
            swap(arr, j, j-1); 
}

void swap(int* arr, int i, int j)
{
	int temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```



#### 算法分析

时间复杂度为O(N^2)，只考虑最坏情况。但在某些情况下，插入排序是优于冒泡排序的。



### 希尔排序

#### 算法实现（使用希尔增量为例）

```c++
void ShellSort(int A[],int N)
{
    int i,j,Increment;
    int tmp;
    for(Increment=N/2;Increment>0;Increment/=2)
    	for{i=Increment;i<N;i++}
    {
        tmp=A[i];
        for(j=i;j>=Inrement;j-=Increment)
            if(tmp<A[j-Increment])
                A[j]=A[j-Increment];
        	else
                break;
        A[j]=tmp;
    }
}
```



#### 算法分析

希尔排序的运行时间依赖于增量序列的选择。

希尔排序的递推公式：
$$
h_t={n\over2},h_k={h_{k+1}\over2}
$$
定理：使用希尔增量时希尔排序的最坏情形运行时间为O(N^2)。

```

```

Hibbard增量序列的递推公式：
$$
h_1=1,h_i=2*h_{i-1}+1
$$
定理：使用Hibbard增量的希尔排序的最坏情形运行时间为O(N^(3/2))。



### 选择排序

#### 算法实现

```c++
int* SelectSort(int* arr, int n)
{
	for (int i = 0; i < n - 1; i++)
	{
		int minIndex = i;
		for (int j = i + 1; j < n; j++)
			minIndex = arr[minIndex] < arr[j] ? minIndex : j;
		swap(arr, minIndex, i);
	}
	return arr;
}

void swap(int* arr, int i, int j)
{
	int temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```



#### 算法分析

因为额外开辟了一个i和j，以及一个minIndex，minIndex每次for循环结束释放，再进入for循环时重新开辟，所以额外空间复杂度为*O(1)*



### 冒泡排序

```c++
int* BubbleSort(int* arr, int n)
{
	for (int e = n - 1; e > 0; e--)
	{
		for (int i = 0; i < e; i++)
			if (arr[i] > arr[i + 1]) swap(arr, i, i + 1);
	}
	return arr;
}

void swap(int* arr, int i, int j)
{
	int temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}

//使用异或进行交换，交换的两个必须是独立的内存空间，在数组中即i位置不能等于j位置
void swap2(int*arr, int i,int j)
{
    arr[i] = arr[i]^arr[j];
    arr[j] = arr[i]^arr[j];
    arr[i] = arr[i]^arr[j];
}
```



### 快速排序

```c++
void QuickSort(int* arr, int l, int r)
{
	if (l == r) return;
	int x = arr[l], i = l - 1, j = r + 1;
	while (i < j)
	{
		do i++; while (arr[i] < x);
		do j--; while (arr[j] > x);
		if (i < j) swap(arr[i], arr[j]);
	}
	QuickSort(arr, l, j);
	QuickSort(arr, j + 1, r);
}
```



#### 快速选择算法

```c++
#include<iostream>

using namespace std;

const int N=100010;

int n,k; // 数组中第k个小的数
int q[N];

int quicksort(int l,int r,int k)
{
	if(l==r) return q[l];
	
	int x=q[l],i=l-1,j=r+1;
	while(i<j)
	{
		while(q[++i]<x);
		while(q[--j]>x);
		if(i<j) swap(q[i],q[j]);
	}
	
	int sl=j-l+1;
	if(k<=sl) return quicksort(l,j,k);
	
	return quicksort(j+1,r,k-sl);
}

int main()
{
	cin>>n>>k;
	for(int i=0;i<n;i++) cin>>q[i];
	
	cout<<quicksort(0,n-1,k)<<endl;
	
	return 0;
}
```



### 归并排序

```c++
void MergeSort(int* arr, int l, int r)
{
	if (l == r) return;
	int mid = (r + l) / 2;
	MergeSort(arr, l, mid), MergeSort(arr, mid + 1, r);

	int k = 0, i = l, j = mid + 1, tmp[10000];
	while (i <= mid && j <= r)
		if (arr[i] <= arr[j]) tmp[k++] = arr[i++];
		else tmp[k++] = arr[j++];
    
	while (i <= mid) tmp[k++] = arr[i++];
	while (j <= r) tmp[k++] = arr[j++];
	
	for (i = l, j = 0; i <= r; i++, j++) arr[i] = tmp[j];
}	
```



## 2、二分

### 整数二分模板

```c++
//区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用
int bsearch_1(int l, int r)
{
    while(l < r)
    {
        int mid = ( l + r ) / 2;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}

//区间[1, r]被分成[1, mid - 1]和[mid, r]
int bsearch_2(int l, int r)
{
    while(l < r)
    {
        int mid = (1 + r + l)/2;
        if(check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

### 浮点数的二分

```c++
#include<iostream>

using namespace std;

int main()
{
	double x;
	cin>>x;
	
	double l=-10000,r=10000;
	while(r-l>1e-8)
	{
		double mid=(r+l)/2;
		if(mid*mid*mid>=x) r=mid;
		else l=mid;	
	} 
	
	printf("%lf",l);
	return 0;
}
```



## 3、高精度

### 高精度加法

```c++
#include<iostream>
#include<vector>

using namespace std;

vector<int> add(vector<int>& A,vector<int>& B)
{
	vector<int> C;
	int t=0; //进位
	for(int i=0;i<A.size()||i<B.size();i++)
	{
		if(i<A.size()) t+=A[i];
		if(i<B.size()) t+=B[i];
		C.push_back(t%10);
		t/=10;
	}
	if(t) C.push_back(1);
	return C;	
}

int main()
{
	string a,b;
	cin>>a>>b;
	vector<int> A,B;
	for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
	for(int i=b.size()-1;i>=0;i--) B.push_back(b[i]-'0');
	
	vector<int> C=add(A,B);
	for(int i=C.size()-1;i>=0;i--) cout<<C[i];
	cout<<endl;
	
	return 0;
}
```



### 高精度减法

```c++
#include<iostream>
#include<vector>

using namespace std;

// 判断是否有A>=B
bool cmp(vector<int>& A,vector<int>& B)
{
	if(A.size()!=B.size()) return A.size()>B.size();
	for(int i=A.size()-1;i>=0;i--)
		if(A[i]!=B[i]) return A[i]>B[i];
	return true;
}

// A>=B 
vector<int> sub(vector<int>& A,vector<int>& B)
{
	vector<int> C;
    int t=0; // 借位
	for(int i=0;i<A.size();i++)
	{
		t=A[i]-t;
		if(i<B.size()) t-=B[i];
		C.push_back((t+10)%10);
		if(t<0) t=1;
		else t=0;
	}
	while(C.size()>1&&C.back()==0) C.pop_back(); // 去掉前导0
	return C;
}

int main()
{
	string a,b;
	vector<int> A,B;
	cin>>a>>b;
	for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
	for(int i=b.size()-1;i>=0;i--) B.push_back(b[i]-'0');
	
	if(cmp(A,B))
	{
		vector<int> C=sub(A,B);
		for(int i=C.size()-1;i>=0;i--) cout<<C[i];
	}
	else
	{
		vector<int> C=sub(B,A);
		cout<<"-";
		for(int i=C.size()-1;i>=0;i--) cout<<C[i];
	}
	return 0;
}
```



### 高精度乘法

一个高精度的整数 × 一个较小的数

```c++
#include<iostream>
#include<vector>

using namespace std;

vector<int> mul(vector<int>& A,int b)
{
	vector<int> C;
	int t=0;
	for(int i=0;i<A.size()||t;i++) 
	{
		if(i<A.size()) t+=A[i]*b;
		C.push_back(t%10);
		t/=10;
	}
	
	return C;
}

int main()
{
	string a;
	int b;
	cin>>a>>b;
    
	vector<int> A;
	for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
    
	vector<int> C=mul(A,b);
	for(int i=C.size()-1;i>=0;i--) cout<<C[i];
    
	return 0;
}
```



### 高精度除法

一个高精度的整数 ÷ 一个较小的数

```c++\
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

// 商是C，余数是r
vector<int> div(vector<int>& A,int b,int& r)
{
	vector<int> C;
	r=0;
	for(int i=A.size()-1;i>=0;i--)
	{
		r=r*10+A[i];
		C.push_back(r/b);
		r%=b;
	}
	
	reverse(C.begin(),C.end()); // #include<algorithm>
	
	while(C.size()>1&&C.back()==0) C.pop_back(); // 去掉前导0
	
	return C;
}

int main()
{
	string a;
	int b;
	cin>>a>>b;
	
	vector<int> A;
	for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
	
	int r;
	vector<int> C=div(A,b,r);
	
	for(int i=C.size()-1;i>=0;i--) cout<<C[i];
	cout<<endl<<r<<endl;
	
	return 0;
}
```



## 4、前缀和与差分

### 前缀和

#### 一维前缀和

- 公式
  $$
  S_{[l,r]}=S_l-S_{[r-1]}
  $$

```c++
#include<iostream>

using namespace std;

const int N=100010;

int n,m;
int a[N],s[N]; // 全局定义的数组初始化为0 

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d",&a[i]);
	
	for(int i=1;i<=n;i++) s[i]=s[i-1]+a[i]; // 前缀和的初始化
	
	while(m--)
	{
		int l,r;
		scanf("%d%d",&l,&r);
		printf("%d\n",s[r]-s[l-1]); // 区间和的计算
	}
	
	return 0;
}
```



#### 二维前缀和

- 公式

$$
s_{[i,j]}=s_{[i,j-1]}+s_{[i-1,j]}-s_{[i-1,j-1]}+a_{[i,j]}
$$

- 某一矩形区域内的和，左上角为[x1,y1]，右上角为[x2,y2]

$$
s_{[x2,y2]}-s_{[x1-1,y2]}-s_{[x2,y1-1]}+s_{[x1-1,y1-1]}
$$

```c++
#include<iostream>
using namespace std;

const int N=1010;

int n,m,q;
int a[N][N],s[N][N];


int main()
{
	scanf("%d%d%d",&n,&m,&q);
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			scanf("%d",&a[i][j]);
			
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1]+a[i][j]; // 前缀和
			
	while(q--)
	{
		int x1,y1,x2,y2;
		scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
		printf("%d\n",s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1]); //某个矩形范围内的和
	}
	return 0;
}
```



### 差分

#### 一维数组

```c++
#include<iostream>

using namespace std;

const int N=100010;
int n,m;
int s[N],b[N];

void insert(int l,int r,int c)
{
	b[l]+=c;
	b[r+1]-=c;
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d",&s[i]); // 构造前缀和数组 
	
	for(int i=1;i<=n;i++) insert(i,i,s[i]); //  构造差分数组 
	
	while(m--)
	{
		int l,r,c;
		scanf("%d%d%d",&l,&r,&c);
		insert(l,r,c);
	}
	
	for(int i=1;i<=n;i++) b[i]+=b[i-1]; // 将差分数组改为前缀和数组 
	
	for(int i=1;i<=n;i++) printf("%d ",b[i]); 
	
	return 0;
}
```

#### 差分矩阵

```c++
#include<iostream>

using namespace std;

const int N=1010;

int n,m,q;
int s[N][N],b[N][N];

void insert(int x1,int y1,int x2,int y2,int c)
{
	b[x1][y1]+=c;
	b[x2+1][y1]-=c;
	b[x1][y2+1]-=c;
	b[x2+1][y2+1]+=c;
}

int main()
{
	scanf("%d%d%d",&n,&m,&q);
	
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			scanf("%d",&s[i][j]); // 构造前缀和数组 
			
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			insert(i,j,i,j,s[i][j]); // 构造差分数组 
	
	while(q--)
	{
		int x1,y1,x2,y2,c;
		cin>>x1>>y1>>x2>>y2>>c;
		insert(x1,y1,x2,y2,c);
	}
	
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			b[i][j]+=b[i-1][j]+b[i][j-1]-b[i-1][j-1]; // 将差分数组改为前缀和数组
	
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=m;j++) printf("%d ",b[i][j]);
		printf("\n");
	}
	
	return 0;
}
```



## 5、双指针算法

> 题目：给定包含若干个单词的字符串，每个单词用空格隔开，将每个单词分开。

```c++
#include<iostream>
#include<string.h>
using namespace std;

int main()
{
	char str[1000];
	gets(str);
	
	int n=strlen(str);
	
	for(int i=0;i<n;i++)
	{
		int j=i;
		while(j<n&&str[j]!=' ') j++;
		
		for(int k=i;k<j;k++) cout<<str[k];
		cout<<endl;
		
		i=j;
	}
	return 0;
}
```

```c++
#include<iostream>
#include<string.h>
using namespace std;

int main()
{
	char str[1000];
	gets(str);
	
	int n=strlen(str);
	
	for(int i=0;i<n;i++)
	{
		int j=i;
		while(j<n&&str[j]!=' ') j++; // j遇到空格停止 
		
		for(;i!=j;i++)
			cout<<str[i];
		cout<<endl; 
	}
	return 0;
}
```



> 最长连续不重复子序列：给定一个长度为 n 的整数序列，找出最长的不包含重复的数的连续区间，输出它的长度。

```c++
#include<iostream>

using namespace std;

const int N=100010;
int n;
int a[N],s[N];

int main()
{
	cin>>n;
	for(int i=0;i<n;i++) cin>>a[i];
	
	int res=0;
	
	for(int i=0,j=0;i<n;i++)
	{
		s[a[i]] ++;
		while(j<i&&s[a[i]]>1) s[a[j++]] --;		 
		res=max(res,i-j+1);	
	}
	
	cout<<res<<endl;
	
	return 0;
}
```



## 6、位运算

> 判断n的二进制表示的第k位是几

```c++
int res = n>>k&1;
```



> 返回n的二进制表示中最后一个1

```c++
/*
在c++中，一个整数的负数是补码的概念，也就是取反＋！
即 -x = ~x + 1
因此 x&-x = x&(~x+1)
最简单的应用为统计x的二进制表示中1的个数
*/

int lowbit(int x)
{
    return x&-x;
}
```



## 7、离散化

**下面示例中的 `find函数` 是离散化的关键**

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

const int N = 300010; // n次插入和m次查询操作所涉及到的最多的坐标数量
int n,m; // n次插入和m次查询
int a[N]; // 存储坐标插入的值
int s[N]; //数组a的前缀和数组
vector<int> alls; // 存储n次插入和m次查询设计的坐标，以此来离散化
vector<pair<int,int> > add, query; // 存储插入和查询操作的数据

int find(int x) // 返回输入坐标x的离散化坐标
{
	int l=0,r=alls.size()-1;
	while(l<r)
	{
		int mid=(l+r)/2;
		if(alls[mid]>=x) r=mid;
		else l=mid+1;
	}
	return r+1;
}  

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)
	{
		int x,c;
		scanf("%d%d",&x,&c);
		add.push_back({x,c});
		alls.push_back(x);
	}
	
	for(int i=1;i<=m;i++)
	{
		int l,r;
		scanf("%d%d",&l,&r);
		query.push_back({l,r});
		alls.push_back(l);
		alls.push_back(r);
	}
	
	// 排序，去重
	sort(alls.begin(),alls.end());
	alls.erase(unique(alls.begin(),alls.end()),alls.end());
	
	// 执行前n次插入操作
	for(auto item : add)
	{
		int x=find(item.first);
		a[x] += item.second;	
	} 
	
	// 前缀和
	for(int i=1;i<=alls.size();i++) s[i]=s[i-1]+a[i];
	
	// 处理后m次查询操作
	for(auto item : query)
	{
		int l=find(item.first);
		int r=find(item.second);
		printf("%d\n",s[r]-s[l-1]);
	} 
	
	return 0;
}
```

## 8、区间和并

快速地将有交集的区间合并

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

vector<pair<int,int> > segs;

int main()
{
	vector<pair<int,int> > res;
	
	int st=-1e9-10,ed=-1e9-10;
	int n;
	cin>>n;
	while(n--)
	{
		int l,r;
		cin>>l>>r;
		segs.push_back({l,r});
	}
	
	sort(segs.begin(),segs.end());
	
	for(auto seg:segs)
	{
		if(ed<seg.first) // 该区间不能合并
		{
			if(ed!=-1e9-10) res.push_back({st,ed}); // 判断是否是第一个区间
			st=seg.first,ed=seg.second; // 维护正在遍历的区间
		}
		else if(ed<seg.second) ed=seg.second; // 合并正在遍历的区间
	}
	
	res.push_back({st,ed}); // 将最后一次合并完成的区间加入
	
	printf("%d",res.size());
	
	return 0;
} 
```



# 二、数据结构

## 1、链表

### 单链表

```c++
#include<iostream>

using namespace std;

const int N=100010;

// head 表示头节点的下标
// e[i] 表示节点i的值
// ne[i] 表示节点i的next指针是多少
// idx 存储当前已经用到了哪个点 
int head,e[N],ne[N],idx;

// 初始化 
void init()
{
	head=-1;
	idx=0;
}

// 将x插到头节点 
void add_to_head(int x) 
{
	e[idx]=x;
	ne[idx]=head;
	head=idx;
	idx++;	
}

// 将x插入到下标是k的节点的后面 
void add(int k,int x)
{
	e[idx]=x;
	ne[idx]=ne[k];
	ne[k]=idx;
	idx++;
}

// 将下标是k的节点的后面的节点删掉 
void remove(int k)
{
	ne[k]=ne[ne[k]];
}


int main()
{
	// 一系列操作 
	return 0;
}
```



### 双链表

```c++
#include<iostream>
using namespace std;

const int N=100010;

int m;
int e[N],l[N],r[N],idx;

void init()
{
	r[0]=1,l[1]=0;
	idx=2;
}

void add(int k,int x)
{
	e[idx]=x;
	r[idx]=r[k];
	l[idx]=k;
	l[r[k]]=idx;
	r[k]=idx;
	idx++;
}

void remove(int k)
{
	l[r[k]]=l[k];
	r[l[k]]=r[k];
}

int main()
{
	init();
	cin>>m;
	int k,x;
	string op;
	while(m--)
	{
		cin>>op;
		if(op=="R")
		{
			cin>>x;
			add(l[1],x);
		}
		else if(op=="L")
		{
			cin>>x;
			add(0,x);
		}
		else if(op=="D")
		{
			cin>>k;
			remove(k+1);
		}
		else if(op=="IL")
		{
			cin>>k>>x;
			add(l[k+1],x);
		}
		else
		{
			cin>>k>>x;
			add(k+1,x);
		}
	}
	
	for(int i = r[0]; i != 1; i = r[i]) printf("%d ",e[i]);

	return 0;
}
```



## 2、栈和队列

### 栈

```c++
#include<iostream>

using namespace std;

const int N=100010;

// tt为栈顶 
int stk[N],tt=0;

int main()
{
	int m;
	cin>>m;
	while(m--)
	{
		string op;
		int x;
		
		cin>>op;
		if(op=="push")
		{
			cin>>x;
			stk[++tt]=x; 
		}
		else if(op=="pop") tt--;
		else if(op=="empty") cout<<(tt?"NO":"YES")<<endl;
		else cout<<stk[tt]<<endl;
	}
	return 0;
}
```



### 队列

```c++
#include <iostream>

using namespace std;

const int N = 100010;

int m;
int q[N], hh, tt = -1; 

int main()
{
	cin>>m;
	while(m--)
	{
		string op;
		int x;
		
		cin>>op;
		if(op=="push")
		{
			cin>>x;
			q[++tt]=x;
		}
		else if(op=="pop") hh++;
		else if(op=="empty") cout << (hh<=tt?"NO":"YES") <<endl;
		else cout<<q[hh]<<endl;
	}
	
	return 0;
}
```



### 单调栈

> 题目：给定一个长度为 N 的整数数列，输出每个数左边第一个比它小的数，如果不存在则输出 −1。

```c++
#include<iostream>

using namespace std;

const int N=100010;

int n;
int stk[N],tt;

int main()
{
	scanf("%d",&n);
	
	for(int i=0;i<n;i++)
	{
		int x;
		scanf("%d",&x);
		while(tt&&stk[tt]>=x) tt--;
		if(tt) printf("%d ",stk[tt]);
		else printf("-1 ");
		
		stk[++tt] = x;
	}
	
	return 0;
}
```



### 单调队列

```c++
#include<iostream>

using namespace std;

const int N=1000010;
int a[N],q[N],hh,tt=-1;

int main()
{
	int n,k;
	scanf("%d%d",&n,&k);
	for(int i=0;i<n;i++)
	{
		scanf("%d",&a[i]);
		if(i-k+1>q[hh]) hh++; // 队首出窗口，hh+1
		while(hh<=tt&&a[i]<=a[q[tt]]) tt--; // 队尾不单调，tt-1，双端队列 
		q[++tt]=i; // 下标加入到队尾中 
		if(i+1>=k) printf("%d ",a[q[hh]]);  // 队首要有数据才能输出 
	}
	cout<<endl;
	
	hh=0,tt=-1;
	for(int i=0;i<n;i++)
	{
		if(i-k+1>q[hh]) hh++;
		while(hh<=tt&&a[i]>=a[q[tt]]) tt--;
		q[++tt]=i;
		if(i+1>=k) printf("%d ",a[q[hh]]);
	}
	
	return 0;
}
```



## 3、KMP

```c++
#include <iostream>

using namespace std;

const int N = 1000010;
char p[N], s[N]; // 用 p 来匹配 s
// “next” 数组，若第 i 位存储值为 k
// 说明 p[0...i] 内最长相等前后缀的前缀的最后一位下标为 k
// 即 p[0...k] == p[i-k...i]
int ne[N]; 
int n, m; // n 是模板串长度 m 是模式串长度

int main()
{
    cin >> n >> p >> m >> s;

    // p[0...0] 的区间内一定没有相等前后缀
    ne[0] = -1;

    // 构造模板串的 next 数组
    for (int i = 1, j = -1; i < n; i ++)
    {
        while (j != -1 && p[i] != p[j + 1])
        {
            // 若前后缀匹配不成功
            // 反复令 j 回退，直至到 -1 或是 s[i] == s[j + 1]
            j = ne[j];
        }
        if (p[i] == p[j + 1]) 
        {
            j ++; // 匹配成功时，最长相等前后缀变长，最长相等前后缀最后一位变大
        }
        ne[i] = j; // 令 ne[i] = j，以方便计算 next[i + 1]
    }

    // kmp start !
    for (int i = 0, j = -1; i < m; i ++)
    {
       while (j != -1 && s[i] != p[j + 1])
       {
           j = ne[j];
       }
       if (s[i] == p[j + 1])
       {
           j ++; // 匹配成功时，模板串指向下一位
       }
       if (j == n - 1) // 模板串匹配完成，第一个匹配字符下标为 0，故到 m - 1
       {
           // 匹配成功时，文本串结束位置减去模式串长度即为起始位置
           cout << i - j << ' ';

           // 模板串在模式串中出现的位置可能是重叠的
           // 需要让 j 回退到一定位置，再让 i 加 1 继续进行比较
           // 回退到 ne[j] 可以保证 j 最大，即已经成功匹配的部分最长
           j = ne[j]; 
       }
    }

   return 0;
}
```

## 4、Trie树

> 用来快速存储和查找字符串集合的一种数据结构

```c++
/* 
一维是结点总数，而结点和结点之间的关系（谁是谁儿子）存在第二个维度，比如[0][1]=3, [0]表示根节点，[1]表示它有一个儿子‘b’,这个儿子的下标是3；接着如果有一个[3][2]=8 ; 说明根节点的儿子‘b’也有一个儿子‘c’，这个孙子的下标就是8；这样传递下去，就是一个字符串。随便给一个结点[x][y], 并不能看出它在第几层，只能知道，它的儿子是谁。
*/

#include<iostream>

using namespace std;

const int N=100010;

int son[N][26],cnt[N],idx; // 下标是0的点，既是根节点，又是空节点
char str[N];

void insert(char str[])
{
	int p=0;
	for(int i=0;str[i];i++)
	{
		int u=str[i]-'a';
		if(!son[p][u]) son[p][u]= ++idx;
		p=son[p][u];	
	}	
	
	cnt[p]++;
}

int query(char str[])
{
	int p=0;
	for(int i=0;str[i];i++)
	{
		int u=str[i]-'a';
		if(!son[p][u]) return 0;
		p=son[p][u];
	}
	
	return cnt[p];
}

int main()
{
	int n;
	cin>>n;
	while(n--)
	{
		char op[2];
		scanf("%s%s",op,str);
		if(op[0]=='I') insert(str);
		else printf("%d\n",query(str));
	}
	
	return 0;
} 
```



## 5、并查集

> - 用途：
>
> 1. 将两个集合合并
> 2. 询问两个元素是否在一个集合当中
>
> - 关键：
>
> 1. find() 函数

```c++
/*
优化：路径压缩
*/

#include<iostream>

using namespace std;

const int N=100010;

int n,m;
int p[N]; // father数组，存储的是每个元素的父节点

int find(int x) // 返回x的祖宗节点 + 路径压缩 
{
	if(p[x]!=x) p[x]=find(p[x]);
	return p[x];	
} 

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) p[i]=i;
	
	while(m--)
	{
		char op[2];
		int a,b;
		scanf("%s%d%d",op,&a,&b);
		
		if(op[0]=='M') p[find(a)]=find(b);
		else{
			if(find(a)==find(b)) puts("Yes");
			else puts("No");
		}
	}
	
	return 0;
 } 
```



> 给定一个包含 nn 个点（编号为 1∼n1∼n）的无向图，初始时图中没有边。
>
> 现在要进行 mm 个操作，操作共有三种：
>
> 1. `C a b`，在点a和点b之间连一条边，a和b可能相等；
> 2. `Q1 a b`，询问点a和点b是否在同一个连通块中，a和b可能相等；
> 3. `Q2 a`，询问点a所在连通块中点的数量；

```c++
#include<iostream>

using namespace std;

const int N=100010;

int n,m;
int p[N],s[N];

int find(int x)
{
	if(p[x]!=x) p[x]=find(p[x]);
	return p[x];	
} 

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) 
	{
		p[i]=i;
		s[i]=1;
	}
	
	while(m--)
	{
		char op[5];
		int a,b;
		scanf("%s",op);
		
		if(op[0]=='C')
		{
			scanf("%d%d",&a,&b);
			if(find(a)==find(b)) continue;
			s[find(b)]+=s[find(a)];
			p[find(a)]=find(b);
		}
		else if(op[1]=='1')
		{
			scanf("%d%d",&a,&b);
			if(find(a)==find(b)) puts("Yes");
			else puts("No");
		}
		else
		{
			scanf("%d",&a);
			printf("%d\n",s[find(a)]);
		}
	}
	
	return 0;
 } 
```



## 6、堆

> - 支持操作
>
> 1. 插入一个数
> 2. 求集合当中的最小值
> 3. 删除最小值
> 4. 删除任意一个元素
> 5. 修改任意一个元素



> 堆排序

```c++
/*
用一维数组存储一个堆
x的左儿子节点为2x，右儿子节点为2x+1
*/

#include<iostream>
#include<algorithm>

using namespace std;

const int N=100010;

int n,m;
int h[N],s; 

void down(int u)
{
	int t=u; // t表示3个数中的最小值 
	if(u*2<=s&&h[u*2]<h[t]) t=u*2;
	if(u*2+1<=s&&h[u*2+1]<h[t]) t=u*2+1;
	if(u!=t)
	{
		swap(h[u],h[t]);
		down(t);
	}
}

/*
堆的up操作
void up(int u)
{
	while(u/2&&h[u/2]>h[u])
	{
		swap(h[u/2],h[u]);
		u/=2;
	}
}
*/

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d",&h[i]);
	s=n;
	
	for(int i=n/2;i;i--) down(i); 
	
	while(m--)
	{
		printf("%d ",h[1]);
		h[1]=h[s];
		s--;
		down(1);
	}
	
	return 0;
}
```



> 带映射的堆模拟

```c++
#include<iostream>
#include<algorithm>
#include<string.h>

using namespace std;

const int N=100010;

int h[N],s,ph[N],hp[N]; // ph[k]=j表示存的第k个数在堆中的下标为j，hp[j]=k表示堆中下标为j的数是第k个存的数 

void heap_swap(int a,int b)
{
	swap(ph[hp[a]],ph[hp[b]]);
	swap(hp[a],hp[b]);
	swap(h[a],h[b]);
}

void down(int u)
{
	int t=u; // t表示3个数中的最小值 
	if(u*2<=s&&h[u*2]<h[t]) t=u*2;
	if(u*2+1<=s&&h[u*2+1]<h[t]) t=u*2+1;
	if(u!=t)
	{
		heap_swap(u,t);
		down(t);
	}
}

void up(int u)
{
	while(u/2&&h[u/2]>h[u])
	{
		heap_swap(u/2,u);
		u/=2;
	}
}

int main()
{
	int n,m=0;
	cin>>n;
	while(n--)
	{
		char op[10];
		int k,x;
		
		scanf("%s",op);
		if(!strcmp(op,"I"))
		{
			scanf("%d",&x);
			s++;
			m++;
			ph[m]=s,hp[s]=m;
			h[s]=x;
			up(s);
		}
		else if(!strcmp(op,"PM")) printf("%d\n",h[1]);
		else if(!strcmp(op,"DM"))
		{
			heap_swap(1,s);
			s--;
			down(1);
		}
		else if(!strcmp(op,"D"))
		{
			cin>>k;
			k=ph[k];
			heap_swap(k,s);
			s--;
			down(k),up(k);
		}
		else
		{
			cin>>k>>x;
			k=ph[k];
			h[k]=x;
			down(k),up(k);
			
		}
	}
	return 0;
}
```



## 7、Hash表

```c++
```



## 8、C++STL使用技巧

```c++
```

