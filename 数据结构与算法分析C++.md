# 排序

## 插入排序

### 算法实现

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



### 算法分析

时间复杂度为O(N^2)，只考虑最坏情况。但在某些情况下，插入排序是优于冒泡排序的。



## 希尔排序

### 算法实现（使用希尔增量为例）

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



### 算法分析

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



## 选择排序

### 算法实现

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



### 算法分析

因为额外开辟了一个i和j，以及一个minIndex，minIndex每次for循环结束释放，再进入for循环时重新开辟，所以额外空间复杂度为*O(1)*



## 冒泡排序

### 算法实现

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



## 快速排序

### 算法实现

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



## 归并排序

### 算法实现

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



# 二分

## 整数二分模板

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



# 高精度

## 高精度加法

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



## 高精度减法

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
	for(int i=0,t=0;i<A.size();i++)
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



## 高精度乘法

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



## 高精度除法

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







# --------------------------

## 最大连续子列和

```c++
int MaxSubSequenceSum1(const int arr[], int n)
{
    int MaxSum = 0;
    for (int i = 0; i < n; i++)
    {
        int ThisSum = 0;
        for (int j = i; j < n; j++)
        {
            ThisSum += arr[j];
            if (ThisSum > MaxSum)
                MaxSum = ThisSum;
        }
    }
    return MaxSum;
}
```

```c++
int MaxSubSequenceSum2(const int arr[], int Left, int Right)
{
    int MaxLeftSum, MaxRightSum;
    int MaxLeftBorderSum, MaxRightBorderSum;
    int LeftBorderSum, RightBorderSum;
    int Center, i;
    if (Left == Right)
        if (arr[Left] > 0)
            return arr[Left];
        else
            return 0;
    Center = (Left + Right) / 2;
    MaxLeftSum = MaxSubSequenceSum2(arr, Left, Center);
    MaxRightSum = MaxSubSequenceSum2(arr, Center + 1, Right);
    MaxLeftBorderSum = 0;
    LeftBorderSum = 0;
    for (i = Center; i >= Left; i--)
    {
        LeftBorderSum += arr[i];
        if (LeftBorderSum > MaxLeftBorderSum)
            MaxLeftBorderSum = LeftBorderSum;
    }
    MaxRightBorderSum = 0;
    RightBorderSum = 0;
    for (i = Center + 1; i <= Right; i++)
    {
        RightBorderSum += arr[i];
        if (RightBorderSum > MaxRightBorderSum)
            MaxRightBorderSum = RightBorderSum;
    }
    if (MaxLeftBorderSum + MaxRightBorderSum > MaxLeftSum && MaxLeftBorderSum + MaxRightBorderSum > MaxRightSum)
        return MaxLeftBorderSum + MaxRightBorderSum;
    else if (MaxLeftSum > MaxLeftBorderSum + MaxRightBorderSum && MaxLeftSum > MaxRightSum)
        return MaxLeftSum;
    else
        return MaxRightSum;
}
```

下面这种算法只对数据进行一次扫描，因此不需要数据被记忆。另外，在任意时刻算法都能对它已经读入的数据给出子序列问题的最佳答案，具有这种特性的算法叫做**联机算法**。仅需要常量空间并以线性时间运行的在线算法几乎是完美的算法。

```c++
int MaxSubSequenceSum3(const int arr[], int n)
{
    int ThisSum, MaxSum, j;
    ThisSum = MaxSum = 0;
    for (j = 0; j < n; j++)
    {
        ThisSum += arr[j];
        if (ThisSum > MaxSum)
            MaxSum = ThisSum;
        else if (ThisSum < 0)
            ThisSum = 0;
    }
    return MaxSum;
}
```

## 对分查找

```c++
int BinarySearch(const int arr[], int x, int n)
{
    int Low, Mid, High;
    Low = 0, High = n - 1;
    while (Low <= High)
    {
        Mid = (Low + High) / 2;
        if (arr[Mid] < x)
            Low = Mid + 1;
        else if (arr[Mid] > x)
            High = Mid - 1;
        else
            return Mid;
    }
    return -1;
}
```

