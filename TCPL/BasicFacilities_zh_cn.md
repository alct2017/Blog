# C++基本机制
*The C++ Programming Language* Part II

## 数据类型和变量声明

### 数据类型
C++数据类型可以分为三类:
1. 基本类型
   1. 布尔型(Boolean type)
   2. 整型(Integer types)
   3. 字符型(Character types), 例如`char`和`wchar_t`.
   4. 浮点型(Floating-point types)
   5. `void`型. 不能声明一个`void`类型的数据. 只能用于函数返回值(表示没有返回值), 或结合指针符号声明一个通用指针类型`void*`.
2. 复合类型
   1. 指针类型(Pointer types)
   2. 数组类型(Array types)
   3. 引用类型(Reference types)
3. 用户自定类型(User-defined types)
   1. 结构(Data Structure)和类(Class)
   2. 枚举(Enumeration types)

### 变量声明
一个变量声明的结构可以分为五个部分:
```
<前缀修饰符>[类型][声明符]<函数后缀修饰符><初始值|函数体>
```
例如:
```cpp
const char∗ kings[] = { "Antigonus", "Seleucus", "Ptolemy" };
```

*前缀修饰符*是例如`virtual`, `static`, `extern`, `constexpr`等的一些与类型本身无关的属性.
*类型*是上面所说的数据类型.

*声明符*是变量名加上可选的声明操作符, 常见的声明操作符有:
|         |             |                       |
| ------- | ----------- | --------------------- |
| prefix  | `*`         | pointer               |
| prefix  | `*const`    | constant pointer      |
| prefix  | `*volatile` | volatile pointer      |
| prefix  | `&`         | lvalue reference      |
| prefix  | `&&`        | rvalue reference      |
| prefix  | `auto`      | funtion               |
| postfix | `[]`        | array                 |
| postfix | `()`        | funtion               |
| postfix | `->`        | returns from function |

*函数后缀修饰符*表明函数的一些属性, 比如`const`和`noexcept`.

### 初始化
相较于C语言, C++有一种不同的初始化方式, 列表初始化(List initialization):
```cpp
data_t a1 {v}; /* Recommended, from C++11 */
data_t a2 = {v};
data_t a3 = v;
data_t a4(v);
```
上面四个语句的作用是相同的. 但是列表初始化有一个特性, 它不会自动类型转换. 例如:
```cpp
void f(double f_val,int i_val)
{
    int x1 = f_val; // OK, x1 will truncation
    int x2 {f_val}; // error: possible truncation

    char c1 = i_val; // OK, equals that c1 = i_val & 0xff (auto casting)
    char c2 = {i_val}; // error: possible narrowing
}
```
当一个使用不匹配的值列表初始化一个变量时,编译器会报错. 作者建议**总是使用列表初始化**, 因为这样可以避免在你不确定的情形下发生自动转换.

只有一种情形除外, 那就是数据类型是`auto`时.
```cpp
auto x1 {1}; // x1 is an initializer_list<int>
auto x2 = 1; // x2 is an int
```

### 自动类型`auto`和`decltype()`
C++提供了两个自动类型机制, `auto`和`decltype()`.

`auto`从初始值决定类型, 它的最佳实践是用于循环中的迭代器(iterator):
```cpp
template<class T>
void f1(vector<T>& arg)
{
    for (vector<T>::iterator p = arg.begin(); p!=arg.end(); ++p)
    ∗p = 7;

    for (auto p = arg.begin(); p!=arg.end(); ++p)
    ∗p = 7;
}
```

`decltype(expr)`可以决定复杂表达式的类型, 比如函数返回值或者泛型类. 它的最佳实践是用于泛型编程:
```cpp
template<class T, class U>
auto operator+(const Matrix<T>& a, const Matrix<U>& b) −> Matrix<decltype(T{}+U{})>
{
    Matrix<decltype(T{}+U{})> res;
    for (int i=0; i!=a.rows(); ++i)
        for (int j=0; j!=a.cols(); ++j)
            res(i,j) += a(i,j) + b(i,j);
    return res;
}
```

