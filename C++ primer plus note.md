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

当且仅当声明函数的形参时，下面两个声明等价，arr都是指向typename的指针：

```c++
typeName arr[];
typeName *arr;
```

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

若函数参数为一个二维数组，则可以像下面这样声明：

```c++
int data[3][4] = {{1,2,3,4},{9,8,7,6},{2,4,6,8}};
int sum(int (*ar2)[4], int size); //表示由4个int组成的数组的指针
int sum(int *ar2[4], int size); //表示一个由4个指向int的指针组成的数组
int sum(int ar2[][4], int size); //第2行的另一种写法，可读性更强
```

上述代码在声明参数ar2时没有使用const，这是因为这种技术只能用于指向基本类型的指针，而ar2是指向指针的指针。

## 7.6 函数和结构

可以像处理基本类型那样来处理结构。然而，按值传递结构有一个缺点。如果结构非常大，则复制结构将增加内存要求，降低系统运行速度。因此可以传递指针的地址或者按引用传递。

### 7.6.1 传递和返回结构

结构比较小时，按值传递是最合理的。

### 7.6.2 另一个处理结构的函数示例

数学库（cmath）中的atan2()函数可以根据x，y的值计算角度

```c++
#include<cmath>
angle = atan2(y,x)
```

## 7.7 函数和string对象

如果用一个数组存储string对象，用cin给每一个数组元素（string对象）赋值，可以使用getline()函数，如下：

```c++
for(int i = 0; i < SIZE; i++) getline(cin,list[i]);
```

## 7.8 函数与array对象

要使用array类要包含头文件array，名称array位于命名空间std中。初始化一个array对象：

```c++
std::array<double,4>expenses;
```

如何在函数原型中声明这种对象：

```c++
void show(std::array<double,4>da); //da是一个对象
void fill(std::array<double,4>*pa); //pa是一个指针
```

模板array并非只能存储基本数据类型，还可以存储类对象。

## 7.10 函数指针

利用函数地址调用函数允许在不同的时间传递不同函数的地址，这意味着可以在不同的时间使用不同的函数。

### 7.10.1 函数指针的基础知识

使用函数名来获取函数的地址。将函数作为参数传递要区分传递的是函数的地址还是函数的返回值。

函数指针的声明应指定函数的返回类型以及函数的特征标（参数列表）。如下：

```c++
double pam(int); //函数声明
double (*pf)(int); //函数指针声明，pf为指针
pf=pam; //函数指针初始化，将pam赋给pf
```

在程序中通过指针调用函数有两种方式

```c++
double y = (*pf)(5);
double y = pf(5);
```

### 7.10.3 深入探讨函数指针

如何声明一个数组（比如含有3个元素），数组中每个元素都是一个函数指针，如下：

```c++
const double *(*pt[3])(const double*, int) = {f1, f2, f3};
```

pt是一个包含3个指针的数组，其中每个指针都指向这样的函数：将const double* 和int作为参数，并返回一个const double。

使用auto自动类型推导，将\*pt赋给pb来声明相同类型的数组：

```c++
auto pb=pt;
```

因为数组名是指向第一个元素的指针，因此pb和pt都是指向函数指针的指针。

由于pt是指向函数指针的指针（数组），因此可以创建一个指向整个数组的指针，即它指向指针的指针。可用自动类型推导：

```c++
auto pa=&pt;
const double *(*(*pt)[3])(const double *, int) = &pt //自己声明
```

注意：pt是数组第一个元素（函数指针）的地址，而&pt是整个数组（3个指针块）的地址。若要得到第一个元素的值，只需要对pt解除一次引用，而对&pt要解除两次引用。

# 第八章 函数探幽

## 8.1 C++内联函数

内联函数相较于常规函数减少了函数调用这一过程，而是将内联函数的编译代码与其他程序代码内联起来。但是会占用更多的内存。如果调用次数很多，则程序中含有的函数的副本相应的也有很多个。因此只有在**函数很短时**才能采用内联方式做法是在函数定义前加上关键字**inline**。通常的做法是省略原型，将整个定义放在原型所在位置。内联函数的值传递与常规函数无区别。

- 注意：内联函数不能调用自身，也就是不能用于递归。

## 8.2 引用变量

### 8.2.5 将引用用于类对象

可以将C风格字符串用作string对象引用参数。如形参为 const string ， 实参为char\*。

### 8.2.3 引用的属性和特别之处

右值引用 **&&**，实现移动语义。

```c++
double && rref =std::sqrt(36.00);
double j = 15.0;
double & jref = 2.0* j + 18.5;
std::cout<<rref<<'\n'; //6.0
std::cout<<jref<<'\n'; //48.5
```

