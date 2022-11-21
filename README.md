<p align="center"><img src="/images/logo.png" alt=""></p>
<h1 align="center">What the f*ck Golang! 😱</h1>
<p align="center">Exploring and understanding Python through surprising snippets.</p>


Python, being a beautifully designed high-level and interpreter-based programming language, provides us with many features for the programmer's comfort. But sometimes, the outcomes of a Python snippet may not seem obvious at first sight.

Here's a fun project attempting to explain what exactly is happening under the hood for some counter-intuitive snippets and lesser-known features in Python.

While some of the examples you see below may not be WTFs in the truest sense, but they'll reveal some of the interesting parts of Python that you might be unaware of. I find it a nice way to learn the internals of a programming language, and I believe that you'll find it interesting too!

If you're an experienced Python programmer, you can take it as a challenge to get most of them right in the first attempt. You may have already experienced some of them before, and I might be able to revive sweet old memories of yours! :sweat_smile:

PS: If you're a returning reader, you can learn about the new modifications [here](https://github.com/satwikkansal/wtfpython/releases/) (the examples marked with asterisk are the ones added in the latest major revision). 

So, here we go...

# Table of Contents

<!-- Generated using "markdown-toc -i README.md --maxdepth 3"-->

<!-- toc -->

- [Structure of the Examples](#structure-of-the-examples)
    + [▶ Some fancy Title](#-some-fancy-title)
- [Usage](#usage)
- [👀 Examples](#-examples)
  * [Section: Strain your brain!](#section-strain-your-brain)
    + [▶ First things first! *](#-first-things-first-)
    + [▶ Strings can be tricky sometimes](#-strings-can-be-tricky-sometimes)
  * [Section: Slippery Slopes](#section-slippery-slopes)
    + [▶ Modifying a dictionary while iterating over it](#-modifying-a-dictionary-while-iterating-over-it)
    + [▶ Stubborn `del` operation](#-stubborn-del-operation)\
  * [Section: The Hidden treasures!](#section-the-hidden-treasures)
    + [▶ Okay Python, Can you make me fly?](#-okay-python-can-you-make-me-fly)
- [Contributing](#contributing)
- [Acknowledgements](#acknowledgements)
- [🎓 License](#-license)
  * [Surprise your friends as well!](#surprise-your-friends-as-well)
  * [More content like this?](#more-content-like-this)

<!-- tocstop -->

# Structure of the Examples

All the examples are structured like below:

> ### ▶ Some fancy Title
>
> ```py
> # Set up the code.
> # Preparation for the magic...
> ```
>
> **Output (Python version(s)):**
>
> ```py
> >>> triggering_statement
> Some unexpected output
> ```
> (Optional): One line describing the unexpected output.
>
>
> #### 💡 Explanation:
>
> * Brief explanation of what's happening and why is it happening.
> ```py
> # Set up code
> # More examples for further clarification (if necessary)
> ```
> **Output (Python version(s)):**
>
> ```py
> >>> trigger # some example that makes it easy to unveil the magic
> # some justified output
> ```

**Note:** All the examples are tested on Python 3.5.2 interactive interpreter, and they should work for all the Python versions unless explicitly specified before the output.

# Usage

A nice way to get the most out of these examples, in my opinion, is to read them in sequential order, and for every example:
- Carefully read the initial code for setting up the example. If you're an experienced Python programmer, you'll successfully anticipate what's going to happen next most of the time.
- Read the output snippets and,
  + Check if the outputs are the same as you'd expect.
  + Make sure if you know the exact reason behind the output being the way it is.
    - If the answer is no (which is perfectly okay), take a deep breath, and read the explanation (and if you still don't understand, shout out! and create an issue [here](https://github.com/satwikkansal/wtfpython/issues/new)).
    - If yes, give a gentle pat on your back, and you may skip to the next example.

PS: You can also read WTFPython at the command line using the [pypi package](https://pypi.python.org/pypi/wtfpython),
```sh
$ pip install wtfpython -U
$ wtfpython
```
---

# 👀 Examples

## Section: Strain your brain!

### ▶ First things first! *

<!-- Example ID: d3d73936-3cf1-4632-b5ab-817981338863 -->
<!-- read-only -->

For some reason, the Python 3.8's "Walrus" operator (`:=`) has become quite popular. Let's check it out,

1\.

```py
# Python version 3.8+

>>> a = "wtf_walrus"
>>> a
'wtf_walrus'

>>> a := "wtf_walrus"
File "<stdin>", line 1
    a := "wtf_walrus"
      ^
SyntaxError: invalid syntax

>>> (a := "wtf_walrus") # This works though
'wtf_walrus'
>>> a
'wtf_walrus'
```

2 \.

```py
# Python version 3.8+

>>> a = 6, 9
>>> a
(6, 9)

>>> (a := 6, 9)
(6, 9)
>>> a
6

>>> a, b = 6, 9 # Typical unpacking
>>> a, b
(6, 9)
>>> (a, b = 16, 19) # Oops
  File "<stdin>", line 1
    (a, b = 16, 19)
          ^
SyntaxError: invalid syntax

>>> (a, b := 16, 19) # This prints out a weird 3-tuple
(6, 16, 19)

>>> a # a is still unchanged?
6

>>> b
16
```



#### 💡 Explanation

**Quick walrus operator refresher**

The Walrus operator (`:=`) was introduced in Python 3.8, it can be useful in situations where you'd want to assign values to variables within an expression.

```py
def some_func():
        # Assume some expensive computation here
        # time.sleep(1000)
        return 5

# So instead of,
if some_func():
        print(some_func()) # Which is bad practice since computation is happening twice

# or
a = some_func()
if a:
    print(a)

# Now you can concisely write
if a := some_func():
        print(a)
```

**Output (> 3.8):**

```py
5
5
5
```

This saved one line of code, and implicitly prevented invoking `some_func` twice.

- Unparenthesized "assignment expression" (use of walrus operator), is restricted at the top level, hence the `SyntaxError` in the `a := "wtf_walrus"` statement of the first snippet. Parenthesizing it worked as expected and assigned `a`.  

- As usual, parenthesizing of an expression containing `=` operator is not allowed. Hence the syntax error in `(a, b = 6, 9)`. 

- The syntax of the Walrus operator is of the form `NAME:= expr`, where `NAME` is a valid identifier, and `expr` is a valid expression. Hence, iterable packing and unpacking are not supported which means, 

  - `(a := 6, 9)` is equivalent to `((a := 6), 9)` and ultimately `(a, 9) ` (where `a`'s value is 6')

    ```py
    >>> (a := 6, 9) == ((a := 6), 9)
    True
    >>> x = (a := 696, 9)
    >>> x
    (696, 9)
    >>> x[0] is a # Both reference same memory location
    True
    ```

  - Similarly, `(a, b := 16, 19)` is equivalent to `(a, (b := 16), 19)` which is nothing but a 3-tuple. 

---

### ▶ Strings can be tricky sometimes

<!-- Example ID: 30f1d3fc-e267-4b30-84ef-4d9e7091ac1a --->
1\.

```py
>>> a = "some_string"
>>> id(a)
140420665652016
>>> id("some" + "_" + "string") # Notice that both the ids are same.
140420665652016
```

2\.
```py
>>> a = "wtf"
>>> b = "wtf"
>>> a is b
True

>>> a = "wtf!"
>>> b = "wtf!"
>>> a is b
False

```

3\.

```py
>>> a, b = "wtf!", "wtf!"
>>> a is b # All versions except 3.7.x
True

>>> a = "wtf!"; b = "wtf!"
>>> a is b # This will print True or False depending on where you're invoking it (python shell / ipython / as a script)
False
```

```py
# This time in file some_file.py
a = "wtf!"
b = "wtf!"
print(a is b)

# prints True when the module is invoked!
```

4\.

**Output (< Python3.7 )**

```py
>>> 'a' * 20 is 'aaaaaaaaaaaaaaaaaaaa'
True
>>> 'a' * 21 is 'aaaaaaaaaaaaaaaaaaaaa'
False
```

Makes sense, right?

#### 💡 Explanation:
+ The behavior in first and second snippets is due to a CPython optimization (called string interning) that tries to use existing immutable objects in some cases rather than creating a new object every time.
+ After being "interned," many variables may reference the same string object in memory (saving memory thereby).
+ In the snippets above, strings are implicitly interned. The decision of when to implicitly intern a string is implementation-dependent. There are some rules that can be used to guess if a string will be interned or not:
  * All length 0 and length 1 strings are interned.
  * Strings are interned at compile time (`'wtf'` will be interned but `''.join(['w', 't', 'f'])` will not be interned)
  * Strings that are not composed of ASCII letters, digits or underscores, are not interned. This explains why `'wtf!'` was not interned due to `!`. CPython implementation of this rule can be found [here](https://github.com/python/cpython/blob/3.6/Objects/codeobject.c#L19)
  ![image](/images/string-intern/string_intern.png)
+ When `a` and `b` are set to `"wtf!"` in the same line, the Python interpreter creates a new object, then references the second variable at the same time. If you do it on separate lines, it doesn't "know" that there's already `"wtf!"` as an object (because `"wtf!"` is not implicitly interned as per the facts mentioned above). It's a compile-time optimization. This optimization doesn't apply to 3.7.x versions of CPython (check this [issue](https://github.com/satwikkansal/wtfpython/issues/100) for more discussion).
+ A compile unit in an interactive environment like IPython consists of a single statement, whereas it consists of the entire module in case of modules. `a, b = "wtf!", "wtf!"` is single statement, whereas `a = "wtf!"; b = "wtf!"` are two statements in a single line. This explains why the identities are different in `a = "wtf!"; b = "wtf!"`, and also explain why they are same when invoked in `some_file.py`
+ The abrupt change in the output of the fourth snippet is due to a [peephole optimization](https://en.wikipedia.org/wiki/Peephole_optimization) technique known as Constant folding. This means the expression `'a'*20` is replaced by `'aaaaaaaaaaaaaaaaaaaa'` during compilation to save a  few clock cycles during runtime. Constant folding only occurs for strings having a length of less than 21. (Why? Imagine the size of `.pyc` file generated as a result of the expression `'a'*10**10`). [Here's](https://github.com/python/cpython/blob/3.6/Python/peephole.c#L288) the implementation source for the same.
+ Note: In Python 3.7, Constant folding was moved out from peephole optimizer to the new AST optimizer with some change in logic as well, so the fourth snippet doesn't work for Python 3.7. You can read more about the change [here](https://bugs.python.org/issue11549). 

---



---
---

# Contributing

A few ways in which you can contribute to wtfpython,

- Suggesting new examples
- Helping with translation (See [issues labeled translation](https://github.com/satwikkansal/wtfpython/issues?q=is%3Aissue+is%3Aopen+label%3Atranslation))
- Minor corrections like pointing out outdated snippets, typos, formatting errors, etc.
- Identifying gaps (things like inadequate explanation, redundant examples, etc.)
- Any creative suggestions to make this project more fun and useful

Please see [CONTRIBUTING.md](/CONTRIBUTING.md) for more details. Feel free to create a new [issue](https://github.com/satwikkansal/wtfpython/issues/new) to discuss things.

PS: Please don't reach out with backlinking requests, no links will be added unless they're highly relevant to the project.

# Acknowledgements

The idea and design for this collection were initially inspired by Denys Dovhan's awesome project [wtfjs](https://github.com/denysdovhan/wtfjs). The overwhelming support by Pythonistas gave it the shape it is in right now.

#### Some nice Links!
* Link 1
* Link 2


# 🎓 License

[![WTFPL 2.0][license-image]][license-url]

&copy; [Dhamodaran pandiyan](https://github.com/dhamodaran-pandiyan)

[license-url]: http://www.wtfpl.net
[license-image]: https://img.shields.io/badge/License-WTFPL%202.0-lightgrey.svg?style=flat-square

## Surprise your friends as well!