### 类型别名
除了C中的`typedef`, C++还提供了另一种声明类型别名的方式:
```cpp
typedef int data_t;
using data_t = int; /* equivalent */

typedef void(*func_t)(int);
using func_t = void(*)(int); /* equivalent */
```

这种方式的好处在于可以用于泛型编程:
```cpp
template<typename T>
    using Vector = std::vector<T, My_allocator<T>>;
```

## 复合数据类型: 指针, 数组和引用

### 通用指针`void*`
通用指针`void*`可以理解为一个指向未知类型数据的指针. 任何已知类型的指针都可以赋值给一个通用指针, 除了函数指针和成员指针.

通用指针可以使用的操作符都不依赖于指向数据类型. 比如, 两个通用指针可以比较是否相等(`ptr1==ptr2`或`ptr1!=ptr2`), 但是不能自增(`ptr++`)或自减(`ptr--`), 因为编译器需要确定数据类型占用的字节大小. 当然更不能解引用(`*ptr`).

使用通用指针访问数据前要显示转换成一个类型的指针. 转换成与原始数据类型不相同的类型, 相当于用不同于原始格式的格式读取, 这是不安全的.

通用指针最初用于传入数据或返回数据不确定的函数, 这些函数通常是系统底层函数.

### 空指针`nullptr`
在C中使用的`NULL`是一个宏定义, 通常是`(void*)0`. 在C++中不能把`NULL`赋给一个已知类型的指针, 否则会发生类型错误, 因为C++中通用指针不会隐式转换成对应类型指针.

`nullptr`是C++中的一个字面量, 表示不指向任何位置的指针, 这个字面量将由编译器在编译时处理为对应类型指针的`0`值. 

### 字符串字面量/字符串常量(String literal)
从C++11开始, 字符串字面量的类型是`const char*`, 下面这个语句是错误的:
```cpp
char* p = "Plato"; // error, but accepted in pre-C++11-standard code
```
因为它把一个`const char*`变量赋给了一个`char*`变量. 从系统底层的角度来理解, 程序中的字符串常量都被储存在可执行文件的`.rodata`节, 运行时被加载到只读层, 因此是不能被改变的. 也因为这个原因, 字符串常量在整个程序运行周期是一直存在的, 所以可以作为返回值.

### 原始字符串
当字符串中含有`\`和`"`, 编写就会比较麻烦, 为了避免由此引发的错误, C++提供了原始字符串机制:
```cpp
string s = R"(\w\\w)"; // the string is \w\\w
string s1 = R"("quoted string")" // the string is "quoted string"
```
`R"(ccc)"`即为字符串`ccc`. `"(`和`)"`只是默认的分隔符, 如果字符串中含有`"(`或`)"`, 可以在`(`前和`)`后添加更多标识符:
```cpp
string s2 = R"∗∗∗("quoted string containing the usual terminator ("))")∗∗∗"
// "quoted string containing the usual terminator ("))"
```
这个原始字符串的分隔符是`"***(`和`)***"`.

正则表达式(`regex`标准库)使用原始字符串编写更简单.

### 大字符集字符串
```cpp
"folder\\file" // implementation character set string
R"(folder\file)" // implementation character raw set string
u8"folder\\file" // UTF-8 string
u8R"(folder\file)" // UTF-8 raw string
u"folder\\file" // UTF-16 string
uR"(folder\file)" // UTF-16 raw string
U"folder\\file" // UTF-32 string
UR"(folder\file)" // UTF-32 raw string
```

### 数组名与指针
在C/C++中, 数组名可以当作一个指向首元素的指针使用, 在大多数情况下, 可以认为这是一个常量指针(`* const`, 即不能改变所指位置的指针). 也就是说, 不能对数组名作自增或自减, 也不能赋值给数组名.

但是, 数组名与常量指针还是不同的. 数组名只是一个编译时的识别符(identifier), 在运行时不存在一个真实指针, 而常量指针在内存上占据实际空间.

一个明显的不同是C/C++的`sizeof`操作符:
```cpp
char array[20];
char* const ptr = array;
cout << sizeof(array) // 20
<< sizeof(ptr); // 4 in 32-bits, 8 in 64-bits
```
`sizeof`对数组名的结果是数组所占的字节数, 而对常量指针则是这个指针所占的字节数.

