---
title: const in parameters
date: 2022-06-24
categories: [C++]
tags: [const, c++]
---

There is a concept in C++ known as top-level and low-level `const`.

**Top-level** `const` indicates that the pointer itself is a `const`.

**Low-level** `const` indicates that the pointer is pointing to a `const` object.

When you initialize a variable such as:

```cpp
int i = 0;
int *const p1 = &i;
```

This is known as a **top-level** `const` as the variable `p1` is a `const` pointer to **non-const** `i` variable. You cannot modify the content of `i` through `p1`.

```cpp
*p1 = 10;
```

This is allowed as the `const` pointer itself has not changed. `i` is now 10.

```cpp
const int ci = 42;
```

This is still a **top-level** `const` because you cannot change the variable itself. This is not a pointer.

```cpp
const int *p2 = &ci;
```

This is a low-level `const` as `p2` is a pointer pointing to a `const` variable `ci`.

## Function parameter
Taking all that `const` property into consideration, we must think carefully about how we want our function parameter to indicate. Using a popular idiom for passing string around, here are some examples of how a string can be passed into a function.

```cpp
void foo_will_not_modify (const std::string &x); // const ref - won't modify
void bar_will_not_modify (std::string x);        // copy - won't modify
void baz_will_modify (std::string &x);           // reference - can modify
```
Another example that uses `const` to help make the function definition easily readable can be:
```cpp
std::string::size_type find_char(const string& s, char c, string::size_type &occurs);
```
The parameters would indicate that `s` will not be modified as it's a `const`. `c` is also not modifiable because it's a copy. `occurs` is modifiable because it's a pass by reference without `const`. So this could suggest to the code reader that the variable being passed in as the `occurs` parameter could change after executing that function. In this case, the variable will indicate how many occurences of the `c` character in `s` string.