- 二分法的详解与扩展

1. 在一个有序数组种，找某个数是否存在
2. 在一个有序数组中，找大于等于某个数最左侧的位置
3. 局部最小值问题

## 欧几里德算法

```c++
unsigned int Gcd(unsigned int M, unsigned N)
{
    unsigned int Rem;
    while (N > 0)
    {
        Rem = M % N;
        M = N;
        N = Rem;
    }
    return M;
}
```

## 高效率的取幂运算

```c++
long long Pow(long long X, unsigned int N)
{
    if (N == 0)
        return 1;
    else if (N == 1)
        return X;
    else if (N % 2 == 0)
        return Pow(X * X, N / 2);
    else
        return Pow(X * X, N / 2) * X;
}
```



# 第三章 表、栈、队列

## 3.1 线性表

线性表是一个逻辑结构，有两种存储结构——顺序存储和链式存储。

---

### 3.1.1 链表

#### 单链表

相较于顺序表，链式存储线性表时不需要使用地址连续的存储单元。即不要求逻辑上相邻的元素在物理位置上也相邻。插入和删除操作不需要移动元素，只需要修改指针，但也会失去顺序表随机存储的优点。

---

```c++
//LinkList.h
typedef struct node {
	int data;
	node* next;
}Node,*LinkList;

LinkList ListHeadInsert(LinkList&); //头插法
LinkList ListTailInsert(LinkList&); //尾插法
Node* GetElem(LinkList, int); //根据位置获取元素
Node* LocateElem(LinkList, int); //根据元素定位位置
void FrontInsert(LinkList, int, int); //前插
void DeleteElem(LinkList, int); //删除
int GetLength(LinkList); //获取链表长度
```

```c++
//LinkList.cpp
#include<iostream>
#include"ListLink.h"

LinkList ListHeadInsert(LinkList& L)
{
	Node* p;
	int x;
	L = new Node; //创建头节点
	L->next = nullptr;
	std::cin >> x;
	while (std::cin)
	{
		p = new Node;
		p->data = x;
		p->next = L->next;
		L->next = p;
		std::cin >> x;
	}
	return L;
}

LinkList ListTailInsert(LinkList& L)
{
	int x;
	L = new Node;
	L->next = nullptr;
	Node* s = L;
	while (std::cin >> x)
	{
		Node* p = new Node;
		p->data = x;
		s->next = p;
		s = p;
	}
	return L;
}

Node* GetElem(LinkList L, int i)
{
	int j = 1;
	Node* p = L->next;
	if (i == 0) return L;
	if (i < 0) return nullptr;
	while (p && j++ < i)
	{
		p = p->next;
	}
	return p;
}

Node* LocateElem(LinkList L, int x)
{
	Node* p = L->next;
	while (p && p->data != x)
	{
		p = p->next;
	}
	return p;
}

void FrontInsert(LinkList L, int x, int i)
{
	Node* s = new Node;
	s->data = x;
	Node* front = GetElem(L, i - 1);
	s->next = front->next;
	front->next = s;
}

void DeleteElem(LinkList L, int i)
{
	if (i == 0) return;
	if (!GetElem(L, i)) return;
	Node* p = GetElem(L, i - 1);
	Node* q = GetElem(L, i);
	p->next = q->next;
	delete q;
}

int GetLength(LinkList L)
{
	int num = 0;
	Node* p = L->next;
	while (p&&++num) p = p->next;
	return num;
}
```

## 3.2 栈

栈的数学性质：n个不同的元素进栈，出栈元素不同排列的个数为
$$
{1\over{n+1}}{C^n_{2n}}
$$
上述公式称为卡特兰（**Catalan**）数。

---

### 3.2.1 顺序栈

