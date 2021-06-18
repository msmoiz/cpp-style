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
namespace coo
{
  ...
}
```

### Macros

Macros should be all uppercase, with words separated by underscors (`_`).

```cpp
#define UGLY_MACRO(...)
```


























<h2 id="Comments">Comments</h2>

<p>Comments are absolutely vital to keeping our code readable. The following rules describe what you
should comment and where. But remember: while comments are very important, the best code is
self-documenting. Giving sensible names to types and variables is much better than using obscure
names that you must then explain through comments.</p>

<p>When writing your comments, write for your audience: the
next
contributor who will need to
understand your code. Be generous — the next
one may be you!</p>

<h3 id="Comment_Style">Comment Style</h3>

<p>Use either the <code>//</code> or <code>/* */</code>
syntax, as long as you are consistent.</p>

<p>You can use either the <code>//</code> or the <code>/*
*/</code> syntax; however, <code>//</code> is
<em>much</em> more common. Be consistent with how you
comment and what style you use where.</p>

<h3 id="File_Comments">File Comments</h3>

<div>
<p>Start each file with license boilerplate.</p>
</div>

<p>File comments describe the contents of a file. If a file declares,
implements, or tests exactly one abstraction that is documented by a comment
at the point of declaration, file comments are not required. All other files
must have file comments.</p>

<h4>Legal Notice and Author
Line</h4>



<div>
<p>Every file should contain license
boilerplate. Choose the appropriate boilerplate for the
license used by the project (for example, Apache 2.0,
BSD, LGPL, GPL).</p>
</div>

<p>If you make significant changes to a file with an
author line, consider deleting the author line.
New files should usually not contain copyright notice or
author line.</p>

<h4>File Contents</h4>

<p>If a <code>.h</code> declares multiple abstractions, the file-level comment
should broadly describe the contents of the file, and how the abstractions are
related. A 1 or 2 sentence file-level comment may be sufficient. The detailed
documentation about individual abstractions belongs with those abstractions,
not at the file level.</p>

<p>Do not duplicate comments in both the <code>.h</code> and the
<code>.cc</code>. Duplicated comments diverge.</p>

<h3 id="Class_Comments">Class Comments</h3>

<p>Every non-obvious class or struct declaration should have an
accompanying comment that describes what it is for and how it should
be used.</p>

<pre>// Iterates over the contents of a GargantuanTable.
// Example:
//    std::unique_ptr&lt;GargantuanTableIterator&gt; iter = table-&gt;NewIterator();
//    for (iter-&gt;Seek("foo"); !iter-&gt;done(); iter-&gt;Next()) {
//      process(iter-&gt;key(), iter-&gt;value());
//    }
class GargantuanTableIterator {
  ...
};
</pre>

<p>The class comment should provide the reader with enough information to know
how and when to use the class, as well as any additional considerations
necessary to correctly use the class. Document the synchronization assumptions
the class makes, if any. If an instance of the class can be accessed by
multiple threads, take extra care to document the rules and invariants
surrounding multithreaded use.</p>

<p>The class comment is often a good place for a small example code snippet
demonstrating a simple and focused usage of the class.</p>

<p>When sufficiently separated (e.g., <code>.h</code> and <code>.cc</code>
files), comments describing the use of the class should go together with its
interface definition; comments about the class operation and implementation
should accompany the implementation of the class's methods.</p>

<h3 id="Function_Comments">Function Comments</h3>

<p>Declaration comments describe use of the function (when it is
non-obvious); comments at the definition of a function describe
operation.</p>

<h4>Function Declarations</h4>

<p>Almost every function declaration should have comments immediately
preceding it that describe what the function does and how to use
it. These comments may be omitted only if the function is simple and
obvious (e.g., simple accessors for obvious properties of the class).
Function comments should be written with an implied subject of
<i>This function</i> and should start with the verb phrase; for example,
"Opens the file", rather than "Open the file". In general, these comments do not
describe how the function performs its task. Instead, that should be
left to comments in the function definition.</p>

<p>Types of things to mention in comments at the function
declaration:</p>

<ul>
  <li>What the inputs and outputs are. If function argument names
      are provided in `backticks`, then code-indexing
      tools may be able to present the documentation better.</li>

  <li>For class member functions: whether the object
  remembers reference arguments beyond the duration of
  the method call, and whether it will free them or
  not.</li>

  <li>If the function allocates memory that the caller
  must free.</li>

  <li>Whether any of the arguments can be a null
  pointer.</li>

  <li>If there are any performance implications of how a
  function is used.</li>

  <li>If the function is re-entrant. What are its
  synchronization assumptions?</li>
 </ul>

<p>Here is an example:</p>

<pre>// Returns an iterator for this table, positioned at the first entry
// lexically greater than or equal to `start_word`. If there is no
// such entry, returns a null pointer. The client must not use the
// iterator after the underlying GargantuanTable has been destroyed.
//
// This method is equivalent to:
//    std::unique_ptr&lt;Iterator&gt; iter = table-&gt;NewIterator();
//    iter-&gt;Seek(start_word);
//    return iter;
std::unique_ptr&lt;Iterator&gt; GetIterator(absl::string_view start_word) const;
</pre>

<p>However, do not be unnecessarily verbose or state the
completely obvious.</p>

<p>When documenting function overrides, focus on the
specifics of the override itself, rather than repeating
the comment from the overridden function.  In many of these
cases, the override needs no additional documentation and
thus no comment is required.</p>

<p>When commenting constructors and destructors, remember
that the person reading your code knows what constructors
and destructors are for, so comments that just say
something like "destroys this object" are not useful.
Document what constructors do with their arguments (for
example, if they take ownership of pointers), and what
cleanup the destructor does. If this is trivial, just
skip the comment. It is quite common for destructors not
to have a header comment.</p>

<h4>Function Definitions</h4>

<p>If there is anything tricky about how a function does
its job, the function definition should have an
explanatory comment. For example, in the definition
comment you might describe any coding tricks you use,
give an overview of the steps you go through, or explain
why you chose to implement the function in the way you
did rather than using a viable alternative. For instance,
you might mention why it must acquire a lock for the
first half of the function but why it is not needed for
the second half.</p>

<p>Note you should <em>not</em> just repeat the comments
given with the function declaration, in the
<code>.h</code> file or wherever. It's okay to
recapitulate briefly what the function does, but the
focus of the comments should be on how it does it.</p>

<h3 id="Variable_Comments">Variable Comments</h3>

<p>In general the actual name of the variable should be
descriptive enough to give a good idea of what the variable
is used for. In certain cases, more comments are required.</p>

<h4>Class Data Members</h4>

<p>The purpose of each class data member (also called an instance
variable or member variable) must be clear. If there are any
invariants (special values, relationships between members, lifetime
requirements) not clearly expressed by the type and name, they must be
commented. However, if the type and name suffice (<code>int
num_events_;</code>), no comment is needed.</p>

<p>In particular, add comments to describe the existence and meaning
of sentinel values, such as nullptr or -1, when they are not
obvious. For example:</p>

<pre>private:
 // Used to bounds-check table accesses. -1 means
 // that we don't yet know how many entries the table has.
 int num_total_entries_;
</pre>

<h4>Global Variables</h4>

<p>All global variables should have a comment describing what they
are, what they are used for, and (if unclear) why it needs to be
global. For example:</p>

<pre>// The total number of test cases that we run through in this regression test.
const int kNumTestCases = 6;
</pre>

<h3 id="Implementation_Comments">Implementation Comments</h3>

<p>In your implementation you should have comments in tricky,
non-obvious, interesting, or important parts of your code.</p>

<h4>Explanatory Comments</h4>

<p>Tricky or complicated code blocks should have comments
before them. Example:</p>

<pre>// Divide result by two, taking into account that x
// contains the carry from the add.
for (int i = 0; i &lt; result-&gt;size(); ++i) {
  x = (x &lt;&lt; 8) + (*result)[i];
  (*result)[i] = x &gt;&gt; 1;
  x &amp;= 1;
}
</pre>

<h4>Line-end Comments</h4>

<p>Also, lines that are non-obvious should get a comment
at the end of the line. These end-of-line comments should
be separated from the code by 2 spaces. Example:</p>

<pre>// If we have enough memory, mmap the data portion too.
mmap_budget = max&lt;int64_t&gt;(0, mmap_budget - index_-&gt;length());
if (mmap_budget &gt;= data_size_ &amp;&amp; !MmapData(mmap_chunk_bytes, mlock))
  return;  // Error already logged.
</pre>

<p>Note that there are both comments that describe what
the code is doing, and comments that mention that an
error has already been logged when the function
returns.</p>

<h4 class="stylepoint_subsection" id="Function_Argument_Comments">Function Argument Comments</h4>

<p>When the meaning of a function argument is nonobvious, consider
one of the following remedies:</p>

<ul>
  <li>If the argument is a literal constant, and the same constant is
  used in multiple function calls in a way that tacitly assumes they're
  the same, you should use a named constant to make that constraint
  explicit, and to guarantee that it holds.</li>

  <li>Consider changing the function signature to replace a <code>bool</code>
  argument with an <code>enum</code> argument. This will make the argument
  values self-describing.</li>

  <li>For functions that have several configuration options, consider
  defining a single class or struct to hold all the options
  ,
  and pass an instance of that.
  This approach has several advantages. Options are referenced by name
  at the call site, which clarifies their meaning. It also reduces
  function argument count, which makes function calls easier to read and
  write. As an added benefit, you don't have to change call sites when
  you add another option.
  </li>

  <li>Replace large or complex nested expressions with named variables.</li>

  <li>As a last resort, use comments to clarify argument meanings at the
  call site. </li>
</ul>

Consider the following example:

<pre class="badcode">// What are these arguments?
const DecimalNumber product = CalculateProduct(values, 7, false, nullptr);
</pre>

<p>versus:</p>

<pre>ProductOptions options;
options.set_precision_decimals(7);
options.set_use_cache(ProductOptions::kDontUseCache);
const DecimalNumber product =
    CalculateProduct(values, options, /*completion_callback=*/nullptr);
</pre>

<h4 id="Implementation_Comment_Donts">Don'ts</h4>

<p>Do not state the obvious. In particular, don't literally describe what
code does, unless the behavior is nonobvious to a reader who understands
C++ well. Instead, provide higher level comments that describe <i>why</i>
the code does what it does, or make the code self describing.</p>

Compare this:

<pre class="badcode">// Find the element in the vector.  &lt;-- Bad: obvious!
if (std::find(v.begin(), v.end(), element) != v.end()) {
  Process(element);
}
</pre>

To this:

<pre>// Process "element" unless it was already processed.
if (std::find(v.begin(), v.end(), element) != v.end()) {
  Process(element);
}
</pre>

Self-describing code doesn't need a comment. The comment from
the example above would be obvious:

<pre>if (!IsAlreadyProcessed(element)) {
  Process(element);
}
</pre>

<h3 id="Punctuation,_Spelling_and_Grammar">Punctuation, Spelling, and Grammar</h3>

<p>Pay attention to punctuation, spelling, and grammar; it is
easier to read well-written comments than badly written
ones.</p>

<p>Comments should be as readable as narrative text, with
proper capitalization and punctuation. In many cases,
complete sentences are more readable than sentence
fragments. Shorter comments, such as comments at the end
of a line of code, can sometimes be less formal, but you
should be consistent with your style.</p>

<p>Although it can be frustrating to have a code reviewer
point out that you are using a comma when you should be
using a semicolon, it is very important that source code
maintain a high level of clarity and readability. Proper
punctuation, spelling, and grammar help with that
goal.</p>

<h3 id="TODO_Comments">TODO Comments</h3>

<p>Use <code>TODO</code> comments for code that is temporary,
a short-term solution, or good-enough but not perfect.</p>

<p><code>TODO</code>s should include the string
<code>TODO</code> in all caps, followed by the

name, e-mail address, bug ID, or other
identifier
of the person or issue with the best context
about the problem referenced by the <code>TODO</code>. The
main purpose is to have a consistent <code>TODO</code> that
can be searched to find out how to get more details upon
request. A <code>TODO</code> is not a commitment that the
person referenced will fix the problem. Thus when you create
a <code>TODO</code> with a name, it is almost always your
name that is given.</p>



<div>
<pre>// TODO(kl@gmail.com): Use a "*" here for concatenation operator.
// TODO(Zeke) change this to use relations.
// TODO(bug 12345): remove the "Last visitors" feature.
</pre>
</div>

<p>If your <code>TODO</code> is of the form "At a future
date do something" make sure that you either include a
very specific date ("Fix by November 2005") or a very
specific event ("Remove this code when all clients can
handle XML responses.").</p>

<h2 id="Formatting">Formatting</h2>

<p>Coding style and formatting are pretty arbitrary, but a

project is much easier to follow
if everyone uses the same style. Individuals may not agree with every
aspect of the formatting rules, and some of the rules may take
some getting used to, but it is important that all

project contributors follow the
style rules so that
they can all read and understand
everyone's code easily.</p>

































## Header Files

<h2 id="Header_Files">Header Files</h2>

<p>In general, every <code>.cc</code> file should have an
associated <code>.h</code> file. There are some common
exceptions, such as unit tests and small <code>.cc</code> files containing
just a <code>main()</code> function.</p>

<p>Correct use of header files can make a huge difference to
the readability, size and performance of your code.</p>

<p>The following rules will guide you through the various
pitfalls of using header files.</p>

<a id="The_-inl.h_Files"></a>
<h3 id="Self_contained_Headers">Self-contained Headers</h3>

<p>Header files should be self-contained (compile on their own) and
end in <code>.h</code>.  Non-header files that are meant for inclusion
should end in <code>.inc</code> and be used sparingly.</p>

<p>All header files should be self-contained. Users and refactoring
tools should not have to adhere to special conditions to include the
header. Specifically, a header should
have <a href="#The__define_Guard">header guards</a> and include all
other headers it needs.</p>

<p>Prefer placing the definitions for template and inline functions in
the same file as their declarations.  The definitions of these
constructs must be included into every <code>.cc</code> file that uses
them, or the program may fail to link in some build configurations.  If
declarations and definitions are in different files, including the
former should transitively include the latter.  Do not move these
definitions to separately included header files (<code>-inl.h</code>);
this practice was common in the past, but is no longer allowed.</p>

<p>As an exception, a template that is explicitly instantiated for
all relevant sets of template arguments, or that is a private
implementation detail of a class, is allowed to be defined in the one
and only <code>.cc</code> file that instantiates the template.</p>

<p>There are rare cases where a file designed to be included is not
self-contained.  These are typically intended to be included at unusual
locations, such as the middle of another file.  They might not
use <a href="#The__define_Guard">header guards</a>, and might not include
their prerequisites.  Name such files with the <code>.inc</code>
extension.  Use sparingly, and prefer self-contained headers when
possible.</p>

<h3 id="The__define_Guard">The #define Guard</h3>

<p>All header files should have <code>#define</code> guards to
prevent multiple inclusion. The format of the symbol name
should be

<code><i>&lt;PROJECT&gt;</i>_<i>&lt;PATH&gt;</i>_<i>&lt;FILE&gt;</i>_H_</code>.</p>



<div>
<p>To guarantee uniqueness, they should
be based on the full path in a project's source tree. For
example, the file <code>foo/src/bar/baz.h</code> in
project <code>foo</code> should have the following
guard:</p>
</div>

<pre>#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_

...

#endif  // FOO_BAR_BAZ_H_
</pre>



<h3 id="Include_What_You_Use">Include What You Use</h3>

<p>If a source or header file refers to a symbol defined elsewhere,
the file should directly include a header file which properly intends
to provide a declaration or definition of that symbol. It should not
include header files for any other reason.
</p>

<p>Do not rely on transitive inclusions. This allows people to remove
no-longer-needed <code>#include</code> statements from their headers without
breaking clients. This also applies to related headers
- <code>foo.cc</code> should include <code>bar.h</code> if it uses a
symbol from it even if <code>foo.h</code>
includes <code>bar.h</code>.</p>

<h3 id="Forward_Declarations">Forward Declarations</h3>

<p>Avoid using forward declarations where possible.
  Instead, <a href="#Include_What_You_Use">include the headers you need</a>.
</p>


<p class="definition"></p>
<p>A "forward declaration" is a declaration of an entity
  without an associated definition.</p>
<pre>// In a C++ source file:
class B;
void FuncInB();
extern int variable_in_b;
ABSL_DECLARE_FLAG(flag_in_b);
</pre>

<p class="pros"></p>
<ul>
  <li>Forward declarations can save compile time, as
  <code>#include</code>s force the compiler to open
  more files and process more input.</li>

  <li>Forward declarations can save on unnecessary
  recompilation. <code>#include</code>s can force
  your code to be recompiled more often, due to unrelated
  changes in the header.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>Forward declarations can hide a dependency, allowing
  user code to skip necessary recompilation when headers
  change.</li>

  <li>A forward declaration as opposed to an #include statement
    makes it difficult for automatic tooling to discover the module
    defining the symbol.</li>

  <li>A forward declaration may be broken by subsequent
  changes to the library. Forward declarations of functions
  and templates can prevent the header owners from making
  otherwise-compatible changes to their APIs, such as
  widening a parameter type, adding a template parameter
  with a default value, or migrating to a new namespace.</li>

  <li>Forward declaring symbols from namespace
  <code>std::</code> yields undefined behavior.</li>

  <li>It can be difficult to determine whether a forward
  declaration or a full <code>#include</code> is needed.
  Replacing an <code>#include</code> with a forward
  declaration can silently change the meaning of
  code:
<pre>// b.h:
struct B {};
struct D : B {};

// good_user.cc:
#include "b.h"
void f(B*);
void f(void*);
void test(D* x) { f(x); }  // calls f(B*)
</pre>
  If the <code>#include</code> was replaced with forward
  decls for <code>B</code> and <code>D</code>,
  <code>test()</code> would call <code>f(void*)</code>.
  </li>

  <li>Forward declaring multiple symbols from a header
  can be more verbose than simply
  <code>#include</code>ing the header.</li>

  <li>Structuring code to enable forward declarations
  (e.g., using pointer members instead of object members)
  can make the code slower and more complex.</li>


</ul>

<p class="decision"></p>
<p>Try to avoid forward declarations of entities
defined in another project.</p>

<h3 id="Inline_Functions">Inline Functions</h3>

<p>Define functions inline only when they are small, say, 10
lines or fewer.</p>

<p class="definition"></p>
<p>You can declare functions in a way that allows the compiler to expand
them inline rather than calling them through the usual
function call mechanism.</p>

<p class="pros"></p>
<p>Inlining a function can generate more efficient object
code, as long as the inlined function is small. Feel free
to inline accessors and mutators, and other short,
performance-critical functions.</p>

<p class="cons"></p>
<p>Overuse of inlining can actually make programs slower.
Depending on a function's size, inlining it can cause the
code size to increase or decrease. Inlining a very small
accessor function will usually decrease code size while
inlining a very large function can dramatically increase
code size. On modern processors smaller code usually runs
faster due to better use of the instruction cache.</p>

<p class="decision"></p>
<p>A decent rule of thumb is to not inline a function if
it is more than 10 lines long. Beware of destructors,
which are often longer than they appear because of
implicit member- and base-destructor calls!</p>

<p>Another useful rule of thumb: it's typically not cost
effective to inline functions with loops or switch
statements (unless, in the common case, the loop or
switch statement is never executed).</p>

<p>It is important to know that functions are not always
inlined even if they are declared as such; for example,
virtual and recursive functions are not normally inlined.
Usually recursive functions should not be inline. The
main reason for making a virtual function inline is to
place its definition in the class, either for convenience
or to document its behavior, e.g., for accessors and
mutators.</p>

<h3 id="Names_and_Order_of_Includes">Names and Order of Includes</h3>

<p>Include headers in the following order: Related header, C system headers,
C++ standard library headers,
other libraries' headers, your project's
headers.</p>

<p>
All of a project's header files should be
listed as descendants of the project's source
directory without use of UNIX directory aliases
<code>.</code> (the current directory) or <code>..</code>
(the parent directory). For example,

<code>google-awesome-project/src/base/logging.h</code>
should be included as:</p>

<pre>#include "base/logging.h"
</pre>

<p>In <code><var>dir/foo</var>.cc</code> or
<code><var>dir/foo_test</var>.cc</code>, whose main
purpose is to implement or test the stuff in
<code><var>dir2/foo2</var>.h</code>, order your includes
as follows:</p>

<ol>
  <li><code><var>dir2/foo2</var>.h</code>.</li>

  <li>A blank line</li>

  <li>C system headers (more precisely: headers in angle brackets with the
    <code>.h</code> extension), e.g., <code>&lt;unistd.h&gt;</code>,
    <code>&lt;stdlib.h&gt;</code>.</li>

  <li>A blank line</li>

  <li>C++ standard library headers (without file extension), e.g.,
    <code>&lt;algorithm&gt;</code>, <code>&lt;cstddef&gt;</code>.</li>

  <li>A blank line</li>

  <div>
  <li>Other libraries' <code>.h</code> files.</li>
  </div>

  <li>
  Your project's <code>.h</code>
  files.</li>
</ol>

<p>Separate each non-empty group with one blank line.</p>

<p>With the preferred ordering, if the related header
<code><var>dir2/foo2</var>.h</code> omits any necessary
includes, the build of <code><var>dir/foo</var>.cc</code>
or <code><var>dir/foo</var>_test.cc</code> will break.
Thus, this rule ensures that build breaks show up first
for the people working on these files, not for innocent
people in other packages.</p>

<p><code><var>dir/foo</var>.cc</code> and
<code><var>dir2/foo2</var>.h</code> are usually in the same
directory (e.g., <code>base/basictypes_test.cc</code> and
<code>base/basictypes.h</code>), but may sometimes be in different
directories too.</p>



<p>Note that the C headers such as <code>stddef.h</code>
are essentially interchangeable with their C++ counterparts
(<code>cstddef</code>).
Either style is acceptable, but prefer consistency with existing code.</p>

<p>Within each section the includes should be ordered
alphabetically. Note that older code might not conform to
this rule and should be fixed when convenient.</p>

<p>For example, the includes in

<code>google-awesome-project/src/foo/internal/fooserver.cc</code>
might look like this:</p>

<pre>#include "foo/server/fooserver.h"

#include &lt;sys/types.h&gt;
#include &lt;unistd.h&gt;

#include &lt;string&gt;
#include &lt;vector&gt;

#include "base/basictypes.h"
#include "base/commandlineflags.h"
#include "foo/server/bar.h"
</pre>

<p><b>Exception:</b></p>

<p>Sometimes, system-specific code needs
conditional includes. Such code can put conditional
includes after other includes. Of course, keep your
system-specific code small and localized. Example:</p>

<pre>#include "foo/public/fooserver.h"

#include "base/port.h"  // For LANG_CXX11.

#ifdef LANG_CXX11
#include &lt;initializer_list&gt;
#endif  // LANG_CXX11
</pre>

<h2 id="Scoping">Scoping</h2>

<h3 id="Namespaces">Namespaces</h3>

<p>With few exceptions, place code in a namespace. Namespaces
should have unique names based on the project name, and possibly
its path. Do not use <i>using-directives</i> (e.g.,
<code>using namespace foo</code>). Do not use
inline namespaces. For unnamed namespaces, see
<a href="#Internal_Linkage">Internal Linkage</a>.

</p><p class="definition"></p>
<p>Namespaces subdivide the global scope
into distinct, named scopes, and so are useful for preventing
name collisions in the global scope.</p>

<p class="pros"></p>

<p>Namespaces provide a method for preventing name conflicts
in large programs while allowing most code to use reasonably
short names.</p>

<p>For example, if two different projects have a class
<code>Foo</code> in the global scope, these symbols may
collide at compile time or at runtime. If each project
places their code in a namespace, <code>project1::Foo</code>
and <code>project2::Foo</code> are now distinct symbols that
do not collide, and code within each project's namespace
can continue to refer to <code>Foo</code> without the prefix.</p>

<p>Inline namespaces automatically place their names in
the enclosing scope. Consider the following snippet, for
example:</p>

<pre class="neutralcode">namespace outer {
inline namespace inner {
  void foo();
}  // namespace inner
}  // namespace outer
</pre>

<p>The expressions <code>outer::inner::foo()</code> and
<code>outer::foo()</code> are interchangeable. Inline
namespaces are primarily intended for ABI compatibility
across versions.</p>

<p class="cons"></p>

<p>Namespaces can be confusing, because they complicate
the mechanics of figuring out what definition a name refers
to.</p>

<p>Inline namespaces, in particular, can be confusing
because names aren't actually restricted to the namespace
where they are declared. They are only useful as part of
some larger versioning policy.</p>

<p>In some contexts, it's necessary to repeatedly refer to
symbols by their fully-qualified names. For deeply-nested
namespaces, this can add a lot of clutter.</p>

<p class="decision"></p>

<p>Namespaces should be used as follows:</p>

<ul>
  <li>Follow the rules on <a href="#Namespace_Names">Namespace Names</a>.
  </li><li>Terminate multi-line namespaces with comments as shown in the given examples.
  </li><li>

  <p>Namespaces wrap the entire source file after
  includes,
  <a href="https://gflags.github.io/gflags/">
  gflags</a> definitions/declarations
  and forward declarations of classes from other namespaces.</p>

<pre>// In the .h file
namespace mynamespace {

// All declarations are within the namespace scope.
// Notice the lack of indentation.
class MyClass {
 public:
  ...
  void Foo();
};

}  // namespace mynamespace
</pre>

<pre>// In the .cc file
namespace mynamespace {

// Definition of functions is within scope of the namespace.
void MyClass::Foo() {
  ...
}

}  // namespace mynamespace
</pre>

  <p>More complex <code>.cc</code> files might have additional details,
  like flags or using-declarations.</p>

<pre>#include "a.h"

ABSL_FLAG(bool, someflag, false, "dummy flag");

namespace mynamespace {

using ::foo::Bar;

...code for mynamespace...    // Code goes against the left margin.

}  // namespace mynamespace
</pre>
  </li>

  <li>To place generated protocol
  message code in a namespace, use the
  <code>package</code> specifier in the
  <code>.proto</code> file. See


  <a href="https://developers.google.com/protocol-buffers/docs/reference/cpp-generated#package">
  Protocol Buffer Packages</a>
  for details.</li>

  <li>Do not declare anything in namespace
  <code>std</code>, including forward declarations of
  standard library classes. Declaring entities in
  namespace <code>std</code> is undefined behavior, i.e.,
  not portable. To declare entities from the standard
  library, include the appropriate header file.</li>

  <li><p>You may not use a <i>using-directive</i>
  to make all names from a namespace available.</p>

<pre class="badcode">// Forbidden -- This pollutes the namespace.
using namespace foo;
</pre>
  </li>

  <li><p>Do not use <i>Namespace aliases</i> at namespace scope
  in header files except in explicitly marked
  internal-only namespaces, because anything imported into a namespace
  in a header file becomes part of the public
  API exported by that file.</p>

<pre>// Shorten access to some commonly used names in .cc files.
namespace baz = ::foo::bar::baz;
</pre>

<pre>// Shorten access to some commonly used names (in a .h file).
namespace librarian {
namespace impl {  // Internal, not part of the API.
namespace sidetable = ::pipeline_diagnostics::sidetable;
}  // namespace impl

inline void my_inline_function() {
  // namespace alias local to a function (or method).
  namespace baz = ::foo::bar::baz;
  ...
}
}  // namespace librarian
</pre>

  </li><li>Do not use inline namespaces.</li>
</ul>

<a id="Unnamed_Namespaces_and_Static_Variables"></a>
<h3 id="Internal_Linkage">Internal Linkage</h3>

<p>When definitions in a <code>.cc</code> file do not need to be
referenced outside that file, give them internal linkage by placing
them in an unnamed namespace or declaring them <code>static</code>.
Do not use either of these constructs in <code>.h</code> files.

</p><p class="definition"></p>
<p>All declarations can be given internal linkage by placing them in unnamed
namespaces. Functions and variables can also be given internal linkage by
declaring them <code>static</code>. This means that anything you're declaring
can't be accessed from another file. If a different file declares something with
the same name, then the two entities are completely independent.</p>

<p class="decision"></p>

<p>Use of internal linkage in <code>.cc</code> files is encouraged
for all code that does not need to be referenced elsewhere.
Do not use internal linkage in <code>.h</code> files.</p>

<p>Format unnamed namespaces like named namespaces. In the
  terminating comment, leave the namespace name empty:</p>

<pre>namespace {
...
}  // namespace
</pre>

<h3 id="Nonmember,_Static_Member,_and_Global_Functions">Nonmember, Static Member, and Global Functions</h3>

<p>Prefer placing nonmember functions in a namespace; use completely global
functions rarely. Do not use a class simply to group static members. Static
methods of a class should generally be closely related to instances of the
class or the class's static data.</p>


<p class="pros"></p>
<p>Nonmember and static member functions can be useful in
some situations. Putting nonmember functions in a
namespace avoids polluting the global namespace.</p>

<p class="cons"></p>
<p>Nonmember and static member functions may make more sense
as members of a new class, especially if they access
external resources or have significant dependencies.</p>

<p class="decision"></p>
<p>Sometimes it is useful to define a
function not bound to a class instance. Such a function
can be either a static member or a nonmember function.
Nonmember functions should not depend on external
variables, and should nearly always exist in a namespace.
Do not create classes only to group static members;
this is no different than just giving the names a
common prefix, and such grouping is usually unnecessary anyway.</p>

<p>If you define a nonmember function and it is only
needed in its <code>.cc</code> file, use
<a href="#Internal_Linkage">internal linkage</a> to limit
its scope.</p>

<h3 id="Local_Variables">Local Variables</h3>

<p>Place a function's variables in the narrowest scope
possible, and initialize variables in the declaration.</p>

<p>C++ allows you to declare variables anywhere in a
function. We encourage you to declare them in as local a
scope as possible, and as close to the first use as
possible. This makes it easier for the reader to find the
declaration and see what type the variable is and what it
was initialized to. In particular, initialization should
be used instead of declaration and assignment, e.g.,:</p>

<pre class="badcode">int i;
i = f();      // Bad -- initialization separate from declaration.
</pre>

<pre>int j = g();  // Good -- declaration has initialization.
</pre>

<pre class="badcode">std::vector&lt;int&gt; v;
v.push_back(1);  // Prefer initializing using brace initialization.
v.push_back(2);
</pre>

<pre>std::vector&lt;int&gt; v = {1, 2};  // Good -- v starts initialized.
</pre>

<p>Variables needed for <code>if</code>, <code>while</code>
and <code>for</code> statements should normally be declared
within those statements, so that such variables are confined
to those scopes.  E.g.:</p>

<pre>while (const char* p = strchr(str, '/')) str = p + 1;
</pre>

<p>There is one caveat: if the variable is an object, its
constructor is invoked every time it enters scope and is
created, and its destructor is invoked every time it goes
out of scope.</p>

<pre class="badcode">// Inefficient implementation:
for (int i = 0; i &lt; 1000000; ++i) {
  Foo f;  // My ctor and dtor get called 1000000 times each.
  f.DoSomething(i);
}
</pre>

<p>It may be more efficient to declare such a variable
used in a loop outside that loop:</p>

<pre>Foo f;  // My ctor and dtor get called once each.
for (int i = 0; i &lt; 1000000; ++i) {
  f.DoSomething(i);
}
</pre>

<h3 id="Static_and_Global_Variables">Static and Global Variables</h3>

<p>Objects with
<a href="http://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration">
static storage duration</a> are forbidden unless they are
<a href="http://en.cppreference.com/w/cpp/types/is_destructible">trivially
destructible</a>. Informally this means that the destructor does not do
anything, even taking member and base destructors into account. More formally it
means that the type has no user-defined or virtual destructor and that all bases
and non-static members are trivially destructible.
Static function-local variables may use dynamic initialization.
Use of dynamic initialization for static class member variables or variables at
namespace scope is discouraged, but allowed in limited circumstances; see below
for details.</p>

<p>As a rule of thumb: a global variable satisfies these requirements if its
declaration, considered in isolation, could be <code>constexpr</code>.</p>

<p class="definition"></p>
<p>Every object has a <dfn>storage duration</dfn>, which correlates with its
lifetime. Objects with static storage duration live from the point of their
initialization until the end of the program. Such objects appear as variables at
namespace scope ("global variables"), as static data members of classes, or as
function-local variables that are declared with the <code>static</code>
specifier. Function-local static variables are initialized when control first
passes through their declaration; all other objects with static storage duration
are initialized as part of program start-up. All objects with static storage
duration are destroyed at program exit (which happens before unjoined threads
are terminated).</p>

<p>Initialization may be <dfn>dynamic</dfn>, which means that something
non-trivial happens during initialization. (For example, consider a constructor
that allocates memory, or a variable that is initialized with the current
process ID.) The other kind of initialization is <dfn>static</dfn>
initialization. The two aren't quite opposites, though: static
initialization <em>always</em> happens to objects with static storage duration
(initializing the object either to a given constant or to a representation
consisting of all bytes set to zero), whereas dynamic initialization happens
after that, if required.</p>

<p class="pros"></p>
<p>Global and static variables are very useful for a large number of
applications: named constants, auxiliary data structures internal to some
translation unit, command-line flags, logging, registration mechanisms,
background infrastructure, etc.</p>

<p class="cons"></p>
<p>Global and static variables that use dynamic initialization or have
non-trivial destructors create complexity that can easily lead to hard-to-find
bugs. Dynamic initialization is not ordered across translation units, and
neither is destruction (except that destruction
happens in reverse order of initialization). When one initialization refers to
another variable with static storage duration, it is possible that this causes
an object to be accessed before its lifetime has begun (or after its lifetime
has ended). Moreover, when a program starts threads that are not joined at exit,
those threads may attempt to access objects after their lifetime has ended if
their destructor has already run.</p>

<p class="decision"></p>
<h4>Decision on destruction</h4>

<p>When destructors are trivial, their execution is not subject to ordering at
all (they are effectively not "run"); otherwise we are exposed to the risk of
accessing objects after the end of their lifetime. Therefore, we only allow
objects with static storage duration if they are trivially destructible.
Fundamental types (like pointers and <code>int</code>) are trivially
destructible, as are arrays of trivially destructible types. Note that
variables marked with <code>constexpr</code> are trivially destructible.</p>
<pre>const int kNum = 10;  // allowed

struct X { int n; };
const X kX[] = {{1}, {2}, {3}};  // allowed

void foo() {
  static const char* const kMessages[] = {"hello", "world"};  // allowed
}

// allowed: constexpr guarantees trivial destructor
constexpr std::array&lt;int, 3&gt; kArray = {{1, 2, 3}};</pre>
<pre class="badcode">// bad: non-trivial destructor
const std::string kFoo = "foo";

// bad for the same reason, even though kBar is a reference (the
// rule also applies to lifetime-extended temporary objects)
const std::string&amp; kBar = StrCat("a", "b", "c");

void bar() {
  // bad: non-trivial destructor
  static std::map&lt;int, int&gt; kData = {{1, 0}, {2, 0}, {3, 0}};
}</pre>

<p>Note that references are not objects, and thus they are not subject to the
constraints on destructibility. The constraint on dynamic initialization still
applies, though. In particular, a function-local static reference of the form
<code>static T&amp; t = *new T;</code> is allowed.</p>

<h4>Decision on initialization</h4>

<p>Initialization is a more complex topic. This is because we must not only
consider whether class constructors execute, but we must also consider the
evaluation of the initializer:</p>
<pre class="neutralcode">int n = 5;    // fine
int m = f();  // ? (depends on f)
Foo x;        // ? (depends on Foo::Foo)
Bar y = g();  // ? (depends on g and on Bar::Bar)
</pre>

<p>All but the first statement expose us to indeterminate initialization
ordering.</p>

<p>The concept we are looking for is called <em>constant initialization</em> in
the formal language of the C++ standard. It means that the initializing
expression is a constant expression, and if the object is initialized by a
constructor call, then the constructor must be specified as
<code>constexpr</code>, too:</p>
<pre>struct Foo { constexpr Foo(int) {} };

int n = 5;  // fine, 5 is a constant expression
Foo x(2);   // fine, 2 is a constant expression and the chosen constructor is constexpr
Foo a[] = { Foo(1), Foo(2), Foo(3) };  // fine</pre>

<p>Constant initialization is always allowed. Constant initialization of
static storage duration variables should be marked with <code>constexpr</code>
or where possible the


<a href="https://github.com/abseil/abseil-cpp/blob/03c1513538584f4a04d666be5eb469e3979febba/absl/base/attributes.h#L540">
<code>ABSL_CONST_INIT</code></a>
attribute. Any non-local static storage
duration variable that is not so marked should be presumed to have
dynamic initialization, and reviewed very carefully.</p>

<p>By contrast, the following initializations are problematic:</p>

<pre class="badcode">// Some declarations used below.
time_t time(time_t*);      // not constexpr!
int f();                   // not constexpr!
struct Bar { Bar() {} };

// Problematic initializations.
time_t m = time(nullptr);  // initializing expression not a constant expression
Foo y(f());                // ditto
Bar b;                     // chosen constructor Bar::Bar() not constexpr</pre>

<p>Dynamic initialization of nonlocal variables is discouraged, and in general
it is forbidden. However, we do permit it if no aspect of the program depends
on the sequencing of this initialization with respect to all other
initializations. Under those restrictions, the ordering of the initialization
does not make an observable difference. For example:</p>
<pre>int p = getpid();  // allowed, as long as no other static variable
                   // uses p in its own initialization</pre>

<p>Dynamic initialization of static local variables is allowed (and common).</p>



<h4>Common patterns</h4>

<ul>
  <li>Global strings: if you require a global or static string constant,
    consider using a simple character array, or a char pointer to the first
    element of a string literal. String literals have static storage duration
    already and are usually sufficient.</li>
  <li>Maps, sets, and other dynamic containers: if you require a static, fixed
    collection, such as a set to search against or a lookup table, you cannot
    use the dynamic containers from the standard library as a static variable,
    since they have non-trivial destructors. Instead, consider a simple array of
    trivial types, e.g., an array of arrays of ints (for a "map from int to
    int"), or an array of pairs (e.g., pairs of <code>int</code> and <code>const
    char*</code>). For small collections, linear search is entirely sufficient
    (and efficient, due to memory locality); consider using the facilities from

    <a href="https://github.com/abseil/abseil-cpp/blob/master/absl/algorithm/container.h">absl/algorithm/container.h</a>


    for the standard operations. If necessary, keep the collection in sorted
    order and use a binary search algorithm. If you do really prefer a dynamic
    container from the standard library, consider using a function-local static
    pointer, as described below.</li>
  <li>Smart pointers (<code>unique_ptr</code>, <code>shared_ptr</code>): smart
    pointers execute cleanup during destruction and are therefore forbidden.
    Consider whether your use case fits into one of the other patterns described
    in this section. One simple solution is to use a plain pointer to a
    dynamically allocated object and never delete it (see last item).</li>
  <li>Static variables of custom types: if you require static, constant data of
    a type that you need to define yourself, give the type a trivial destructor
    and a <code>constexpr</code> constructor.</li>
  <li>If all else fails, you can create an object dynamically and never delete
    it by using a function-local static pointer or reference (e.g., <code>static
    const auto&amp; impl = *new T(args...);</code>).</li>
</ul>

<h3 id="thread_local">thread_local Variables</h3>

<p><code>thread_local</code> variables that aren't declared inside a function
must be initialized with a true compile-time constant,
and this must be enforced by using the


<a href="https://github.com/abseil/abseil-cpp/blob/master/absl/base/attributes.h">
<code>ABSL_CONST_INIT</code></a>
attribute. Prefer
<code>thread_local</code> over other ways of defining thread-local data.</p>

<p class="definition"></p>
<p>Starting with C++11, variables can be declared with the
<code>thread_local</code> specifier:</p>
<pre>thread_local Foo foo = ...;
</pre>
<p>Such a variable is actually a collection of objects, so that when different
threads access it, they are actually accessing different objects.
<code>thread_local</code> variables are much like
<a href="#Static_and_Global_Variables">static storage duration variables</a>
in many respects. For instance, they can be declared at namespace scope,
inside functions, or as static class members, but not as ordinary class
members.</p>

<p><code>thread_local</code> variable instances are initialized much like
static variables, except that they must be initialized separately for each
thread, rather than once at program startup. This means that
<code>thread_local</code> variables declared within a function are safe, but
other <code>thread_local</code> variables are subject to the same
initialization-order issues as static variables (and more besides).</p>

<p><code>thread_local</code> variable instances are destroyed when their thread
terminates, so they do not have the destruction-order issues of static
variables.</p>

<p class="pros"></p>
<ul>
  <li>Thread-local data is inherently safe from races (because only one thread
    can ordinarily access it), which makes <code>thread_local</code> useful for
    concurrent programming.</li>
  <li><code>thread_local</code> is the only standard-supported way of creating
    thread-local data.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>Accessing a <code>thread_local</code> variable may trigger execution of
    an unpredictable and uncontrollable amount of other code.</li>
  <li><code>thread_local</code> variables are effectively global variables,
    and have all the drawbacks of global variables other than lack of
    thread-safety.</li>
  <li>The memory consumed by a <code>thread_local</code> variable scales with
    the number of running threads (in the worst case), which can be quite large
    in a  program.</li>
  <li>An ordinary class member cannot be <code>thread_local</code>.</li>
  <li><code>thread_local</code> may not be as efficient as certain compiler
    intrinsics.</li>
</ul>

<p class="decision"></p>
  <p><code>thread_local</code> variables inside a function have no safety
    concerns, so they can be used without restriction. Note that you can use
    a function-scope <code>thread_local</code> to simulate a class- or
    namespace-scope <code>thread_local</code> by defining a function or
    static method that exposes it:</p>

<pre>Foo&amp; MyThreadLocalFoo() {
  thread_local Foo result = ComplicatedInitialization();
  return result;
}
</pre>

<p><code>thread_local</code> variables at class or namespace scope must be
initialized with a true compile-time constant (i.e., they must have no
dynamic initialization). To enforce this, <code>thread_local</code> variables
at class or namespace scope must be annotated with


<a href="https://github.com/abseil/abseil-cpp/blob/master/absl/base/attributes.h">
<code>ABSL_CONST_INIT</code></a>
(or <code>constexpr</code>, but that should be rare):</p>

<pre>ABSL_CONST_INIT thread_local Foo foo = ...;
</pre>

<p><code>thread_local</code> should be preferred over other mechanisms for
defining thread-local data.</p>

<h2 id="Classes">Classes</h2>

<p>Classes are the fundamental unit of code in C++. Naturally,
we use them extensively. This section lists the main dos and
don'ts you should follow when writing a class.</p>

<h3 id="Doing_Work_in_Constructors">Doing Work in Constructors</h3>

<p>Avoid virtual method calls in constructors, and avoid
initialization that can fail if you can't signal an error.</p>

<p class="definition"></p>
<p>It is possible to perform arbitrary initialization in the body
of the constructor.</p>

<p class="pros"></p>
<ul>
  <li>No need to worry about whether the class has been initialized or
  not.</li>

  <li>Objects that are fully initialized by constructor call can
  be <code>const</code> and may also be easier to use with standard containers
  or algorithms.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>If the work calls virtual functions, these calls
  will not get dispatched to the subclass
  implementations. Future modification to your class can
  quietly introduce this problem even if your class is
  not currently subclassed, causing much confusion.</li>

  <li>There is no easy way for constructors to signal errors, short of
  crashing the program (not always appropriate) or using exceptions
  (which are <a href="#Exceptions">forbidden</a>).</li>

  <li>If the work fails, we now have an object whose initialization
  code failed, so it may be an unusual state requiring a <code>bool
  IsValid()</code> state checking mechanism (or similar) which is easy
  to forget to call.</li>

  <li>You cannot take the address of a constructor, so whatever work
  is done in the constructor cannot easily be handed off to, for
  example, another thread.</li>
</ul>

<p class="decision"></p>
<p>Constructors should never call virtual functions. If appropriate
for your code ,
terminating the program may be an appropriate error handling
response. Otherwise, consider a factory function
or <code>Init()</code> method as described in
<a href="https://abseil.io/tips/42">TotW #42</a>
.
Avoid <code>Init()</code> methods on objects with
no other states that affect which public methods may be called
(semi-constructed objects of this form are particularly hard to work
with correctly).</p>

<a id="Explicit_Constructors"></a>
<h3 id="Implicit_Conversions">Implicit Conversions</h3>

<p>Do not define implicit conversions. Use the <code>explicit</code>
keyword for conversion operators and single-argument
constructors.</p>

<p class="definition"></p>
<p>Implicit conversions allow an
object of one type (called the <dfn>source type</dfn>) to
be used where a different type (called the <dfn>destination
type</dfn>) is expected, such as when passing an
<code>int</code> argument to a function that takes a
<code>double</code> parameter.</p>

<p>In addition to the implicit conversions defined by the language,
users can define their own, by adding appropriate members to the
class definition of the source or destination type. An implicit
conversion in the source type is defined by a type conversion operator
named after the destination type (e.g., <code>operator
bool()</code>). An implicit conversion in the destination
type is defined by a constructor that can take the source type as
its only argument (or only argument with no default value).</p>

<p>The <code>explicit</code> keyword can be applied to a constructor
or (since C++11) a conversion operator, to ensure that it can only be
used when the destination type is explicit at the point of use,
e.g., with a cast. This applies not only to implicit conversions, but to
C++11's list initialization syntax:</p>
<pre>class Foo {
  explicit Foo(int x, double y);
  ...
};

void Func(Foo f);
</pre>
<pre class="badcode">Func({42, 3.14});  // Error
</pre>
This kind of code isn't technically an implicit conversion, but the
language treats it as one as far as <code>explicit</code> is concerned.

<p class="pros"></p>
<ul>
<li>Implicit conversions can make a type more usable and
    expressive by eliminating the need to explicitly name a type
    when it's obvious.</li>
<li>Implicit conversions can be a simpler alternative to
    overloading, such as when a single
    function with a <code>string_view</code> parameter takes the
    place of separate overloads for <code>std::string</code> and
    <code>const char*</code>.</li>
<li>List initialization syntax is a concise and expressive
    way of initializing objects.</li>
</ul>

<p class="cons"></p>
<ul>
<li>Implicit conversions can hide type-mismatch bugs, where the
    destination type does not match the user's expectation, or
    the user is unaware that any conversion will take place.</li>

<li>Implicit conversions can make code harder to read, particularly
    in the presence of overloading, by making it less obvious what
    code is actually getting called.</li>

<li>Constructors that take a single argument may accidentally
    be usable as implicit type conversions, even if they are not
    intended to do so.</li>

<li>When a single-argument constructor is not marked
    <code>explicit</code>, there's no reliable way to tell whether
    it's intended to define an implicit conversion, or the author
    simply forgot to mark it.</li>

<li>Implicit conversions can lead to call-site ambiguities, especially
    when there are bidirectional implicit conversions. This can be caused
    either by having two types that both provide an implicit conversion,
    or by a single type that has both an implicit constructor and an
    implicit type conversion operator.</li>

<li>List initialization can suffer from the same problems if
    the destination type is implicit, particularly if the
    list has only a single element.</li>
</ul>

<p class="decision"></p>
<p>Type conversion operators, and constructors that are
callable with a single argument, must be marked
<code>explicit</code> in the class definition. As an
exception, copy and move constructors should not be
<code>explicit</code>, since they do not perform type
conversion.</p>

<p>Implicit conversions can sometimes be necessary and appropriate for
types that are designed to be interchangeable, for example when objects
of two types are just different representations of the same underlying
value. In that case, contact
your project leads to request a waiver
of this rule.
</p>

<p>Constructors that cannot be called with a single argument
may omit <code>explicit</code>. Constructors that
take a single <code>std::initializer_list</code> parameter should
also omit <code>explicit</code>, in order to support copy-initialization
(e.g., <code>MyType m = {1, 2};</code>).</p>

<h3 id="Copyable_Movable_Types">Copyable and Movable Types</h3>
<a id="Copy_Constructors"></a>

<p>A class's public API must make clear whether the class is copyable,
move-only, or neither copyable nor movable. Support copying and/or
moving if these operations are clear and meaningful for your type.</p>

<p class="definition"></p>
<p>A movable type is one that can be initialized and assigned
from temporaries.</p>

<p>A copyable type is one that can be initialized or assigned from
any other object of the same type (so is also movable by definition), with the
stipulation that the value of the source does not change.
<code>std::unique_ptr&lt;int&gt;</code> is an example of a movable but not
copyable type (since the value of the source
<code>std::unique_ptr&lt;int&gt;</code> must be modified during assignment to
the destination). <code>int</code> and <code>std::string</code> are examples of
movable types that are also copyable. (For <code>int</code>, the move and copy
operations are the same; for <code>std::string</code>, there exists a move operation
that is less expensive than a copy.)</p>

<p>For user-defined types, the copy behavior is defined by the copy
constructor and the copy-assignment operator. Move behavior is defined by the
move constructor and the move-assignment operator, if they exist, or by the
copy constructor and the copy-assignment operator otherwise.</p>

<p>The copy/move constructors can be implicitly invoked by the compiler
in some situations, e.g., when passing objects by value.</p>

<p class="pros"></p>
<p>Objects of copyable and movable types can be passed and returned by value,
which makes APIs simpler, safer, and more general. Unlike when passing objects
by pointer or reference, there's no risk of confusion over ownership,
lifetime, mutability, and similar issues, and no need to specify them in the
contract. It also prevents non-local interactions between the client and the
implementation, which makes them easier to understand, maintain, and optimize by
the compiler. Further, such objects can be used with generic APIs that
require pass-by-value, such as most containers, and they allow for additional
flexibility in e.g., type composition.</p>

<p>Copy/move constructors and assignment operators are usually
easier to define correctly than alternatives
like <code>Clone()</code>, <code>CopyFrom()</code> or <code>Swap()</code>,
because they can be generated by the compiler, either implicitly or
with <code>= default</code>.  They are concise, and ensure
that all data members are copied. Copy and move
constructors are also generally more efficient, because they don't
require heap allocation or separate initialization and assignment
steps, and they're eligible for optimizations such as

<a href="http://en.cppreference.com/w/cpp/language/copy_elision">
copy elision</a>.</p>

<p>Move operations allow the implicit and efficient transfer of
resources out of rvalue objects. This allows a plainer coding style
in some cases.</p>

<p class="cons"></p>
<p>Some types do not need to be copyable, and providing copy
operations for such types can be confusing, nonsensical, or outright
incorrect. Types representing singleton objects (<code>Registerer</code>),
objects tied to a specific scope (<code>Cleanup</code>), or closely coupled to
object identity (<code>Mutex</code>) cannot be copied meaningfully.
Copy operations for base class types that are to be used
polymorphically are hazardous, because use of them can lead to
<a href="https://en.wikipedia.org/wiki/Object_slicing">object slicing</a>.
Defaulted or carelessly-implemented copy operations can be incorrect, and the
resulting bugs can be confusing and difficult to diagnose.</p>

<p>Copy constructors are invoked implicitly, which makes the
invocation easy to miss. This may cause confusion for programmers used to
languages where pass-by-reference is conventional or mandatory. It may also
encourage excessive copying, which can cause performance problems.</p>

<p class="decision"></p>

<p>Every class's public interface must make clear which copy and move
operations the class supports. This should usually take the form of explicitly
declaring and/or deleting the appropriate operations in the <code>public</code>
section of the declaration.</p>

<p>Specifically, a copyable class should explicitly declare the copy
operations, a move-only class should explicitly declare the move operations, and
a non-copyable/movable class should explicitly delete the copy operations. A
copyable class may also declare move operations in order to support efficient
moves. Explicitly declaring or deleting all four copy/move operations is
permitted, but not required. If you provide a copy or move assignment operator,
you must also provide the corresponding constructor.</p>

<pre>class Copyable {
 public:
  Copyable(const Copyable&amp; other) = default;
  Copyable&amp; operator=(const Copyable&amp; other) = default;

  // The implicit move operations are suppressed by the declarations above.
  // You may explicitly declare move operations to support efficient moves.
};

class MoveOnly {
 public:
  MoveOnly(MoveOnly&amp;&amp; other) = default;
  MoveOnly&amp; operator=(MoveOnly&amp;&amp; other) = default;

  // The copy operations are implicitly deleted, but you can
  // spell that out explicitly if you want:
  MoveOnly(const MoveOnly&amp;) = delete;
  MoveOnly&amp; operator=(const MoveOnly&amp;) = delete;
};

class NotCopyableOrMovable {
 public:
  // Not copyable or movable
  NotCopyableOrMovable(const NotCopyableOrMovable&amp;) = delete;
  NotCopyableOrMovable&amp; operator=(const NotCopyableOrMovable&amp;)
      = delete;

  // The move operations are implicitly disabled, but you can
  // spell that out explicitly if you want:
  NotCopyableOrMovable(NotCopyableOrMovable&amp;&amp;) = delete;
  NotCopyableOrMovable&amp; operator=(NotCopyableOrMovable&amp;&amp;)
      = delete;
};
</pre>

<p>These declarations/deletions can be omitted only if they are obvious:</p>
<ul>
<li>If the class has no <code>private</code> section, like a
    <a href="#Structs_vs._Classes">struct</a> or an interface-only base class,
    then the copyability/movability can be determined by the
    copyability/movability of any public data members.
</li><li>If a base class clearly isn't copyable or movable, derived classes
    naturally won't be either.  An interface-only base class that leaves these
    operations implicit is not sufficient to make concrete subclasses clear.
</li><li>Note that if you explicitly declare or delete either the constructor or
    assignment operation for copy, the other copy operation is not obvious and
    must be declared or deleted.  Likewise for move operations.
</li></ul>

<p>A type should not be copyable/movable if the meaning of
copying/moving is unclear to a casual user, or if it incurs unexpected
costs. Move operations for copyable types are strictly a performance
optimization and are a potential source of bugs and complexity, so
avoid defining them unless they are significantly more efficient than
the corresponding copy operations.  If your type provides copy operations, it is
recommended that you design your class so that the default implementation of
those operations is correct. Remember to review the correctness of any
defaulted operations as you would any other code.</p>

<p>Due to the risk of slicing, prefer to avoid providing a public assignment
operator or copy/move constructor for a class that's
intended to be derived from (and prefer to avoid deriving from a class
with such members). If your base class needs to be
copyable, provide a public virtual <code>Clone()</code>
method, and a protected copy constructor that derived classes
can use to implement it.</p>



<h3 id="Structs_vs._Classes">Structs vs. Classes</h3>

<p>Use a <code>struct</code> only for passive objects that
      carry data; everything else is a <code>class</code>.</p>

<p>The <code>struct</code> and <code>class</code>
keywords behave almost identically in C++. We add our own
semantic meanings to each keyword, so you should use the
appropriate keyword for the data-type you're
defining.</p>

<p><code>structs</code> should be used for passive objects that carry
data, and may have associated constants. All fields must be public. The
struct must not have invariants that imply relationships between
different fields, since direct user access to those fields may
break those invariants. Constructors, destructors, and helper methods may
be present; however, these methods must not require or enforce any
invariants.</p>

<p>If more functionality or invariants are required, a
<code>class</code> is more appropriate. If in doubt, make
it a <code>class</code>.</p>

<p>For consistency with STL, you can use
<code>struct</code> instead of <code>class</code> for
stateless types, such as traits,
<a href="#Template_metaprogramming">template metafunctions</a>,
and some functors.</p>

<p>Note that member variables in structs and classes have
<a href="#Variable_Names">different naming rules</a>.</p>

<h3 id="Structs_vs._Tuples">Structs vs. Pairs and Tuples</h3>

<p>Prefer to use a <code>struct</code> instead of a pair or a
tuple whenever the elements can have meaningful names.</p>

<p>
  While using pairs and tuples can avoid the need to define a custom type,
  potentially saving work when <em>writing</em> code, a meaningful field
  name will almost always be much clearer when <em>reading</em> code than
  <code>.first</code>, <code>.second</code>, or <code>std::get&lt;X&gt;</code>.
  While C++14's introduction of <code>std::get&lt;Type&gt;</code> to access a
  tuple element by type rather than index (when the type is unique) can
  sometimes partially mitigate this, a field name is usually substantially
  clearer and more informative than a type.
</p>

<p>
  Pairs and tuples may be appropriate in generic code where there are not
  specific meanings for the elements of the pair or tuple. Their use may
  also be required in order to interoperate with existing code or APIs.
</p>

<a id="Multiple_Inheritance"></a>
<h3 id="Inheritance">Inheritance</h3>

<p>Composition is often more appropriate than inheritance.
When using inheritance, make it <code>public</code>.</p>

<p class="definition"></p>
<p> When a sub-class
inherits from a base class, it includes the definitions
of all the data and operations that the base class
defines. "Interface inheritance" is inheritance from a
pure abstract base class (one with no state or defined
methods); all other inheritance is "implementation
inheritance".</p>

<p class="pros"></p>
<p>Implementation inheritance reduces code size by re-using
the base class code as it specializes an existing type.
Because inheritance is a compile-time declaration, you
and the compiler can understand the operation and detect
errors. Interface inheritance can be used to
programmatically enforce that a class expose a particular
API. Again, the compiler can detect errors, in this case,
when a class does not define a necessary method of the
API.</p>

<p class="cons"></p>
<p>For implementation inheritance, because the code
implementing a sub-class is spread between the base and
the sub-class, it can be more difficult to understand an
implementation. The sub-class cannot override functions
that are not virtual, so the sub-class cannot change
implementation.</p>

<p>Multiple inheritance is especially problematic, because
it often imposes a higher performance overhead (in fact,
the performance drop from single inheritance to multiple
inheritance can often be greater than the performance
drop from ordinary to virtual dispatch), and because
it risks leading to "diamond" inheritance patterns,
which are prone to ambiguity, confusion, and outright bugs.</p>

<p class="decision"></p>

<p>All inheritance should be <code>public</code>. If you
want to do private inheritance, you should be including
an instance of the base class as a member instead.</p>

<p>Do not overuse implementation inheritance. Composition
is often more appropriate. Try to restrict use of
inheritance to the "is-a" case: <code>Bar</code>
subclasses <code>Foo</code> if it can reasonably be said
that <code>Bar</code> "is a kind of"
<code>Foo</code>.</p>

<p>Limit the use of <code>protected</code> to those
member functions that might need to be accessed from
subclasses. Note that <a href="#Access_Control">data
members should be private</a>.</p>

<p>Explicitly annotate overrides of virtual functions or virtual
destructors with exactly one of an <code>override</code> or (less
frequently) <code>final</code> specifier. Do not
use <code>virtual</code> when declaring an override.
Rationale: A function or destructor marked
<code>override</code> or <code>final</code> that is
not an override of a base class virtual function will
not compile, and this helps catch common errors. The
specifiers serve as documentation; if no specifier is
present, the reader has to check all ancestors of the
class in question to determine if the function or
destructor is virtual or not.</p>

<p>Multiple inheritance is permitted, but multiple <em>implementation</em>
inheritance is strongly discouraged.</p>

<h3 id="Operator_Overloading">Operator Overloading</h3>

<p>Overload operators judiciously. Do not use user-defined literals.</p>

<p class="definition"></p>
<p>C++ permits user code to
<a href="http://en.cppreference.com/w/cpp/language/operators">declare
overloaded versions of the built-in operators</a> using the
<code>operator</code> keyword, so long as one of the parameters
is a user-defined type. The <code>operator</code> keyword also
permits user code to define new kinds of literals using
<code>operator""</code>, and to define type-conversion functions
such as <code>operator bool()</code>.</p>

<p class="pros"></p>
<p>Operator overloading can make code more concise and
intuitive by enabling user-defined types to behave the same
as built-in types. Overloaded operators are the idiomatic names
for certain operations (e.g., <code>==</code>, <code>&lt;</code>,
<code>=</code>, and <code>&lt;&lt;</code>), and adhering to
those conventions can make user-defined types more readable
and enable them to interoperate with libraries that expect
those names.</p>

<p>User-defined literals are a very concise notation for
creating objects of user-defined types.</p>

<p class="cons"></p>
<ul>
  <li>Providing a correct, consistent, and unsurprising
  set of operator overloads requires some care, and failure
  to do so can lead to confusion and bugs.</li>

  <li>Overuse of operators can lead to obfuscated code,
  particularly if the overloaded operator's semantics
  don't follow convention.</li>

  <li>The hazards of function overloading apply just as
  much to operator overloading, if not more so.</li>

  <li>Operator overloads can fool our intuition into
  thinking that expensive operations are cheap, built-in
  operations.</li>

  <li>Finding the call sites for overloaded operators may
  require a search tool that's aware of C++ syntax, rather
  than e.g., grep.</li>

  <li>If you get the argument type of an overloaded operator
  wrong, you may get a different overload rather than a
  compiler error. For example, <code>foo &lt; bar</code>
  may do one thing, while <code>&amp;foo &lt; &amp;bar</code>
  does something totally different.</li>

  <li>Certain operator overloads are inherently hazardous.
  Overloading unary <code>&amp;</code> can cause the same
  code to have different meanings depending on whether
  the overload declaration is visible. Overloads of
  <code>&amp;&amp;</code>, <code>||</code>, and <code>,</code>
  (comma) cannot match the evaluation-order semantics of the
  built-in operators.</li>

  <li>Operators are often defined outside the class,
  so there's a risk of different files introducing
  different definitions of the same operator. If both
  definitions are linked into the same binary, this results
  in undefined behavior, which can manifest as subtle
  run-time bugs.</li>

  <li>User-defined literals (UDLs) allow the creation of new
  syntactic forms that are unfamiliar even to experienced C++
  programmers, such as <code>"Hello World"sv</code> as a
  shorthand for <code>std::string_view("Hello World")</code>.
  Existing notations are clearer, though less terse.</li>

  <li>Because they can't be namespace-qualified, uses of UDLs also require
  use of either using-directives (which <a href="#Namespaces">we ban</a>) or
  using-declarations (which <a href="#Aliases">we ban in header files</a> except
  when the imported names are part of the interface exposed by the header
  file in question).  Given that header files would have to avoid UDL
  suffixes, we prefer to avoid having conventions for literals differ
  between header files and source files.
  </li>
</ul>

<p class="decision"></p>
<p>Define overloaded operators only if their meaning is
obvious, unsurprising, and consistent with the corresponding
built-in operators. For example, use <code>|</code> as a
bitwise- or logical-or, not as a shell-style pipe.</p>

<p>Define operators only on your own types. More precisely,
define them in the same headers, .cc files, and namespaces
as the types they operate on. That way, the operators are available
wherever the type is, minimizing the risk of multiple
definitions. If possible, avoid defining operators as templates,
because they must satisfy this rule for any possible template
arguments. If you define an operator, also define
any related operators that make sense, and make sure they
are defined consistently. For example, if you overload
<code>&lt;</code>, overload all the comparison operators,
and make sure <code>&lt;</code> and <code>&gt;</code> never
return true for the same arguments.</p>

<p>Prefer to define non-modifying binary operators as
non-member functions. If a binary operator is defined as a
class member, implicit conversions will apply to the
right-hand argument, but not the left-hand one. It will
confuse your users if <code>a &lt; b</code> compiles but
<code>b &lt; a</code> doesn't.</p>

<p>Don't go out of your way to avoid defining operator
overloads. For example, prefer to define <code>==</code>,
<code>=</code>, and <code>&lt;&lt;</code>, rather than
<code>Equals()</code>, <code>CopyFrom()</code>, and
<code>PrintTo()</code>. Conversely, don't define
operator overloads just because other libraries expect
them. For example, if your type doesn't have a natural
ordering, but you want to store it in a <code>std::set</code>,
use a custom comparator rather than overloading
<code>&lt;</code>.</p>

<p>Do not overload <code>&amp;&amp;</code>, <code>||</code>,
<code>,</code> (comma), or unary <code>&amp;</code>. Do not overload
<code>operator""</code>, i.e., do not introduce user-defined
literals.  Do not use any such literals provided by others
(including the standard library).</p>

<p>Type conversion operators are covered in the section on
<a href="#Implicit_Conversions">implicit conversions</a>.
The <code>=</code> operator is covered in the section on
<a href="#Copy_Constructors">copy constructors</a>. Overloading
<code>&lt;&lt;</code> for use with streams is covered in the
section on <a href="#Streams">streams</a>. See also the rules on
<a href="#Function_Overloading">function overloading</a>, which
apply to operator overloading as well.</p>

<h3 id="Access_Control">Access Control</h3>

<p>Make classes' data members <code>private</code>, unless they are
<a href="#Constant_Names">constants</a>. This simplifies reasoning about invariants, at the cost
of some easy boilerplate in the form of accessors (usually <code>const</code>) if necessary.</p>

<p>For technical
reasons, we allow data members of a test fixture class defined in a .cc file to
be <code>protected</code> when using


<a href="https://github.com/google/googletest">Google
Test</a>.
If a test fixture class is defined outside of the .cc file it is used in, for example in a .h file,
make data members <code>private</code>.</p>

<h3 id="Declaration_Order">Declaration Order</h3>

<p>Group similar declarations together, placing public parts
earlier.</p>

<p>A class definition should usually start with a
<code>public:</code> section, followed by
<code>protected:</code>, then <code>private:</code>.  Omit
sections that would be empty.</p>

<p>Within each section, prefer grouping similar
kinds of declarations together, and prefer the
following order: types (including <code>typedef</code>,
<code>using</code>, <code>enum</code>, and nested structs and classes),
constants, factory functions, constructors and assignment
operators, destructor, all other methods, data members.</p>

<p>Do not put large method definitions inline in the
class definition. Usually, only trivial or
performance-critical, and very short, methods may be
defined inline. See <a href="#Inline_Functions">Inline
Functions</a> for more details.</p>

<h2 id="Functions">Functions</h2>

<a id="Function_Parameter_Ordering"></a>
<a id="Output_Parameters"></a>
<h3 id="Inputs_and_Outputs">Inputs and Outputs</h3>

<p>The output of a C++ function is naturally provided via
a return value and sometimes via output parameters (or in/out parameters).</p>

<p>Prefer using return values over output parameters: they
improve readability, and often provide the same or better
performance.</p>

<p>Prefer to return by value or, failing that, return by reference.
Avoid returning a pointer unless it can be null.</p>

<p>Parameters are either inputs to the function, outputs from the
function, or both. Non-optional input parameters should usually be values
or <code>const</code> references, while non-optional output and
input/output parameters should usually be references (which cannot be null).
Generally, use <code>absl::optional</code> to represent optional by-value
inputs, and use a <code>const</code> pointer when the non-optional form would
have used a reference. Use non-<code>const</code> pointers to represent
optional outputs and optional input/output parameters.</p>



<p>
Avoid defining functions that require a <code>const</code> reference parameter
to outlive the call, because <code>const</code> reference parameters bind
to temporaries. Instead, find a way to eliminate the lifetime requirement
(for example, by copying the parameter), or pass it by <code>const</code>
pointer and document the lifetime and non-null requirements.

</p>

<p>When ordering function parameters, put all input-only
parameters before any output parameters. In particular,
do not add new parameters to the end of the function just
because they are new; place new input-only parameters before
the output parameters. This is not a hard-and-fast rule. Parameters that
are both input and output muddy the waters, and, as always,
consistency with related functions may require you to bend the rule.
Variadic functions may also require unusual parameter ordering.</p>

<h3 id="Write_Short_Functions">Write Short Functions</h3>

<p>Prefer small and focused functions.</p>

<p>We recognize that long functions are sometimes
appropriate, so no hard limit is placed on functions
length. If a function exceeds about 40 lines, think about
whether it can be broken up without harming the structure
of the program.</p>

<p>Even if your long function works perfectly now,
someone modifying it in a few months may add new
behavior. This could result in bugs that are hard to
find. Keeping your functions short and simple makes it
easier for other people to read and modify your code.
Small functions are also easier to test.</p>

<p>You could find long and complicated functions when
working with
some code. Do not be
intimidated by modifying existing code: if working with
such a function proves to be difficult, you find that
errors are hard to debug, or you want to use a piece of
it in several different contexts, consider breaking up
the function into smaller and more manageable pieces.</p>

<h3 id="Function_Overloading">Function Overloading</h3>

<p>Use overloaded functions (including constructors) only if a
reader looking at a call site can get a good idea of what
is happening without having to first figure out exactly
which overload is being called.</p>

<p class="definition"></p>
<p>You may write a function that takes a <code>const
std::string&amp;</code> and overload it with another that
takes <code>const char*</code>. However, in this case consider
std::string_view
 instead.</p>

<pre>class MyClass {
 public:
  void Analyze(const std::string &amp;text);
  void Analyze(const char *text, size_t textlen);
};
</pre>

<p class="pros"></p>
<p>Overloading can make code more intuitive by allowing an
identically-named function to take different arguments.
It may be necessary for templatized code, and it can be
convenient for Visitors.</p>
<p>Overloading based on const or ref qualification may make utility
  code more usable, more efficient, or both.
  (See <a href="http://abseil.io/tips/148">TotW 148</a> for more.)
</p>

<p class="cons"></p>
<p>If a function is overloaded by the argument types alone,
a reader may have to understand C++'s complex matching
rules in order to tell what's going on. Also many people
are confused by the semantics of inheritance if a derived
class overrides only some of the variants of a
function.</p>

<p class="decision"></p>
<p>You may overload a function when there are no semantic differences
between variants. These overloads may vary in types, qualifiers, or
argument count. However, a reader of such a call must not need to know
which member of the overload set is chosen, only that <b>something</b>
from the set is being called. If you can document all entries in the
overload set with a single comment in the header, that is a good sign
that it is a well-designed overload set.</p>

<h3 id="Default_Arguments">Default Arguments</h3>

<p>Default arguments are allowed on non-virtual functions
when the default is guaranteed to always have the same
value. Follow the same restrictions as for <a href="#Function_Overloading">function overloading</a>, and
prefer overloaded functions if the readability gained with
default arguments doesn't outweigh the downsides below.</p>

<p class="pros"></p>
<p>Often you have a function that uses default values, but
occasionally you want to override the defaults. Default
parameters allow an easy way to do this without having to
define many functions for the rare exceptions. Compared
to overloading the function, default arguments have a
cleaner syntax, with less boilerplate and a clearer
distinction between 'required' and 'optional'
arguments.</p>

<p class="cons"></p>
<p>Defaulted arguments are another way to achieve the
semantics of overloaded functions, so all the <a href="#Function_Overloading">reasons not to overload
functions</a> apply.</p>

<p>The defaults for arguments in a virtual function call are
determined by the static type of the target object, and
there's no guarantee that all overrides of a given function
declare the same defaults.</p>

<p>Default parameters are re-evaluated at each call site,
which can bloat the generated code. Readers may also expect
the default's value to be fixed at the declaration instead
of varying at each call.</p>

<p>Function pointers are confusing in the presence of
default arguments, since the function signature often
doesn't match the call signature. Adding
function overloads avoids these problems.</p>

<p class="decision"></p>
<p>Default arguments are banned on virtual functions, where
they don't work properly, and in cases where the specified
default might not evaluate to the same value depending on
when it was evaluated. (For example, don't write <code>void
f(int n = counter++);</code>.)</p>

<p>In some other cases, default arguments can improve the
readability of their function declarations enough to
overcome the downsides above, so they are allowed. When in
doubt, use overloads.</p>

<h3 id="trailing_return">Trailing Return Type Syntax</h3>

<p>Use trailing return types only where using the ordinary syntax (leading
  return types) is impractical or much less readable.</p>

<p class="definition"></p>
<p>C++ allows two different forms of function declarations. In the older
  form, the return type appears before the function name. For example:</p>
<pre>int foo(int x);
</pre>
<p>The newer form, introduced in C++11, uses the <code>auto</code>
  keyword before the function name and a trailing return type after
  the argument list. For example, the declaration above could
  equivalently be written:</p>
<pre>auto foo(int x) -&gt; int;
</pre>
<p>The trailing return type is in the function's scope. This doesn't
  make a difference for a simple case like <code>int</code> but it matters
  for more complicated cases, like types declared in class scope or
  types written in terms of the function parameters.</p>

<p class="pros"></p>
<p>Trailing return types are the only way to explicitly specify the
  return type of a <a href="#Lambda_expressions">lambda expression</a>.
  In some cases the compiler is able to deduce a lambda's return type,
  but not in all cases. Even when the compiler can deduce it automatically,
  sometimes specifying it explicitly would be clearer for readers.
</p>
<p>Sometimes it's easier and more readable to specify a return type
  after the function's parameter list has already appeared. This is
  particularly true when the return type depends on template parameters.
  For example:</p>
  <pre>    template &lt;typename T, typename U&gt;
    auto add(T t, U u) -&gt; decltype(t + u);
  </pre>
  versus
  <pre>    template &lt;typename T, typename U&gt;
    decltype(declval&lt;T&amp;&gt;() + declval&lt;U&amp;&gt;()) add(T t, U u);
  </pre>

<p class="cons"></p>
<p>Trailing return type syntax is relatively new and it has no
  analogue in C++-like languages such as C and Java, so some readers may
  find it unfamiliar.</p>
<p>Existing code bases have an enormous number of function
  declarations that aren't going to get changed to use the new syntax,
  so the realistic choices are using the old syntax only or using a mixture
  of the two. Using a single version is better for uniformity of style.</p>

<p class="decision"></p>
<p>In most cases, continue to use the older style of function
  declaration where the return type goes before the function name.
  Use the new trailing-return-type form only in cases where it's
  required (such as lambdas) or where, by putting the type after the
  function's parameter list, it allows you to write the type in a much
  more readable way. The latter case should be rare; it's mostly an
  issue in fairly complicated template code, which is
  <a href="#Template_metaprogramming">discouraged in most cases</a>.</p>


<h2 id="Google-Specific_Magic">Google-Specific Magic</h2>



<div>
<p>There are various tricks and utilities that
we use to make C++ code more robust, and various ways we use
C++ that may differ from what you see elsewhere.</p>
</div>



<h3 id="Ownership_and_Smart_Pointers">Ownership and Smart Pointers</h3>

<p>Prefer to have single, fixed owners for dynamically
allocated objects. Prefer to transfer ownership with smart
pointers.</p>

<p class="definition"></p>
<p>"Ownership" is a bookkeeping technique for managing
dynamically allocated memory (and other resources). The
owner of a dynamically allocated object is an object or
function that is responsible for ensuring that it is
deleted when no longer needed. Ownership can sometimes be
shared, in which case the last owner is typically
responsible for deleting it. Even when ownership is not
shared, it can be transferred from one piece of code to
another.</p>

<p>"Smart" pointers are classes that act like pointers,
e.g., by overloading the <code>*</code> and
<code>-&gt;</code> operators. Some smart pointer types
can be used to automate ownership bookkeeping, to ensure
these responsibilities are met.
<a href="http://en.cppreference.com/w/cpp/memory/unique_ptr">
<code>std::unique_ptr</code></a> is a smart pointer type
introduced in C++11, which expresses exclusive ownership
of a dynamically allocated object; the object is deleted
when the <code>std::unique_ptr</code> goes out of scope.
It cannot be copied, but can be <em>moved</em> to
represent ownership transfer.
<a href="http://en.cppreference.com/w/cpp/memory/shared_ptr">
<code>std::shared_ptr</code></a> is a smart pointer type
that expresses shared ownership of
a dynamically allocated object. <code>std::shared_ptr</code>s
can be copied; ownership of the object is shared among
all copies, and the object is deleted when the last
<code>std::shared_ptr</code> is destroyed. </p>

<p class="pros"></p>
<ul>
  <li>It's virtually impossible to manage dynamically
  allocated memory without some sort of ownership
  logic.</li>

  <li>Transferring ownership of an object can be cheaper
  than copying it (if copying it is even possible).</li>

  <li>Transferring ownership can be simpler than
  'borrowing' a pointer or reference, because it reduces
  the need to coordinate the lifetime of the object
  between the two users.</li>

  <li>Smart pointers can improve readability by making
  ownership logic explicit, self-documenting, and
  unambiguous.</li>

  <li>Smart pointers can eliminate manual ownership
  bookkeeping, simplifying the code and ruling out large
  classes of errors.</li>

  <li>For const objects, shared ownership can be a simple
  and efficient alternative to deep copying.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>Ownership must be represented and transferred via
  pointers (whether smart or plain). Pointer semantics
  are more complicated than value semantics, especially
  in APIs: you have to worry not just about ownership,
  but also aliasing, lifetime, and mutability, among
  other issues.</li>

  <li>The performance costs of value semantics are often
  overestimated, so the performance benefits of ownership
  transfer might not justify the readability and
  complexity costs.</li>

  <li>APIs that transfer ownership force their clients
  into a single memory management model.</li>

  <li>Code using smart pointers is less explicit about
  where the resource releases take place.</li>

  <li><code>std::unique_ptr</code> expresses ownership
  transfer using C++11's move semantics, which are
  relatively new and may confuse some programmers.</li>

  <li>Shared ownership can be a tempting alternative to
  careful ownership design, obfuscating the design of a
  system.</li>

  <li>Shared ownership requires explicit bookkeeping at
  run-time, which can be costly.</li>

  <li>In some cases (e.g., cyclic references), objects
  with shared ownership may never be deleted.</li>

  <li>Smart pointers are not perfect substitutes for
  plain pointers.</li>
</ul>

<p class="decision"></p>
<p>If dynamic allocation is necessary, prefer to keep
ownership with the code that allocated it. If other code
needs access to the object, consider passing it a copy,
or passing a pointer or reference without transferring
ownership. Prefer to use <code>std::unique_ptr</code> to
make ownership transfer explicit. For example:</p>

<pre>std::unique_ptr&lt;Foo&gt; FooFactory();
void FooConsumer(std::unique_ptr&lt;Foo&gt; ptr);
</pre>



<p>Do not design your code to use shared ownership
without a very good reason. One such reason is to avoid
expensive copy operations, but you should only do this if
the performance benefits are significant, and the
underlying object is immutable (i.e.,
<code>std::shared_ptr&lt;const Foo&gt;</code>).  If you
do use shared ownership, prefer to use
<code>std::shared_ptr</code>.</p>

<p>Never use <code>std::auto_ptr</code>. Instead, use
<code>std::unique_ptr</code>.</p>

<h3 id="cpplint">cpplint</h3>

<p>Use <code>cpplint.py</code> to detect style errors.</p>

<p><code>cpplint.py</code>
is a tool that reads a source file and identifies many
style errors. It is not perfect, and has both false
positives and false negatives, but it is still a valuable
tool. </p>



<div>
<p>Some projects have instructions on
how to run <code>cpplint.py</code> from their project
tools. If the project you are contributing to does not,
you can download
<a href="https://raw.githubusercontent.com/google/styleguide/gh-pages/cpplint/cpplint.py">
<code>cpplint.py</code></a> separately.</p>
</div>



<h2 id="Other_C++_Features">Other C++ Features</h2>

<h3 id="Rvalue_references">Rvalue References</h3>

<p>Use rvalue references only in certain special cases listed below.</p>

<p class="definition"></p>
<p> Rvalue references
are a type of reference that can only bind to temporary
objects. The syntax is similar to traditional reference
syntax. For example, <code>void f(std::string&amp;&amp;
s);</code> declares a function whose argument is an
rvalue reference to a std::string.</p>

<p id="Forwarding_references"> When the token '&amp;&amp;' is applied to
an unqualified template argument in a function
parameter, special template argument deduction
rules apply. Such a reference is called forwarding reference.</p>

<p class="pros"></p>
<ul>
  <li>Defining a move constructor (a constructor taking
  an rvalue reference to the class type) makes it
  possible to move a value instead of copying it. If
  <code>v1</code> is a <code>std::vector&lt;std::string&gt;</code>,
  for example, then <code>auto v2(std::move(v1))</code>
  will probably just result in some simple pointer
  manipulation instead of copying a large amount of data.
  In many cases this can result in a major performance
  improvement.</li>

  <li>Rvalue references make it possible to implement
  types that are movable but not copyable, which can be
  useful for types that have no sensible definition of
  copying but where you might still want to pass them as
  function arguments, put them in containers, etc.</li>

  <li><code>std::move</code> is necessary to make
  effective use of some standard-library types, such as
  <code>std::unique_ptr</code>.</li>

  <li><a href="#Forwarding_references">Forwarding references</a> which
  use the rvalue reference token, make it possible to write a
  generic function wrapper that forwards its arguments to
  another function, and works whether or not its
  arguments are temporary objects and/or const.
  This is called 'perfect forwarding'.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>Rvalue references are not yet widely understood. Rules like reference
  collapsing and the special deduction rule for forwarding references
  are somewhat obscure.</li>

  <li>Rvalue references are often misused. Using rvalue
  references is counter-intuitive in signatures where the argument is expected
  to have a valid specified state after the function call, or where no move
  operation is performed.</li>
</ul>

<p class="decision"></p>
<p>Do not use rvalue references (or apply the <code>&amp;&amp;</code>
qualifier to methods), except as follows:</p>
<ul>
  <li>You may use them to define move constructors and move assignment
  operators (as described in
  <a href="#Copyable_Movable_Types">Copyable and Movable Types</a>).
  </li>

  <li>You may use them to define <code>&amp;&amp;</code>-qualified methods that
  logically "consume" <code>*this</code>, leaving it in an unusable
  or empty state. Note that this applies only to method qualifiers (which come
  after the closing parenthesis of the function signature); if you want to
  "consume" an ordinary function parameter, prefer to pass it by value.</li>

  <li>You may use forwarding references in conjunction with <code>
  <a href="http://en.cppreference.com/w/cpp/utility/forward">std::forward</a></code>,
  to support perfect forwarding.</li>

  <li>You may use them to define pairs of overloads, such as one taking
  <code>Foo&amp;&amp;</code> and the other taking <code>const Foo&amp;</code>.
  Usually the preferred solution is just to pass by value, but an overloaded
  pair of functions sometimes yields better performance and is sometimes
  necessary in generic code that needs to support a wide variety of types.
  As always: if you're writing more complicated code for the sake of
  performance, make sure you have evidence that it actually helps.</li>
</ul>

<h3 id="Friends">Friends</h3>

<p>We allow use of <code>friend</code> classes and functions,
within reason.</p>

<p>Friends should usually be defined in the same file so
that the reader does not have to look in another file to
find uses of the private members of a class. A common use
of <code>friend</code> is to have a
<code>FooBuilder</code> class be a friend of
<code>Foo</code> so that it can construct the inner state
of <code>Foo</code> correctly, without exposing this
state to the world. In some cases it may be useful to
make a unittest class a friend of the class it tests.</p>

<p>Friends extend, but do not break, the encapsulation
boundary of a class. In some cases this is better than
making a member public when you want to give only one
other class access to it. However, most classes should
interact with other classes solely through their public
members.</p>

<h3 id="Exceptions">Exceptions</h3>

<p>We do not use C++ exceptions.</p>

<p class="pros"></p>
<ul>
  <li>Exceptions allow higher levels of an application to
  decide how to handle "can't happen" failures in deeply
  nested functions, without the obscuring and error-prone
  bookkeeping of error codes.</li>



  <div>
  <li>Exceptions are used by most other
  modern languages. Using them in C++ would make it more
  consistent with Python, Java, and the C++ that others
  are familiar with.</li>
  </div>

  <li>Some third-party C++ libraries use exceptions, and
  turning them off internally makes it harder to
  integrate with those libraries.</li>

  <li>Exceptions are the only way for a constructor to
  fail. We can simulate this with a factory function or
  an <code>Init()</code> method, but these require heap
  allocation or a new "invalid" state, respectively.</li>

  <li>Exceptions are really handy in testing
  frameworks.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>When you add a <code>throw</code> statement to an
  existing function, you must examine all of its
  transitive callers. Either they must make at least the
  basic exception safety guarantee, or they must never
  catch the exception and be happy with the program
  terminating as a result. For instance, if
  <code>f()</code> calls <code>g()</code> calls
  <code>h()</code>, and <code>h</code> throws an
  exception that <code>f</code> catches, <code>g</code>
  has to be careful or it may not clean up properly.</li>

  <li>More generally, exceptions make the control flow of
  programs difficult to evaluate by looking at code:
  functions may return in places you don't expect. This
  causes maintainability and debugging difficulties. You
  can minimize this cost via some rules on how and where
  exceptions can be used, but at the cost of more that a
  developer needs to know and understand.</li>

  <li>Exception safety requires both RAII and different
  coding practices. Lots of supporting machinery is
  needed to make writing correct exception-safe code
  easy. Further, to avoid requiring readers to understand
  the entire call graph, exception-safe code must isolate
  logic that writes to persistent state into a "commit"
  phase. This will have both benefits and costs (perhaps
  where you're forced to obfuscate code to isolate the
  commit). Allowing exceptions would force us to always
  pay those costs even when they're not worth it.</li>

  <li>Turning on exceptions adds data to each binary
  produced, increasing compile time (probably slightly)
  and possibly increasing address space pressure.
  </li>

  <li>The availability of exceptions may encourage
  developers to throw them when they are not appropriate
  or recover from them when it's not safe to do so. For
  example, invalid user input should not cause exceptions
  to be thrown. We would need to make the style guide
  even longer to document these restrictions!</li>
</ul>

<p class="decision"></p>
<p>On their face, the benefits of using exceptions
outweigh the costs, especially in new projects. However,
for existing code, the introduction of exceptions has
implications on all dependent code. If exceptions can be
propagated beyond a new project, it also becomes
problematic to integrate the new project into existing
exception-free code. Because most existing C++ code at
Google is not prepared to deal with exceptions, it is
comparatively difficult to adopt new code that generates
exceptions.</p>

<p>Given that Google's existing code is not
exception-tolerant, the costs of using exceptions are
somewhat greater than the costs in a new project. The
conversion process would be slow and error-prone. We
don't believe that the available alternatives to
exceptions, such as error codes and assertions, introduce
a significant burden. </p>

<p>Our advice against using exceptions is not predicated
on philosophical or moral grounds, but practical ones.
 Because we'd like to use our open-source
projects at Google and it's difficult to do so if those
projects use exceptions, we need to advise against
exceptions in Google open-source projects as well.
Things would probably be different if we had to do it all
over again from scratch.</p>

<p>This prohibition also applies to the exception handling related
features added in C++11, such as
<code>std::exception_ptr</code> and
<code>std::nested_exception</code>.</p>

<p>There is an <a href="#Windows_Code">exception</a> to
this rule (no pun intended) for Windows code.</p>

<h3 id="noexcept"><code>noexcept</code></h3>

<p>Specify <code>noexcept</code> when it is useful and correct.</p>

<p class="definition"></p>
<p>The <code>noexcept</code> specifier is used to specify whether
a function will throw exceptions or not. If an exception
escapes from a function marked <code>noexcept</code>, the program
crashes via <code>std::terminate</code>.</p>

<p>The <code>noexcept</code> operator performs a compile-time
check that returns true if an expression is declared to not
throw any exceptions.</p>

<p class="pros"></p>
<ul>
  <li>Specifying move constructors as <code>noexcept</code>
  improves performance in some cases, e.g.,
  <code>std::vector&lt;T&gt;::resize()</code> moves rather than
  copies the objects if T's move constructor is
  <code>noexcept</code>.</li>

  <li>Specifying <code>noexcept</code> on a function can
  trigger compiler optimizations in environments where
  exceptions are enabled, e.g., compiler does not have to
  generate extra code for stack-unwinding, if it knows
  that no exceptions can be thrown due to a
  <code>noexcept</code> specifier.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>

  In projects following this guide
  that have exceptions disabled it is hard
  to ensure that <code>noexcept</code>
  specifiers are correct, and hard to define what
  correctness even means.</li>

  <li>It's hard, if not impossible, to undo <code>noexcept</code>
  because it eliminates a guarantee that callers may be relying
  on, in ways that are hard to detect.</li>
</ul>

<p class="decision"></p>
<p>You may use <code>noexcept</code> when it is useful for
performance if it accurately reflects the intended semantics
of your function, i.e., that if an exception is somehow thrown
from within the function body then it represents a fatal error.
You can assume that <code>noexcept</code> on move constructors
has a meaningful performance benefit. If you think
there is significant performance benefit from specifying
<code>noexcept</code> on some other function, please discuss it
with
your project leads.</p>

<p>Prefer unconditional <code>noexcept</code> if exceptions are
completely disabled (i.e., most Google C++ environments).
Otherwise, use conditional <code>noexcept</code> specifiers
with simple conditions, in ways that evaluate false only in
the few cases where the function could potentially throw.
The tests might include type traits check on whether the
involved operation might throw (e.g.,
<code>std::is_nothrow_move_constructible</code> for
move-constructing objects), or on whether allocation can throw
(e.g., <code>absl::default_allocator_is_nothrow</code> for
standard default allocation). Note in many cases the only
possible cause for an exception is allocation failure (we
believe move constructors should not throw except due to
allocation failure), and there are many applications where it’s
appropriate to treat memory exhaustion as a fatal error rather
than an exceptional condition that your program should attempt
to recover from.  Even for other
potential failures you should prioritize interface simplicity
over supporting all possible exception throwing scenarios:
instead of writing a complicated <code>noexcept</code> clause
that depends on whether a hash function can throw, for example,
simply document that your component doesn’t support hash
functions throwing and make it unconditionally
<code>noexcept</code>.</p>

<h3 id="Run-Time_Type_Information__RTTI_">Run-Time Type
Information (RTTI)</h3>

<p>Avoid using run-time type information (RTTI).</p>

<p class="definition"></p>
<p> RTTI allows a
programmer to query the C++ class of an object at
run-time. This is done by use of <code>typeid</code> or
<code>dynamic_cast</code>.</p>

<p class="pros"></p>
<p>The standard alternatives to RTTI (described below)
require modification or redesign of the class hierarchy
in question. Sometimes such modifications are infeasible
or undesirable, particularly in widely-used or mature
code.</p>

<p>RTTI can be useful in some unit tests. For example, it
is useful in tests of factory classes where the test has
to verify that a newly created object has the expected
dynamic type. It is also useful in managing the
relationship between objects and their mocks.</p>

<p>RTTI is useful when considering multiple abstract
objects. Consider</p>

<pre>bool Base::Equal(Base* other) = 0;
bool Derived::Equal(Base* other) {
  Derived* that = dynamic_cast&lt;Derived*&gt;(other);
  if (that == nullptr)
    return false;
  ...
}
</pre>

<p class="cons"></p>
<p>Querying the type of an object at run-time frequently
means a design problem. Needing to know the type of an
object at runtime is often an indication that the design
of your class hierarchy is flawed.</p>

<p>Undisciplined use of RTTI makes code hard to maintain.
It can lead to type-based decision trees or switch
statements scattered throughout the code, all of which
must be examined when making further changes.</p>

<p class="decision"></p>
<p>RTTI has legitimate uses but is prone to abuse, so you
must be careful when using it. You may use it freely in
unittests, but avoid it when possible in other code. In
particular, think twice before using RTTI in new code. If
you find yourself needing to write code that behaves
differently based on the class of an object, consider one
of the following alternatives to querying the type:</p>

<ul>
  <li>Virtual methods are the preferred way of executing
  different code paths depending on a specific subclass
  type. This puts the work within the object itself.</li>

  <li>If the work belongs outside the object and instead
  in some processing code, consider a double-dispatch
  solution, such as the Visitor design pattern. This
  allows a facility outside the object itself to
  determine the type of class using the built-in type
  system.</li>
</ul>

<p>When the logic of a program guarantees that a given
instance of a base class is in fact an instance of a
particular derived class, then a
<code>dynamic_cast</code> may be used freely on the
object.  Usually one
can use a <code>static_cast</code> as an alternative in
such situations.</p>

<p>Decision trees based on type are a strong indication
that your code is on the wrong track.</p>

<pre class="badcode">if (typeid(*data) == typeid(D1)) {
  ...
} else if (typeid(*data) == typeid(D2)) {
  ...
} else if (typeid(*data) == typeid(D3)) {
...
</pre>

<p>Code such as this usually breaks when additional
subclasses are added to the class hierarchy. Moreover,
when properties of a subclass change, it is difficult to
find and modify all the affected code segments.</p>

<p>Do not hand-implement an RTTI-like workaround. The
arguments against RTTI apply just as much to workarounds
like class hierarchies with type tags. Moreover,
workarounds disguise your true intent.</p>

<h3 id="Casting">Casting</h3>

<p>Use C++-style casts
like <code>static_cast&lt;float&gt;(double_value)</code>, or brace
initialization for conversion of arithmetic types like
<code>int64_t y = int64_t{1} &lt;&lt; 42</code>. Do not use
cast formats like <code>(int)x</code> unless the cast is to
<code>void</code>. You may use cast formats like `T(x)` only when
`T` is a class type.</p>

<p class="definition"></p>
<p> C++ introduced a
different cast system from C that distinguishes the types
of cast operations.</p>

<p class="pros"></p>
<p>The problem with C casts is the ambiguity of the operation;
sometimes you are doing a <em>conversion</em>
(e.g., <code>(int)3.5</code>) and sometimes you are doing
a <em>cast</em> (e.g., <code>(int)"hello"</code>). Brace
initialization and C++ casts can often help avoid this
ambiguity. Additionally, C++ casts are more visible when searching for
them.</p>

<p class="cons"></p>
<p>The C++-style cast syntax is verbose and cumbersome.</p>

<p class="decision"></p>
<p>In general, do not use C-style casts. Instead, use these C++-style
casts when explicit type conversion is necessary.
</p>

<ul>
  <li>Use brace initialization to convert arithmetic types
  (e.g., <code>int64_t{x}</code>).  This is the safest approach because code
  will not compile if conversion can result in information loss.  The
  syntax is also concise.</li>



  <li>Use <code>static_cast</code> as the equivalent of a C-style cast
  that does value conversion, when you need to
  explicitly up-cast a pointer from a class to its superclass, or when
  you need to explicitly cast a pointer from a superclass to a
  subclass.  In this last case, you must be sure your object is
  actually an instance of the subclass.</li>



  <li>Use <code>const_cast</code> to remove the
  <code>const</code> qualifier (see <a href="#Use_of_const">const</a>).</li>

  <li>Use <code>reinterpret_cast</code> to do unsafe conversions of
  pointer types to and from integer and other pointer
  types,
  including <code>void*</code>. Use this
  only if you know what you are doing and you understand the aliasing
  issues. Also, consider the alternative
  <code>absl::bit_cast</code>.</li>

  <li>Use <code>absl::bit_cast</code> to interpret the raw bits of a
  value using a different type of the same size (a type pun), such as
  interpreting the bits of a <code>double</code> as
  <code>int64_t</code>.</li>
</ul>

<p>See the <a href="#Run-Time_Type_Information__RTTI_">
RTTI section</a> for guidance on the use of
<code>dynamic_cast</code>.</p>

<h3 id="Streams">Streams</h3>

<p>Use streams where appropriate, and stick to "simple"
usages. Overload <code>&lt;&lt;</code> for streaming only for types
representing values, and write only the user-visible value, not any
implementation details.</p>

<p class="definition"></p>
<p>Streams are the standard I/O abstraction in C++, as
exemplified by the standard header <code>&lt;iostream&gt;</code>.
They are widely used in Google code, mostly for debug logging
and test diagnostics.</p>

<p class="pros"></p>
<p>The <code>&lt;&lt;</code> and <code>&gt;&gt;</code>
stream operators provide an API for formatted I/O that
is easily learned, portable, reusable, and extensible.
<code>printf</code>, by contrast, doesn't even support
<code>std::string</code>, to say nothing of user-defined types,
and is very difficult to use portably.
<code>printf</code> also obliges you to choose among the
numerous slightly different versions of that function,
and navigate the dozens of conversion specifiers.</p>

<p>Streams provide first-class support for console I/O
via <code>std::cin</code>, <code>std::cout</code>,
<code>std::cerr</code>, and <code>std::clog</code>.
The C APIs do as well, but are hampered by the need to
manually buffer the input. </p>

<p class="cons"></p>
<ul>
<li>Stream formatting can be configured by mutating the
state of the stream. Such mutations are persistent, so
the behavior of your code can be affected by the entire
previous history of the stream, unless you go out of your
way to restore it to a known state every time other code
might have touched it. User code can not only modify the
built-in state, it can add new state variables and behaviors
through a registration system.</li>

<li>It is difficult to precisely control stream output, due
to the above issues, the way code and data are mixed in
streaming code, and the use of operator overloading (which
may select a different overload than you expect).</li>

<li>The practice of building up output through chains
of <code>&lt;&lt;</code> operators interferes with
internationalization, because it bakes word order into the
code, and streams' support for localization is <a href="http://www.boost.org/doc/libs/1_48_0/libs/locale/doc/html/rationale.html#rationale_why">
flawed</a>.</li>





<li>The streams API is subtle and complex, so programmers must
develop experience with it in order to use it effectively.</li>

<li>Resolving the many overloads of <code>&lt;&lt;</code> is
extremely costly for the compiler. When used pervasively in a
large code base, it can consume as much as 20% of the parsing
and semantic analysis time.</li>
</ul>

<p class="decision"></p>
<p>Use streams only when they are the best tool for the job.
This is typically the case when the I/O is ad-hoc, local,
human-readable, and targeted at other developers rather than
end-users. Be consistent with the code around you, and with the
codebase as a whole; if there's an established tool for
your problem, use that tool instead.
In particular,

logging libraries are usually a better
choice than <code>std::cerr</code> or <code>std::clog</code>
for diagnostic output, and the libraries in

<code>absl/strings</code>
or the equivalent are usually a
better choice than <code>std::stringstream</code>.</p>

<p>Avoid using streams for I/O that faces external users or
handles untrusted data. Instead, find and use the appropriate
templating libraries to handle issues like internationalization,
localization, and security hardening.</p>

<p>If you do use streams, avoid the stateful parts of the
streams API (other than error state), such as <code>imbue()</code>,
<code>xalloc()</code>, and <code>register_callback()</code>.
Use explicit formatting functions (see e.g.,

<code>absl/strings</code>)
rather than
stream manipulators or formatting flags to control formatting
details such as number base, precision, or padding.</p>

<p>Overload <code>&lt;&lt;</code> as a streaming operator
for your type only if your type represents a value, and
<code>&lt;&lt;</code> writes out a human-readable string
representation of that value. Avoid exposing implementation
details in the output of <code>&lt;&lt;</code>; if you need to print
object internals for debugging, use named functions instead
(a method named <code>DebugString()</code> is the most common
convention).</p>

<h3 id="Preincrement_and_Predecrement">Preincrement and Predecrement</h3>

<p>Use the prefix form (<code>++i</code>) of the increment
and decrement operators unless you need postfix semantics.</p>

<p class="definition"></p>
<p> When a variable
is incremented (<code>++i</code> or <code>i++</code>) or
decremented (<code>--i</code> or <code>i--</code>) and
the value of the expression is not used, one must decide
whether to preincrement (decrement) or postincrement
(decrement).</p>

<p class="pros"></p>

<p>A postfix increment/decrement expression evaluates to the value
<i>as it was before it was modified</i>. This can result in code that is more
compact but harder to read. The prefix form is generally more readable, is
never less efficient, and can be more efficient because it doesn't need to
make a copy of the value as it was before the operation.
</p>

<p class="cons"></p>
<p>The tradition developed, in C, of using post-increment, even
when the expression value is not used, especially in
<code>for</code> loops.</p>

<p class="decision"></p>
<p>Use prefix increment/decrement, unless the code explicitly
needs the result of the postfix increment/decrement expression.</p>

<h3 id="Use_of_const">Use of const</h3>

<p>In APIs, use <code>const</code> whenever it makes sense.
<code>constexpr</code> is a better choice for some uses of
const.</p>

<p class="definition"></p>
<p> Declared variables and parameters can be preceded
by the keyword <code>const</code> to indicate the variables
are not changed (e.g., <code>const int foo</code>). Class
functions can have the <code>const</code> qualifier to
indicate the function does not change the state of the
class member variables (e.g., <code>class Foo { int
Bar(char c) const; };</code>).</p>

<p class="pros"></p>
<p>Easier for people to understand how variables are being
used. Allows the compiler to do better type checking,
and, conceivably, generate better code. Helps people
convince themselves of program correctness because they
know the functions they call are limited in how they can
modify your variables. Helps people know what functions
are safe to use without locks in multi-threaded
programs.</p>

<p class="cons"></p>
<p><code>const</code> is viral: if you pass a
<code>const</code> variable to a function, that function
must have <code>const</code> in its prototype (or the
variable will need a <code>const_cast</code>). This can
be a particular problem when calling library
functions.</p>

<p class="decision"></p>
<p>We strongly recommend using <code>const</code>
in APIs (i.e., on function parameters, methods, and
non-local variables) wherever it is meaningful and accurate. This
provides consistent, mostly compiler-verified documentation
of what objects an operation can mutate. Having
a consistent and reliable way to distinguish reads from writes
is critical to writing thread-safe code, and is useful in
many other contexts as well. In particular:</p>

<ul>
  <li>If a function guarantees that it will not modify an argument
  passed by reference or by pointer, the corresponding function parameter
  should be a reference-to-const (<code>const T&amp;</code>) or
  pointer-to-const (<code>const T*</code>), respectively.</li>

  <li>For a function parameter passed by value, <code>const</code> has
  no effect on the caller, thus is not recommended in function
  declarations. See


  <a href="https://abseil.io/tips/109">TotW #109</a>.


  </li><li>Declare methods to be <code>const</code> unless they
  alter the logical state of the object (or enable the user to modify
  that state, e.g., by returning a non-const reference, but that's
  rare), or they can't safely be invoked concurrently.</li>
</ul>

<p>Using <code>const</code> on local variables is neither encouraged
nor discouraged.</p>

<p>All of a class's <code>const</code> operations should be safe
to invoke concurrently with each other. If that's not feasible, the class must
be clearly documented as "thread-unsafe".</p>


<h4>Where to put the const</h4>

<p>Some people favor the form <code>int const *foo</code>
to <code>const int* foo</code>. They argue that this is
more readable because it's more consistent: it keeps the
rule that <code>const</code> always follows the object
it's describing. However, this consistency argument
doesn't apply in codebases with few deeply-nested pointer
expressions since most <code>const</code> expressions
have only one <code>const</code>, and it applies to the
underlying value. In such cases, there's no consistency
to maintain. Putting the <code>const</code> first is
arguably more readable, since it follows English in
putting the "adjective" (<code>const</code>) before the
"noun" (<code>int</code>).</p>

<p>That said, while we encourage putting
<code>const</code> first, we do not require it. But be
consistent with the code around you!</p>

<h3 id="Use_of_constexpr">Use of constexpr</h3>

<p>Use <code>constexpr</code> to define true
constants or to ensure constant initialization.</p>

<p class="definition"></p>
<p> Some variables can be declared <code>constexpr</code>
to indicate the variables are true constants, i.e., fixed at
compilation/link time. Some functions and constructors
can be declared <code>constexpr</code> which enables them
to be used in defining a <code>constexpr</code>
variable.</p>

<p class="pros"></p>
<p>Use of <code>constexpr</code> enables definition of
constants with floating-point expressions rather than
just literals; definition of constants of user-defined
types; and definition of constants with function
calls.</p>

<p class="cons"></p>
<p>Prematurely marking something as constexpr may cause
migration problems if later on it has to be downgraded.
Current restrictions on what is allowed in constexpr
functions and constructors may invite obscure workarounds
in these definitions.</p>

<p class="decision"></p>
<p><code>constexpr</code> definitions enable a more
robust specification of the constant parts of an
interface. Use <code>constexpr</code> to specify true
constants and the functions that support their
definitions. Avoid complexifying function definitions to
enable their use with <code>constexpr</code>. Do not use
<code>constexpr</code> to force inlining.</p>

<h3 id="Integer_Types">Integer Types</h3>

<p>Of the built-in C++ integer types, the only one used
 is
<code>int</code>. If a program needs a variable of a
different size, use a precise-width integer type from
<code>&lt;stdint.h&gt;</code>, such as
<code>int16_t</code>. If your variable represents a
value that could ever be greater than or equal to 2^31
(2GiB), use a 64-bit type such as <code>int64_t</code>.
Keep in mind that even if your value won't ever be too large
for an <code>int</code>, it may be used in intermediate
calculations which may require a larger type. When in doubt,
choose a larger type.</p>

<p class="definition"></p>
<p> C++ does not specify the sizes of integer types
like <code>int</code>. Typically people assume
that <code>short</code> is 16 bits,
<code>int</code> is 32 bits, <code>long</code> is 32 bits
and <code>long long</code> is 64 bits.</p>

<p class="pros"></p>
<p>Uniformity of declaration.</p>

<p class="cons"></p>
<p>The sizes of integral types in C++ can vary based on
compiler and architecture.</p>

<p class="decision"></p>

<p>We use <code>int</code> very often, for integers we
know are not going to be too big, e.g., loop counters.
Use plain old <code>int</code> for such things. You
should assume that an <code>int</code> is

at least 32 bits, but don't
assume that it has more than 32 bits. If you need a 64-bit
integer type, use <code>int64_t</code> or <code>uint64_t</code>.

</p><p>For integers we know can be "big",
 use
<code>int64_t</code>.
</p>

<p>You should not use the unsigned integer types such as
<code>uint32_t</code>, unless there is a valid
reason such as representing a bit pattern rather than a
number, or you need defined overflow modulo 2^N. In
particular, do not use unsigned types to say a number
will never be negative. Instead, use

assertions for this.</p>



<p>If your code is a container that returns a size, be
sure to use a type that will accommodate any possible
usage of your container. When in doubt, use a larger type
rather than a smaller type.</p>

<p>Use care when converting integer types. Integer conversions and
promotions can cause undefined behavior, leading to security bugs and
other problems.</p>

<h4>On Unsigned Integers</h4>

<p>Unsigned integers are good for representing bitfields and modular
arithmetic. Because of historical accident, the C++ standard also uses
unsigned integers to represent the size of containers - many members
of the standards body believe this to be a mistake, but it is
effectively impossible to fix at this point. The fact that unsigned
arithmetic doesn't model the behavior of a simple integer, but is
instead defined by the standard to model modular arithmetic (wrapping
around on overflow/underflow), means that a significant class of bugs
cannot be diagnosed by the compiler. In other cases, the defined
behavior impedes optimization.</p>

<p>That said, mixing signedness of integer types is responsible for an
equally large class of problems. The best advice we can provide: try
to use iterators and containers rather than pointers and sizes, try
not to mix signedness, and try to avoid unsigned types (except for
representing bitfields or modular arithmetic). Do not use an unsigned
type merely to assert that a variable is non-negative.</p>

<h3 id="64-bit_Portability">64-bit Portability</h3>

<p>Code should be 64-bit and 32-bit friendly. Bear in mind
problems of printing, comparisons, and structure alignment.</p>

<ul>
  <li>
  <p>Correct portable <code>printf()</code> conversion specifiers for
  some integral typedefs rely on macro expansions that we find unpleasant to
  use and impractical to require (the <code>PRI</code> macros from
  <code>&lt;cinttypes&gt;</code>). Unless there is no reasonable alternative
  for your particular case, try to avoid or even upgrade APIs that rely on the
  <code>printf</code> family. Instead use a library supporting typesafe numeric
  formatting, such as


    <a href="https://github.com/abseil/abseil-cpp/blob/master/absl/strings/str_cat.h"><code>StrCat</code></a>

    or


    <a href="https://github.com/abseil/abseil-cpp/blob/master/absl/strings/substitute.h"><code>Substitute</code></a>

    for fast simple conversions,

    or <a href="#Streams"><code>std::ostream</code></a>.</p>

  <p>Unfortunately, the <code>PRI</code> macros are the only portable way to
  specify a conversion for the standard bitwidth typedefs (e.g.,
  <code>int64_t</code>, <code>uint64_t</code>, <code>int32_t</code>,
  <code>uint32_t</code>, etc).
  Where possible, avoid passing arguments of types specified by bitwidth
  typedefs to <code>printf</code>-based APIs. Note that it is acceptable
  to use typedefs for which printf has dedicated length modifiers, such as
  <code>size_t</code> (<code>z</code>),
  <code>ptrdiff_t</code> (<code>t</code>), and
  <code>maxint_t</code> (<code>j</code>).</p>
  </li>

  <li>Remember that <code>sizeof(void *)</code> !=
  <code>sizeof(int)</code>. Use <code>intptr_t</code> if
  you want a pointer-sized integer.</li>

  <li>You may need to be careful with structure
  alignments, particularly for structures being stored on
  disk. Any class/structure with a <code>int64_t</code>/<code>uint64_t</code>
  member will by default end up being 8-byte aligned on a
  64-bit system. If you have such structures being shared
  on disk between 32-bit and 64-bit code, you will need
  to ensure that they are packed the same on both
  architectures.
  Most compilers offer a way to
  alter structure alignment. For gcc, you can use
  <code>__attribute__((packed))</code>. MSVC offers
  <code>#pragma pack()</code> and
  <code>__declspec(align())</code>.</li>

  <li>
  <p>Use <a href="#Casting">braced-initialization</a> as needed to create
  64-bit constants. For example:</p>
<pre>int64_t my_value{0x123456789};
uint64_t my_mask{3ULL &lt;&lt; 48};
</pre>
  </li>
</ul>

<h3 id="Preprocessor_Macros">Preprocessor Macros</h3>

<p>Avoid defining macros, especially in headers; prefer
inline functions, enums, and <code>const</code> variables.
Name macros with a project-specific prefix. Do not use
macros to define pieces of a C++ API.</p>

<p>Macros mean that the code you see is not the same as
the code the compiler sees. This can introduce unexpected
behavior, especially since macros have global scope.</p>

<p>The problems introduced by macros are especially severe
when they are used to define pieces of a C++ API,
and still more so for public APIs. Every error message from
the compiler when developers incorrectly use that interface
now must explain how the macros formed the interface.
Refactoring and analysis tools have a dramatically harder
time updating the interface. As a consequence, we
specifically disallow using macros in this way.
For example, avoid patterns like:</p>

<pre class="badcode">class WOMBAT_TYPE(Foo) {
  // ...

 public:
  EXPAND_PUBLIC_WOMBAT_API(Foo)

  EXPAND_WOMBAT_COMPARISONS(Foo, ==, &lt;)
};
</pre>

<p>Luckily, macros are not nearly as necessary in C++ as
they are in C. Instead of using a macro to inline
performance-critical code, use an inline function.
Instead of using a macro to store a constant, use a
<code>const</code> variable. Instead of using a macro to
"abbreviate" a long variable name, use a reference.
Instead of using a macro to conditionally compile code
... well, don't do that at all (except, of course, for
the <code>#define</code> guards to prevent double
inclusion of header files). It makes testing much more
difficult.</p>

<p>Macros can do things these other techniques cannot,
and you do see them in the codebase, especially in the
lower-level libraries. And some of their special features
(like stringifying, concatenation, and so forth) are not
available through the language proper. But before using a
macro, consider carefully whether there's a non-macro way
to achieve the same result. If you need to use a macro to
define an interface, contact
your project leads to request
a waiver of this rule.</p>

<p>The following usage pattern will avoid many problems
with macros; if you use macros, follow it whenever
possible:</p>

<ul>
  <li>Don't define macros in a <code>.h</code> file.</li>

  <li><code>#define</code> macros right before you use
  them, and <code>#undef</code> them right after.</li>

  <li>Do not just <code>#undef</code> an existing macro
  before replacing it with your own; instead, pick a name
  that's likely to be unique.</li>

  <li>Try not to use macros that expand to unbalanced C++
  constructs, or at least document that behavior
  well.</li>

  <li>Prefer not using <code>##</code> to generate
  function/class/variable names.</li>
</ul>

<p>Exporting macros from headers (i.e., defining them in a header
without <code>#undef</code>ing them before the end of the header)
is extremely strongly discouraged. If you do export a macro from a
header, it must have a globally unique name. To achieve this, it
must be named with a prefix consisting of your project's namespace
name (but upper case). </p>

<h3 id="0_and_nullptr/NULL">0 and nullptr/NULL</h3>

<p>Use <code>nullptr</code> for pointers, and <code>'\0'</code> for chars (and
not the <code>0</code> literal).</p>

<p>For pointers (address values), use <code>nullptr</code>, as this
provides type-safety.</p>

<p>For C++03 projects, prefer <code>NULL</code> to <code>0</code>. While the
values are equivalent, <code>NULL</code> looks more like a pointer to the
reader, and some C++ compilers provide special definitions of <code>NULL</code>
which enable them to give useful warnings. Never use <code>NULL</code> for
numeric (integer or floating-point) values.</p>

<p>Use <code>'\0'</code> for the null character. Using the correct type makes
the code more readable.</p>

<h3 id="sizeof">sizeof</h3>

<p>Prefer <code>sizeof(<var>varname</var>)</code> to
<code>sizeof(<var>type</var>)</code>.</p>

<p>Use <code>sizeof(<var>varname</var>)</code> when you
take the size of a particular variable.
<code>sizeof(<var>varname</var>)</code> will update
appropriately if someone changes the variable type either
now or later. You may use
<code>sizeof(<var>type</var>)</code> for code unrelated
to any particular variable, such as code that manages an
external or internal data format where a variable of an
appropriate C++ type is not convenient.</p>

<pre>MyStruct data;
memset(&amp;data, 0, sizeof(data));
</pre>

<pre class="badcode">memset(&amp;data, 0, sizeof(MyStruct));
</pre>

<pre>if (raw_size &lt; sizeof(int)) {
  LOG(ERROR) &lt;&lt; "compressed record not big enough for count: " &lt;&lt; raw_size;
  return false;
}
</pre>

<a id="auto"></a>
<h3 id="Type_deduction">Type Deduction (including auto)</h3>

<p>Use type deduction only if it makes the code clearer to readers who aren't
  familiar with the project, or if it makes the code safer. Do not use it
  merely to avoid the inconvenience of writing an explicit type.</p>

<p class="definition"></p>

<p>There are several contexts in which C++ allows (or even requires) types to
be deduced by the compiler, rather than spelled out explicitly in the code:</p>
<dl>
  <dt><a href="https://en.cppreference.com/w/cpp/language/template_argument_deduction">Function template argument deduction</a></dt>
  <dd>A function template can be invoked without explicit template arguments.
    The compiler deduces those arguments from the types of the function
    arguments:
    <pre class="neutralcode">template &lt;typename T&gt;
void f(T t);

f(0);  // Invokes f&lt;int&gt;(0)</pre>
  </dd>
  <dt><a href="https://en.cppreference.com/w/cpp/language/auto"><code>auto</code> variable declarations</a></dt>
  <dd>A variable declaration can use the <code>auto</code> keyword in place
    of the type. The compiler deduces the type from the variable's
    initializer, following the same rules as function template argument
    deduction with the same initializer (so long as you don't use curly braces
    instead of parentheses).
    <pre class="neutralcode">auto a = 42;  // a is an int
auto&amp; b = a;  // b is an int&amp;
auto c = b;   // c is an int
auto d{42};   // d is an int, not a std::initializer_list&lt;int&gt;
</pre>
    <code>auto</code> can be qualified with <code>const</code>, and can be
    used as part of a pointer or reference type, but it can't be used as a
    template argument. A rare variant of this syntax uses
    <code>decltype(auto)</code> instead of <code>auto</code>, in which case
    the deduced type is the result of applying
    <a href="https://en.cppreference.com/w/cpp/language/decltype"><code>decltype</code></a>
    to the initializer.
  </dd>
  <dt><a href="https://en.cppreference.com/w/cpp/language/function#Return_type_deduction">Function return type deduction</a></dt>
  <dd><code>auto</code> (and <code>decltype(auto)</code>) can also be used in
    place of a function return type. The compiler deduces the return type from
    the <code>return</code> statements in the function body, following the same
    rules as for variable declarations:
    <pre class="neutralcode">auto f() { return 0; }  // The return type of f is int</pre>
    <a href="#Lambda_expressions">Lambda expression</a> return types can be
    deduced in the same way, but this is triggered by omitting the return type,
    rather than by an explicit <code>auto</code>. Confusingly,
    <a href="#trailing_return">trailing return type</a> syntax for functions
    also uses <code>auto</code> in the return-type position, but that doesn't
    rely on type deduction; it's just an alternate syntax for an explicit
    return type.
  </dd>
  <dt><a href="https://isocpp.org/wiki/faq/cpp14-language#generic-lambdas">Generic lambdas</a></dt>
  <dd>A lambda expression can use the <code>auto</code> keyword in place of
    one or more of its parameter types. This causes the lambda's call operator
    to be a function template instead of an ordinary function, with a separate
    template parameter for each <code>auto</code> function parameter:
    <pre class="neutralcode">// Sort `vec` in decreasing order
std::sort(vec.begin(), vec.end(), [](auto lhs, auto rhs) { return lhs &gt; rhs; });</pre>
  </dd>
  <dt><a href="https://isocpp.org/wiki/faq/cpp14-language#lambda-captures">Lambda init captures</a></dt>
  <dd>Lambda captures can have explicit initializers, which can be used to
    declare wholly new variables rather than only capturing existing ones:
    <pre class="neutralcode">[x = 42, y = "foo"] { ... }  // x is an int, and y is a const char*</pre>
    This syntax doesn't allow the type to be specified; instead, it's deduced
    using the rules for <code>auto</code> variables.
  </dd>
  <dt><a href="https://en.cppreference.com/w/cpp/language/class_template_argument_deduction">Class template argument deduction</a></dt>
  <dd>See <a href="#CTAD">below</a>.</dd>
  <dt><a href="https://en.cppreference.com/w/cpp/language/structured_binding">Structured bindings</a></dt>
  <dd>When declaring a tuple, struct, or array using <code>auto</code>, you can
    specify names for the individual elements instead of a name for the whole
    object; these names are called "structured bindings", and the whole
    declaration is called a "structured binding declaration". This syntax
    provides no way of specifying the type of either the enclosing object
    or the individual names:
    <pre class="neutralcode">auto [iter, success] = my_map.insert({key, value});
if (!success) {
  iter-&gt;second = value;
}</pre>
    The <code>auto</code> can also be qualified with <code>const</code>,
    <code>&amp;</code>, and <code>&amp;&amp;</code>, but note that these qualifiers
    technically apply to the anonymous tuple/struct/array, rather than the
    individual bindings. The rules that determine the types of the bindings
    are quite complex; the results tend to be unsurprising, except that
    the binding types typically won't be references even if the declaration
    declares a reference (but they will usually behave like references anyway).
  </dd>
</dl>

<p>(These summaries omit many details and caveats; see the links for further
  information.)</p>

<p class="pros"></p>

<ul>
  <li>C++ type names can be long and cumbersome, especially when they
    involve templates or namespaces.</li>
  <li>When a C++ type name is repeated within a single declaration or a
    small code region, the repetition may not be aiding readability.</li>
  <li>It is sometimes safer to let the type be deduced, since that avoids
    the possibility of unintended copies or type conversions.</li>
</ul>

<p class="cons"></p>
<p>C++ code is usually clearer when types are explicit,
  especially when type deduction would depend on information from
  distant parts of the code. In expressions like:</p>

<pre class="badcode">auto foo = x.add_foo();
auto i = y.Find(key);
</pre>

<p>it may not be obvious what the resulting types are if the type
  of <code>y</code> isn't very well known, or if <code>y</code> was
  declared many lines earlier.</p>

<p>Programmers have to understand when type deduction will or won't
  produce a reference type, or they'll get copies when they didn't
  mean to.</p>

<p>If a deduced type is used as part of an interface, then a
  programmer might change its type while only intending to
  change its value, leading to a more radical API change
  than intended.</p>

<p class="decision"></p>

<p>The fundamental rule is: use type deduction only to make the code
  clearer or safer, and do not use it merely to avoid the
  inconvenience of writing an explicit type. When judging whether the
  code is clearer, keep in mind that your readers are not necessarily
  on your team, or familiar with your project, so types that you and
  your reviewer experience as unnecessary clutter will very often
  provide useful information to others. For example, you can assume that
  the return type of <code>make_unique&lt;Foo&gt;()</code> is obvious,
  but the return type of <code>MyWidgetFactory()</code> probably isn't.</p>

  <p>These principles apply to all forms of type deduction, but the
  details vary, as described in the following sections.</p>

<h4>Function template argument deduction</h4>

<p>Function template argument deduction is almost always OK. Type deduction
  is the expected default way of interacting with function templates,
  because it allows function templates to act like infinite sets of ordinary
  function overloads. Consequently, function templates are almost always
  designed so that template argument deduction is clear and safe, or
  doesn't compile.</p>

<h4>Local variable type deduction</h4>

<p>For local variables, you can use type deduction to make the code clearer
  by eliminating type information that is obvious or irrelevant, so that
  the reader can focus on the meaningful parts of the code:</p>
  <pre class="neutralcode">std::unique_ptr&lt;WidgetWithBellsAndWhistles&gt; widget_ptr =
    absl::make_unique&lt;WidgetWithBellsAndWhistles&gt;(arg1, arg2);
absl::flat_hash_map&lt;std::string,
                    std::unique_ptr&lt;WidgetWithBellsAndWhistles&gt;&gt;::const_iterator
    it = my_map_.find(key);
std::array&lt;int, 6&gt; numbers = {4, 8, 15, 16, 23, 42};</pre>

  <pre class="goodcode">auto widget_ptr = absl::make_unique&lt;WidgetWithBellsAndWhistles&gt;(arg1, arg2);
auto it = my_map_.find(key);
std::array numbers = {4, 8, 15, 16, 23, 42};</pre>

<p>Types sometimes contain a mixture of useful information and boilerplate,
  such as <code>it</code> in the example above: it's obvious that the
  type is an iterator, and in many contexts the container type and even the
  key type aren't relevant, but the type of the values is probably useful.
  In such situations, it's often possible to define local variables with
  explicit types that convey the relevant information:</p>
  <pre class="goodcode">if (auto it = my_map_.find(key); it != my_map_.end()) {
  WidgetWithBellsAndWhistles&amp; widget = *it-&gt;second;
  // Do stuff with `widget`
}</pre>
<p>If the type is a template instance, and the parameters are
  boilerplate but the template itself is informative, you can use
  class template argument deduction to suppress the boilerplate. However,
  cases where this actually provides a meaningful benefit are quite rare.
  Note that class template argument deduction is also subject to a
  <a href="#CTAD">separate style rule</a>.</p>

<p>Do not use <code>decltype(auto)</code> if a simpler option will work,
  because it's a fairly obscure feature, so it has a high cost in code
  clarity.</p>

<h4>Return type deduction</h4>

<p>Use return type deduction (for both functions and lambdas) only if the
  function body has a very small number of <code>return</code> statements,
  and very little other code, because otherwise the reader may not be able
  to tell at a glance what the return type is. Furthermore, use it only
  if the function or lambda has a very narrow scope, because functions with
  deduced return types don't define abstraction boundaries: the implementation
  <em>is</em> the interface. In particular, public functions in header files
  should almost never have deduced return types.</p>

<h4>Parameter type deduction</h4>

<p><code>auto</code> parameter types for lambdas should be used with caution,
  because the actual type is determined by the code that calls the lambda,
  rather than by the definition of the lambda. Consequently, an explicit
  type will almost always be clearer unless the lambda is explicitly called
  very close to where it's defined (so that the reader can easily see both),
  or the lambda is passed to an interface so well-known that it's
  obvious what arguments it will eventually be called with (e.g.,
  the <code>std::sort</code> example above).</p>

<h4>Lambda init captures</h4>

<p>Init captures are covered by a <a href="#Lambda_expressions">more specific
    style rule</a>, which largely supersedes the general rules for
  type deduction.</p>

<h4>Structured bindings</h4>

<p>Unlike other forms of type deduction, structured bindings can actually
  give the reader additional information, by giving meaningful names to the
  elements of a larger object. This means that a structured binding declaration
  may provide a net readability improvement over an explicit type, even in cases
  where <code>auto</code> would not. Structured bindings are especially
  beneficial when the object is a pair or tuple (as in the <code>insert</code>
  example above), because they don't have meaningful field names to begin with,
  but note that you generally <a href="#Structs_vs._Tuples">shouldn't use
    pairs or tuples</a> unless a pre-existing API like <code>insert</code>
  forces you to.</p>

<p>If the object being bound is a struct, it may sometimes be helpful to
  provide names that are more specific to your usage, but keep in mind that
  this may also mean the names are less recognizable to your reader than the
  field names. We recommend using a comment to indicate the name of the
  underlying field, if it doesn't match the name of the binding, using the
  same syntax as for function parameter comments:</p>
  <pre>auto [/*field_name1=*/ bound_name1, /*field_name2=*/ bound_name2] = ...</pre>
<p>As with function parameter comments, this can enable tools to detect if
  you get the order of the fields wrong.</p>

<h3 id="CTAD">Class Template Argument Deduction</h3>

<p>Use class template argument deduction only with templates that have
  explicitly opted into supporting it.</p>

<p class="definition"></p>
<p><a href="https://en.cppreference.com/w/cpp/language/class_template_argument_deduction">Class
    template argument deduction</a> (often abbreviated "CTAD") occurs when
  a variable is declared with a type that names a template, and the template
  argument list is not provided (not even empty angle brackets):</p>
  <pre class="neutralcode">std::array a = {1, 2, 3};  // `a` is a std::array&lt;int, 3&gt;</pre>
<p>The compiler deduces the arguments from the initializer using the
  template's "deduction guides", which can be explicit or implicit.</p>

<p>Explicit deduction guides look like function declarations with trailing
  return types, except that there's no leading <code>auto</code>, and the
  function name is the name of the template. For example, the above example
  relies on this deduction guide for <code>std::array</code>:</p>
  <pre class="neutralcode">namespace std {
template &lt;class T, class... U&gt;
array(T, U...) -&gt; std::array&lt;T, 1 + sizeof...(U)&gt;;
}</pre>
<p>Constructors in a primary template (as opposed to a template specialization)
  also implicitly define deduction guides.</p>

<p>When you declare a variable that relies on CTAD, the compiler selects
  a deduction guide using the rules of constructor overload resolution,
  and that guide's return type becomes the type of the variable.</p>

<p class="pros"></p>
<p>CTAD can sometimes allow you to omit boilerplate from your code.</p>

<p class="cons"></p>
<p>The implicit deduction guides that are generated from constructors
  may have undesirable behavior, or be outright incorrect. This is
  particularly problematic for constructors written before CTAD was
  introduced in C++17, because the authors of those constructors had no
  way of knowing about (much less fixing) any problems that their
  constructors would cause for CTAD. Furthermore, adding explicit deduction
  guides to fix those problems might break any existing code that relies on
  the implicit deduction guides.</p>

<p>CTAD also suffers from many of the same drawbacks as <code>auto</code>,
  because they are both mechanisms for deducing all or part of a variable's
  type from its initializer. CTAD does give the reader more information
  than <code>auto</code>, but it also doesn't give the reader an obvious
  cue that information has been omitted.</p>

<p class="decision"></p>
<p>Do not use CTAD with a given template unless the template's maintainers
  have opted into supporting use of CTAD by providing at least one explicit
  deduction guide (all templates in the <code>std</code> namespace are
  also presumed to have opted in). This should be enforced with a compiler
  warning if available.</p>

<p>Uses of CTAD must also follow the general rules on
  <a href="#Type_deduction">Type deduction</a>.</p>

<h3 id="Designated_initializers">Designated Initializers</h3>

<p>Use designated initializers only in their C++20-compliant form.</p>

<p class="definition"></p>
<p><a href="https://en.cppreference.com/w/cpp/language/aggregate_initialization#Designated_initializers">
  Designated initializers</a> are a syntax that allows for initializing an
  aggregate ("plain old struct") by naming its fields explicitly:</p>
  <pre class="neutralcode">  struct Point {
    float x = 0.0;
    float y = 0.0;
    float z = 0.0;
  };

  Point p = {
    .x = 1.0,
    .y = 2.0,
    // z will be 0.0
  };</pre>
<p>The explicitly listed fields will be initialized as specified, and others
  will be initialized in the same way they would be in a traditional aggregate
  initialization expression like <code>Point{1.0, 2.0}</code>.</p>

<p class="pros"></p>
<p>Designated initializers can make for convenient and highly readable
aggregate expressions, especially for structs with less straightforward
ordering of fields than the <code>Point</code> example above.</p>

<p class="cons"></p>
<p>While designated initializers have long been part of the C standard and
supported by C++ compilers as an extension, only recently have they made it
into the draft C++ standard. They are on track for publishing in C++20.</p>

<p>The rules in the draft C++ standard are stricter than in C and compiler
extensions, requiring that the designated initializers appear in the same order
as the fields appear in the struct definition. So in the example above it is
legal according to draft C++20 to initialize <code>x</code> and then
<code>z</code>, but not <code>y</code> and then <code>x</code>.</p>

<p class="decision"></p>
<p>Use designated initializers only in the form that is compatible with the
draft C++20 standard: with initializers in the same order as the corresponding
fields appear in the struct definition.</p>



<h3 id="Lambda_expressions">Lambda Expressions</h3>

<p>Use lambda expressions where appropriate. Prefer explicit captures
when the lambda will escape the current scope.</p>

<p class="definition"></p>
<p> Lambda expressions are a concise way of creating anonymous
function objects. They're often useful when passing
functions as arguments. For example:</p>

<pre>std::sort(v.begin(), v.end(), [](int x, int y) {
  return Weight(x) &lt; Weight(y);
});
</pre>

<p> They further allow capturing variables from the enclosing scope either
explicitly by name, or implicitly using a default capture. Explicit captures
require each variable to be listed, as
either a value or reference capture:</p>

<pre>int weight = 3;
int sum = 0;
// Captures `weight` by value and `sum` by reference.
std::for_each(v.begin(), v.end(), [weight, &amp;sum](int x) {
  sum += weight * x;
});
</pre>


<p>Default captures implicitly capture any variable referenced in the
lambda body, including <code>this</code> if any members are used:</p>

<pre>const std::vector&lt;int&gt; lookup_table = ...;
std::vector&lt;int&gt; indices = ...;
// Captures `lookup_table` by reference, sorts `indices` by the value
// of the associated element in `lookup_table`.
std::sort(indices.begin(), indices.end(), [&amp;](int a, int b) {
  return lookup_table[a] &lt; lookup_table[b];
});
</pre>

<p>A variable capture can also have an explicit initializer, which can
  be used for capturing move-only variables by value, or for other situations
  not handled by ordinary reference or value captures:</p>
  <pre>std::unique_ptr&lt;Foo&gt; foo = ...;
[foo = std::move(foo)] () {
  ...
}</pre>
<p>Such captures (often called "init captures" or "generalized lambda captures")
  need not actually "capture" anything from the enclosing scope, or even have
  a name from the enclosing scope; this syntax is a fully general way to define
  members of a lambda object:</p>
  <pre class="neutralcode">[foo = std::vector&lt;int&gt;({1, 2, 3})] () {
  ...
}</pre>
<p>The type of a capture with an initializer is deduced using the same rules
  as <code>auto</code>.</p>

<p class="pros"></p>
<ul>
  <li>Lambdas are much more concise than other ways of
   defining function objects to be passed to STL
   algorithms, which can be a readability
   improvement.</li>

  <li>Appropriate use of default captures can remove
    redundancy and highlight important exceptions from
    the default.</li>

   <li>Lambdas, <code>std::function</code>, and
   <code>std::bind</code> can be used in combination as a
   general purpose callback mechanism; they make it easy
   to write functions that take bound functions as
   arguments.</li>
</ul>

<p class="cons"></p>
<ul>
  <li>Variable capture in lambdas can be a source of dangling-pointer
  bugs, particularly if a lambda escapes the current scope.</li>

  <li>Default captures by value can be misleading because they do not prevent
  dangling-pointer bugs. Capturing a pointer by value doesn't cause a deep
  copy, so it often has the same lifetime issues as capture by reference.
  This is especially confusing when capturing 'this' by value, since the use
  of 'this' is often implicit.</li>

  <li>Captures actually declare new variables (whether or not the captures have
    initializers), but they look nothing like any other variable declaration
    syntax in C++. In particular, there's no place for the variable's type,
    or even an <code>auto</code> placeholder (although init captures can
    indicate it indirectly, e.g., with a cast). This can make it difficult to
    even recognize them as declarations.</li>

  <li>Init captures inherently rely on <a href="#Type_deduction">type
      deduction</a>, and suffer from many of the same drawbacks as
    <code>auto</code>, with the additional problem that the syntax doesn't
    even cue the reader that deduction is taking place.</li>

  <li>It's possible for use of lambdas to get out of
  hand; very long nested anonymous functions can make
  code harder to understand.</li>

</ul>

<p class="decision"></p>
<ul>
<li>Use lambda expressions where appropriate, with formatting as
described <a href="#Formatting_Lambda_Expressions">below</a>.</li>
<li>Prefer explicit captures if the lambda may escape the current scope.
For example, instead of:
<pre class="badcode">{
  Foo foo;
  ...
  executor-&gt;Schedule([&amp;] { Frobnicate(foo); })
  ...
}
// BAD! The fact that the lambda makes use of a reference to `foo` and
// possibly `this` (if `Frobnicate` is a member function) may not be
// apparent on a cursory inspection. If the lambda is invoked after
// the function returns, that would be bad, because both `foo`
// and the enclosing object could have been destroyed.
</pre>
prefer to write:
<pre>{
  Foo foo;
  ...
  executor-&gt;Schedule([&amp;foo] { Frobnicate(foo); })
  ...
}
// BETTER - The compile will fail if `Frobnicate` is a member
// function, and it's clearer that `foo` is dangerously captured by
// reference.
</pre>
</li>
<li>Use default capture by reference ([&amp;]) only when the
lifetime of the lambda is obviously shorter than any potential
captures.
</li>
<li>Use default capture by value ([=]) only as a means of binding a
few variables for a short lambda, where the set of captured
variables is obvious at a glance. Prefer not to write long or
complex lambdas with default capture by value.
</li>
<li>Use captures only to actually capture variables from the enclosing scope.
  Do not use captures with initializers to introduce new names, or
  to substantially change the meaning of an existing name. Instead,
  declare a new variable in the conventional way and then capture it,
  or avoid the lambda shorthand and define a function object explicitly.</li>
<li>See the section on <a href="#Type_deduction">type deduction</a>
  for guidance on specifying the parameter and return types.</li>

</ul>

<h3 id="Template_metaprogramming">Template Metaprogramming</h3>

<p>Avoid complicated template programming.</p>

<p class="definition"></p>
<p>Template metaprogramming refers to a family of techniques that
exploit the fact that the C++ template instantiation mechanism is
Turing complete and can be used to perform arbitrary compile-time
computation in the type domain.</p>

<p class="pros"></p>
<p>Template metaprogramming allows extremely flexible interfaces that
are type safe and high performance. Facilities like

<a href="https://github.com/google/googletest">GoogleTest</a>,
<code>std::tuple</code>, <code>std::function</code>, and
Boost.Spirit would be impossible without it.</p>

<p class="cons"></p>
<p>The techniques used in template metaprogramming are often obscure
to anyone but language experts. Code that uses templates in
complicated ways is often unreadable, and is hard to debug or
maintain.</p>

<p>Template metaprogramming often leads to extremely poor compile
time error messages: even if an interface is simple, the complicated
implementation details become visible when the user does something
wrong.</p>

<p>Template metaprogramming interferes with large scale refactoring by
making the job of refactoring tools harder. First, the template code
is expanded in multiple contexts, and it's hard to verify that the
transformation makes sense in all of them. Second, some refactoring
tools work with an AST that only represents the structure of the code
after template expansion. It can be difficult to automatically work
back to the original source construct that needs to be
rewritten.</p>

<p class="decision"></p>
<p>Template metaprogramming sometimes allows cleaner and easier-to-use
interfaces than would be possible without it, but it's also often a
temptation to be overly clever. It's best used in a small number of
low level components where the extra maintenance burden is spread out
over a large number of uses.</p>

<p>Think twice before using template metaprogramming or other
complicated template techniques; think about whether the average
member of your team will be able to understand your code well enough
to maintain it after you switch to another project, or whether a
non-C++ programmer or someone casually browsing the code base will be
able to understand the error messages or trace the flow of a function
they want to call.  If you're using recursive template instantiations
or type lists or metafunctions or expression templates, or relying on
SFINAE or on the <code>sizeof</code> trick for detecting function
overload resolution, then there's a good chance you've gone too
far.</p>

<p>If you use template metaprogramming, you should expect to put
considerable effort into minimizing and isolating the complexity. You
should hide metaprogramming as an implementation detail whenever
possible, so that user-facing headers are readable, and you should
make sure that tricky code is especially well commented. You should
carefully document how the code is used, and you should say something
about what the "generated" code looks like. Pay extra attention to the
error messages that the compiler emits when users make mistakes.  The
error messages are part of your user interface, and your code should
be tweaked as necessary so that the error messages are understandable
and actionable from a user point of view.</p>

<h3 id="Boost">Boost</h3>

<p>Use only approved libraries from the Boost library
collection.</p>

<p class="definition"></p>
<p> The
<a href="https://www.boost.org/">
Boost library collection</a> is a popular collection of
peer-reviewed, free, open-source C++ libraries.</p>

<p class="pros"></p>
<p>Boost code is generally very high-quality, is widely
portable, and fills many important gaps in the C++
standard library, such as type traits and better binders.</p>

<p class="cons"></p>
<p>Some Boost libraries encourage coding practices which can
hamper readability, such as metaprogramming and other
advanced template techniques, and an excessively
"functional" style of programming. </p>

<p class="decision"></p>



<div>
<p>In order to maintain a high level of readability for
all contributors who might read and maintain code, we
only allow an approved subset of Boost features.
Currently, the following libraries are permitted:</p>

<ul>
  <li>
  <a href="https://www.boost.org/libs/utility/call_traits.htm">
  Call Traits</a> from <code>boost/call_traits.hpp</code></li>

  <li><a href="https://www.boost.org/libs/utility/compressed_pair.htm">
  Compressed Pair</a> from  <code>boost/compressed_pair.hpp</code></li>

  <li><a href="https://www.boost.org/libs/graph/">
  The Boost Graph Library (BGL)</a> from <code>boost/graph</code>,
  except serialization (<code>adj_list_serialize.hpp</code>) and
   parallel/distributed algorithms and data structures
   (<code>boost/graph/parallel/*</code> and
   <code>boost/graph/distributed/*</code>).</li>

  <li><a href="https://www.boost.org/libs/property_map/">
  Property Map</a> from <code>boost/property_map</code>, except
  parallel/distributed property maps (<code>boost/property_map/parallel/*</code>).</li>

  <li><a href="https://www.boost.org/libs/iterator/">
  Iterator</a> from <code>boost/iterator</code></li>

  <li>The part of <a href="https://www.boost.org/libs/polygon/">
  Polygon</a> that deals with Voronoi diagram
  construction and doesn't depend on the rest of
  Polygon:
  <code>boost/polygon/voronoi_builder.hpp</code>,
  <code>boost/polygon/voronoi_diagram.hpp</code>, and
  <code>boost/polygon/voronoi_geometry_type.hpp</code></li>

  <li><a href="https://www.boost.org/libs/bimap/">
  Bimap</a> from <code>boost/bimap</code></li>

  <li><a href="https://www.boost.org/libs/math/doc/html/dist.html">
  Statistical Distributions and Functions</a> from
  <code>boost/math/distributions</code></li>

  <li><a href="https://www.boost.org/libs/math/doc/html/special.html">
  Special Functions</a> from <code>boost/math/special_functions</code></li>

  <li><a href="https://www.boost.org/libs/math/doc/html/root_finding.html">
  Root Finding Functions</a> from <code>boost/math/tools</code></li>

  <li><a href="https://www.boost.org/libs/multi_index/">
  Multi-index</a> from <code>boost/multi_index</code></li>

  <li><a href="https://www.boost.org/libs/heap/">
  Heap</a> from <code>boost/heap</code></li>

  <li>The flat containers from
  <a href="https://www.boost.org/libs/container/">Container</a>:
  <code>boost/container/flat_map</code>, and
  <code>boost/container/flat_set</code></li>

  <li><a href="https://www.boost.org/libs/intrusive/">Intrusive</a>
  from <code>boost/intrusive</code>.</li>

  <li><a href="https://www.boost.org/libs/sort/">The
  <code>boost/sort</code> library</a>.</li>

  <li><a href="https://www.boost.org/libs/preprocessor/">Preprocessor</a>
  from <code>boost/preprocessor</code>.</li>
</ul>

<p>We are actively considering adding other Boost
features to the list, so this list may be expanded in
the future.</p>
</div>

<h3 id="std_hash">std::hash</h3>

<p>Do not define specializations of <code>std::hash</code>.</p>

<p class="definition"></p>
<p><code>std::hash&lt;T&gt;</code> is the function object that the
C++11 hash containers use to hash keys of type <code>T</code>,
unless the user explicitly specifies a different hash function. For
example, <code>std::unordered_map&lt;int, std::string&gt;</code> is a hash
map that uses <code>std::hash&lt;int&gt;</code> to hash its keys,
whereas <code>std::unordered_map&lt;int, std::string, MyIntHash&gt;</code>
uses <code>MyIntHash</code>.</p>

<p><code>std::hash</code> is defined for all integral, floating-point,
pointer, and <code>enum</code> types, as well as some standard library
types such as <code>string</code> and <code>unique_ptr</code>. Users
can enable it to work for their own types by defining specializations
of it for those types.</p>

<p class="pros"></p>
<p><code>std::hash</code> is easy to use, and simplifies the code
since you don't have to name it explicitly. Specializing
<code>std::hash</code> is the standard way of specifying how to
hash a type, so it's what outside resources will teach, and what
new engineers will expect.</p>

<p class="cons"></p>
<p><code>std::hash</code> is hard to specialize. It requires a lot
of boilerplate code, and more importantly, it combines responsibility
for identifying the hash inputs with responsibility for executing the
hashing algorithm itself. The type author has to be responsible for
the former, but the latter requires expertise that a type author
usually doesn't have, and shouldn't need. The stakes here are high
because low-quality hash functions can be security vulnerabilities,
due to the emergence of
<a href="https://emboss.github.io/blog/2012/12/14/breaking-murmur-hash-flooding-dos-reloaded/">
hash flooding attacks</a>.</p>

<p>Even for experts, <code>std::hash</code> specializations are
inordinately difficult to implement correctly for compound types,
because the implementation cannot recursively call <code>std::hash</code>
on data members. High-quality hash algorithms maintain large
amounts of internal state, and reducing that state to the
<code>size_t</code> bytes that <code>std::hash</code>
returns is usually the slowest part of the computation, so it
should not be done more than once.</p>

<p>Due to exactly that issue, <code>std::hash</code> does not work
with <code>std::pair</code> or <code>std::tuple</code>, and the
language does not allow us to extend it to support them.</p>

<p class="decision"></p>
<p>You can use <code>std::hash</code> with the types that it supports
"out of the box", but do not specialize it to support additional types.
If you need a hash table with a key type that <code>std::hash</code>
does not support, consider using legacy hash containers (e.g.,
<code>hash_map</code>) for now; they use a different default hasher,
which is unaffected by this prohibition.</p>

<p>If you want to use the standard hash containers anyway, you will
need to specify a custom hasher for the key type, e.g.,</p>
<pre>std::unordered_map&lt;MyKeyType, Value, MyKeyTypeHasher&gt; my_map;
</pre><p>
Consult with the type's owners to see if there is an existing hasher
that you can use; otherwise work with them to provide one,
 or roll your own.</p>

<p>We are planning to provide a hash function that can work with any type,
using a new customization mechanism that doesn't have the drawbacks of
<code>std::hash</code>.</p>



<h3 id="Other_Features"><a id="C++11">Other C++ Features</a></h3>


<p>As with <a href="#Boost">Boost</a>, some modern C++
extensions encourage coding practices that hamper
readability—for example by removing
checked redundancy (such as type names) that may be
helpful to readers, or by encouraging template
metaprogramming. Other extensions duplicate functionality
available through existing mechanisms, which may lead to confusion
and conversion costs.</p>

<p class="decision"></p>
<p>In addition to what's described in the rest of the style
guide, the following C++ features may not be used:</p>

<ul>


  <li>Compile-time rational numbers
  (<code>&lt;ratio&gt;</code>), because of concerns that
  it's tied to a more template-heavy interface
  style.</li>

  <li>The <code>&lt;cfenv&gt;</code> and
  <code>&lt;fenv.h&gt;</code> headers, because many
  compilers do not support those features reliably.</li>

  <li>The <code>&lt;filesystem&gt;</code> header, which

  does not have sufficient support for testing, and suffers
  from inherent security vulnerabilities.</li>


</ul>

<h3 id="Nonstandard_Extensions">Nonstandard Extensions</h3>

<p>Nonstandard extensions to C++ may not be used unless otherwise specified.</p>

<p class="definition"></p>
<p>Compilers support various extensions that are not part of standard C++. Such
  extensions include GCC's <code>__attribute__</code>, intrinsic functions such
  as <code>__builtin_prefetch</code>, inline assembly, <code>__COUNTER__</code>,
  <code>__PRETTY_FUNCTION__</code>, compound statement expressions (e.g.,
  <code>foo = ({ int x; Bar(&amp;x); x })</code>, variable-length arrays and
  <code>alloca()</code>, and the "<a href="https://en.wikipedia.org/wiki/Elvis_operator">Elvis Operator</a>"
  <code>a?:b</code>.</p>

<p class="pros"></p>
  <ul>
    <li>Nonstandard extensions may provide useful features that do not exist
      in standard C++.</li>
    <li>Important performance guidance to the compiler can only be specified
      using extensions.</li>
  </ul>

<p class="cons"></p>
  <ul>
    <li>Nonstandard extensions do not work in all compilers. Use of nonstandard
      extensions reduces portability of code.</li>
    <li>Even if they are supported in all targeted compilers, the extensions
      are often not well-specified, and there may be subtle behavior differences
      between compilers.</li>
    <li>Nonstandard extensions add to the language features that a reader must
      know to understand the code.</li>
  </ul>

<p class="decision"></p>
<p>Do not use nonstandard extensions. You may use portability wrappers that
  are implemented using nonstandard extensions, so long as those wrappers

  are provided by a designated project-wide
  portability header.</p>

<h3 id="Aliases">Aliases</h3>

<p>Public aliases are for the benefit of an API's user, and should be clearly documented.</p>

<p class="definition"></p>
<p>There are several ways to create names that are aliases of other entities:</p>
<pre>typedef Foo Bar;
using Bar = Foo;
using other_namespace::Foo;
</pre>

  <p>In new code, <code>using</code> is preferable to <code>typedef</code>,
  because it provides a more consistent syntax with the rest of C++ and works
  with templates.</p>

  <p>Like other declarations, aliases declared in a header file are part of that
  header's public API unless they're in a function definition, in the private portion of a class,
  or in an explicitly-marked internal namespace. Aliases in such areas or in .cc files are
  implementation details (because client code can't refer to them), and are not restricted by this
  rule.</p>

<p class="pros"></p>
  <ul>
    <li>Aliases can improve readability by simplifying a long or complicated name.</li>
    <li>Aliases can reduce duplication by naming in one place a type used repeatedly in an API,
      which <em>might</em> make it easier to change the type later.
    </li>
  </ul>

<p class="cons"></p>
  <ul>
    <li>When placed in a header where client code can refer to them, aliases increase the
      number of entities in that header's API, increasing its complexity.</li>
    <li>Clients can easily rely on unintended details of public aliases, making
      changes difficult.</li>
    <li>It can be tempting to create a public alias that is only intended for use
      in the implementation, without considering its impact on the API, or on maintainability.</li>
    <li>Aliases can create risk of name collisions</li>
    <li>Aliases can reduce readability by giving a familiar construct an unfamiliar name</li>
    <li>Type aliases can create an unclear API contract:
      it is unclear whether the alias is guaranteed to be identical to the type it aliases,
      to have the same API, or only to be usable in specified narrow ways</li>
  </ul>

<p class="decision"></p>
<p>Don't put an alias in your public API just to save typing in the implementation;
  do so only if you intend it to be used by your clients.</p>
<p>When defining a public alias, document the intent of
the new name, including whether it is guaranteed to always be the same as the type
it's currently aliased to, or whether a more limited compatibility is
intended. This lets the user know whether they can treat the types as
substitutable or whether more specific rules must be followed, and can help the
implementation retain some degree of freedom to change the alias.</p>
<p>Don't put namespace aliases in your public API. (See also <a href="#Namespaces">Namespaces</a>).
</p>

<p>For example, these aliases document how they are intended to be used in client code:</p>
<pre>namespace mynamespace {
// Used to store field measurements. DataPoint may change from Bar* to some internal type.
// Client code should treat it as an opaque pointer.
using DataPoint = ::foo::Bar*;

// A set of measurements. Just an alias for user convenience.
using TimeSeries = std::unordered_set&lt;DataPoint, std::hash&lt;DataPoint&gt;, DataPointComparator&gt;;
}  // namespace mynamespace
</pre>

<p>These aliases don't document intended use, and half of them aren't meant for client use:</p>

<pre class="badcode">namespace mynamespace {
// Bad: none of these say how they should be used.
using DataPoint = ::foo::Bar*;
using ::std::unordered_set;  // Bad: just for local convenience
using ::std::hash;           // Bad: just for local convenience
typedef unordered_set&lt;DataPoint, hash&lt;DataPoint&gt;, DataPointComparator&gt; TimeSeries;
}  // namespace mynamespace
</pre>

<p>However, local convenience aliases are fine in function definitions, private sections of
  classes, explicitly marked internal namespaces, and in .cc files:</p>

<pre>// In a .cc file
using ::foo::Bar;
</pre>

<h2 id="Inclusive_Language">Inclusive Language</h2>

<p>In all code, including naming and comments, use inclusive language
and avoid terms that other programmers might find disrespectful or offensive
(such as "master" and "slave", "blacklist" and "whitelist", or "redline"),
even if the terms also have an ostensibly neutral meaning.
Similarly, use gender-neutral language unless you're referring
to a specific person (and using their pronouns). For example,
use "they"/"them"/"their" for people of unspecified gender
(<a href="https://apastyle.apa.org/style-grammar-guidelines/grammar/singular-they">even
when singular</a>), and "it"/"its" for software, computers, and other
things that aren't people.</p>



<h2 id="Naming">Naming</h2>

<p>The most important consistency rules are those that govern
naming. The style of a name immediately informs us what sort of
thing the named entity is: a type, a variable, a function, a
constant, a macro, etc., without requiring us to search for the
declaration of that entity. The pattern-matching engine in our
brains relies a great deal on these naming rules.
</p>

<p>Naming rules are pretty arbitrary, but
 we feel that
consistency is more important than individual preferences in this
area, so regardless of whether you find them sensible or not,
the rules are the rules.</p>

<h3 id="General_Naming_Rules">General Naming Rules</h3>

<p>Optimize for readability using names that would be clear
even to people on a different team.</p>

<p>Use names that describe the purpose or intent of the object.
Do not worry about saving horizontal space as it is far
more important to make your code immediately
understandable by a new reader. Minimize the use of
abbreviations that would likely be unknown to someone outside
your project (especially acronyms and initialisms). Do not
abbreviate by deleting letters within a word. As a rule of thumb,
an abbreviation is probably OK if it's listed in
 Wikipedia. Generally speaking, descriptiveness should be
proportional to the name's scope of visibility. For example,
<code>n</code> may be a fine name within a 5-line function,
but within the scope of a class, it's likely too vague.</p>

<pre>class MyClass {
 public:
  int CountFooErrors(const std::vector&lt;Foo&gt;&amp; foos) {
    int n = 0;  // Clear meaning given limited scope and context
    for (const auto&amp; foo : foos) {
      ...
      ++n;
    }
    return n;
  }
  void DoSomethingImportant() {
    std::string fqdn = ...;  // Well-known abbreviation for Fully Qualified Domain Name
  }
 private:
  const int kMaxAllowedConnections = ...;  // Clear meaning within context
};
</pre>

<pre class="badcode">class MyClass {
 public:
  int CountFooErrors(const std::vector&lt;Foo&gt;&amp; foos) {
    int total_number_of_foo_errors = 0;  // Overly verbose given limited scope and context
    for (int foo_index = 0; foo_index &lt; foos.size(); ++foo_index) {  // Use idiomatic `i`
      ...
      ++total_number_of_foo_errors;
    }
    return total_number_of_foo_errors;
  }
  void DoSomethingImportant() {
    int cstmr_id = ...;  // Deletes internal letters
  }
 private:
  const int kNum = ...;  // Unclear meaning within broad scope
};
</pre>

<p>Note that certain universally-known abbreviations are OK, such as
<code>i</code> for an iteration variable and <code>T</code> for a
template parameter.</p>

<p>For the purposes of the naming rules below, a "word" is anything that you
would write in English without internal spaces. This includes abbreviations,
such as acronyms and initialisms. For names written in mixed case (also
sometimes referred to as
"<a href="https://en.wikipedia.org/wiki/Camel_case">camel case</a>" or
"<a href="https://en.wiktionary.org/wiki/Pascal_case">Pascal case</a>"), in
which the first letter of each word is capitalized, prefer to capitalize
abbreviations as single words, e.g., <code>StartRpc()</code> rather than
<code>StartRPC()</code>.</p>

<p>Template parameters should follow the naming style for their
category: type template parameters should follow the rules for
<a href="#Type_Names">type names</a>, and non-type template
parameters should follow the rules for <a href="#Variable_Names">
variable names</a>.



<div>
<p>To help you format code correctly, we've created a
<a href="https://raw.githubusercontent.com/google/styleguide/gh-pages/google-c-style.el">
settings file for emacs</a>.</p>
</div>

<h3 id="Line_Length">Line Length</h3>

<p>Each line of text in your code should be at most 80
characters long.</p>



<div>
<p>We recognize that this rule is
controversial, but so much existing code already adheres
to it, and we feel that consistency is important.</p>
</div>

<p class="pros"></p>
<p>Those who favor  this rule
argue that it is rude to force them to resize
their windows and there is no need for anything longer.
Some folks are used to having several code windows
side-by-side, and thus don't have room to widen their
windows in any case. People set up their work environment
assuming a particular maximum window width, and 80
columns has been the traditional standard. Why change
it?</p>

<p class="cons"></p>
<p>Proponents of change argue that a wider line can make
code more readable. The 80-column limit is an hidebound
throwback to 1960s mainframes;  modern equipment has wide screens that
can easily show longer lines.</p>

<p class="decision"></p>
<p> 80 characters is the maximum.</p>

<p>A line may exceed 80 characters if it is</p>

<ul>
  <li>a comment line which is not feasible to split without harming
  readability, ease of cut and paste or auto-linking -- e.g., if a line
  contains an example command or a literal URL longer than 80 characters.</li>

  <li>a raw-string literal with content that exceeds 80 characters.  Except for
  test code, such literals should appear near the top of a file.</li>

  <li>an include statement.</li>

  <li>a <a href="#The__define_Guard">header guard</a></li>

  <li>a using-declaration</li>
</ul>

<h3 id="Non-ASCII_Characters">Non-ASCII Characters</h3>

<p>Non-ASCII characters should be rare, and must use UTF-8
formatting.</p>

<p>You shouldn't hard-code user-facing text in source,
even English, so use of non-ASCII characters should be
rare. However, in certain cases it is appropriate to
include such words in your code. For example, if your
code parses data files from foreign sources, it may be
appropriate to hard-code the non-ASCII string(s) used in
those data files as delimiters. More commonly, unittest
code (which does not  need to be localized) might
contain non-ASCII strings. In such cases, you should use
UTF-8, since that is  an encoding
understood by most tools able to handle more than just
ASCII.</p>

<p>Hex encoding is also OK, and encouraged where it
enhances readability — for example,
<code>"\xEF\xBB\xBF"</code>, or, even more simply,
<code>u8"\uFEFF"</code>, is the Unicode zero-width
no-break space character, which would be invisible if
included in the source as straight UTF-8.</p>

<p>Use the <code>u8</code> prefix
to guarantee that a string literal containing
<code>\uXXXX</code> escape sequences is encoded as UTF-8.
Do not use it for strings containing non-ASCII characters
encoded as UTF-8, because that will produce incorrect
output if the compiler does not interpret the source file
as UTF-8. </p>

<p>You shouldn't use the C++11 <code>char16_t</code> and
<code>char32_t</code> character types, since they're for
non-UTF-8 text. For similar reasons you also shouldn't
use <code>wchar_t</code> (unless you're writing code that
interacts with the Windows API, which uses
<code>wchar_t</code> extensively).</p>

<h3 id="Spaces_vs._Tabs">Spaces vs. Tabs</h3>

<p>Use only spaces, and indent 2 spaces at a time.</p>

<p>We use spaces for indentation. Do not use tabs in your
code. You should set your editor to emit spaces when you
hit the tab key.</p>

<h3 id="Function_Declarations_and_Definitions">Function Declarations and Definitions</h3>

<p>Return type on the same line as function name, parameters
on the same line if they fit. Wrap parameter lists which do
not fit on a single line as you would wrap arguments in a
<a href="#Function_Calls">function call</a>.</p>

<p>Functions look like this:</p>


<pre>ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) {
  DoSomething();
  ...
}
</pre>

<p>If you have too much text to fit on one line:</p>

<pre>ReturnType ClassName::ReallyLongFunctionName(Type par_name1, Type par_name2,
                                             Type par_name3) {
  DoSomething();
  ...
}
</pre>

<p>or if you cannot fit even the first parameter:</p>

<pre>ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
    Type par_name1,  // 4 space indent
    Type par_name2,
    Type par_name3) {
  DoSomething();  // 2 space indent
  ...
}
</pre>

<p>Some points to note:</p>

<ul>
  <li>Choose good parameter names.</li>

  <li>A parameter name may be omitted only if the parameter is not used in the
  function's definition.</li>

  <li>If you cannot fit the return type and the function
  name on a single line, break between them.</li>

  <li>If you break after the return type of a function
  declaration or definition, do not indent.</li>

  <li>The open parenthesis is always on the same line as
  the function name.</li>

  <li>There is never a space between the function name
  and the open parenthesis.</li>

  <li>There is never a space between the parentheses and
  the parameters.</li>

  <li>The open curly brace is always on the end of the last line of the function
  declaration, not the start of the next line.</li>

  <li>The close curly brace is either on the last line by
  itself or on the same line as the open curly brace.</li>

  <li>There should be a space between the close
  parenthesis and the open curly brace.</li>

  <li>All parameters should be aligned if possible.</li>

  <li>Default indentation is 2 spaces.</li>

  <li>Wrapped parameters have a 4 space indent.</li>
</ul>

<p>Unused parameters that are obvious from context may be omitted:</p>

<pre>class Foo {
 public:
  Foo(const Foo&amp;) = delete;
  Foo&amp; operator=(const Foo&amp;) = delete;
};
</pre>

<p>Unused parameters that might not be obvious should comment out the variable
name in the function definition:</p>

<pre>class Shape {
 public:
  virtual void Rotate(double radians) = 0;
};

class Circle : public Shape {
 public:
  void Rotate(double radians) override;
};

void Circle::Rotate(double /*radians*/) {}
</pre>

<pre class="badcode">// Bad - if someone wants to implement later, it's not clear what the
// variable means.
void Circle::Rotate(double) {}
</pre>

<p>Attributes, and macros that expand to attributes, appear at the very
beginning of the function declaration or definition, before the
return type:</p>
<pre>ABSL_MUST_USE_RESULT bool IsOk();
</pre>

<h3 id="Formatting_Lambda_Expressions">Lambda Expressions</h3>

<p>Format parameters and bodies as for any other function, and capture
lists like other comma-separated lists.</p>

<p>For by-reference captures, do not leave a space between the
ampersand (&amp;) and the variable name.</p>
<pre>int x = 0;
auto x_plus_n = [&amp;x](int n) -&gt; int { return x + n; }
</pre>
<p>Short lambdas may be written inline as function arguments.</p>
<pre>std::set&lt;int&gt; to_remove = {7, 8, 9};
std::vector&lt;int&gt; digits = {3, 9, 1, 8, 4, 7, 1};
digits.erase(std::remove_if(digits.begin(), digits.end(), [&amp;to_remove](int i) {
               return to_remove.find(i) != to_remove.end();
             }),
             digits.end());
</pre>

<h3 id="Floating_Literals">Floating-point Literals</h3>

<p>Floating-point literals should always have a radix point, with digits on both
sides, even if they use exponential notation. Readability is improved if all
floating-point literals take this familiar form, as this helps ensure that they
are not mistaken for integer literals, and that the
<code>E</code>/<code>e</code> of the exponential notation is not mistaken for a
hexadecimal digit. It is fine to initialize a floating-point variable with an
integer literal (assuming the variable type can exactly represent that integer),
but note that a number in exponential notation is never an integer literal.
</p>

<pre class="badcode">float f = 1.f;
long double ld = -.5L;
double d = 1248e6;
</pre>

<pre class="goodcode">float f = 1.0f;
float f2 = 1;   // Also OK
long double ld = -0.5L;
double d = 1248.0e6;
</pre>


<h3 id="Function_Calls">Function Calls</h3>

<p>Either write the call all on a single line, wrap the
arguments at the parenthesis, or start the arguments on a new
line indented by four spaces and continue at that 4 space
indent. In the absence of other considerations, use the
minimum number of lines, including placing multiple arguments
on each line where appropriate.</p>

<p>Function calls have the following format:</p>
<pre>bool result = DoSomething(argument1, argument2, argument3);
</pre>

<p>If the arguments do not all fit on one line, they
should be broken up onto multiple lines, with each
subsequent line aligned with the first argument. Do not
add spaces after the open paren or before the close
paren:</p>
<pre>bool result = DoSomething(averyveryveryverylongargument1,
                          argument2, argument3);
</pre>

<p>Arguments may optionally all be placed on subsequent
lines with a four space indent:</p>
<pre>if (...) {
  ...
  ...
  if (...) {
    bool result = DoSomething(
        argument1, argument2,  // 4 space indent
        argument3, argument4);
    ...
  }
</pre>

<p>Put multiple arguments on a single line to reduce the
number of lines necessary for calling a function unless
there is a specific readability problem. Some find that
formatting with strictly one argument on each line is
more readable and simplifies editing of the arguments.
However, we prioritize for the reader over the ease of
editing arguments, and most readability problems are
better addressed with the following techniques.</p>

<p>If having multiple arguments in a single line decreases
readability due to the complexity or confusing nature of the
expressions that make up some arguments, try creating
variables that capture those arguments in a descriptive name:</p>
<pre>int my_heuristic = scores[x] * y + bases[x];
bool result = DoSomething(my_heuristic, x, y, z);
</pre>

<p>Or put the confusing argument on its own line with
an explanatory comment:</p>
<pre>bool result = DoSomething(scores[x] * y + bases[x],  // Score heuristic.
                          x, y, z);
</pre>

<p>If there is still a case where one argument is
significantly more readable on its own line, then put it on
its own line. The decision should be specific to the argument
which is made more readable rather than a general policy.</p>

<p>Sometimes arguments form a structure that is important
for readability. In those cases, feel free to format the
arguments according to that structure:</p>
<pre>// Transform the widget by a 3x3 matrix.
my_widget.Transform(x1, x2, x3,
                    y1, y2, y3,
                    z1, z2, z3);
</pre>

<h3 id="Braced_Initializer_List_Format">Braced Initializer List Format</h3>

<p>Format a braced initializer list exactly like you would format a function
call in its place.</p>

<p>If the braced list follows a name (e.g., a type or
variable name), format as if the <code>{}</code> were the
parentheses of a function call with that name. If there
is no name, assume a zero-length name.</p>

<pre>// Examples of braced init list on a single line.
return {foo, bar};
functioncall({foo, bar});
std::pair&lt;int, int&gt; p{foo, bar};

// When you have to wrap.
SomeFunction(
    {"assume a zero-length name before {"},
    some_other_function_parameter);
SomeType variable{
    some, other, values,
    {"assume a zero-length name before {"},
    SomeOtherType{
        "Very long string requiring the surrounding breaks.",
        some, other, values},
    SomeOtherType{"Slightly shorter string",
                  some, other, values}};
SomeType variable{
    "This is too long to fit all in one line"};
MyType m = {  // Here, you could also break before {.
    superlongvariablename1,
    superlongvariablename2,
    {short, interior, list},
    {interiorwrappinglist,
     interiorwrappinglist2}};
</pre>

<h3 id="Conditionals">Conditionals</h3>

<p>In an <code>if</code> statement, including its optional <code>else if</code>
and <code>else</code> clauses, put one space between the <code>if</code> and the
opening parenthesis, and between the closing parenthesis and the curly brace (if
any), but no spaces between the parentheses and the condition or initializer. If
the optional initializer is present, put a space or newline after the semicolon,
but not before.</p>

<pre class="badcode">if(condition) {              // Bad - space missing after IF
if ( condition ) {           // Bad - space between the parentheses and the condition
if (condition){              // Bad - space missing before {
if(condition){               // Doubly bad

if (int a = f();a == 10) {   // Bad - space missing after the semicolon
</pre>

<p>Use curly braces for the controlled statements following
<code>if</code>, <code>else if</code> and <code>else</code>. Break the line
immediately after the opening brace, and immediately before the closing brace. A
subsequent <code>else</code>, if any, appears on the same line as the preceding
closing brace, separated by a space.</p>

<pre>if (condition) {                   // no spaces inside parentheses, space before brace
  DoOneThing();                    // two space indent
  DoAnotherThing();
} else if (int a = f(); a != 3) {  // closing brace on new line, else on same line
  DoAThirdThing(a);
} else {
  DoNothing();
}
</pre>

<p>For historical reasons, we allow one exception to the above rules: if an
<code>if</code> statement has no <code>else</code> or <code>else if</code>
clauses, then the curly braces for the controlled statement or the line breaks
inside the curly braces may be omitted if as a result the entire <code>if</code>
statement appears on either a single line (in which case there is a space
between the closing parenthesis and the controlled statement) or on two lines
(in which case there is a line break after the closing parenthesis and there are
no braces). For example, the following forms are allowed under this
exception.</p>

<pre>if (x == kFoo) return new Foo();

if (x == kBar)
  return new Bar(arg1, arg2, arg3);

if (x == kQuz) { return new Quz(1, 2, 3); }
</pre>

<p>Use this style only when the statement is brief, and consider that
conditional statements with complex conditions or controlled statements may be
more readable with curly braces. Some
projects
require curly braces always.</p>

<p>Finally, these are not permitted:</p>

<pre class="badcode">// Bad - IF statement with ELSE is missing braces
if (x) DoThis();
else DoThat();

// Bad - IF statement with ELSE does not have braces everywhere
if (condition)
  foo;
else {
  bar;
}

// Bad - IF statement is too long to omit braces
if (condition)
  // Comment
  DoSomething();

// Bad - IF statement is too long to omit braces
if (condition1 &amp;&amp;
    condition2)
  DoSomething();
</pre>

<h3 id="Loops_and_Switch_Statements">Loops and Switch Statements</h3>

<p>Switch statements may use braces for blocks. Annotate
non-trivial fall-through between cases.
Braces are optional for single-statement loops.
Empty loop bodies should use either empty braces or <code>continue</code>.</p>

<p><code>case</code> blocks in <code>switch</code>
statements can have curly braces or not, depending on
your preference. If you do include curly braces they
should be placed as shown below.</p>

<p>If not conditional on an enumerated value, switch
statements should always have a <code>default</code> case
(in the case of an enumerated value, the compiler will
warn you if any values are not handled). If the default
case should never execute, treat this as an error. For example:

</p>

<div>
<pre>switch (var) {
  case 0: {  // 2 space indent
    ...      // 4 space indent
    break;
  }
  case 1: {
    ...
    break;
  }
  default: {
    assert(false);
  }
}
</pre>
</div>

<p>Fall-through from one case label to
another must be annotated using the
<code>ABSL_FALLTHROUGH_INTENDED;</code> macro (defined in

<code>absl/base/macros.h</code>).
<code>ABSL_FALLTHROUGH_INTENDED;</code> should be placed at a
point of execution where a fall-through to the next case
label occurs. A common exception is consecutive case
labels without intervening code, in which case no
annotation is needed.</p>

<pre>switch (x) {
  case 41:  // No annotation needed here.
  case 43:
    if (dont_be_picky) {
      // Use this instead of or along with annotations in comments.
      ABSL_FALLTHROUGH_INTENDED;
    } else {
      CloseButNoCigar();
      break;
    }
  case 42:
    DoSomethingSpecial();
    ABSL_FALLTHROUGH_INTENDED;
  default:
    DoSomethingGeneric();
    break;
}
</pre>

<p> Braces are optional for single-statement loops.</p>

<pre>for (int i = 0; i &lt; kSomeNumber; ++i)
  printf("I love you\n");

for (int i = 0; i &lt; kSomeNumber; ++i) {
  printf("I take it back\n");
}
</pre>


<p>Empty loop bodies should use either an empty pair of braces or
<code>continue</code> with no braces, rather than a single semicolon.</p>

<pre>while (condition) {
  // Repeat test until it returns false.
}
for (int i = 0; i &lt; kSomeNumber; ++i) {}  // Good - one newline is also OK.
while (condition) continue;  // Good - continue indicates no logic.
</pre>

<pre class="badcode">while (condition);  // Bad - looks like part of do/while loop.
</pre>

<h3 id="Pointer_and_Reference_Expressions">Pointer and Reference Expressions</h3>

<p>No spaces around period or arrow. Pointer operators do not
have trailing spaces.</p>

<p>The following are examples of correctly-formatted
pointer and reference expressions:</p>

<pre>x = *p;
p = &amp;x;
x = r.y;
x = r-&gt;y;
</pre>

<p>Note that:</p>

<ul>
  <li>There are no spaces around the period or arrow when
  accessing a member.</li>

   <li>Pointer operators have no space after the
   <code>*</code> or <code>&amp;</code>.</li>
</ul>

<p>When referring to a pointer or reference (variable declarations or definitions, arguments,
return types, template parameters, etc), you may place the space before or after the
asterisk/ampersand. In the trailing-space style, the space is elided in some cases (template
parameters, etc).</p>

<pre>// These are fine, space preceding.
char *c;
const std::string &amp;str;
int *GetPointer();
std::vector&lt;char *&gt;

// These are fine, space following (or elided).
char* c;
const std::string&amp; str;
int* GetPointer();
std::vector&lt;char*&gt;  // Note no space between '*' and '&gt;'
</pre>

<p>You should do this consistently within a single
file.
When modifying an existing file, use the style in
that file.</p>

It is allowed (if unusual) to declare multiple variables in the same
declaration, but it is disallowed if any of those have pointer or
reference decorations. Such declarations are easily misread.
<pre>// Fine if helpful for readability.
int x, y;
</pre>
<pre class="badcode">int x, *y;  // Disallowed - no &amp; or * in multiple declaration
int* x, *y;  // Disallowed - no &amp; or * in multiple declaration; inconsistent spacing
char * c;  // Bad - spaces on both sides of *
const std::string &amp; str;  // Bad - spaces on both sides of &amp;
</pre>

<h3 id="Boolean_Expressions">Boolean Expressions</h3>

<p>When you have a boolean expression that is longer than the
<a href="#Line_Length">standard line length</a>, be
consistent in how you break up the lines.</p>

<p>In this example, the logical AND operator is always at
the end of the lines:</p>

<pre>if (this_one_thing &gt; this_other_thing &amp;&amp;
    a_third_thing == a_fourth_thing &amp;&amp;
    yet_another &amp;&amp; last_one) {
  ...
}
</pre>

<p>Note that when the code wraps in this example, both of
the <code>&amp;&amp;</code> logical AND operators are at
the end of the line. This is more common in Google code,
though wrapping all operators at the beginning of the
line is also allowed. Feel free to insert extra
parentheses judiciously because they can be very helpful
in increasing readability when used
appropriately, but be careful about overuse. Also note that you
should always use the punctuation operators, such as
<code>&amp;&amp;</code> and <code>~</code>, rather than
the word operators, such as <code>and</code> and
<code>compl</code>.</p>

<h3 id="Return_Values">Return Values</h3>

<p>Do not needlessly surround the <code>return</code>
expression with parentheses.</p>

<p>Use parentheses in <code>return expr;</code> only
where you would use them in <code>x = expr;</code>.</p>

<pre>return result;                  // No parentheses in the simple case.
// Parentheses OK to make a complex expression more readable.
return (some_long_condition &amp;&amp;
        another_condition);
</pre>

<pre class="badcode">return (value);                // You wouldn't write var = (value);
return(result);                // return is not a function!
</pre>



<h3 id="Variable_and_Array_Initialization">Variable and Array Initialization</h3>

<p>You may choose between <code>=</code>,
<code>()</code>, and <code>{}</code>; the following are
all correct:</p>

<pre>int x = 3;
int x(3);
int x{3};
std::string name = "Some Name";
std::string name("Some Name");
std::string name{"Some Name"};
</pre>

<p>Be careful when using a braced initialization list <code>{...}</code>
on a type with an <code>std::initializer_list</code> constructor.
A nonempty <i>braced-init-list</i> prefers the
<code>std::initializer_list</code> constructor whenever
possible. Note that empty braces <code>{}</code> are special, and
will call a default constructor if available. To force the
non-<code>std::initializer_list</code> constructor, use parentheses
instead of braces.</p>

<pre>std::vector&lt;int&gt; v(100, 1);  // A vector containing 100 items: All 1s.
std::vector&lt;int&gt; v{100, 1};  // A vector containing 2 items: 100 and 1.
</pre>

<p>Also, the brace form prevents narrowing of integral
types. This can prevent some types of programming
errors.</p>

<pre>int pi(3.14);  // OK -- pi == 3.
int pi{3.14};  // Compile error: narrowing conversion.
</pre>

<h3 id="Preprocessor_Directives">Preprocessor Directives</h3>

<p>The hash mark that starts a preprocessor directive should
always be at the beginning of the line.</p>

<p>Even when preprocessor directives are within the body
of indented code, the directives should start at the
beginning of the line.</p>

<pre>// Good - directives at beginning of line
  if (lopsided_score) {
#if DISASTER_PENDING      // Correct -- Starts at beginning of line
    DropEverything();
# if NOTIFY               // OK but not required -- Spaces after #
    NotifyClient();
# endif
#endif
    BackToNormal();
  }
</pre>

<pre class="badcode">// Bad - indented directives
  if (lopsided_score) {
    #if DISASTER_PENDING  // Wrong!  The "#if" should be at beginning of line
    DropEverything();
    #endif                // Wrong!  Do not indent "#endif"
    BackToNormal();
  }
</pre>

<h3 id="Class_Format">Class Format</h3>

<p>Sections in <code>public</code>, <code>protected</code> and
<code>private</code> order, each indented one space.</p>

<p>The basic format for a class definition (lacking the
comments, see <a href="#Class_Comments">Class
Comments</a> for a discussion of what comments are
needed) is:</p>

<pre>class MyClass : public OtherClass {
 public:      // Note the 1 space indent!
  MyClass();  // Regular 2 space indent.
  explicit MyClass(int var);
  ~MyClass() {}

  void SomeFunction();
  void SomeFunctionThatDoesNothing() {
  }

  void set_some_var(int var) { some_var_ = var; }
  int some_var() const { return some_var_; }

 private:
  bool SomeInternalFunction();

  int some_var_;
  int some_other_var_;
};
</pre>

<p>Things to note:</p>

<ul>
  <li>Any base class name should be on the same line as
  the subclass name, subject to the 80-column limit.</li>

  <li>The <code>public:</code>, <code>protected:</code>,
  and <code>private:</code> keywords should be indented
  one space.</li>

  <li>Except for the first instance, these keywords
  should be preceded by a blank line. This rule is
  optional in small classes.</li>

  <li>Do not leave a blank line after these
  keywords.</li>

  <li>The <code>public</code> section should be first,
  followed by the <code>protected</code> and finally the
  <code>private</code> section.</li>

  <li>See <a href="#Declaration_Order">Declaration
  Order</a> for rules on ordering declarations within
  each of these sections.</li>
</ul>

<h3 id="Constructor_Initializer_Lists">Constructor Initializer Lists</h3>

<p>Constructor initializer lists can be all on one line or
with subsequent lines indented four spaces.</p>

<p>The acceptable formats for initializer lists are:</p>

<pre>// When everything fits on one line:
MyClass::MyClass(int var) : some_var_(var) {
  DoSomething();
}

// If the signature and initializer list are not all on one line,
// you must wrap before the colon and indent 4 spaces:
MyClass::MyClass(int var)
    : some_var_(var), some_other_var_(var + 1) {
  DoSomething();
}

// When the list spans multiple lines, put each member on its own line
// and align them:
MyClass::MyClass(int var)
    : some_var_(var),             // 4 space indent
      some_other_var_(var + 1) {  // lined up
  DoSomething();
}

// As with any other code block, the close curly can be on the same
// line as the open curly, if it fits.
MyClass::MyClass(int var)
    : some_var_(var) {}
</pre>

<h3 id="Namespace_Formatting">Namespace Formatting</h3>

<p>The contents of namespaces are not indented.</p>

<p><a href="#Namespaces">Namespaces</a> do not add an
extra level of indentation. For example, use:</p>

<pre>namespace {

void foo() {  // Correct.  No extra indentation within namespace.
  ...
}

}  // namespace
</pre>

<p>Do not indent within a namespace:</p>

<pre class="badcode">namespace {

  // Wrong!  Indented when it should not be.
  void foo() {
    ...
  }

}  // namespace
</pre>

<h3 id="Horizontal_Whitespace">Horizontal Whitespace</h3>

<p>Use of horizontal whitespace depends on location. Never put
trailing whitespace at the end of a line.</p>

<h4>General</h4>

<pre>void f(bool b) {  // Open braces should always have a space before them.
  ...
int i = 0;  // Semicolons usually have no space before them.
// Spaces inside braces for braced-init-list are optional.  If you use them,
// put them on both sides!
int x[] = { 0 };
int x[] = {0};

// Spaces around the colon in inheritance and initializer lists.
class Foo : public Bar {
 public:
  // For inline function implementations, put spaces between the braces
  // and the implementation itself.
  Foo(int b) : Bar(), baz_(b) {}  // No spaces inside empty braces.
  void Reset() { baz_ = 0; }  // Spaces separating braces from implementation.
  ...
</pre>

<p>Adding trailing whitespace can cause extra work for
others editing the same file, when they merge, as can
removing existing trailing whitespace. So: Don't
introduce trailing whitespace. Remove it if you're
already changing that line, or do it in a separate
clean-up
operation (preferably when no-one
else is working on the file).</p>

<h4>Loops and Conditionals</h4>

<pre>if (b) {          // Space after the keyword in conditions and loops.
} else {          // Spaces around else.
}
while (test) {}   // There is usually no space inside parentheses.
switch (i) {
for (int i = 0; i &lt; 5; ++i) {
// Loops and conditions may have spaces inside parentheses, but this
// is rare.  Be consistent.
switch ( i ) {
if ( test ) {
for ( int i = 0; i &lt; 5; ++i ) {
// For loops always have a space after the semicolon.  They may have a space
// before the semicolon, but this is rare.
for ( ; i &lt; 5 ; ++i) {
  ...

// Range-based for loops always have a space before and after the colon.
for (auto x : counts) {
  ...
}
switch (i) {
  case 1:         // No space before colon in a switch case.
    ...
  case 2: break;  // Use a space after a colon if there's code after it.
</pre>

<h4>Operators</h4>

<pre>// Assignment operators always have spaces around them.
x = 0;

// Other binary operators usually have spaces around them, but it's
// OK to remove spaces around factors.  Parentheses should have no
// internal padding.
v = w * x + y / z;
v = w*x + y/z;
v = w * (x + z);

// No spaces separating unary operators and their arguments.
x = -5;
++x;
if (x &amp;&amp; !y)
  ...
</pre>

<h4>Templates and Casts</h4>

<pre>// No spaces inside the angle brackets (&lt; and &gt;), before
// &lt;, or between &gt;( in a cast
std::vector&lt;std::string&gt; x;
y = static_cast&lt;char*&gt;(x);

// Spaces between type and pointer are OK, but be consistent.
std::vector&lt;char *&gt; x;
</pre>

<h3 id="Vertical_Whitespace">Vertical Whitespace</h3>

<p>Minimize use of vertical whitespace.</p>

<p>This is more a principle than a rule: don't use blank lines when
you don't have to. In particular, don't put more than one or two blank
lines between functions, resist starting functions with a blank line,
don't end functions with a blank line, and be sparing with your use of
blank lines. A blank line within a block of code serves like a
paragraph break in prose: visually separating two thoughts.</p>

<p>The basic principle is: The more code that fits on one screen, the
easier it is to follow and understand the control flow of the
program. Use whitespace purposefully to provide separation in that
flow.</p>

<p>Some rules of thumb to help when blank lines may be
useful:</p>

<ul>
  <li>Blank lines at the beginning or end of a function
  do not help readability.</li>

  <li>Blank lines inside a chain of if-else blocks may
  well help readability.</li>

  <li>A blank line before a comment line usually helps
  readability — the introduction of a new comment suggests
  the start of a new thought, and the blank line makes it clear
  that the comment goes with the following thing instead of the
  preceding.</li>

  <li>Blank lines immediately inside a declaration of a namespace or block of
  namespaces may help readability by visually separating the load-bearing
  content from the (largely non-semantic) organizational wrapper. Especially
  when the first declaration inside the namespace(s) is preceded by a comment,
  this becomes a special case of the previous rule, helping the comment to
  "attach" to the subsequent declaration.</li>
</ul>

<h2 id="Exceptions_to_the_Rules">Exceptions to the Rules</h2>

<p>The coding conventions described above are mandatory.
However, like all good rules, these sometimes have exceptions,
which we discuss here.</p>



<div>
<h3 id="Existing_Non-conformant_Code">Existing Non-conformant Code</h3>

<p>You may diverge from the rules when dealing with code that
does not conform to this style guide.</p>

<p>If you find yourself modifying code that was written
to specifications other than those presented by this
guide, you may have to diverge from these rules in order
to stay consistent with the local conventions in that
code. If you are in doubt about how to do this, ask the
original author or the person currently responsible for
the code. Remember that <em>consistency</em> includes
local consistency, too.</p>

</div>



<h3 id="Windows_Code">Windows Code</h3>

<p> Windows
programmers have developed their own set of coding
conventions, mainly derived from the conventions in Windows
headers and other Microsoft code. We want to make it easy
for anyone to understand your code, so we have a single set
of guidelines for everyone writing C++ on any platform.</p>

<p>It is worth reiterating a few of the guidelines that
you might forget if you are used to the prevalent Windows
style:</p>

<ul>
  <li>Do not use Hungarian notation (for example, naming
  an integer <code>iNum</code>). Use the Google naming
  conventions, including the <code>.cc</code> extension
  for source files.</li>

  <li>Windows defines many of its own synonyms for
  primitive types, such as <code>DWORD</code>,
  <code>HANDLE</code>, etc. It is perfectly acceptable,
  and encouraged, that you use these types when calling
  Windows API functions. Even so, keep as close as you
  can to the underlying C++ types. For example, use
  <code>const TCHAR *</code> instead of
  <code>LPCTSTR</code>.</li>

  <li>When compiling with Microsoft Visual C++, set the
  compiler to warning level 3 or higher, and treat all
  warnings as errors.</li>

  <li>Do not use <code>#pragma once</code>; instead use
  the standard Google include guards. The path in the
  include guards should be relative to the top of your
  project tree.</li>

  <li>In fact, do not use any nonstandard extensions,
  like <code>#pragma</code> and <code>__declspec</code>,
  unless you absolutely must. Using
  <code>__declspec(dllimport)</code> and
  <code>__declspec(dllexport)</code> is allowed; however,
  you must use them through macros such as
  <code>DLLIMPORT</code> and <code>DLLEXPORT</code>, so
  that someone can easily disable the extensions if they
  share the code.</li>
</ul>

<p>However, there are just a few rules that we
occasionally need to break on Windows:</p>

<ul>
  <li>Normally we <a href="#Multiple_Inheritance">strongly discourage
  the use of multiple implementation inheritance</a>;
  however, it is required when using COM and some ATL/WTL
  classes. You may use multiple implementation
  inheritance to implement COM or ATL/WTL classes and
  interfaces.</li>

  <li>Although you should not use exceptions in your own
  code, they are used extensively in the ATL and some
  STLs, including the one that comes with Visual C++.
  When using the ATL, you should define
  <code>_ATL_NO_EXCEPTIONS</code> to disable exceptions.
  You should investigate whether you can also disable
  exceptions in your STL, but if not, it is OK to turn on
  exceptions in the compiler. (Note that this is only to
  get the STL to compile. You should still not write
  exception handling code yourself.)</li>

  <li>The usual way of working with precompiled headers
  is to include a header file at the top of each source
  file, typically with a name like <code>StdAfx.h</code>
  or <code>precompile.h</code>. To make your code easier
  to share with other projects, avoid including this file
  explicitly (except in <code>precompile.cc</code>), and
  use the <code>/FI</code> compiler option to include the
  file automatically.</li>

  <li>Resource headers, which are usually named
  <code>resource.h</code> and contain only macros, do not
  need to conform to these style guidelines.</li>
</ul>

<h2 id="Parting_Words">Parting Words</h2>

<p>Use common sense and <em>BE CONSISTENT</em>.</p>

<p>If you are editing code, take a few minutes to look at the
code around you and determine its style. If they use spaces
around their <code>if</code> clauses, you should, too. If their
comments have little boxes of stars around them, make your
comments have little boxes of stars around them too.</p>

<p>The point of having style guidelines is to have a common
vocabulary of coding so people can concentrate on what you are
saying, rather than on how you are saying it. We present global
style rules here so people know the vocabulary. But local style
is also important. If code you add to a file looks drastically
different from the existing code around it, the discontinuity
throws readers out of their rhythm when they go to read it. Try
to avoid this.</p>



<p>OK, enough writing about writing code; the code itself is much
more interesting. Have fun!</p>

<hr>
</div>
</body>
</html>
