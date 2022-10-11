# 第五章 循环和关系表达式

## 5.1 for循环

### 5.1.11 其它语法技巧——逗号运算符

逗号运算符允许将两个表达式放到C++句法只允许放一个表达式的地方。例如：

```c++
++j,--i;
```

C++规定，逗号表达式的值是第二部分的值，并且逗号运算符的优先级最低，例如：

```c++
cata = 17,240;
(cats = 17) , 240;
```

第一行cata的值为17，第二行cats值为17，240不起作用。

### 5.1.14 C风格字符串的比较

```c++
char word[4]="mate";
word="mate";
```

数组名是地址，同样，用括号括起的字符串常量也是其地址，因此上面的关系表达式不是判断两个字符串是否相同，而是查看他们是否存储在相同的地址上。C++将C风格字符串视为地址。若要测试C风格字符串是否相等，可以使用strcmp()函数

### 5.1.15 比较string类字符串

string类设计能够使用关系运算符进行比较，这是因为类函数重载了这些运算符。

## 5.2 while循环

### 5.2.2 等待一段时间：编写延时循环

C++库和ANSI C库中有一个函数clock()返回程序开始执行后所用的系统时间。首先，clock()返回时间的单位不一定是秒；其次，该函数的返回类型在不同系统上可能不一样。头文件ctime（time.h）定义了一个符号常量——CLOCK_PER_SEC，该常量等于每秒钟包含的系统时间单位数。其次，ctime将clock_t作为clock()返回类型的别名，这意味着可以将变量声明为clock_t类型，编译器将自动将其转换为适合系统的其他类型。下面的程序演示了如何创建延迟循环：

```c++
#include<iostream>
#include<time.h>
int main()
{
    using namespace std;
    float secs;
    cin>>secs;
    clock_t delay = secs * CLOCK_PER_SEC;
    clock_t start = clock();
    while(clock()-start<delay);
    cout<<"done\n";
    return 0;
}
```

C++为类型建立别名的方式有两种，一种是使用预处理器，第二种是使用C++的关键字typedef

```c++
#define BYTE char
typedef typename aliasName;
```

## 5.4 基于范围的for循环

对数组（或容器类）的每个元素执行相同的操作

```c++
double prices[5] = {4.99,10.99,6.87,7.99,8.49};
for(double x : prices)
    cout<< x << std::endl;
```

还可以结合初始化列表使用

```c++
for(int x : {3,5,2,8,6})
    cout<< x <<" ";
cout<<endl;
```

## 5.5 循环和文本输入

利用while循环来逐字符地读取来自文件或键盘的文本。cin对象支持3种不同模式的单字符输入，下面介绍在while循环中使用者三种模式。

### 5.5.1 使用原始的cin输入

利用**哨兵字符**来标记停止点。

```c++
#include<iostream>
int main() {
    using namespace std;
    char ch;
    int count = 0;
    cin >> ch;
    //将 # 作为哨兵字符
    while (ch != '#') {
        cout << "输入的字符为:" << ch << endl;
        count++;
        cin >> ch;
    }
    cout << endl << count << endl;
    return 0;
}
```

如果在程序中输入空格，则程序会在输出时省略空格。原因在于cin读取char值时会忽略空格和换行符。因此空格不会被回显，也不会被计数。更复杂的是，发送给cin的输入被缓冲，只有在回车键之后才会被发送给程序，因此可以在#后面输入字符。但程序遇到#字符后会结束对输入的处理。

### 5.5.2 使用cin.get(char)弥补

若想检查包括空格、制表符和换行符之类的字符。cin.get(ch)函数可以读取，并将其赋值给变量ch。

```c++
#include<iostream>
int main()
{
    using namespace std;
    char ch;
    int count = 0;
    cin.get(ch);
    while(ch!='#')
    {
        cout<<ch<<endl;
        count++;
        cin.get(ch);
    }
    cout<<endl<<count<<endl;
    return 0;
}
```

在C语言中，修改变量的值必须把变量的地址传递给函数，但cin.get(ch)传递的是ch，而不是&ch。C++在C的基础上增加了引用这一新的类型。头文件iostream将cin.get(ch)中的参数声明为引用类型。因此可以修改变量的值。

### 5.5.3 三种版本的cin.get()

```c++
cin.get(name,ArSize);
cin.get(char);
cin.get();
```

第一个版本接受两个参数：数组名（字符串的地址）和ArSize（int类型的整数）。第二个版本接受一个char参数。第三个版本不接受任何参数。这就是被称为函数重载的OOP特性。

```c++
cin.get(name,ArSize).get()
```

### 5.5.4 文件尾条件 *

检测文件尾（EOF）将文件尾这种信息告知程序。

## 5.6 嵌套循环与二维数组