```c++
//SqStack.h
#define MaxSize 50
typedef struct {
	int data[MaxSize];
	int top;
} SqStack;

void InitStack(SqStack&); //初始化栈
bool StackEmpty(SqStack); //判断栈是否为空
void Push(SqStack&, int); //进栈
void Pop(SqStack&); //出栈
int GetTop(SqStack&); //获取栈顶元素
void DestroyStack(SqStack&); //释放栈
```

```c++
//SqStack.cpp
#include"SqStack.h"

void InitStack(SqStack& S)
{
	S.top = -1;
}

bool StackEmpty(SqStack& S)
{
	if (S.top == -1) return true;
	else return false;
}

void Push(SqStack& S, int x)
{
	if (S.top == MaxSize - 1) return;
	S.data[++S.top] = x;
}

void Pop(SqStack& S)
{
	if (StackEmpty(S)) return;
	S.top--;
}

int GetTop(SqStack& S)
{
	return S.data[S.top];
}

void DestroyStack(SqStack& S)
{
	S.top = -1;
}
```

### 3.2.2 共享栈

利用栈底位置相对不变的特性，让两个顺序栈共享一个一维数组空间，将两个栈的栈底分别设置在共享空间的两端，两个栈顶向共享空间的中间延申。

### 3.2.3 链栈（栈的链式存储结构）

**优点**：便于多个栈共享存储空间和提高效率，且不存在栈满的情况，通常用单链表实现，并规定所有操作都是在单链表的表头进行。

---

栈的链式存储结构可描述为：

```c++
typedef struct LinkNode{
    ElemType data;
    struct LinkNode* next;
} *LinkStack;
```

## 3.3 队列

### 3.3.1 队列的顺序存储

顺序存储存在“假溢出”的情况，会造成空间浪费。

#### 循环队列

针对顺序存储“假溢出”的情况，在逻辑上将存储队列元素的表视为一个环。当队首指针**Q.front==MaxSize-1**后，再前进一个位置就自动到0，这可以利用取余来实现。

---

循环队列的判空，**Q.front==Q.rear**，但是无法区分队空还是队满。常用的方法是牺牲一个单元来区分队空和队满，入队时少用一个队列单元，以队头指针在队尾指针的下一位置作为队满的标志。

![循环队列区分队满和队空](img/循环队列区分队满和队空.png)

队满条件：(Q.rear+1)%MaxSize==Q.front

队空条件：Q.front==Q.rear

队列中元素的个数：(Q.rear-Q.front+MaxSize)%MaxSize

类型中增设表示元素个数的数据成员和tag数据成员，tag数据成员用于区分是队满还是队空。若因删除导致Q.front==Q.rear，则为队空；tag=1时，若因插入导致Q.front==Q.rear，则为队满。

---

```c++
//SqQueue.h
#define MaxSize 50

typedef struct {
	int data[MaxSize];
	int front, rear;
}SqQueue;

void InitQueue(SqQueue&); //初始化队列
bool IsEmpty(const SqQueue&); //判断队列是否为空
void EnQueue(SqQueue&, const int); //进队
int DeQueue(SqQueue&); //出队，并返回出队元素
int GetHead(SqQueue&); //获取对头元素
```

```c++
//SqQueue.cpp
#include<iostream>
#include"SqQueue.h"

void InitQueue(SqQueue& Q)
{
	Q.rear = Q.front = 0;
}

bool IsEmpty(const SqQueue& Q)
{
	if (Q.rear == Q.front) return true;
	return false;
}

void EnQueue(SqQueue& Q, const int x)
{
	if ((Q.rear + 1) % MaxSize == Q.front) return;
	Q.data[Q.rear] = x;
	Q.rear = (Q.rear + 1) % MaxSize;
}

int DeQueue(SqQueue& Q)
{
	if (Q.rear == Q.front) return false;
	int x = Q.data[Q.front];
	Q.front = (Q.front + 1) % MaxSize;
	return x;
}

int GetHead(SqQueue& Q)
{
	if (Q.rear == Q.front) std::exit(-1);
	return Q.data[Q.front];
}
```