### 数组下标
使用较早的编译器编译时, 使用指针对数组遍历通常比使用下标更快:
```cpp
// index version
void fi(char v[])
{
    for (int i = 0; v[i]!=0; ++i)
        use(v[i]);
}
// pointer version
void fp(char v[])
{
    for (char∗ p = v; ∗p!=0; ++p)
        use(∗p);
}
```
这是因为使用指针遍历时, 避免了每次验证都要计算新地址(`v+i`). 现代编译器在这一点上做了优化, 两种写法的编译结果应该是一致的, 所以可以任选一种.

以下几种访问数组`a`的第`i`个元素的写法都是等价的:
```cpp
a[i] == ∗(&a[0]+i) == ∗(a+i) == ∗(i+a) == i[a]
```
`a[i] == i[a]`, 但是应该选择更有语义的写法.

### 传递数组作为参数
在机器层面, 传递参数只是把数据放在一个指定寄存器(或栈上), 因此不可能传递整个数组. 实际上, 当数组作为参数时, 只是传递了它的首地址. 比如:
```cpp
void comp(double arg[10]) // arg is a double*
{
    for (int i=0; i!=10; ++i)
        arg[i]+=99;
}
```
函数定义中的数组长度没有实际作用, 编译器不会检查传入参数是否为长度为10的`double`数组, 任何`double *`类型的变量都可以作为参数传入:
```cpp
void f()
{
    double a1[10];
    double a2[5];
    double a3[100];
    comp(a1);
    comp(a2); // disaster!
    comp(a3); // uses only the first 10 elements
};
```

对于多维数组来说, 第一个维度的长度会被忽略, 但是其他维度的长度是必须的. 因为每一个维度的定位需要其后所有维度的长度, 举例来说:
```cpp
void print_mi5(int m[][5], int dim1) // correct
{
    for (int i = 0; i!=dim1; i++) {
        for (int j = 0; j!=5; j++)
            cout << m[i][j] << '\t';
            cout << '\n';
    }
}
void print_mij(int m[][], int dim1, int dim2) // doesn’t behave as most people would think
{
    for (int i = 0; i!=dim1; i++) {
        for (int j = 0; j!=dim2; j++)
            cout << m[i][j] << '\t'; // surprise!
        cout << '\n';
    }
}
```
第二个函数会编译错误, `m[][]`不能作为一个参数传入. 那么把函数定义改成`int** m`呢? 这样可以通过编译, 但是会产生非预期的效果: `m[i][j]`会被当作`*(*(m+i)+j)`! 也就是说, 从`m`所指位置后的第`i`个位置取值, 把这个值当作地址, 再从这个地址往后第`j`个位置取值.
> 注: 这是作者在书中的说法. 我编译这段程序时, 编译器报错不允许对`int**`变量做`[][]`操作.

一个正确的写法是:
```cpp
void print_mij(int∗ m, int dim1, int dim2)
{
    for (int i = 0; i!=dim1; i++) {
        for (int j = 0; j!=dim2; j++)
            cout << m[i∗dim2+j] << '\t'; // obscure
        cout << '\n';
    }
}
```

更好的做法是, 不要使用内置数组传递参数, 把数组放在一个类中, 因为类保存了成员的信息. `std::array`容器就是这么做的. 或者把需要数组参数的函数定义为成员函数, 比如`std::string`和`std::vector`类提供的许多函数.

### 指针和`const`: 常量指针, 指向常量的指针
`const`表示变量在运行时不可修改. 它更强调不能通过这个标识符更改值, 并非这个内存区域不能更改, 这体现在函数参数:
```cpp
void g(const X∗ p)
{
    // can’t modify *p here
}
void h()
{
    X val; // val can be modified here
    g(&val);
    // ...
}
```