### 8.2.6 对象、继承和引用

继承：使得能够将特性从一个类传递给另一个类的语言特性被称为继承。

继承的另一个特征是，基类引用可以指向派生类对象，而无需进行强制类型转换。例如参数类型为 ostream& 的函数可以接受 ostream 对象（如cout）或 ofstream 对象作为参数。

### 8.2.7 何时使用引用参数

- 需要修改调用函数中的数据对象。

- 通过传递引用而不是整个数据对象，可以提高程序的运行速度。

**选择参数传递方法的一些指导原则：**

- 如果数据对象很小，如内置数据类型或小型结构，则按值传递

- 如果数据对象是数组，则使用指针，并将指针声明为指向const的指针

- 如果数据对象是较大的结构，则使用const指针或const引用，以提高程序的运行效率

- 如果数据对象是类对象，则使用const引用。类设计的语义常常要求使用引用

- 如果数据对象是内置数据类型，则使用指针

- 如果数据对象是数组，则只能使用指针

- 如果数据对象是结构，则使用引用或指针

- 如果数据对象是类对象，则使用引用

## 8.4 函数重载

函数重载的关键是函数的参数列表——也称为函数特征标。

一些看起来彼此不同的特征标是不能共存的，例如

```c++
double cube(double x);
double cube(double &x);
```

编译器在检查函数特征标时，将类型引用和类型本身视为同一个特征标。

是特征标，而不是函数类型使得可以对函数进行重载，例如，下面两个声明是互斥的：

```c++
long gronk(int n, float m);
double gronk(int n, float m);
```

返回类型可以不同，但特征标也必须不同。 

## 8.5 函数模板

需要多个将同一算法用于不同类型的函数时就可以使用模板。模板关键词：

```c++
template<typename T>
```

- 注：函数模板不能缩短可执行程序。最终的代码不包含任何模板，而只包含了为程序生成的实际函数。使用模板的好处在于使生成多个函数定义更简单可靠。更常见的情形是，**将模板放在头文件中，并在需要使用模板的文件中包含头文件**。

### 8.5.1 重载的模板

和常规重载一样，被重载的模板的函数特征标必须不同。如：

```c++
template<typename T>
void Swap(T &a, T &b);

template<typename T>
void Swap(T *a, T *b, int n);
```

由第二个模板函数的原型可以看出，并非所有的模板参数都必须是模板参数类型。

### 8.5.2 模板的局限性

函数模板可能无法处理某些类型。例如在函数体中有a=b这一操作，如果T为数组，则这行代码将不能实现。对于这些特殊类型，有两种解决方案，第一种是重载运算符，第二种是为特定类型提供具体化的模板定义。

### 8.5.3 显式具体化

关于具体化机制的C++标准定义：

- 对于给定的函数名，可以有非模板函数、模板函数和显式具体化模板函数以及它们的重载版本

- 显式具体化的原型和定义应以template<>打头，并通过名称来指出类型

- 显式具体化优先于常规模板，而非模板函数优先于具体化和常规模板

假设有一个名为job的结构，下面是用于交换job结构的非模板函数、模板函数和显式具体化的原型：

```c++
void Swap(job &a, job &b);

template<typename T>
void Swap(T &, T &);

template<> void Swap<job>(job &, job &); //<job>是可选的
//等价于 template<> void Swap(job &, job &);
```

### 8.5.4 实例化和具体化

在代码中包含函数模板本身并不会生成函数定义，模板只是用于生成函数定义的方案。编译器使用模板为特定类型生成函数定义时，得到的是模板实例，这种实例化方式被称为隐式实例化。显示实例化例子如下：

```c++
template void Swap<int>(int, int); //使用Swap()模板生成一个使用int类型的实例。
```

显式具体化使用下面两个等价的声明之一：

```c++
template<> void Swap<int>(int &, int &);
template<> void Swap(int &, int &);
```

与显式实例化不同在于显式具体化必须有自己的函数定义，即不使用Swap()模板来生成函数定义。显示具体化声明在关键词template后包含<>，而显式实例化没有。在同一文件中不能同时使用一种类型的显式实例化和显式具体化。

# 第九章 内存模型和名称空间

## 9.1 单独编译

C++鼓励将组件函数放在独立的文件中，可以单独编译这些文件，然后将它们链接成可执行程序。一个程序可以分为3个部分：

- 头文件：包含结构声明和使用这些结构的函数的原型。

- 源代码文件：包含与结构有关的函数的代码。

- 源代码文件：包含调用与结构相关的函数的代码。

---

**头文件**中常包含的内容：

- 函数原型

- 使用#define或const定义的符号常量

