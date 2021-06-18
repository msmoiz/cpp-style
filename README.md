# C++ Style

This is a remix of the ubiquitous [Google C++ style guide](https://google.github.io/styleguide/cppguide.html). It has been tweaked to taste, keeping in mind the guiding principles of readability, expressiveness, and consistency. Some of the ensuing rules simply flow from preference, but rationales are provided periodically as well. Use it if you find it fits your needs.

## C++ Version

New code should target C++17.

## Naming Conventions

Read this carefully. Though buried towards the bottom of their original guide, Google notes that *the most important consistency rules are those that govern naming*. When it comes to naming, a paraphrase of the old adage is apt: "A rule for every name, and every name following its rule." The conventions below are designed to provide each group of constructs with a unique and easily discernable style, with no room for conflicts.

### Files

Files should contain only lowercase letters and underscores (`_`). This is consistent with the standard library. Source files should have the *.cpp* extension, and header files should have the *.h* extension.

```shell
foo_bar.h
boo_baz.cpp
```

### Types

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

### Functions

Functions should be all lowercase, with words separated by underscores (`_`).

```cpp
void boo(...)
```

### Namespaces

Namespaces should be all lowercase, with words separated by underscores (`_`).

```cpp
namespace coo { ... }
```

### Macros

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

## Comments

Use multi-line comment syntax (`/**/`) to document interfaces, and single-line comment syntax (`//`) to document implementations.
