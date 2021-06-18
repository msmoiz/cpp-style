# C++ Style

This is a remix of the ubiquitous [Google C++ style guide](https://google.github.io/styleguide/cppguide.html). It has been tweaked to taste, keeping in mind the guiding principles of readability, expressiveness, and consistency. Some of the ensuing rules simply flow from preference, but rationales are provided periodically as well. Use it if you find it fits your needs.

## C++ Version

New code should target C++17.

## Naming Conventions

Read this carefully. Though buried towards the bottom of their original guide, Google notes that *the most important consistency rules are those that govern naming*. When it comes to naming, a paraphrase of the old adage is apt: "A rule for every name, and every name following its rule." The conventions below are designed to provide each group of constructs with a unique and easily discernable style, with no room for conflicts.

### File Names

Files should contain only lowercase letters and underscores (`_`). This is consistent with the standard library. Source files should have the *.cpp* extension, and header files should have the *.h* extension.

```shell
foo_bar.h
boo_baz.cpp
```

### Type Names

Types are Pascal-cased, with the first letter of each word capitalized. No underscores. This applies to types of every kind, including classes and structs, enumerations, type template parameters, and aliases.

```cpp
class Foo;
struct FooBar;
enum class BazPolicy : uint8 { Val, Vax };
template<typename Value>...;
using Zoo = Foo;
```

### Variable Names

Variable names are tricky. The following rules are designed to indicate source and avoid shadowing.

#### Casing

All variables are all lowercase, with words separated by underscores (`_`).

#### Globals

Global variables should be prefixed with `g_`. This does not apply to variables declared within named namespaces.

```cpp
int g_foo;
```

#### Members

Member variables should be suffixed with `_`, irrespective of access level. This prevents collisions with functions and function parameters.

```cpp
class Foo
{
  int boo_;
  char baz_;
}
```

#### Parameters

Parameter variables should have no suffix or prefix.

```cpp
void foo(int boo, char baz) { ... }
```

### Function Names

Functions should be all lowercase, with words separated by underscores (`_`).

```cpp
void boo(...)
```

### Namespace Names

Namespaces should be all lowercase, with words separated by underscores (`_`).

```cpp
namespace coo { ... }
```

### Macro Names

Macros should be all uppercase, with words separated by underscores (`_`).

```cpp
#define UGLY_MACRO(...)
```

## Headers

Every header should contain a `#pragma once` directive. Include what you use, and do not rely on transitive inclusions. Forward declare everything you can, as this can significantly improve compile times.

### Order of Includes

Include headers in the following order:

1. Matching header
2. C system headers (e.g. `<unistd.h>`)
3. C++ standard library headers (e.g. `<iostream>`)
4. Other library headers
5. Project headers
6. Conditional headers

## Functions

The following guidelines apply to the structure and uses of functions.

### Inputs and Outputs

Prefer return values over output parameters. They improve readability, and often provide the same or better performance. For example:

```cpp
int calculate(int a, int b); // good
void calculate(int a, int b, int& ret); // bad
```

Prefer to return by value or, failing that, return by reference. Avoid returning a pointer unless it can be null.

Parameters are either inputs to the function, outputs from the function, or both. Non-optional input parameters should usually be values or const references, while non-optional output and input/output parameters should usually be references (which cannot be null). Use non-const pointers to represent optional outputs and optional input/output parameters.

Avoid defining functions that require a const reference parameter to outlive the call, because const reference parameters bind to temporaries. Instead, find a way to eliminate the lifetime requirement (for example, by copying the parameter), or pass it by const pointer and document the lifetime and non-null requirements.

When ordering function parameters, put all input-only parameters before any output parameters. In particular, do not add new parameters to the end of the function just because they are new; place new input-only parameters before the output parameters. This is not a hard-and-fast rule. Parameters that are both input and output muddy the waters, and, as always, consistency with related functions may require you to bend the rule. Variadic functions may also require unusual parameter ordering.

### Write Short Functions

Prefer small and focused functions.

We recognize that long functions are sometimes appropriate, so no hard limit is placed on functions length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program. Even if your long function works perfectly now, someone modifying it in a few months may add new behavior. This could result in bugs that are hard to find. Keeping your functions short and simple makes it easier for other people to read and modify your code. Small functions are also easier to test.

You could find long and complicated functions when working with some code. Do not be intimidated by modifying existing code: if working with such a function proves to be difficult, you find that errors are hard to debug, or you want to use a piece of it in several different contexts, consider breaking up the function into smaller and more manageable pieces.

### Function Overloading

Use overloaded functions (including constructors) only if a reader looking at a call site can get a good idea of what is happening without having to first figure out exactly which overload is being called.

You may write a function that takes a `const std::string&` and overload it with another that takes `const char*`. However, in this case consider `std::string_view` instead.

```cpp
class MyClass 
{
 public:
  void analyze(const std::string &text);
  void analyze(const char *text, size_t textlen);
};
```

Overloading can make code more intuitive by allowing an identically-named function to take different arguments. It may be necessary for templatized code, and it can be convenient for visitors. Overloading based on `const` or ref qualification may make utility code more usable, more efficient, or both. You may overload a function when there are no semantic differences between variants. These overloads may vary in types, qualifiers, or argument count. However, a reader of such a call must not need to know which member of the overload set is chosen, only that something from the set is being called. If you can document all entries in the overload set with a single comment in the header, that is a good sign that it is a well-designed overload set.

### Default Arguments

Default arguments are allowed on non-virtual functions when the default is guaranteed to always have the same value. Follow the same restrictions as for function overloading, and prefer overloaded functions if the readability gained with default arguments doesn't outweigh the downsides. In some cases, default arguments can improve the readability of their function declarations enough to overcome the downsides, so they are allowed. When in doubt, use overloads.

### Trailing Return Type

Use trailing return types when using a leading return type is impractical, when the return type depends on the function parameters, or when writing a lambda. The trailing return type syntax, introduced in C++11, uses the auto keyword before the function name and a trailing return type after the argument list. For example:

```cpp
auto foo(int x) -> int { ... }
auto foo = [](int x) -> int { ... };
```

### Use Lambdas

Lambdas are fantastic, and should be used in most contexts where a function would be used in a single scope. For example, they should be used when passing custom sort logic to a standard library algorithm or as a predicate to find operations.

#### Capture Explicitly

Never use the implicit capture syntax. Instead, always explicitly specify captures by name.

```cpp
auto foo = [=]() { ... }; // bad
auto bar = [&]() { ... }; // bad
auto baz = [this, &a, b]() { ... }; // good
```

## Classes

Classes are the fundamental unit of code in C++. Naturally, we use them extensively.

### When to Use a Struct or a Class

Use a `struct` only for passive objects that carry data. Everything else is a `class`. Structs should be used for passive objects that carry data, and may have associated constants. All fields must be public. The struct must not have invariants that imply relationships between different fields, since direct user access to those fields may break those invariants. Constructors, destructors, and helper methods may be present; however, these methods must not require or enforce any invariants.

If more functionality or invariants are required, a class is more appropriate. If in doubt, make it a class.

### Doing Work in Constructors

Work in a constructor should be easy to follow. Do not call virtual methods. Do not conduct initialization that can fail, if you cannot signal an error. That said, it is generally obtuse to use `init` functions as an alternative to initialize partially constructed objects.

### Explicit Conversions

Do not define implicit conversions. Single-argument constructors and conversion operators should be marked `explicit`.

### Copy and Move

A class's public API must make clear whether the class is copyable, move-only, or neither copyable nor movable. Support copying and/or moving if these operations are clear and meaningful for your type. This should usually take the form of explicitly declaring and/or deleting the appropriate operations in the public section of the declaration. Specifically, a copyable class should explicitly declare the copy operations, a move-only class should explicitly declare the move operations, and a non-copyable/movable class should explicitly delete the copy operations. A copyable class may also declare move operations in order to support efficient moves. Explicitly declaring or deleting all four copy/move operations is permitted, but not required. If you provide a copy or move assignment operator, you must also provide the corresponding constructor.

A type should not be copyable/movable if the meaning of copying/moving is unclear to a casual user, or if it incurs unexpected costs. Move operations for copyable types are strictly a performance optimization and are a potential source of bugs and complexity, so avoid defining them unless they are significantly more efficient than the corresponding copy operations. If your type provides copy operations, it is recommended that you design your class so that the default implementation of those operations is correct. Remember to review the correctness of any defaulted operations as you would any other code.

### Declaration Order

Every class should have only three sections, one for each access level - i.e. `public`, `protected`, `protected`, in that order. Empty sections should be omitted. Within each section, elements of a class should be declared in the following order:

1. Nested types
2. Type aliases
3. Constants
4. Factory functions
5. Special functions
6. Other member functions
7. Member variables

Related elements should be grouped together.

## Comments

Use multi-line comment syntax (`/**/`) to document interfaces, and single-line comment syntax (`//`) to document implementations.