- 结构声明

- 类声明

- 模板声明

- 内联函数

被声明为const的数据和内联函数有特殊的链接属性，因此可以将其放在头文件中。

---

包含头文件时有两种包含方式，一种是用引号“ ”，一种是用尖括号<>，区别在于：

- 如果使用尖括号，编译器将在存储标准头文件的主机系统的文件系统中查找。

- 如果包含在双引号中，则编译器首先查找当前的工作目录或源代码目录。没找到头文件时才在标准位置查找。
  
  ---

启动组合程序的步骤——使用能够创建项目并将其与源代码文件关联起来的菜单。**头文件不用加入到项目中去**，因为可以用#include指令管理头文件，

---

**头文件管理**：在无意中可能将头文件包含多次。下面的指令意味着仅当没有使用预处理器编译指令#define定义名称COORDIN_H_时才处理#ifndef和#endif之间的语句，可以避免多次包含同一个头文件：

```c++
#ifndef COORDIN_H_
···
···
#endif
```

---

**多个库的链接**：名称修饰的存在使得不同编译器创建的二进制模块可能无法正确链接。因此在链接编译模块时要确保所有对象文件或库都由同一个编译器生成。

## 9.2 存储持续性、作用域和链接性

C++存储数据的方案：

- 自动存储持续性：函数中定义的变量（包括函数参数）

- 静态存储持续性：函数外定义和使用关键字static定义的变量

- 线程存储持续性：并行编程的内容，不讨论

- 动态存储持续性（也被称为自由存储或堆）

### 9.2.1 作用域和链接

作用域描述了名称在翻译单元的多大范围内可见，分为全局和局部

链接性描述了名称如何在不同单元共享，链接性分为外部和内部

### 9.2.2 自动存储持续性

默认情况下，函数中声明的函数参数和变量的存储持续性为自动，作用域为局部，没有链接性。如果在代码块中定义了变量，则该变量的存在时间和作用域都被限制在该代码块中。如果内外部代码块存在同名变量，则程序执行到内部代码块中的该变量时，将用新的定义隐藏旧的定义，当内部代码块结束时，旧的定义又重新可见。

### 9.2.3 静态持续变量

C++为静态存储持续性的变量提供了3中链接性：

- 外部链接性（可在其他文件中访问）：在代码块的外部声明；

- 内部链接性（只能在当前文件中访问）：在代码块的外部声明，并使用static限定符；

- 无链接性（只能在当前函数或代码块中访问）；在代码块内声明并使用static限定符；

**这三种链接性都在程序执行期间存在。** 下面为实例：

```c++
int global = 1000; //外部链接性
static int one file = 50; //内部链接性
int main()
{

}
void funct1(int n)
{
    static int count = 0; //无链接性
    int llama = 0; //自动变量
}
void funct2(int q)
{

}
```

所有的静态持续变量都有下述初始化特征：未被初始化的静态变量的所有位都被设置为0.这种变量被称为零初始化的。

### 9.2.4 静态持续性、外部链接性

**单定义规则：** 变量只能有一次定义。C++有两种变量声明，一种是定义声明（简称为定义），另一种为引用声明（引用声明使用关键字**extern**，不进行初始化，否则，声明为定义，导致分配内存空间）。如果要在多个文件中使用外部变量，只需在一个文件中包含该变量的定义（单定义规则），但在使用该变量的其他所有文件中都必须使用关键字extern声明。

**注意：** 单定义规则并非意味着不能有多个变量的名称相同。例如，不同函数中声明的同名自动变量是彼此独立的。另外，局部变量可能隐藏同名的全局变量。

在函数中使用关键字extern声明全局变量（外部变量）的作用是通过该名称使用在外部定义的变量。另外，该声明是可选的。

在必要的时候要区分全局变量和局部变量，因为若要保持数据的完整性，应尽量避免对数据进行不必要的访问。外部存储尤其适合表示常量数据，因为可以使用关键字const来防止数据被修改。

### 9.2.5 静态持续性、内部链接性

将static限定符用于作用域为整个文件的变量时，该变量的链接性为内部，意味着该变量只能在所属的文件中使用。如果文件定义了一个静态外部变量（使用static，链接性为内部），其名称与另一个文件中声明的常规外部变量相同，则在该文件中，静态变量将隐藏常规外部变量。

### 9.2.6 静态存储持续性、无链接性

无链接性的局部变量虽然只在代码块中可用，但它在代码块不处于活动状态时仍然存在。因此在两次函数调用之间，静态局部变量的值将保持不变（再生）。示例代码如下：

