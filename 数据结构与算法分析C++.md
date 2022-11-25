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

