---
title: Namespace
date: 2022-02-17
categories: [C++]
tags: [namespace]
---

`::` is known as a **scope operator** in C++.
It tells the codes to look at the scope of the left-hand operand for the name of the right-hand operand. For example, `std::cout` would say that we want to use `cout` from the namespace `std`. That specifies the library name `std`.

There are ways around having to specify the library every time we want to use something from there. The safest way is a `using` declaration. It let us use a name from a namespace without qualifying the name with a `namespace::` prefix.

```cpp
#include <iostream>

using std::cout;
int main()
{
    cout << "hello world"; // This will work because we have the using declaration
    cerr << "hello world"; // This will not work because of no declaration
    return 0;
}
```

```cpp
#include <iostream>

using namespace std;
int main()
{
    cout << "hello world" << endl; // This will work because we have the using declaration
    cerr << "hello world" << endl; // This will work because we have the using declaration
    return 0;
}
```

## Headers and Namespace Declarations
It is a bad idea to include `using` in a header file because those codes get copied directly through `#include`. As such, if a header file has a particular `using` declaration, every cpp file using the header file will be forced to conform.

