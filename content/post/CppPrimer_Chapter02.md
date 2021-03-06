---
title: "CppPrimer02.变量和基本类型"
date: 2020-04-24T12:57:12+08:00
lastmod: 2020-04-24T12:57:12+08:00
draft: false
tags: [C++, 学习笔记]
categories: [C++ Primer 学习笔记]
author: "Dereen"
comment: true
toc: true
autoCollapseToc: false
---

本章主要介绍内置类型，并初步说明C++语言是如何支持更复杂数据类型的。

<!--more-->

## **基本数据类型**

C++定义了一套包括算术类型和空类型在内的基本数据类型。其中算术类型包括了字符、整数型、布尔值和浮点型。**空类型不对应任何具体的值，仅用于一些特殊场合，**例如当函数不返回任何值时使用空类型作为其返回值。

### **算术类型**

算数类型分为两类，整型（包括字符和布尔类型）和浮点型。

|    类型     |      含义      |   最小尺寸   |
| :---------: | :------------: | :----------: |
|    bool     |    布尔类型    |    未定义    |
|    char     |      字符      |     8位      |
|   wchar_t   |     宽字符     |     16位     |
|  char16_t   |  Unicode字符   |     16位     |
|  char32_t   |  Unicode字符   |     32位     |
|    short    |     短整型     |     16位     |
|     int     |      整型      |     16位     |
|    long     |     长整型     |     32位     |
|  long long  |     长整型     |     64位     |
|    float    |  单精度浮点数  | 6位有效数字  |
|   double    |  双精度浮点数  | 10位有效数字 |
| long double | 扩展精度浮点数 | 10位有效数字 |

C++提供了多种字符类型，基本的字符类型是char，一个char的空间应确保可以存放机器基本字符集中任意字符对应的数字值。也就是说，**一个char的大小和一个机器字节一样。**

> 机器字长是指计算机进行一次整数运算所能处理的二进制数据的位数（整数运算即定点整数运算）。因为计算机中数的表示有定点数和浮点数之分，定点数又有定点整数和定点小数之分，这里所说的整数运算即定点整数运算。机器字长也就是运算器进行定点数运算的字长，通常也是CPU内部数据通道的宽度。

### **字节和字**

💡大多数计算机以2的整数次幂个比特（bit）作为块来处理内存，可寻址的最小内存块成为“字节（byte）”，存储的基本单元称为“字（word）”，它通常由几个字节组成。在C++语言中，一个字节要至少能容纳机器基本字符集中的字符。大多数计算机的字节由8bit构成，字则由32或64bit构成，也就是4或8字节。

### **带符号类型和无符号类型**

除去布尔和扩展的字符型之外，其他整型可以划分为带符号的和无符号的两种。带符号的可表示正数、负数或0，无符号类型则仅能表示大于等于0的值。通过在类型前加上unsigned标识即可定义一个无符号类型，例如：unsigned int。

与上面所述不同的是，字符型被分为了3种：char、signed char和unsigned char。需要注意的是，类型char和类型signed char并不相同。尽管字符型有三种，但是字符的表现形式却只有两种：带符号的和无符号的，类型char实际上会表现为哪一种由编译器决定。

### **如何选择类型**

- 当明确数值不可能为负时，选用无符号类型
- 使用int执行整形运算，如果数值超过了int的表示范围，选用long long
- 执行浮点数运算选用double，这是因为其计算代价与float相差无几，而精度反而更高

### **类型转换**

自动类型转换：

- 当我们把一个非布尔类型的算数值赋给布尔类型时，初始值为0则结果为false，否则结果为true
- 当我们把一个布尔值赋给非布尔类型时，true变为1，false则变为0
- 当我们把浮点型赋给整型时，只保留整数部分的值
- 当我们把一个整数值赋给浮点类型时，小数部分为0，如果该整数所占的空间超过了浮点类型的容量，精度可能有损失
- **当我们赋给无符号类型一个超出它表示范围的值时，结果是初始值对无符号类型的表示数值总数取模后的余数。**例如，将-1赋给一个unsigned char，则结果为255()