当使用指针时, 实际上有两个相关的内存区域: 指针本身所占内存和指针所指向的地址. 当`const`在`*`之前, 指针所指地址是不可修改的, 也就是说不能通过这个指针修改所指地址的值. 当`const`在`*`之后, 指针本身是不可修改的, 不能修改指向的地址.
```cpp
void f1(char∗ p)
{
    char s[] = "Gorm";
    const char∗ pc = s; // pointer to constant
    pc[3] = 'g'; // error : pc points to constant
    pc = p; // OK
    char ∗const cp = s; // constant pointer
    cp[3] = 'a'; // OK
    cp = p; // error : cp is constant
    const char ∗const cpc = s; // const pointer to const
    cpc[3] = 'a'; // error : cpc points to constant
    cpc = p; // error : cpc is constant
}
```
一个便于记忆的方法是从右到左读: `const char*`和`char const*`读成"pointer to a const char(指向字符常量的指针)"; `char* const`读成"const pointer to a char(指向字符的常量指针)".

某些接受指针参数的函数为了保证不会对所指区域产生副作用, 在声明中把指针声明为指向常量的指针.

### 引用(References)
使用指针会有一些问题, 比如: 忘记检查指针是否为`nullptr`.

C++提供了另一个机制, **引用(Reference)**. 它和指针的区别包括但不限于: 
1. 引用的语法和直接使用变量名完全相同
2. 引用永远指向同一个值, 也就是初始值. 因此引用必须初始化.
3. 没有空引用

根据引用在左右值(lvalue/rvalue, 即是否有地址)和常变量(const/non-const)上的区别, 引用可以分为三类:
1. 左值引用(lvalue references): 可以改变值的引用
2. 常量引用(const references): 不能改变值的引用
3. 右值引用(rvalue references): 使用后不需要维护的引用, 比如对临时值的引用

### 左值引用(lvalue references)
`X&`是类型`X`的左值引用类型. 示例:
```cpp
void f()
{
    int var = 1;
    int& r {var}; // r and var now refer to the same int
    int x = r; // x becomes 1
    r = 2; // var becomes 2
}
```
编译上面的代码得到:
```x86asm
movl   $0x1,-0x18(%rbp) ;int var = 1
lea    -0x18(%rbp),%rax 
mov    %rax,-0x10(%rbp) ;int& r {var}
mov    -0x10(%rbp),%rax
mov    (%rax),%eax
mov    %eax,-0x14(%rbp) ;int x = r
mov    -0x10(%rbp),%rax
movl   $0x2,(%rax) ;r = 2
```
可以看到, 在汇编层面引用与指针相同, 只是编译器自动完成解引用.

在语法层面, 对引用`r`的任何操作都等同于对变量`var`直接操作. 首先, 自增(`++`)自减(`--`)这类操作只会改变引用的变量, 不会像指针一样改变指向的地址.

其次, 取值(`&`)会得到引用变量的地址, 而不会得到引用本身的地址. 虽然在这个例子中, 引用本身确实占用了内存(实际上是一个指针), 但是语法保证了编译器永远不会取一个引用的地址(有时编译器会把引用优化为原变量,这时引用就不占用任何空间了). 因此, 我们可以把引用看作一个编译时的别名, 不占用内存空间. 另外, 这也表明**没有指向引用的指针**.

### 常量引用(const references)
`const X&`是类型`X`的常量引用类型. 常量引用和左值引用几乎相同, 除了:
1. 不能通过常量引用改变引用的变量
2. 可以用常量或临时变量初始化, 比如:
   ```cpp
   double& dr = 1; // error : lvalue needed
   const double& cdr {1}; // OK
   ```

### 右值引用(rvalue references)
右值引用只能引用一个右值, 即临时值.
```cpp
string var {"Cambridge"};
string f();
string& r1 {var}; // lvalue reference, bind r1 to var (an lvalue)
string& r2 {f()}; // lvalue reference, error : f() is an rvalue
string& r3 {"Princeton"}; // lvalue reference, error : cannot bind to temporar y
string&& rr1 {f()}; // rvalue reference, fine: bind rr1 to rvalue (a temporar y)
string&& rr2 {var}; // rvalue reference, error : var is an lvalue
string&& rr3 {"Oxford"}; // rr3 refers to a temporar y holding "Oxford"
const string cr1& {"Harvard"}; // OK: make temporar y and bind to cr1
```
