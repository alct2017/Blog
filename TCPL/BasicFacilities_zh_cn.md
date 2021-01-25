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
