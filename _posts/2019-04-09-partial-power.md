---
title: The power of partial
author:
    name: Mei Chu
date: 2019-04-09
categories: [Python]
tags: [oop]
---

A little something that I have learned when making a PySide widget at work is the usage of functools' partial. My most basic understanding of this is that it helps passes parameters to a function. A basic representation of partial can be seen here:

```python
from functools import partial


def add(a, b, c):
    return a + b + c


add_part = partial(add, c=2, b=1)

print(add_part(3))
print(add(3, 1, 2))
```

Both using the partial function and the function itself will return 6. So, on the surface, it doesn't seem all that special or important. However, consider that Qt signals don't allow for the passing of parameters, partial can be used to circumvent that. Consider this particular [Qt signal tutorial](https://www.pythoncentral.io/pysidepyqt-tutorial-creating-your-own-signals-and-slots/). The basic set up of that signal tutorial is as such:

```python
from PySide.QtCore import QObject, Signal, Slot
from functools import partial


class PunchingBag(QObject):
    ''' Represents a punching bag; when you punch it, it
        emits a signal that indicates that it was punched. '''
    punched = Signal()

    def __init__(self):
        # Initialize the PunchingBag as a QObject
        QObject.__init__(self)

    def punch(self):
        ''' Punch the bag '''
        self.punched.emit()


@Slot()
def say_punched():
    ''' Give evidence that a bag was punched. '''
    print('Bag was punched.')


bag = PunchingBag()
# Connect the bag's punched signal to the say_punched slot
bag.punched.connect(say_punched)

for i in range(10):
    bag.punch()
```

The ``say_punched()`` function does not take any parameter, unfortunately. So when the signal is emitted, it can only do whatever is in the function without parameters passed into it. However, using partial, we can make it happen and passed through the string "Bag". You can see the result here and understand how powerful this could be.

```python
from PySide.QtCore import QObject, Signal, Slot
from functools import partial


class PunchingBag(QObject):
    ''' Represents a punching bag; when you punch it, it
        emits a signal that indicates that it was punched. '''
    punched = Signal()

    def __init__(self):
        # Initialize the PunchingBag as a QObject
        QObject.__init__(self)

    def punch(self):
        ''' Punch the bag '''
        self.punched.emit()


@Slot()
def say_punched(x):
    ''' Give evidence that a bag was punched. '''
    print('%s was punched.' % x)


bag = PunchingBag()
# Connect the bag's punched signal to the say_punched slot
bag.punched.connect(partial(say_punched, 'Bag'))

for i in range(10):
    bag.punch()
```

This now allows me to pass through the string "Bag" to the ``say_punched()`` and that could be very useful. Now, you can pass through any other string such as "Purse", "Pillow" and so on. This is very useful in a particular situation at work where I created a function that performs a generic action for QTableWidget. However, the default connect signal does not allow me to re-use this same function for multiple QTableWidget, leading to duplication of codes. Having the ability to use partial to pass through the appropriate QTableWidget into the generic function discards the need to have such duplicate codes.

In the future, I will dwelve more into signals as well.