```c++
#include<iostream>
const int ArSize = 10;
void strcount(const char * str);

int main()
{
    using namespace std;
    char input[ArSize];
    char next;
    cout<<"Enter a line:\n";
    cin.get(input,ArSize);
    while(cin)
    {
        cin.get(next);
        while(next!='\n') cin.get(next);
        strcount(input);
        cout<<"Enter next line(empty line to quit):\n";
        cin.get(input,ArSize);
    }
    cout << "Bye\n";
    return 0;
}

void strcount(const char * str)
{
    using namespace std;
    static int total = 0;
    int count = 0;
    cout<<"str contains ";
    while(*str++) count++;
    total += count;
    cout << count << " characters\n";
    cout << total << " characters total\n";
}
```

该程序演示了静态局部变量的**再生**特性，还演示了一种处理行输入可能长于目标数组的方法。方法cin.get(input,ArSize)将一直读取输入，直到到达了行尾或读取了ArSize-1个字符为止。它把换行符留在输入队列中。该程序使用cin.get(next)读取行输入后的字符。如果next是换行符，则说明cin.get(input,ArSize)读取了整行；否则说明还有字符没有读取。随后，程序使用一个循环来丢弃余下的字符。可以修改代码使程序读取行中余下的字符。该程序还使用了这样一个事实：用get(char*,int)来读取空行将导致cin为false。

### 9.2.7 说明符和限定符

#### mutable

声明const结构或类中的某个成员可以被修改。

#### 再谈const

默认情况下全局变量链接性为外部的，但const全局变量的链接性为内部的，全局const定义就像使用了static说明符一样。这种规则是为了避免在多个源文件中使用同一头文件中定义的全局常量时违背单定义规则。因为链接性为内部，所以可以在所有文件中使用相同的声明。内部链接性还意味着每个文件都有自己的一组常量，而不是所有文件共享一组常量，每个定义都是其所属文件私有的。如果希望某个常量的链接性为外部的，则可以使用extern来覆盖默认的内部链接性：

```c++
extern const int n = 50;
```

在函数或代码块中声明const时，其作用域为代码块。

### 9.2.8 函数和链接性

所有函数的存储持续性都自动为静态的，即在整个程序执行期间都一直存在。默认情况下，函数的链接性为外部的，即可以在文件间共享。可以使用关键字static将函数的链接性设置为内部的。在定义静态函数的文件中，静态函数将覆盖外部定义，因此即使在外部定义了同名的函数，该文件仍将使用静态函数。

### 9.2.10 存储方案和动态分配

例：

```c++
float *pt = new float[20];
```

上述语句使用new分配的80个字节（假设float为4个字节）的内存将一直保存在内存中，直到使用delete释放内存，但当包含该声明的语句块执行完毕时，pt指针将消失，但指针指向的内容将不会消失。

#### 使用new运算符初始化

```c++
int *pi = new int(6);
double *pd = new double(99.99);

struct where{double x; double y; double z};
where *one = new where{2.5,5.3,7.2};
int *ar = new int[4]{2,4,6,8};
```

#### 定位new运算符

定位new运算符是new的一个变体，能够指定要使用的地址。用这种特性可以设置内存管理规程、处理需要通过特定地址进行访问的硬件或在特定位置创建对象。示例如下：

```c++
#include<new> //定位new运算符在该头文件中
struct chaff
{
    char dross[20];
    int slag;
}
char buffer1[50];
char buffer2[500];
int main()
{
    chaff *p1,*p2;
    int *p3,*p4;
    p1=new chaff;
    p3=new int[20];
    p2=new(buffer1) chaff; //在buffer1中分配空间给结构chaff
    p4=new(buffer2) int[20]; //在buffer2中分配空间给一个包含20个元素的int数组
    return 0;
}
```

在某些情况下不能使用delete来释放使用定位new运算符分配的内存，在上述例子中buffer指定的内存是静态内存，而delete只能用于这样的指针：指向常规new运算符分配的堆内存。如果buffer是使用常规new运算符创建的，则可以使用常规delete运算符来释放整个内存块。

定位new运算符的一种用法是，**与初始化结合使用，从而将信息放在特定的硬件地址处。**

## 9.3 名称空间

### 9.3.2 新的名称空间特性

用关键字namespace创建一个名称空间，一个名称空间中的名称不会与另一个名称空间的相同名称发生冲突。同时允许程序的其它部分使用该名称空间中声明的东西。例子如下：

```c++
namespace Jack{
    double pail;
    void fetch();
    int pal;
    struct Well{···};
}
```

名称空间可以是全局的，也可以位于另一个名称空间中。但不能位于代码块中，默认情况下名称空间中声明的名称的链接性为外部的（除非引用了常量）。