### 3.3.2 队列的链式存储

实质是一个同时带有队头指针和队尾指针的单链表。头指针指向队头结点，尾指针指向队尾结点。

链式存储结构可描述为：

```c++
//LinkQueue.h
typedef struct LinkNode { //链式队列结点
	int data;
	struct LinkNode* next;
}LinkNode;

typedef struct { //链式队列，头出尾进
	LinkNode* front, * rear;
}LinkQueue;
```

---

通常将链式队列设计成一个带头结点的单链表，这样进队和出队的操作就统一了。另外，假如要使用多个队列最好使用链式队列。

---

```c++
//LinkQueue.h
void InitQueue(LinkQueue&); //队列初始化
bool IsEmpty(LinkQueue&); //判空
void EnQueue(LinkQueue&, int); //进队
bool DeQueue(LinkQueue&, int&); //出队，并返回出队元素
```

```c++
//LinkQueue.cpp
#include"LinkQueue.h"
void InitQueue(LinkQueue& Q)
{
	Q.front = Q.rear = new LinkNode;
	Q.front->next = nullptr;
}

bool IsEmpty(LinkQueue& Q)
{
	if (Q.front == Q.rear) return true;
	return false;
}

void EnQueue(LinkQueue& Q, int x)
{
	LinkNode* p = new LinkNode;
	p->data = x;
	Q.rear->next = p;
	Q.rear = p;
}

bool DeQueue(LinkQueue& Q, int& x)
{
	if (Q.front == Q.rear) return false;
	LinkNode* p = Q.front->next;
	x = p->data;
	Q.front->next = p->next;
	if (Q.rear == p) Q.rear = Q.front;
	delete p;
	return true;
}
```

## 3.4 栈和队列的应用



## 3.5特殊矩阵

特殊矩阵的压缩存储方法：找出特殊矩阵中值相同的矩阵元素的分布规律，把那些呈现规律性分布的、值相同的多个矩阵元素压缩存储到一个存储空间中。

---

### 3.5.1 对称矩阵

元素下标之间的对应关系：
$$
下三角区和主对角线:k={i(i-1)\over2}+j-1
$$

$$
上三角区：k={j(j-1)\over2}+i-1
$$

### 3.5.2 三角矩阵

下三角矩阵元素下标之间的对应关系：
$$
下三角区和主对角线：k={i(i-1)\over2}+j-1
$$

$$
上三角区：{k=n(n+1)\over2}
$$

上三角矩阵元素下标之间的对应关系：
$$
上三角区和主对角线：k={(i-1)(2n-i+2)\over2}+(j-i)
$$

$$
下三角区：k={n(n+1)\over2}
$$

### 3.5.3 三对角矩阵





# 位运算

## 异或

面试题例：

1. 一个数组中某个数字出现了奇数次，其他数字出现了偶数次，求出现了奇数次的数字
2. 一个数组中某两个数字出现了奇数次，其他数字出现了偶数次，求出现了奇数次的数字

---

```c++
//1.
int func(int* arr, int n)
{
	int eor = 0;
	for (int i = 0; i < n; i++)
		eor ^= arr[i];
	return eor;
}
```

```c++
//2.
void func(int* arr, int n)
{
	int eor = 0;
	for (int i = 0; i < n; i++)
		eor ^= arr[i]; //a^b
	int rightone = eor & (~eor + 1); //最右边的1
	int onlyone = 0; //eor' = a or b
	for (int i = 0; i < n; i++)
		if ((arr[i] & onlyone) == 1) onlyone ^= arr[i];
	cout << "one number = " << onlyone + 1 << endl;
	cout << "another number = " << (onlyone ^ eor) + 1 << endl; ;
}
```

- 注：提取出某个不为0的数最右边的1：

```c++
int rightone = eor & (~eor + 1);
```

---