💡无符号类型和带符号类型做运算时，带符号类型会首先被转换为无符号类型再做运算，这样造成的结果往往是未知的。

### **字面值常量**

一个形如82的值被称作字面值常量，这样的值一看便知。每个字面值常量都对应一种数据类型，字面值常量的形式和值决定了它的数据类型。

#### **整型和浮点型字面值**

我们可以将整型字面值写作十进制数、八进制数或十六进制数的形式。以0开头的整数代表八进制数，以0x或0X开头的代表十六进制数。20的三种表示方法：20、024、0x14。

整型字面值具体的数据类型由它的值和符号决定。**默认情况下，十进制字面值是带符号数，八进制和十六进制字面值极可能是带符号的也可能是无符号的。**十进制字面值的类型是int、long、long long中尺寸最小的那个，当然前提是能够容纳下当前的值。八进制和十六进制字面值的类型是能容纳其数值的int、unsigned int、long、unsigned long、long long和unsigned long long中的尺寸最小者。

💡尽管整型字面值可以存储在带符号数据类型中，但严格来说，十进制字面值不会是负数。如果我们使用了一个形如-42的负十进制字面值，那个负号并不在字面值之内，它的作用仅仅是对字面值取负值而已。

浮点型字面值表现为一个小数或以科学计数法表示的指数，其中指数部分使用E或e标识：3.1415926、3.14159E0、0.、0e0、.001，默认的浮点型字面值是一个double。

#### 字符和字符串字面值

‘a’：字符字面值，默认类型为char

“Hello”：字符串字面值，默认类型为常量字符构成的数组，即const char[]

💡编译器会在每个字符串的结尾处添加一个空字符（'\0'），因此字符串字面值的实际长度要比它的内容多1。

#### 转义序列

有两类字符程序员不能直接使用：一类是不可打印的字符，如退格或其他控制字符，因为它们没有可视的图符；另一类是在C++语言中有特殊含义的字符（单引号、双引号、问号、反斜杠）。在这些情况下需要使用转义序列，转义序列均以反斜杠作为开始，C++语言规定的转义序列包括：

```
换行符		\\n		横向制表符	\\t		报警(响铃)符	\\a
纵向制表符	\\v		退格符		\\b		双引号			\\"
反斜线		\\\\		问号		\\?		单引号			\\'
回车符		\\r		换页符		\\f
```

除此之外,**我们也可以使用泛化的转义序列,其形式是\x后紧跟1个或多个十六进制数字，或\后紧跟1~3个八进制数字。其中数字部分表示的是字符对应的数值。例子：**

```cpp
	std::cout << "Hi \\x4dO\\115!\\n";  // 输出Hi MOM！并换行
```

值得注意的是，如果\后面跟着的八进制数字超过3个，则只有前三个与\构成转义序列，而\x要用到后面跟着的所有数字。

#### 指定字面值的类型

通过添加下表中的前缀和后缀，可以改变整型、浮点型和字符型字面值的默认类型。

| 前缀 |             含义              |   类型   |
| :--: | :---------------------------: | :------: |
|  u   |        Unicode 16 字符        | char16_t |
|  U   |        Unicode 32 字符        | char32_t |
|  L   |            宽字符             | wchar_t  |
|  u8  | UTF-8（仅用于字符串字面常量） |   char   |

整型字面值（后缀）：

- u 或 U    unsigned
- l 或 L    long
- ll 或 LL    long long

浮点型字面值（后缀）：

- f 或 F    float
- l 或 L    long double

#### 布尔字面值和指针字面值

true和false是布尔类型的字面值：bool test = false;

**nullptr是指针字面值。**

## 变量

变量提供一个具名的、可供程序操作的**存储空间。**C++中的每个变量都有其数据类型，数据类型决定着变量所占内存空间的大小和布局方式，该空间能存储的值的范围，以及变量能参与的运算。对C++程序员来说，“变量”和“对象”一般可以互换使用。

💡一般认为对象是具有某种数据类型的内存空间。

### 变量定义

变量定义的基本形式：类型说明符+一个或多个变量名组成的列表。

```cpp
int i = 0;
```