名称空间是开放的，可以把名称加入到已有的名称空间中，如：

```c++
namespace Jill(
    char* goose(const char*);
)
```

给名称空间创建别名，常用于给嵌套名称空间创建别名。示例如下：

```c++
namespace my_favorite_things {};
namespace mft=my_favorite things;
```

未命名的名称空间不能再未命名名称空间所属文件之外的其它文件中使用该名称空间中的名称，也就是说提供了**链接性为内部的静态变量的替代品**。

### 9.3.3 名称空间示例

头文件：

```c++
#include<string>
namespace pers
{
    struct Person
    {
        std::string fname;
        std::string lname;
    };
    void getPerson(Person&);
    void showPerson(const Person&);
}

namespace debts
{
    using namespace pers;
    struct Debt
    {
        Person name;
        double amount;
    };
    void getDebt(Debt&);
    void showDebt(const Debt&);
    double sumDebts(const Debt ar[], int n);
}
```

源文件1：

```c++
#include<iostream>
#include"namesp.h"

namespace pers
{
    using std::cout;
    using std::cin;
    void getPerson(Person& rp)
    {
        cout << "Enter first name: ";
        cin >> rp.fname;
        cout << "Enter last name: ";
        cin >> rp.lname;
    }
    void showPerson(const Person& rp)
    {
        cout << rp.lname << "," << rp.fname ;
    }
}

namespace debts
{
    void getDebts(Debt& rd)
    {
        getPerson(rd.name);
        std::cout << "Enter debt: ";
        std::cin >> rd.amount;
    }
    void showDebt(const Debt& rd)
    {
        showPerson(rd.name);
        std::cout << ":$" << rd.amount << std::endl;
    }
    double sumDebts(const Debt ar[], int n)
    {
        double total = 0;
        for (int i = 0; i < n; i++)
            total += ar[i].amount;
        return total;
    }
}
```

源文件2：

```c++
#include<iostream>
#include"namesp.h"

void other(void);
void another(void);
int main(void)
{
    using debts::Debt;
    using debts::showDebt;
    Debt golf = { {"Benny","Goatsniff"},120.0 };
    showDebt(golf);
    other();
    another();
    return 0;
}

void other(void)
{
    using std::cout;
    using std::endl;
    using namespace debts;
    Person dg = { "Doodles","Glister" };
    showPerson(dg);
    cout << endl;
    Debt zippy[3];
    int i;
    for (i = 0; i < 3; i++)
        getDebt(zippy[i]);
    for (i = 0; i < 3; i++)
        showDebt(zippy[i]);
    cout << "Total debt:$" << sumDebts(zippy, 3) << endl;
}

void another(void)
{
    using pers::Person;
    Person collector = { "Milo","Rightshift" };
    pers::showPerson(collector);
    std::cout << std::endl;
}
```

### 9.3.4 名称空间及其前途

关于名称空间的统一编程理念：

- 使用在已命名的名称空间中声明的变量，而不是使用外部全局变量。

- 使用在已命名的名称空间中声明的变量，而不是使用静态全局变量。

- 如果开发了一个函数库或类库，将其放在一个名称空间中，事实上，C++当前提倡将标准函数库放在名称空间std中。

- 仅将编译指令using作为一种将旧代码转换为使用名称空间的权宜之计

- 不要在头文件中使用using编译指令。

- 导入名称时首选使用作用域解析运算符或using声明的方法。

- 对于using声明，首选将其作用域设置为局部而不是全局。

**使用名称空间的主旨是简化大型变成项目的管理工作**

## 9.4 总结

C++鼓励在开发程序时使用多个文件，一种有效的组织策略是，使用头文件来定义用户类型，为操纵用户类型的函数提供函数原型；并将函数定义放在一个独立的源代码文件中。头文件和源代码文件一起定义和实现了用户定义的类型及其使用方式。最后，将main（）和其它使用这些函数的函数放在第三个文件中。

# 第十章 对象和类

## 10.2 抽象和类

### 10.2.2 C++中的类

只能通过公有成员函数（或友元函数）访问对象的私有成员。因此，公有成员函数是程序和对象的私有成员之间的桥梁，提供了对象和程序之间的接口。防止程序直接访问数据被称为数据隐藏。类设计尽可能将公有接口与实现细节分开。将实现细节放在一起并将它们与抽象分开被称为封装。数据隐藏也是一种封装。原则是将实现细节从接口设计中分离出来。如果以后找到了更好的、实现数据表示或成员函数细节的方法，可以对这些细节进行修改，而无需修改程序接口，这使得程序更容易维护。

类对象的默认访问控制为private

```c++
class World
{
    float mass;
    char name[20];
public:
    void tellall(void);
};
```