### 5.6.1二维数组的声明与初始化

一维数组的声明与初始化

```c++
int arr[5]={1,2,3,4,5};
```

二维数组的声明与初始化（4行5列的二维数组）

```c++
int maxtemps[4][5]=
{
    {1,2,3,4,5},
    {6,7,8,9,10},
    {11,12,13,14,15},
    {16,17,18,19,20}
}
```

### 5.6.2 使用嵌套循环输出二维数组

```c++
for(int row=0;row<4;row++)
{
    for(int col=0;col<5;col++)
    {
        cout<<maxtemps[row][col]<<"\t";
    }
    cout<<endl;
}
```

# 第六章 分支语句和逻辑运算符

## 6.2 逻辑表达式

### 6.2.1 逻辑OR运算符：||

C++规定，||运算符是个顺序点。如果左侧的表达式为true，则C++不会去判定右侧的表达式。只要有一个表达式为true，则整个逻辑表达式为true。（冒号和逗号运算符也是顺序点）。

### 6.2.2 逻辑AND运算符：&&

仅当两个表达式都为true时，得到的表达式的值才为true。和||运算符一样，&&运算符也是一个顺序点。因此将首先判定左侧，并且在右侧被判定之前产生所有的副作用。如果左侧为false，则不必计算右侧，整个逻辑表达式必定为false。

### 6.2.4 逻辑NOT运算符：！

！运算符将它后面的表达式的真值取反。下面的程序来筛选可赋给int变量的数字输入。

```c++
#include<iostream>
#include<climits>
bool is_int(double);
int main()
{
    using namespace std;
    double num;
    cin>>num;
    while(!is_int(num))
    {
        cout<<"Out of range--plz try again:";
        cin>>num;
    }
    int val= int (num);
    cout<<"You've enteren the int:"<<val<<endl;
    return 0;
}
bool is_int(double x)
{
    if(x<=INT_MAX && x>+INT_MIN)
        return true;
    else
        return false;
}
```

如果给读取int值得程序输入一个过大值，很多C++实现会将这个值截短为合适的大小，这就导致可能丢失数据。故用double类型，精度足以存储典型的int值且取值范围更大。 也可以使用 long long类型。布尔函数is_int()使用了climits文件中定义的两个符号常量INT_MAX和INT_MIN来确定参数是否位于适当的范围内。

### 6.2.5 逻辑运算符细节

OR和AND运算符的优先级都低于关系运算符，但是！运算符的优先级高于所有的关系运算符和算术运算符。因此要对表达式求反必须用括号括起来。AND运算符优先级高于OR运算符。C++确保程序从左向右进行逻辑表达式，并在知道答案后立刻停止。

## 6.3 字符函数库cctype

**提供了一组方便强大的工具分析字符输入**

例如：如果ch是一个字母，则isalpha(ch)函数返回一个非零值，否则返回0。同样，如果ch是标点符号，函数ispunct(ch)将返回true。这些函数的返回类型为int。使用这些函数比使用AND和OR运算符更方便。

- isalpha() 是否为字母字符

- isdigits() 是否为数字字符

- isspace() 是否为空白

- ispunct() 是否为标点符号

- isalnum() 是否为字母或数字

- islower() 是否为小写字母

- isupper() 是否为大写字母

- tolower() 如果参数为大写字符，则返回其小写，否则返回该参数

- toupper() 如果参数为小写字符，则返回其大写，否则返回该参数

## 6.4 ?:运算符

条件运算符。格式为：exp1?exp2:exp3

如果exp1为true，则返回exp2.否则返回exp3。

条件表达式与if else的区别在于条件运算符生成一个表达式，因此是一个值，可以赋给变量或放到一个更大的表达式中。

## 6.5 switch语句

switch中的integer-expresssion必须是一个结果为整数值的表达式。另外每个标签都必须是整数常量表达式。标签可以是int或char常量，也可以是枚举量。C++中的case标签只是行标签，而不是选项之间的界限。要让程序执行完特定一组语句后停止必须使用break语句。如果所有选项都可以使用整型常量来标识，则可以使用switch或if else语句。就代码长度和执行速度而言，switch语句的效率更高。 

## 6.7 读取数字的循环 *

## 6.8 简单文件I/O

### 6.8.2 写入

1. 包含头文件 fstream

2. 创建一个ofstream对象，还需要自己命名

3. 将该ofstream对象同一个文件关联起来，使用open()

4. 像使用cout那样使用该ofstream对象

5. 使用完后要关闭close()

### 6.8.3 读取 *

同写入差不多。使用get()方法来读取一个字符，使用getlineI()方法读取一行字符。

可以结合使用eof()、fail()方法判断输入是否成功。

当ifstream对象本身被用作test-exp时，如果最后一个读取操作成功，它将被转换为bool值true。

