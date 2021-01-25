# Basic Facilities
Part II of *The C++ Programming Language* by *Bjarne Stroustrup*

## Types

### Fundamental Types

1. Simple type:
   1. Boolean type
   2. Character types
   3. Integer types
   4. Floating-point types
   5. `void` type

2. Complex type:
   1. Pointer types
   2. Array types
   3. Reference types

3. User-defined types:
   1. Data structures and classes
   2. Enumeration types

### Declarations

The structrue of declarations is
```
<prefix specifiers>[base type][declarator]<suffix function specifiers><initializer|function body>
```

1. `<prefix specifiers>`: `virtual`, `extern`, `constexpr`, ...
2. `[base type]` is a type above
3. `[declarator]` is composed of a name, and optionally some declarator operators including

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

### Initialization
*List initialization* is way of initialization differ from C.
```cpp
data_t a1 {v}; /* Recommended, from C++11 */
data_t a2 = {v};
data_t a3 = v;
data_t a4(v);
```

An advantage of list initialization is that it does not allow **narrowing**, or **type casting**. For example:
```cpp
void f(double f_val,int i_val)
{
    int x1 = f_val; // OK, x1 will truncation
    int x2 {f_val}; // error: possible truncation

    char c1 = i_val; // OK, equals that c1 = i_val & 0xff (auto casting)
    char c2 = {i_val}; // error: possible narrowing
}
```
Always using list initialization will avoid undefined behavior. Only use `=` when using `auto`.

### Deducing a Type: `auto` and `decltype()`
The best practice of `auto` type is using for iterator in a loop. For example:
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
It's recommended to use `=` rather than `{}` for objects spcified `auto`.

`decltype()` is mostly useful in generic programming. For example:
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

### Type Aliases
There is one more way to aliase a type than in C.
```cpp
typedef int data_t;
using data_t = int; /* equivalent */

typedef void(*func_t)(int);
using func_t = void(*)(int); /* equivalent */
```

A useful way of `using` is to introduce a template alias. For example:
```cpp
template<typename T>
    using Vector = std::vector<T, My_allocator<T>>;
```