### 10.2.3 实现类成员函数

定义成员函数时使用作用域解析运算符来表示函数所属的类。

定义位于类声明中的函数都将自动成为内联函数。如果要在类声明之外定义成员函数，并使其成为内联函数，只需在类实现部分中定义函数时使用inline限定符即可。

## 10.3 类的构造函数和析构函数

### 10.3.1 声明和定义构造函数

构造函数的原型和函数头没有返回值，也没有被声明为void类型。实际上，构造函数没有声明类型。

为了避免构造函数的参数名与类成员名称相同，一般在数据成员名中使用 **m_** 前缀（或使用 **_** 后缀）：

```c++
class Stock
{
private:
    string m_company;
    long m_shares;  
};
```

### 10.3.2 使用构造函数

构造函数可以显式调用或隐式调用：

```c++
Stock food = Stock("World Cabbage",250,1.25); //显式
Stock garment("Furr Mason",50,2.5); //隐式
Stock *pstock = new Stock("Electroshock Games",18,19.0); //构造函数与new一起使用
```

### 10.3.3 默认构造函数

默认构造函数是在未提供显式初始化值时用来创建对象的构造函数。

为类定义了构造函数后，必须同时提供默认构造函数。

定义默认构造函数的方式有两种，一种是给已有构造函数的所有参数提供默认值；另一种方式是通过函数重载来定义另一个构造函数（一个没有参数的构造函数）。

在设计类时，通常应提供对所有类成员做隐式初始化的默认构造函数。

### 10.3.4 析构函数

析构函数的名称在类名前加上\~。析构函数也可以没有返回值和声明类型，但是析构函数**没有参数**。

析构函数不应显式调用，调用时机由编译器决定。

如果构造函数使用了new，则必须提供使用delete的析构函数。

### 10.3.5 改进Stock类

列表初始化语法也适用于类，只要提供与某个构造函数的参数列表匹配的内容并用大括号将它们括起。

只要类方法不修改调用对象，则应将其声明为const。方法如下：

```c++
void show() const; //函数原型
void Stock::show() const; //函数头
```

## 10.4 this指针

this指针用来指向调用成员函数的对象（this被作为隐藏参数传递给方法）。

每个成员函数（包括构造和析构函数）都有一个this指针，this指针指向调用对象。\*this就是对象本身。

## 10.5 对象数组

声明对象数组的方法与声明标准类型数组相同。

```c++
Stock mystuff[4];
```

当程序创建未被显式初始化的类对象时，总是调用默认构造函数。上述声明要求这个类要么没有显式地定义任何构造函数（将使用不执行任何操作的隐式默认构造函数），要么定义了一个显式默认构造函数。可以用构造函数来初始化数组元素。这种情况下必须为每个元素调用构造函数。

```c++
const int STKS = 4;
Stock stocks[STKS] = {
    Stock("n",12.5,20),
    ······
}
```

初始化对象数组地方案是，首先使用默认构造函数创建数组元素，然后花括号中的构造函数将创建临时对象，然后将临时对象的内容复制到相应的元素中。因此，要创建类对象数组，则这个类必须有默认构造函数。

## 10.6 类作用域

### 10.6.3 作用域为类的常量

使符号常量的作用域为类的错误方法：

```c++
class Bakery
{
private:
    const int Months = 12;
    double costs[Months];  
};
```

正确的方法有两种：

```c++
//第一种方式是在类中声明一个枚举
class Bakery
{
private:
    enum {months = 12};
    double costs[Months];  
};
```

用这种方式声明枚举不会创建类数据成员，这意味着所有对象都不含枚举。Months只是一个符号名称，在作用域为整个类的代码中遇到他时编译器会用30来替换它。这里使用枚举只是为了创建符号常量，不创建枚举类型的变量，因此不需要提供枚举名。

```c++
//第二种方式是使用关键字static
class Bakery
{
private:
    static const int Months = 12;
    double costs[Months];  
};
```

类中使用static限定的常量将与其它静态变量存储在一起，而不是存储在对象中，因此只有一个Months常量被所有的Bakery对象共享。

## 10.7 抽象数据类型

用类来实现ADT，以栈为例：

```c++
//stack.h
#ifndef STACK_H_
#define STACK_H_
typedef unsigned long Item;
class Stack
{
private:
    enum{MAX=10};
    Item items[MAX];
    int top;
public:
    Stack();
    bool isempty() const;
    bool isfull() const;
    bool push(const Item& item);
    bool pop(Item& item);
};
#endif
```