#### 初始值

当对象在创建时获得了一个特定的值，我们说这个对象被初始化了。**当一次定义了两个或多个变量时，对象的名字随着定义也就马上可以使用了。**因此在同一条定义语句中，可以使用先定义的变量值初始化后定义的其他变量。

```cpp
double price = 109.99, discount = price * 0.7;
```

初始化并不是赋值，初始化的含义是创建变量时赋予其一个初始值，而赋值的含义是把对象的当前值擦除，而以一个新值来替代。

#### 列表初始化

C++语言定义了初始化的好几种不同形式，这也是初始化问题复杂性的一个体现。例如，要想定义一个名为units_sold的int变量并初始化为0，以下的4条语句都可以做到这一点：

```cpp
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
```

使用花括号来为初始化变量被称为列表初始化，而且这种方法也可以用来为对象赋新值。**当用于内置类型的变量时，这种初始化形式有一个重要特点：如果我们使用列表初始化且初始值存在丢失信息的风险，则编译器将报错。**

```cpp
long double ld = 3.1415242131241；
int a{ld}, b = {ld};   // 错误：转换未执行，因为存在丢失信息的风险
int c(ld), c = ld;     // 正确：转换执行，且确实丢失了精度
```

#### 默认初始化

如果定义变量时没有指定初值，则变量被默认初始化，此时变量被赋予了“默认值”。默认值到底是什么由变量类型决定，同时定义变量的位置也会对此有影响。

### 变量声明和定义的关系

为了增强代码的可读性，C++语言支持分离式编译机制，该机制允许将程序分割为若干个文件，每个文件可被独立编译。

为了支持分离式编译，C++语言将声明和定义区分开来。**声明（declaration）**使得名字为程序所知，**一个文件如果想使用别处定义的名字则必须包含对那个名字的声明。**

💡变量声明规定了变量的类型和名字，在这一点上与变量的定义相同，但不同的是，定义还申请存储空间，也可能会为变量赋一个初始值。

如果想要声明一个变量而非定义它，就在变量名前加上关键字extern，而且不要显式的初始化变量；

```cpp
extern int i;  // 声明i而非定义i
int j;         // 声明并定义j
```

💡变量能且只能被定义一次，但可以被声明多次。

### 标识符

C++的标识符（identifier）由字母、数字和下划线组成，其中必须以字母或下划线开头。标识符的长度没有限制，但是对大小写字母敏感。