检查文件是否被成功打开可以使用方法is_open()，可以使用下面的代码：

```c++
inFile.open("bowling.txt");
if(!inFile.is_open()) exit(EXIT_FAILURE);
```

函数exit()原型在头文件cstdlib中定义，EXIT_FAILURE为该头文件中定义的用于操作系统通信的参数值。exit()终止程序。

# 第七章 函数——C++的编程模块

## 7.1 复习函数的基本知识

C++对于返回值的类型有一定的限制，不能是数组。如果想要返回一个数组，做法是返回一个指向数组的指针。

C++的编程风格是将main()放在最前面，因此通常要在此之前声明函数原型。函数原型是一条语句，因此必须以分号结束。函数原型不需要提供变量名，有类型列表就足够了。原型的参数列表中可以包括变量名，也可以不包括，原型中的变量名相当于占位符，不必与函数定义中的变量名相同。一个函数原型如：

```c++
double cube(int);
```

在编译阶段进行的原型化被称为**静态类型检查**，可以捕获许多在运行阶段难以捕获的错误。

## 7.2 函数参数和按值传递

```c++
double cube(double x)
```

函数在传递参数时会将原来的数据复制一份，这不会影响到main()中的数据，例如上面的代码中cube函数会创建一个新的名为x的double变量然后将其初始化。用于接受传递值的变量称为形参（parameter），传递给函数的值称为实参（argument）。在函数中修改形参的值不会影响调用程序中的数据。

在函数中声明的变量（包括参数）是函数私有的，这样的变量被称为局部变量。函数调用时分配，结束时释放。形参与函数中声明的局部变量的区别在于：形参从调用函数处获得自己的值，而其它变量从函数中获得自己的值。

## 7.3 函数和数组

### 7.3.2 将数组作为参数意味着什么

将数组和数组长度传递给函数参数时，实际上是吧数组的位置（指针）、包含的元素种类（类型）以及元素数目（n变量）提交给函数。传递常规变量时，函数将使用该变量的拷贝；但传递数组时将使用原来的数组，这意味着函数将使用原始数据，这一操作有利也有弊。但是，可以用const限定符来消除其弊端。

使用sizeof()可以获得整个数组的长度，但是对函数参数中的数组形参使用sizeof()只能获得指针变量的长度。这就是必须显式传递数组长度的原因——指针本身并没有指出数组的长度。

为将数组类型和元素数量告诉数组处理函数，必须通过两个不同的参数传递它们，同时，这两个参数也是可以根据自身需要修改的。第二个参数可以不是数组长度，而是要读取的最大元素数。

### 7.3.4 使用数组区间的函数

向函数参数传递数组时还可以通过传递两个指针来完成，一个指针标识数组的开头，另一个指针标识数组的尾部（STL使用**超尾**来标识数组的尾部）也就是说，对于数组而言标识数组结尾的参数是指向最后一个元素后面的指针，例：

```c++
#include<iostream>
const int ArSize=8;
int sum_arr(const int* begin,const int* end);
int main()
{
    using namespace std;
    int cookies(ArSize)={1,2,3,4,5,6,7,8};
    int sum=sun_arr(cookies,cookies+ArSize);
    sum=sum_arr(cookies,cookies+3);
    sum=sum_arr(cookies+4,cookies+8);
    return 0;
}
int sum_arr(const int*begin,const int*end)
{
    const int* pt;
    int total=0;
    for (pt=begin,pt!=end;pt++) total=total+*pt;
    return total;
}
```

根据指针减法规则，在sum_arr()中，表达式end-begin是一个整数值，等于数组的元素数目。

### 7.3.5 指针和const

const用于指针有两种方式，第一种是让指针指向一个常量对象，防止使用该指针来修改所指向的值，第二种是将指针本身声明为常量，防止改变指针指向的位置。

第一种：

```c++
int age=39;
const int* pt = &age;
```

这里pt的声明并不意味着它指向的值是一个常量，而只是意味着对pt而言这个值是常量。可以通过age变量来修改age的值，但不能用pt指针来修改它。但是反过来，C++禁止将const的地址赋给非const指针。例如下面这种情况是非法的：

```c++
const float g_earth = 9.80;
float* pm = &g_earth;
```

假设有一个由const数据组成的数组，则禁止将常量数组的地址赋给非常量指针，这说明不能将数组名作为参数传递给使用非常量形参的函数。因此，在声明指针参数时应尽量将其声明为指向常量数据的指针，理由如下：

1. 可以避免无意间修改数据

2. 使用const使得函数能处理const和非const实参，否则只能接受非const数据

第二种：

```c++
int age = 39;
int* const ps = &age;
```

该声明不允许修改ps指针的值。

## 7.4 函数与二维数组
