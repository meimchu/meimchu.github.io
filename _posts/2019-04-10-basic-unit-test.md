---
layout: post
title: Basic unit testing
date: 2019-04-06
---

Today I have begun learning and writing unit tests for some updates I have made to existing python functions. The idea of the unit test is to make sure the updated python modules/functions/classes all produce the same results as before the update. The update is simply swapping existing module usages with new module usages. I am using python's default ``unittest`` module and for now, I am doing ``test case``. Per the [definition on python.org](https://docs.python.org/2.7/library/unittest.html), ``test case`` is defined as

> A test case is the smallest unit of testing. It checks for a specific response to a particular set of inputs. unittest provides a base class, TestCase, which may be used to create new test cases.

Using the [basic example on python.org](https://docs.python.org/2.7/library/unittest.html#basic-example)
```python
import unittest

class TestStringMethods(unittest.TestCase):
```
That will create an class inheriting the unittest's TestCase object.
```python
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

if __name__ == '__main__':
    unittest.main()
```
This now adds a function called ``test_upper`` that will test whether ``foo.upper()`` equals to ``FOO``. That is what ``assertEqual()`` does. There are other assert tests such as assertFalse(), assertTrue()... etc as well. It also appears that the test unit functions must start with the name ``test`` or it will not work. One thing I have also noticed from running a test like that is that it seems that it will call a form of ``exit()`` after completing the test. There is also another way to call the ``TestStringMethods`` without using
```python
if __name__ == '__main__':
    unittest.main()
```
Per the website, we can replace the last two lines with these instead
```python
suite = unittest.TestLoader().loadTestsFromTestCase(TestStringMethods)
unittest.TextTestRunner(verbosity=2).run(suite)
```
Perhaps this method won't exit out the script editor when finished. That remains to be seen when I test this out further in the near future. Either way, very cool discovery today with unit testing!