```c++
//stack.cpp
#include"stack.h"
Stack::Stack()
{
    top = 0;
}

bool Stack::isempty() const
{
    return top == 0;
}

bool Stack::isfull() const
{
    return top == MAX;
}

bool Stack::push(const Item& item)
{
    if (top < MAX)
    {
        items[top++] = item;
        return true;
    }
    else
        return false;
}

bool Stack::pop(Item& item)
{
    if (top > 0)
    {
        item = items[--top];
        return true;
    }
    else
        return false;
}
```

# 第十一章 使用类

 **轻松地使用这种语言。不要觉得必须使用所有的特性，不要在第一次学习时就试图使用所有的特性。**

## 11.1 运算符重载

要重载运算符需要使用被称为运算符函数的特殊函数形式。运算符函数的格式如下：

```c++
operator op(argument list)
//operator +()
```

## 11.2 计算时间：一个运算符重载示例

### 11.2.2 重载限制

- 重载后的运算符必须至少有一个操作数是用户定义的类型。

- 使用运算符时不能违反运算符原来的句法规则。不能修改运算符的优先级。

- 不能创建新的运算符。

- 不能重载下面的运算符。

sizeof  .  \*  ::  ?:  

- 下面的运算符只能通过成员函数进行重载

=  ()  []  ->

## 11.3 友元

除了公有类，C++提供了友元这种形式的访问权限：友元函数。友元类。友元成员函数。

友元函数是非成员函数，但是它可以访问类中的私有成员。

### 11.3.1 创建友元

第一步：将函数原型放在类声明中，并在原型前声明关键字 friend

```c++
friend Time operator*(double m,const Time&);
```

operator*()函数不是成员函数，不能使用成员运算符来调用；但是它与成员函数的访问权限相同。

将友元函数编写为非友元函数：

```c++
Time operator*(double m,const Time&t)
{
    return t*m;
}
```

这个版本将Time对象t作为一个整体使用，让成员函数来处理私有数据，因此不必是友元。

---

第二步：编写函数定义，不用加类名限定符，函数头中不用friend关键字。

**如果要为类重载运算符，并将非类的项作为其第一个操作数，则可以用友元函数来反转操作数的顺序**

---

<<重载的第一个版本，如果用类成员函数来重载<<，会令人迷惑。但通过使用友元函数可以像下面这样重载：

```c++
void operator<<(ostream&os,const Time&t)
{
    os<<t.hours; //os为一个ostream对象，可以是cout等
}
```

---

<<重载的第一个版本不能实现多个<<连用，因为<<运算符要求左侧是一个ostream对象，如果要连用<<运算符，只需要将第一个版本中函数的返回值修改为ostream对象的引用即可：

```c++
ostream& operator<<(ostream&os,const Time &t)
{
    os<<t.hours;
    return os;
}
```

## 11.5 再谈重载：一个矢量类

矢量类Vector声明如下：

```c++
//vector.h
class Vector
{
public:
    enum Mode{RECT,POL}; //状态成员，控制构造函数使用哪种形式
private:
    double x;
    double y;
    double mag;
    double ang;
    Mode mode;
    void set_mag();
    void set_ang();
    void set_x();
    void set_y();
public:
    Vector();
    Vector(double n1,double n2,Mode form=RECT);
    void reset(double n1,double n2,Mode form = RECT);
    ~Vector();
    double xval() const {return x;}
    doubel yval() const {return y;}
    doubel magval() const {return mag;}
    doubel angval() const {return ang;}
    void polar_mode();
    void rect_mode();
    Vector operator+(const Vector &b) const;
    Vector operator-(const Vector &b) const;
    Vector operator-() const;
    Vector operator*(double n) const;
    friend Vector operator*(double n,const Vector &a);
    friend std::ostream& operator<<(std::ostream& os, const Vector& v);
}
```

构造函数的定义：

```c++
Vector::Vector(double n1,double n2,Mode form)
{
    mode=form;
    if(form==RECT)
    {
        x=n1;
        y=n2;
        set_mag();
        set_ang();
    }
    else if(form==POL)
    {
        mag=n1;
        ang=n2;
        set_x();
        set_y();    
    }
    else
    {
        x=y=mag=ang=0.0;
        mode=RECT;    
    }
}
```

## 11.6 类的自动转换和强制类型转换*

# 第十二章 类和动态内存分配

## 12.1 动态内存和类

静态类成员的特点：所有的实例对象都只创建一个静态类变量副本，所有对象共享一个静态成员。不能在声明中初始化静态成员变量。对于静态类成员，可以在类声明之外使用单独的语句来初始化，因为静态类成员是单独存储的，而不是对象的组成部分。初始化在方法文件（声明描述如何分配内存，但并不分配内存）。但如果静态成员是整型或枚举型const，则可以在类声明中初始化。

