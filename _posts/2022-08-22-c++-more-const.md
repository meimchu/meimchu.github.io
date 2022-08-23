---
title: more const
date: 2022-08-22
categories: [C++]
tags: [const, c++]
---

As I was trying to understand myself due to multiple ways a `const` might exist in code, I have found one neat little tip I found regarding `const` that helps us identify what that `const` means. I found that out through this website [The C++ 'const' Declaration: Why & How](http://duramecho.com/ComputerInformation/WhyHowCppConst.html). The key setence I want to highlight is:

```
Basically ‘const’ applies to whatever is on its immediate left (other than if there is nothing there in which case it applies to whatever is its immediate right).
```

So for example, very simply:
```
const int i;
```
Nothing on the left, so i is a constant integer. The most basic const of all.

```
const int * i;
```
Nothing on the left, so i is a variable pointer to a constant integer.

```
int const * i;
```
i is the same as above since `const` refers to the "thing" to its left.

```
int * const i;
```
The left of `const` is `*`, so this means this is a constant pointer to an integer variable.

```
int const * const i;
```
There are two `const` here each with something to its left. i is a constant pointer to a constant integer.

Those are some basic tips and there will be more `const` stuff as always!