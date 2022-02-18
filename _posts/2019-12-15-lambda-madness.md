---
title: Lambda madness
author:
    name: Mei Chu
date: 2019-12-15
categories: [Python]
tags: [lambda]
---

A little while back, I have encountered something rather interesting with regards to Python's lambda.
I searched around online and found other people with similar issues such as this one [What do (lambda) function closures capture?](https://stackoverflow.com/questions/2295290/what-do-lambda-function-closures-capture/2295372) Using the example posed by the thread starter:

```python
adders = [0, 1, 2, 3]
for i in [0, 1, 2, 3]:
    adders[i] = lambda a: i+a
print adders[1](3)
```

So what this is supposed to be doing is replacing the ``adders`` list with a little mini lambda function. So what we are expecting should be

```python
adders[0] = [0+a]
adders[1] = [1+a]
adders[2] = [2+a]
adders[3] = [3+a]
```
But as the poster have said, that would mean ``adders[1](3)`` would mean ``1+3=4``. However, to everybody's surprise, it returned a ``6``, which would indicate that it is actually getting it from ``adders[3]`` with a lambda of ``3+3=6``. It would appear that Python lambda when going through a loop like that, ``i`` is referencing the last value of the loop. Since all the variables are using the same name and within the same scope, we need to find a way to break that out. There are two easy way to do that as suggested by posters in the thread.

```python
adders = [0,1,2,3]
for i in [0,1,2,3]:
    adders[i]=lambda a,i=i: i+a
print adders[1](3)
```

This method will force the lambda to capture the ``i`` as part of its expression. This should separate the scope of ``i`` in the lambda expression to itself.

```python
adders = [0,1,2,3]
for i in [0,1,2,3]:
    adders[i] = (lambda b: lambda a: b + a)(i)
print adders[1](3)
```

This method as well, in its own way, is also separating out the scopes so it's localized to the lambda expression itself.

This is very handy to know because I have had situations where I am procedurally generating buttons for a GUI that triggers procedurally generated function via a loop. Ultimately all the buttons point to the same function, which is the last function in the loop list. That caused a lot of confusion!