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

Functions can be powerful when used properly.

### Inputs and Outputs

The output of a C++ function is naturally provided via a return value and sometimes via output parameters (or in/out parameters).

Prefer using return values over output parameters: they improve readability, and often provide the same or better performance.

Prefer to return by value or, failing that, return by reference. Avoid returning a pointer unless it can be null.

Parameters are either inputs to the function, outputs from the function, or both. Non-optional input parameters should usually be values or const references, while non-optional output and input/output parameters should usually be references (which cannot be null). Generally, use absl::optional to represent optional by-value inputs, and use a const pointer when the non-optional form would have used a reference. Use non-const pointers to represent optional outputs and optional input/output parameters.

Avoid defining functions that require a const reference parameter to outlive the call, because const reference parameters bind to temporaries. Instead, find a way to eliminate the lifetime requirement (for example, by copying the parameter), or pass it by const pointer and document the lifetime and non-null requirements.

When ordering function parameters, put all input-only parameters before any output parameters. In particular, do not add new parameters to the end of the function just because they are new; place new input-only parameters before the output parameters. This is not a hard-and-fast rule. Parameters that are both input and output muddy the waters, and, as always, consistency with related functions may require you to bend the rule. Variadic functions may also require unusual parameter ordering.
Write Short Functions

Prefer small and focused functions.

We recognize that long functions are sometimes appropriate, so no hard limit is placed on functions length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program.

Even if your long function works perfectly now, someone modifying it in a few months may add new behavior. This could result in bugs that are hard to find. Keeping your functions short and simple makes it easier for other people to read and modify your code. Small functions are also easier to test.

You could find long and complicated functions when working with some code. Do not be intimidated by modifying existing code: if working with such a function proves to be difficult, you find that errors are hard to debug, or you want to use a piece of it in several different contexts, consider breaking up the function into smaller and more manageable pieces.
Function Overloading

Use overloaded functions (including constructors) only if a reader looking at a call site can get a good idea of what is happening without having to first figure out exactly which overload is being called.

You may write a function that takes a const std::string& and overload it with another that takes const char*. However, in this case consider std::string_view instead.

class MyClass {
 public:
  void Analyze(const std::string &text);
  void Analyze(const char *text, size_t textlen);
};

Overloading can make code more intuitive by allowing an identically-named function to take different arguments. It may be necessary for templatized code, and it can be convenient for Visitors.

Overloading based on const or ref qualification may make utility code more usable, more efficient, or both. (See TotW 148 for more.)

If a function is overloaded by the argument types alone, a reader may have to understand C++'s complex matching rules in order to tell what's going on. Also many people are confused by the semantics of inheritance if a derived class overrides only some of the variants of a function.

You may overload a function when there are no semantic differences between variants. These overloads may vary in types, qualifiers, or argument count. However, a reader of such a call must not need to know which member of the overload set is chosen, only that something from the set is being called. If you can document all entries in the overload set with a single comment in the header, that is a good sign that it is a well-designed overload set.
Default Arguments

Default arguments are allowed on non-virtual functions when the default is guaranteed to always have the same value. Follow the same restrictions as for function overloading, and prefer overloaded functions if the readability gained with default arguments doesn't outweigh the downsides below.

Often you have a function that uses default values, but occasionally you want to override the defaults. Default parameters allow an easy way to do this without having to define many functions for the rare exceptions. Compared to overloading the function, default arguments have a cleaner syntax, with less boilerplate and a clearer distinction between 'required' and 'optional' arguments.

Defaulted arguments are another way to achieve the semantics of overloaded functions, so all the reasons not to overload functions apply.

The defaults for arguments in a virtual function call are determined by the static type of the target object, and there's no guarantee that all overrides of a given function declare the same defaults.

Default parameters are re-evaluated at each call site, which can bloat the generated code. Readers may also expect the default's value to be fixed at the declaration instead of varying at each call.

Function pointers are confusing in the presence of default arguments, since the function signature often doesn't match the call signature. Adding function overloads avoids these problems.

Default arguments are banned on virtual functions, where they don't work properly, and in cases where the specified default might not evaluate to the same value depending on when it was evaluated. (For example, don't write void f(int n = counter++);.)

In some other cases, default arguments can improve the readability of their function declarations enough to overcome the downsides above, so they are allowed. When in doubt, use overloads.
Trailing Return Type Syntax

Use trailing return types only where using the ordinary syntax (leading return types) is impractical or much less readable.

C++ allows two different forms of function declarations. In the older form, the return type appears before the function name. For example:

int foo(int x);

The newer form, introduced in C++11, uses the auto keyword before the function name and a trailing return type after the argument list. For example, the declaration above could equivalently be written:

auto foo(int x) -> int;

The trailing return type is in the function's scope. This doesn't make a difference for a simple case like int but it matters for more complicated cases, like types declared in class scope or types written in terms of the function parameters.

Trailing return types are the only way to explicitly specify the return type of a lambda expression. In some cases the compiler is able to deduce a lambda's return type, but not in all cases. Even when the compiler can deduce it automatically, sometimes specifying it explicitly would be clearer for readers.

Sometimes it's easier and more readable to specify a return type after the function's parameter list has already appeared. This is particularly true when the return type depends on template parameters. For example:

    template <typename T, typename U>
    auto add(T t, U u) -> decltype(t + u);
  

versus

    template <typename T, typename U>
    decltype(declval<T&>() + declval<U&>()) add(T t, U u);
  

Trailing return type syntax is relatively new and it has no analogue in C++-like languages such as C and Java, so some readers may find it unfamiliar.

Existing code bases have an enormous number of function declarations that aren't going to get changed to use the new syntax, so the realistic choices are using the old syntax only or using a mixture of the two. Using a single version is better for uniformity of style.

In most cases, continue to use the older style of function declaration where the return type goes before the function name. Use the new trailing-return-type form only in cases where it's required (such as lambdas) or where, by putting the type after the function's parameter list, it allows you to write the type in a much more readable way. The latter case should be rare; it's mostly an issue in fairly complicated template code, which is discouraged in most cases.

## Comments

Use multi-line comment syntax (`/**/`) to document interfaces, and single-line comment syntax (`//`) to document implementations.