---

在构造函数中使用new来分配内存时，必须在相应的析构函数中使用delete来释放内存，如果使用new[]来分配内存，则应使用delete[]来释放内存，记住，new分配的内存在堆区。

---

### 12.1.2 特殊成员函数

C++自动提供了如下的成员函数（编译器自动生成）：

- 默认构造函数（如果没有定义构造函数）

- 默认析构函数（如果没有定义）

- 复制构造函数（如果没有定义）

- 赋值运算符（如果没有定义）

- 地址运算符（如果没有定义）

---

**复制构造函数**：将一个对象复制到新创建的对象中，用于初始化过程而不是常规的赋值过程。原型通常如下：

```c++
Class_name(const Class_name&);
```

**调用时机**：假设motto是一个StringBad对象，下面四种声明都将调用复制构造函数

```c++
StringBad ditto(motto);
StringBad metoo=motto;
StringBad also=StringBad(motto);
StringBad * pStringBad=new StringBad(motto); //用motto初始化一个匿名对象，然后将新对象的地址赋给pStringBad指针
```

当函数按值传递对象或函数返回对象时都将使用复制构造函数。按值传递对象将调用复制构造函数，因此应该按引用传递对象，这样可以节省调用构造函数的时间以及存储新对象的空间。

---

**默认的复制构造函数的功能**：逐个复制非静态成员（成员复制也称为浅复制）。如果成员本身就是类对象，则将使用这个类的复制构造函数来复制成员对象。静态函数不受影响，因为他们属于整个类。一个复制构造函数（深度复制）的示例：

```c++
StringBad::StringBad(const StringBad& st)
{
    num_strings++; //这是一个静态变量
    len=st.len;
    str=new char[len+1];
    std::strcpy(str,st.str);
}
```

如果类中包含了使用new初始化的指针成员，应当定义一个复制构造函数以复制指向的数据，而不是指针，这被称为深度复制（deep copy）。浅复制只是复制指针值，这在某些情况下将导致错误。

---

**赋值运算符**：赋值运算符的隐式实现也对成员进行逐个复制。如果成员本身就是类对象，则程序将使用为这个类定义的赋值运算符来复制该成员，但静态数据成员不受影响。但是这种浅复制就很有可能当值和默认复制构造函数一样的问题：复制指针值而不是指针指向的内容。为StringBad类编写赋值运算符（赋值运算符只能由类成员函数重载）：

```c++
StringBad& StringBad::operator=(const StringBad&st)
{
    if(this==&st) return *this;
    delete[] str;
    len=st.len;
    str=new char[len+1];
    std::strcpy(str,st.str);
    return *this;
}
```

使用delete释放str指向的内存后再将一个全新字符串的地址赋给str，否则，未释放的字符串将保留在内存中导致内存浪费。

## 12.3 在构造函数中使用new时应注意的事项

- 如果在构造函数中使用new来初始化指针成员，则应在析构函数中使用delete。

- new和delete必须互相兼容。

- 如果有多个构造函数，则必须以相同的方式使用new，要么都带中括号，要么都不带，因为只有一个析构函数，所有的构造函数必须与它兼容。

- 应定义一个复制构造函数，通过深度复制将一个对象初始化为另一个对象。具体地说就是复制构造函数应该分配足够的空间存储复制的数据，并复制数据，而不仅仅是数据的地址。还应该更新所有受影响的静态类成员、

- 应定义一个赋值运算符，通过深度复制将一个对象复制给另一个对象。该方法应该：检查自我赋值的情况，释放成员指针以前指向的内存，复制数据不仅仅是数据的地址，并返回一个指向调用对象的引用。

## 12.4 有关返回对象的说明

### 12.4.1 返回指向const对象的引用

使用这一方法的目的是提高效率。如果函数返回传递给它的对象，可以通过返回引用来提高效率。注意：返回对象将调用复制构造函数，返回引用不会。引用指向的对象应该在调用函数执行时存在。const也应该相匹配。

### 12.4.2 返回指向非const对象的引用

常见情形是重载赋值运算符和重载与cout一起使用的<<运算符。

### 12.4.3 返回对象

如果被返回的对象是被调用函数中的局部变量，则不应该返回引用。因为被调用函数执行完毕时局部对象将调用其析构函数。在这种情况下，将调用复制构造函数来生成返回的对象。如果方法或函数返回一个没有公有复制构造函数的类的对象（如ostream类），它必须返回一个指向这种对象的引用。最后，有些方法和函数可以返回对象，也可以返回指向对象的引用，这时应首选引用来提高效率。

## 12.6 复习各种技术
