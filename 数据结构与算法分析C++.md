# 第二章 算法分析

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

## 单链表

```c++
//LinkList.h
typedef struct node {
	int data;
	node* next;
}NODE;

class LinkList
{
private:
	NODE* head;
public:
	LinkList() { head = nullptr; }
	~LinkList();
	int LengthList() const; //长度
	void InsertList(int,int); //在指定位置后面插入指定值
	bool IsEmpyt() const; //判断为空
	int GetElemList(int loc) const; //查找loc处的值
	void DeleteList(int loc); //删除loc处的结点
	void AddList(int); //在表头增加结点
	void ShowList(); //遍历链表
	void ClearList(); //清空链表
};
```

```c++
//LinkList.cpp
#include"LinkList.h"
#include<iostream>
using std::cout;
using std::endl;

LinkList::~LinkList()
{
	NODE* p = head;
	while (p)
	{
		head = head->next;
		delete p;
		p = head;
	}
	cout << "析构函数调用" << endl;
}

int LinkList::LengthList() const
{
	int len = 0;
	NODE* p = head;
	while (p)
	{
		len++;
		p = p->next;
	}
	return len;
}

void LinkList::InsertList(int x, int loc)
{
	NODE* p = new NODE;
	p->data = x;
	if (loc > LengthList())
	{
		cout << "out of range" << endl;
		return;
	}
	NODE* current = head;
	while (--loc)
	{
		current = current->next;
	}
	p->next = current->next;
	current->next=p;
}

bool LinkList::IsEmpyt() const
{
	return head == nullptr;
}

int LinkList::GetElemList(int loc) const
{
	if (loc > LengthList())
	{
		cout << "out of range ";
		return -1 ;
	};
	NODE* p = head;
	while (--loc)
	{
		p = p->next;
	}
	return p->data;
}

void LinkList::DeleteList(int loc)
{
	if (loc > LengthList())
	{
		cout << "out of range" << endl;
		return;
	}
	if (loc == 1)
	{
		NODE* tmp = head;
		head = head->next;
		delete tmp;
		return;
	}
	NODE* current = head->next;
	NODE* prev = head;
	while (loc - 2 && --loc)
	{
		current = current->next;
		prev = prev->next;
	}
	prev->next = current->next;
	delete current;
	return;
}

void LinkList::AddList(int x)
{
	NODE* p = new NODE;
	p->data = x;
	p->next = head;
	head = p;
}

void LinkList::ShowList()
{
	NODE* p = head;
	while (p)
	{
		cout << p->data << " ";
		p = p->next;
	}
	cout << endl;

}

void LinkList::ClearList()
{
	NODE* p = head;
	while (p)
	{
		head = head->next;
		delete p;
		p = head;
	}
}
```

## 双链表

## 循环链表

# 第七章 排序

## 插入排序

### 算法实现

```c++
void InsertSort(int arr[],int n)
{
    int j,p;
    int tmp;
    for(p=1;p<n;p++)
    {
        tmp=arr[p];
        for(j=p;j>0&&arr[j-1]>tmp;j--) arr[j]=arr[j-1];
        arr[j]=tmp;
    }
}
```

---

### 算法分析

插入排序的平均情形也是O(N^2)。

插入排序的运行时间是O(I+N)，其中I为原始数组中的逆序数。

定理：

N个互异数的数组的平均逆序数是N(N-1)/4。

## 希尔排序（缩小增量排序）

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
