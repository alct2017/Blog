# Basic Facilities
Part II of *The C++ Programming Language* by *Bjarne Stroustrup*

## Types

### Fundamental Types

Simple type:
1. Boolean type
2. Character types
3. Integer types
4. Floating-point types
5. `void` type

Complex type:
1. Pointer types
2. Array types
3. Reference types

User-defined types:
1. Data structures and classes
2. Enumeration types

### Declarations

The structrue of declarations is
`<prefix specifiers>[base type][declarator]<suffix function specifiers><initializer|function body>`

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
*List initialization*