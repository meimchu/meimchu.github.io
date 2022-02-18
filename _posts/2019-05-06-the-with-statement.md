---
layout: post
title: Using python's "with" statement
author: Mei
date: 2019-05-06
---

Recently, I have learned about the usage of ``with`` statement in python. It has some pretty interesting properties in the sense that it can be nicely used for contextual purposes. Acting very similarly as a ``@ decorator``, the ``with`` is often used to execute codes before and after codes that it wrapped around. Consider this very basic example:

```python
class with_statement(object):
    def __enter__(self):
        print('Entering!')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('Exiting!')


with with_statement():
    print('Inbetween string!')
```

So if you were to execute the codes above, here is the result you will see:

```
Entering!
Inbetween string!
Exiting!
```

As you can see, when you wrapped the ``Inbetween string!`` print statement with the ``with with_statement():``, it will execute the ``__enter__`` codes first. Then your wrapped codes, then the ``__exit__`` codes. So the basically idea is that it'll execute the codes in those specific orders, which can be very powerful. To elaborate a bit more on this, here are some expanded idea on the previous codes that allows it to do a little bit more.

```python
class with_statement_two(object):
    def __init__(self, dictKey):
        self.dictKey = dictKey

    def __enter__(self):
        self.withDict = {}
        for i in range(0, 5):
            self.withDict[i] = 'I have %s cat(s)!' % i
        print('Calculating how many cats I have...')
        return self.withDict

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.dictKey in self.withDict:
            print(self.withDict[self.dictKey])


with with_statement_two(2):
    print('Inbetween string!')
```

```
Calculating how many cats I have...
Inbetween string!
I have 2 cat(s)!
```

So in this scenario, we can pass a parameter into the ``with_statement_two`` class object. With that parameter, you can see that the ``with_statement_two`` class object is using that parameter to search a dictionary it created and output a dictionary value accordingly. So in this scenario, a dictionary is created in the ``__enter__`` function, meaning, the moment the ``with`` statement is called. Then, using the parameter it recieved upon its initialization of ``__init__``, it searches the dictionary key for a dictionary value with the parameter passed in. In this case, ``I have 2 cat(s)`` is the resulting output because an integer of ``2`` is the parameter being passed through.

Another common example of using the ``with`` statement is with the native python functionality of opening text documents. So instead of the typing:

```python
myFile = open('fileName.txt', 'r')
for line in myFile:
    do something
```

You can re-write it to be like this, which is considered to be a little bit cleaner!

```python
with open('fileName.txt', 'r') as myFile:
    for line in myFile:
        do something
```