⛔同时，C++也为标准库保留了一些名字，用户自定义的标识符中这些名字不能使用。![Untitled](https://raw.githubusercontent.com/DereenMec/pics/master/img/Untitled.png)

**同时，用户自定义的标识符中：**

- 不能连续出现两个下划线
- 不能以下划线紧连大写字母开头
- 定义在函数体外的标识符不能以下划线开头

#### 变量命名规范

- 标识符要能体现具体含义
- 变量名一般使用小写字母，如index
- 用户自定义的类名一般以大写字母开头，如Sales_item
- 如果标识符由多个单词组成，则单词间应有明显区分，如student_loan或studentLoan

### 名字的作用域

作用域（scope）是程序的一部分，在其中名字有其特定的含义。C++中大多数作用域都以花括号分割。

```cpp
#include <iostream>

int main() {
    int sum = 0;
    for (int val = 1; val <= 10; ++val) sum += val;
    std::cout << "Sum of 1 to 10 inclusive is " << sum << std::endl;
    return 0;
}
```

定义在函数体之外的名字拥有**全局作用域**（global scope），一旦声明之后，全局作用域内的名字在整个程序的范围内都可使用，如main。名字sum定义于main函数所限定的作用域之内，从声明sum到main函数结束为止都可访问它，但是除了main函数所在的块就无法访问了，因此说变量sum拥有**块作用域**（block scope）。名字val定义与for语句内，在for语句之内可以访问val，但是在main函数的其他位置就不能访问它了。

💡一般来说，在对象第一次被使用的地方附近定义它是一种好的选择，因为这样做有助于更容易地找到变量的定义。

#### 嵌套的作用域

作用域能够彼此嵌套或称包含，被包含的作用域称为内层作用域，包含着别的作用域的作用域被称为外层作用域。

作用域中一旦声明了某个名字，它的内层作用域中都能访问该名字，同时，允许在内层作用域中重新定义外层作用域已有的名字。

❗如果函数有可能用到某全局变量，则不宜在定义一个同名的局部变量。

## 复合类型

复合类型（compound type）是指基于其他类型定义的类型。C++语言有几种复合类型，这里介绍指针和引用。

### 引用

引用（reference）为对象起了另一个名字，**引用类型**引用另一种类型。通过将声明符写成&d的形式来定义引用类型，其中d是声明的变量名。**一旦初始化完成，引用将和它的初始值对象一直绑定在一起。因为无法将引用重新绑定到其他对象上，所以引用必须初始化。**

因为引用本身不是一个对象，所以不能定义引用的引用。

```cpp
int a = 3;
int &b = a;
b = 4;
std::cout << a << std::endl;  //输出为4
```

### 指针

指针（pointer）是指向另一种类型的复合类型。指针与引用类似，也实现了对其他对象的间接访问。然而指针与引用相比有很多的不同点：

- 指针本身就是一个对象，允许对指针赋值和拷贝，而且在指针的生命周期内它可以先后指向几个不同的对象
- 指针无需在定义时赋初值。和其他内置类型一样，在块作用域内定义的指针如果没有被初始化，也将拥有一个不确定的值。

#### 获取对象的地址

指针存放某个对象的地址，要想获取该地址，需要使用**取地址符（操作符&）：**

```cpp
int val = 42；
int *p = &val;
```

**因为引用不是对象，没有实际地址，所以不能定义指向引用的指针。**

因为在声明语句中指针的类型实际上被用于指定他所指向对象的类型，所以二者必须匹配。如果指针指向了一个其他类型的对象，对该对象的操作将发生错误。

#### 指针值

指针的值（即地址）应该属于下列4种状态之一：

1. 指向一个对象。
2. 指向紧邻对象所占空间的下一个位置。
3. 空指针，意味着指针没有指向任何对象。
4. 无效指针，也就是上述情况之外的其他值。

#### 利用指针访问对象

如果指针指向了一个对象，则允许使用解引用符（*）来访问该对象：

```cpp
int val = 42;
int *p = &val;
*p = 43;
std::cout << val << std::endl;  //输出43
```

#### 空指针

空指针（null pointer）不指向任何对象，在试图使用一个指针之前代码可以首先检查它是否为空。以下列出几个生成空指针的方法：

```cpp
int *p1 = nullptr;  // C++11中引入
int *p2 = 0;

#include <cstdlib>
int *p3 = NULL;
```

💡在新标准下，现在的C++程序最好使用nullptr，同时尽量避免使用NULL。

**把int变量直接赋给指针是错误的操作，即使int变量的值恰好等于0也不行。**

💡建议初始化所有指针。

#### 其他指针操作

只要指针拥有一个合法值，就能将他用在条件表达式中。和采用算数值作为条件遵循的规则类似，如果指针的值是0（即是一个空指针），条件取false，否则取true。

```cpp
#include <cstdlib>
#include <iostream>

int main() {
    int *p1 = nullptr;
    if (p1) std::cout << "p1" << std::endl;  // p1不会输出
    int a;
    int *p2 = &a;
    if (p2) std::cout << "p2" << std::endl;  // p2会被打印
    return 0;
}
```

#### void*指针

void*指针是一种特殊的指针类型，可用于存放任意对象的地址。一个void*指针存放着一个地址。这一点和其他指针类似。不同的是，我们对改地址中到底是个什么类型的对象并不了解。

⛔不能直接操作void*指针所指的对象，因为我们并不知道这个对象到底是什么类型，也就无法确定能在这个对象上做哪些操作。

### 理解复合类型的声明

变量的定义包括一个基本数据类型和一组声明符（*和&是声明符的一部分罢了）。在同一条定义语句中虽然基本数据类型只有一个，但是声明符的形式却可以不同。也就是说，一条定义语句可能定义出不同类型的变量：

```cpp
int i = 42, &j = i, *p = &i;
// i是一个int型的数，p是一个int型指针，r是一个int型引用。
```

#### 指向指针的指针

一般来说，声明符中的修饰符（*和&）个数并没有限制。当有多个修饰符连写到一起的时候，按照其逻辑关系祥加解释即可。以指针为例：

通过*的个数可以区分指针的级别。也就是说，**表示指向指针的指针，***表示指向指针的指针的指针。

```cpp
int val = 42;
int *p1 = &val;  // p1是一个int型指针
int **p2 = &p1;  // p2是一个指向int型指针的指针
```

#### 指向指针的引用

引用本身不是一个对象，因此不能定义指向引用的指针，但是我们可以定义指针的引用：

```cpp
int val = 42;
int *p = &val;
int *&r = p;
std::cout << *r <<std::endl; // 输出42
```

💡要理解r的类型到底是什么，我们可以采用**从右向左阅读法**。离变量名最近的符号（如上面例子中的&）对变量的类型有最直接的影响，因此r是一个引用。声明符的其余部分用以确定r引用的类型是什么，此例中的符号*说明r引用的是一个指针。最后，声明的基本数据类型部分指出r引用的是一个int型指针。

## const限定符

有时我们需要定义一种值不能被改变的变量。例如，用一个变量来表示固定的缓冲区的大小。巍峨了满足这一要求，可以用关键字const对变量的类型加以限定：

```cpp
const int i = 42;
```

**因为const对象一旦创建后其值就不能再改变，所以const对象必须初始化。**

### 默认状态下，const对象仅在文件内有效

当以编译时初始化的方式定义一个const对象时，就如对bufSize的定义一样：

```cpp
const int bufSize = 512;
```

编译器将在编译过程中把用到该变量的地方都替换成对应的值。也就是说，编译器会找到代码中所有用到bufSize的地方，然后以512替换。为了执行这个替换，编译器必须知道变量的初始值。如果程序存在多个文件，则每个用了const对象的文件都必须得能访问到它的初始值才行。**要做到这一点，就必须在每一个用到变量的文件中都有对它的定义。**

💡为了支持这一用法，同时避免对同一变量的重复定义，默认情况下，const对象被设置为只在当前文件内有效。当多个文件中出现了同名的const变量时，其实等同于在不同文件中分别定义了独立的变量。

某些时候有这样一种const，**它的初始值不是一个常量表达式，**但又确实有必要在文件间共享。这种情况下，我们不希望编译器为每个文件生成独立的变量。相反，我们希望这类const对象向其他非常量对象一样工作，**也就是说只在一个文件中定义，而在多个文件中使用。解决办法是：对于const变量不管是声明还是定义都添加extern关键字，这样只需定义一次就够了。**

```cpp
// file_1.cpp定义并初始化了一个常量，该常量能被其他文件访问
extern const int bufSize = 512;
// file_1.h头文件
extern const int bufSize;  //与file_1.cpp中的bufSize为同一个
```

### const的引用

可以把引用绑定到const对象上，我们称之为对常量的引用。与普通引用不同的是，**对常量的引用不能被用作修改它所绑定的对象。**

```cpp
const int bufSize = 512;
const int &r1 = bufSize;
r1 = 513;  // 会出错❗，不允许修改
int &r2 = bufSize;  //会出错❗，不允许把非常量引用绑定到常量对象上
```

#### 初始化和对const的引用

一般来说，引用的类型必须与其所引用对象的类型一致，但是也有两个例外：

- 在初始化常量引用时允许使用任意表达式作为初始值，只要该表达式能够转换成引用的类型即可
- 允许一个常量引用绑定非常量的对象、字面值，甚至是个一般表达式

```cpp
const double i = 42.14;
const int &r1 = i;  // 注意❗，但伴随着精度损失
std::cout << r1 << std::endl;  // 合法，输出为42
std::cout << i << std::endl;   // 输出为42.14

int j = 512;
const int &r2 = j; // 合法
```

在例外一中，编译过程发生了下面的事情：

```cpp
const int temp = i;    // 由双精度浮点数生成一个临时的整型常量
const int &r1 = temp;  // 让r1绑定这个临时量
```

在这种情况下，r1绑定了一个**临时量（temporary）对象。**所谓临时量对象就是当编译器需要一个空间来暂存表达式的求值结果时临时创建的一个未命名的对象。C++程序员常把临时量对象称为临时量。

**值得注意的是，类似于例外一的这种伴随类型转换的非常量引用都是非法的：**

```cpp
double i = 42.11;
int &r1 = i;
```

### 指针和const

与引用一样，也可以令指针指向一个常量对象，**指向常量对象的指针不能改变其所指对象的值，要想存放常量对象的地址，只能使用指向常量的指针。**

这里存在一种例外情况，即指向常量的指针可以指向一个非常量对象，所以下面的语句是合法的：

```cpp
double i = 42.11;
const double* p = &i;
```

#### const指针

**指针是对象而引用不是，所以允许把指针本身定义为常量。**这就意味着指针中存放的那个地址不可被改变，即不能更换指向的对象。把*放在const之前就可以说明指针是一个常量。

💡这里注意区分常量指针和指向常量的指针，`const int* p = &i;`说明p是一个指向常量的指针，它指向的对象的值不可被改变，但是它指谁是可以被改变的；而`int* const p`则是一个常量指针，它指向的对象的值可以被改变，但是它指谁是不可变的。这两者可以同时使用，即`const int* const p`，造成的结果就是指谁和指的对象的值都不可改变。

### 顶层const

鉴于指针本身是不是常量以及指针所指的对象是不是一个常量是两个独立的问题。用名词**顶层const（top-level const）表示指针本身是个常量，\**而用名词\**底层const（low-level const）表示指针所指对象是一个常量。**

更一般的，**顶层const可以表示任意的对象是常量，这一点对任何数据类型都适用，**如算数类型、类、指针等。**底层const则与指针和引用等[复合类型](https://www.notion.so/dereen/Chapter_02-3ae7c7c1804f4e1f8f64461b9b08ec77#8501b6ff46544a29a313d4655b27d8de)的基本类型部分有关。**

💡指针的特殊之处在于它既可以是顶层const又可以是底层const。

```cpp
int i = 5123;
int *const p = &i; // 顶层const，即不能更换指向对象，但可改变所指对象的值
const int ci;  // 顶层const
const int *p2 = &ci;  // 底层const，即可以更换指向对象，但不可以改变所指对象的值
const int * const p3 = p2;  // 既是顶层const，又是底层const
const int &r = ci;  //引用的const必然是底层const
```

**当执行拷贝操作时，常量是顶层const还是底层const带来的区别明显。其中顶层const不受什么影响。**

```cpp
i = ci;  // 合法，拷贝ci的值，ci是一个顶层const，对此操作无影响
p2 = p3;  // 合法，p2和p3所指向的对象相同，均为const int，p3的顶层const部分对此操作无影响
```

**而底层const（复合类型独有）的限制却不能忽略。当执行对象的拷贝操作时，拷入和拷出的对象必须具有相同的底层const资格，或者两个对象的数据类型必须能够转换（非const转为const，反之则不行）**

```cpp
int *p = p3;  // 错误，p3包含底层const，想想要是p把通过p3都不允许改的值改了怎么办
p2 = p3; // 正确，p2和p3都是底层const，都不允许修改指向对象的值
p2 = &i; // 正确，在这个过程中伴随着非const转为const
int &r = ci; // 错误，普通的int不能绑定到const int的ci身上
const int &r2 = i;  // 正确，int可以转为const int
```

### constexpr和常量表达式

常量表达式（const expression）是指值不会改变并且在编译过程就能得到计算结果的表达式。显然，[字面值](https://www.notion.so/dereen/Chapter_02-3ae7c7c1804f4e1f8f64461b9b08ec77#f52d5c89fac248ff95da324151e9ac28)属于常量表达式。用字面值初始化的const对象也是常量表达式。

**一个对象（或表达式）是不是常量表达式由它的数据类型（是否为const）和初始值共同决定，例如：**

```cpp
const int max_files = 20;               // √
const int limit = max_files + 1;        // √
int staff_size = 27;                    // 虽然staff_size的初始值是个字面值常量，但由于它的数据类型只是一个普通int，所以不是常量表达式
const int sz = get_size();              // 虽然左侧是const int类型，但它的具体值直到运行时才能获取，所以也不是常量表达式
```

#### constexpr变量

在实际工作中，我们很难分辨一个初始值到底是不是常量表达式。C++11新标准规定，允许**将变量声明为constexpr类型以便由编译器自己检查变量的值是否是一个常量表达式。如果不是，就会报错。**

```cpp
constexpr int mf = 22;            // √
constexpr int sf = mf + 1;        // √
constexpr int ef = size();        // 只有当size()是一个constexpr函数时才是一条正确的语句
```

💡一般来说，如果你认定变量是一个常量表达式，那就把它声明成constexpr类型。

#### 字面值类型

常量表达式的值需要在编译时就得到计算，所以能够声明成constexpr的类型就有限。因为这些类型一般比较简单，值也显而易见、容易得到，就把它们统称为“字面值类型”。算术类型、引用和指针都**属于**字面值类型。自定义类、IO库、string类型则**不属于**字面值类型。也就不能被定义为constexpr。

尽管指针和引用都能定义成constexpr，但它们的初始值却受到严格限制。一个constexpr指针的初始值必须是nullptr或者0，或者时存储与某个固定地址中的对象。一般来说，函数体内定义的变量并非存放在固定的地址中，因此constexpr指针不能指向这样的变量。相反的，定义与所有函数体之外的对象，其地址不变，能用来初始化constexpr指针。同时，函数体中定义的一类有效范围超出函数本身的变量，这类变量和定义在函数体之外的变量一样也有固定地址。因此，constexpr引用能绑定到这样的变量上，constexpr指针也能指向这样的变量。

#### 指针和constexpr

在constexpr声明中如果定义了一个指针，限定符constexpr仅对指针有效，与指针所指的对象无关（顶层const）。

```cpp
const int *p = nullptr;        // 底层const，所指的对象的值不可变
constexpr int *p = nullptr;    // 顶层const，不能更换所指的对象
```

与其他常量指针类似，constexpr指针既可以指向常量对象，也可以指向非常量对象（只是通过这个指针不能修改）。

## 处理类型

用于类型名称太复杂，写着写着就忘了，以及不知道返回值是什么类型的时候。

### 类型别名

由两种方法为类型建立别名。一是**typedef方法**，二是**using**方法。

```cpp
typedef double base, *p;    // base是double的同义词，p是double*的同义词
using base = double;        
using p = double*;          // 同样的效果
```

#### 指针、常量和类型别名

如果某个类型别名指代的是复合类型或常量，那么把它用到声明语句里就会产生意想不到的后果。

```cpp
typedef char *pString;        // pString是一个char*类型，即指向char对象的指针
char a = 'A';
const pString cstr = &a;      // const pString并不是const char*类型，而是char* const 类型
*cstr = 'B';                  // 正确
cstr = nullptr;               // 错误
```

### auto类型说明符

C++11新标准引入了auto类型说明符，用它就能让编译器替我们去分析表达式所属的类型。和原来那些只对应一种特定类型的说明符（如int）不同，auto让编译器通过初始值来推算变量的类型。**显然，auto定义的变量必须有初始值。**

💡使用auto也能在一条语句中声明多个变量。因为一条声明语句只能有一个基本数据类型，**所以该语句中所有变量的初始基本数据类型都必须一样：**

```cpp
auto sz = 0, pi = 3.14;      // 错误，类型必须一致
auto sz = .0, pi = 3.14;     // 正确
```

#### 复合类型、常量和auto

auto一般会忽略掉顶层const，同时底层const会被保留下来，比如当初始值是一个指向常量的指针时。

```cpp
const int ci = 42, &cr = ci;
auto a = ci;                    // a是一个整数，ci的顶层const特性被忽略了
auto b = cr;                    // b是一个整数，（cr是ci的别名，ci本身是一个顶层const）
auto c = &ci;                   // c是一个指向整数常量的指针（对常量对象取地址是一种底层const）
```

**如果希望推断出的auto类型是一个顶层const，需要明确指出。**

```cpp
const auto f = ci;
```

还可以将引用的类型设置为auto，此时原来的初始化规则仍然适用。

```cpp
auto &g = ci;             // 正确，g是一个整型常量引用，绑定到ci
auto &h = 42;             // 错误，不能为非常量引用绑定字面值
const auto &s = 42;       // 正确，可以为常量引用绑定字面值
```

### decltype类型的指示符

有时，我们希望从表达式的类型推断出要定义的变量的类型，但是**不想用该表达式的值初始化变量。\**为了满足这一要求，C++11新标准引入了第二种类型说明符decltype，它的作用是\**选择并返回操作数的数据类型，却不实际计算表达式的值。**

```cpp
decltype(f()) sum = x;     // sum的类型就是f的返回值类型
```

**decltype处理顶层const和引用的方式与auto有些不同。**如果decltype使用的表达式是一个变量，则decltype返回该变量的类型（包括顶层const和引用在内）：

```cpp
const int ci = 0, &cj = ci;
decltype(ci) x = 0;              // x的类型为const int
decltype(cj) y = x;              // y的类型为const int&，绑定到变量x
decltype(cj) z;                  // 错误❗，z是一个引用，必须被初始化
```

💡引用从来都是作为其所指对象的同义词出现，只有用在decltype处是一个例外。

#### decltype和引用

如果decltype使用的表达式不是一个变量，则decltype返回表达式结果对应的类型。

```cpp
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;            // 合法，加法表达式的结果是int，因此b是一个未初始化的int
decltype(r) b;                // 错误，引用必须初始化
decltype(*p) c;               // c是int&，必须初始化
```

**💡如果表达式的内容是解引用操作，则decltype将得到引用类型。**正如我们所熟知的，解引用指针可以得到指针所指的对象，而且还能给这个对象赋值。因此decltype(*p)的结果类型就是int&，而非int。

另一个例子显示了变量名加上一对括号和不加括号的区别：

```cpp
int i = 42;
decltype((i)) d;             // 错误：d是int&，必须初始化
decltype(i) e;               // 正确，e是一个未初始化的int
```

**❗切记：decltype((variable))双层括号的结果永远是引用，而decltype(variable)结果只有当variable本身是一个引用时才是引用。**

## 自定义数据结构

从最基本的层面理解，数据结构是把一组相关的数据元素组织起来然后使用它们的策略和方法。

### 定义Sales_data类型

```cpp
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

❗这里需要注意的是，不要漏掉花括号后面的分号，这是因为类体后面可以直接跟变量名，所以分号必不可少。

#### 类数据成员

类的数据成员定义了类的对象的具体内容，每个对象有自己的一份数据成员拷贝。修改一个对象的数据成员，不会影响其他Sales_data的对象。

❗注意，对类内初始值的限制与之前类似；或者放在花括号里，或者放在等号右边，记住不可使用圆括号。

[1]: https://www.zhihu.com/question/37019538/answer/137353519	"为什么c++ 中类内初始值不能使用圆括号初始化？"

### 编写自己的头文件

为了确保各个文件中类的定义一致，类通常被定义在头文件中，而且**类所在头文件的名字应该与类的名字一样。**

#### 使用#pragma once确保不会重复包含

确保头文件多次包含仍能安全工作的常用技术是预处理器（preprocessor）。例如#include，当预处理器看到#include标记时就会把该头文件内的全部内容复制到当前文件中来。

C++中常用的一项技术是头文件保护符（header guard），有两种方法用来避免头文件重复被包含。

```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```

这种是使用**ifndef**方法；另一种方法是使用**pragma once：**

```cpp
#pragma once
```

💡头文件即使（目前还）没有被包含在任何头文件中，也应该设置保护符。头文件保护符很简单，程序员只要习惯性的加上就可以了，没必要太在乎你的程序到底需要不需要。