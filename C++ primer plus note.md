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

## 12.8 总结

在类构造函数中，可以使用new为数据分配内存，然后将内存地址赋给类成员。这样，类便可以处理长度不同的字符串，而不用在类设计时提前固定数组长度。

---

C++为类构造函数提供了一种可用来初始化数据成员的特殊语法。这些初始化操作是在对象创建时进行。语法如下

```c++
queue(int qs):qsize(qs),items(0),front(NULL),rear(NULL){}
```

类内初始化也是允许的，但只能用于非静态const成员。

使用成员初始化列表的构造函数将覆盖相应的类内初始化。

# 第十三章 类继承

## 13.1 一个简单的基类

### 13.1.1 派生一个类

```c++
class RatePlayer:public TableTennisPlayer
{}
```

冒号指出RatePlayer类的基类是TableTennisPlayer类，public指出是公有派生：基类的公有成员将称为派生类的公有成员；基类的私有部分也将称为派生类的一部分，但只能通过基类的公有和保护方法访问。

---

继承特性中需要额外添加：

- 派生类自己的构造函数

- 派生类可以根据需要添加额外的数据成员和成员函数

### 13.1.2 构造函数：访问权限的考虑

派生类只能通过基类方法访问基类的私有成员。具体来说就是派生类构造函数必须使用基类构造函数。示例：

```c++
RatePlayer(unsigned int r,const string & fn,const string & ln,bool ht):TableTennisPlayer(fn,ln,ht)
{
    rating = r;
}
```

如果不调用基类构造函数，程序将使用默认的基类构造函数。除非要使用默认构造函数，否则应显式调用正确的基类构造函数。

---

有关派生类构造函数的要点：

- 首先创建基类对象

- 派生类构造函数应通过成员初始化列表将基类信息传递给基类构造函数

- 派生类构造函数应初始化派生类新增的数据成员

- 释放对象的顺序与创建对象的顺序相反

- 类只能将值传递回相邻的基类，成员初始化列表只能用于构造函数

### 13.1.4 派生类和基类之间的特殊关系

派生类对象可以使用基类的方法，条件是方法不是私有的。

基类指针可以在不进行显式类型转换的情况下指向派生类对象；基类引用可以在不进行显式类型转换的情况下引用派生类对象。

基类指针或引用只能用于调用基类方法。

对于形参为指向基类的指针的函数，也可以使用基类对象的地址或派生类对象的地址作为实参。

派生对象也可以赋给基类对象，程序将使用隐式重载赋值运算符。

## 13.3 多态公有继承

实现多态公有继承的两种重要机制：

- 在派生类中重新定义基类的方法
- 使用虚方法

---

如果要在派生类中重新定义基类中的方法，需要在声明前加上关键字virtual。程序将根据对象是基类对象还是派生类对象来确定使用方法的版本。

在基类中将派生类会重新定义的方法声明为虚方法。方法在基类中被声明为虚的后，将在派生类中自动称为虚方法。在派生类声明中使用关键字virtual来指出哪些函数是虚函数也不失为一个好方法。

对于在派生类中重新定义的方法，如果要调用基类中相同的方法，标准技术是使用作用域解析运算符来调用基类方法。

---

虚方法的原理是使用一个指针数组（vtbl），指针既能指向基类对象，也能指向派生类对象，用一个数组来表示多种类型的对象，这就是多态性。

---

为基类声明一个虚析构函数也是一种惯例。如果析构函数不是虚的，则将只调用对应于指针类型的析构函数。如果析构函数是虚的，将调用相应对象类型的析构函数。这是因为继承允许隐式向上强制类型转换，意思是基类的指针可以指向派生类对象。因为所有的派生类对象都继承了基类的数据成员和成员函数，可以对基类对象执行的任何操作都适用于派生类对象。如果要向下强制类型转换，必须使用显式类型转换。

## 13.4 静态联编和动态联编

### 13.4.2 虚成员函数和动态联编

编译器对非虚函数使用静态联编，对虚方法使用动态联编（根据对象类型确定调用哪种版本的方法）。

C++指导原则之一：不要为不使用的特性付出代价。

---

虚函数工作原理：

编译器处理虚函数的方法是为每个对象添加一个隐藏成员，隐藏成员中保存了一个指向函数地址数组的指针。这个数组称为虚函数表（vtbl）。虚函数表中存储了为类对象进行声明的虚函数的地址。原理图如下：![虚函数表][虚函数表]

派生类和基类对象各自包含一个指向独立地址表的指针，如果派生类提供了虚函数的新定义，该虚函数表将保存新函数的地址；如果没有重新定义虚函数，将保存函数原始版本的地址。如果派生类定义了新的虚函数，该函数地址也被添加到vtbl中。

---

使用虚函数时的成本：

- 每个对象都将增大，增大量为存储地址的空间
- 编译器将为每个类创建一个虚函数的地址表
- 每个函数调用都将执行一项额外的操作，即到表中查找地址

### 13.4.3 有关虚函数的注意事项

- 在基类方法的声明中使用关键字virtual可使该方法在基类以及所有的派生类中都是虚的
- 基类指针或引用可以指向派生类对象
- 如果定义的类被用作基类，则应将那些要在派生类中重新定义的类方法声明为虚的

---

构造函数：构造函数不能为虚函数，因为创建派生类对象时是调用派生类的构造函数，然后，派生类的构造函数将使用基类的一个构造函数。

---

析构函数：析构函数应当为虚函数，除非类不作为基类。这是因为允许隐式向下强制类型转换。即使基类不需要显式析构函数提供服务，也不能依赖于默认的析构函数，也要自己提供一个虚析构函数，即使它不执行任何操作。

---

友元：友元不能为虚，只有成员才能为虚函数。

---

没有重新定义：将使用派生链中最新的虚函数版本。

---

重新定义：如果像下面这样重新定义：

```c++
//基类中
virtual void showperks(int a) const;
//派生类中
virtual void showperks() const;
```

重新定义继承的方法并不是重载，而是隐藏同名的基类方法。这说明：

- 如果要重新定义继承的方法，应确保与原来的原型完全相同。但如果返回类型是基类引用或者指针，可以修改为指向派生类的引用或指针，这种特性被称为返回类型协变
- 如果基类声明被重载，则应在派生类中重新定义所有的基类版本。如果只重新定义一个版本，则另外两个版本将被隐藏，派生类对象将无法使用它们。如果不需要修改，则新定义可定义为调用基类版本，如：

```c++
void Hovel::showperks() const {Dwelling::showperks();}
```

## 13.5 控制访问：protected



# 附录A：插图

[虚函数表]:data:image/png;base64,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

