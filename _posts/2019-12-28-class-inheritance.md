---
layout: post
title: Class inheritance
date: 2019-12-28
---

Class inheritance is an important subject to understand when it comes to objected oriented programming. It is the basis in which we can code as little as possible yet still have it contain as much information as possible. I want to focus on the initialization aspect of class inheritance and step through it so it becomes more clear.

Consider this simple class example at first:
```python
class A():
    def __init__(self):
        print('Initialize A')


a_obj = A()
```

You will see that the moment ``a_obj`` is called, it initialized the ``class A`` which will print
```python
>>> Initialize A
```
So far so good!

Now we will consider creating a ``class B`` that will inherit from ``class A`` and go from there.

```python
class B(A):
    def __init__(self):
        A.__init__(self)
        print('Initialize B')
```

What is happening here is ``class B(A)`` is telling us that this ``class B`` is going to inherit from ``class A``. But just that by itself does not do much. Typically, a well written python module would separate class and instance-level stuff by declaring them at the appropriate places. Instance variables, for example, would typically be delcared inside a class object's ``__init__`` method. Class variables would be declared outside the scope of the ``__init__`` method. As such, you must initiate the ``class A`` inside ``class B`` in order to be able to use all the stuff that's initiated inside ``class A``. That is where ``A.__init_(self)`` comes in handy.

Now, if you step through this:

```python
class A():
    def __init__(self):
        print('Initialize A')


class B(A):
    def __init__(self):
        A.__init__(self)
        print('Initialize B')


b_obj = B()
```
We will step through this process now step-by-step.
```python
b_obj = B() #We begin by initiating the class B via b_obj object.
```
```python
class B(A): #b_obj will bring us to class B which inherits from A.
```
```python
def __init__(self): #B.__init__() will be initiated.
```
```python
A.__init__(self) #Now this will initialize class A.
```
```python
def __init__(self): #We enter A.__init__()
```
```python
print('Initialize A') #This print statement will now be executed and exit A.__init__().
```
```python
print('Initialize B') #This print statement will now be executed and exit B.__init__().
```
So ultimately what you will see as a result of the code blocks above is:
```python
>>> Initialize A
>>> Initialize B
```
As you can see, ``A.__init__(self)`` is key to initializing ``class A``. It also means that you can place that statement not necessarily immediately after B's ``__init__`` if you so choose.