# Introduction to classes

## Olav Vahtras

Computational Python

---

layout: false

## Classes and Exceptions

### Object orientation

* The term object orientation refers to a way of grouping data and functions together.
* A class is associated with data members and methods (functions) operating on the data
* The class definition is a template for variable construction
* Invoking the class name as if it were a function is said to create an instance of the class
* The `__init__` method of the class initializes the new object
* One objective of of object oriented approach is to reuse old code
* sub-classing: reusing an existing class, modifying parts
* inheritance: a subclass inherits all properties of the parent class 

---

### First example

```
>>> class Fruit:
...     """A fruit object class"""
...
...     def __init__(self, name):
...         print("entering Fruit.__init__")
...         self.name = name
...
...     def __str__(self):
...         print("entering Fruit.__str__")
...         return "%s tastes sweet!" % self.name

```
```
>>> fruit = Fruit("Banana")
entering Fruit.__init__
>>> print(fruit)
entering Fruit.__str__
Banana tastes sweet!

```
---

### A modified class

* A subclass of ``Fruit`` (parent or super class)
* Just redefine the ``__str__`` function

```
>>> class Citrus(Fruit):
...     def __str__(self):
...         print("entering Citrus.__str__")
...         return "%s tastes sour" % self.name

```
```
    >>> lemon = Citrus("Lemon")
    entering Fruit.__init__
    >>> print(lemon)
    entering Citrus.__str__
    Lemon tastes sour

```

* Note that we enter the `__init__` method of the parent class `Fruit`

---

### A more general class

Consider a more specialized class

* Let taste be an attribute of the object (that can change) 
* We overwride both the ``__init__`` and ``__str__``
* Call the ``__init__`` function of the parent class to initialize the inherited attributes

```
>>> class FruitWithTaste(Fruit):
...     """ A fruit object class with taste"""
...     
...     def __init__(self, name, taste="sweet"):
...         print("entering FruitWithTaste.__init__")
...         Fruit.__init__(self, name) 
...         self.taste = taste
...
...     def __str__(self):
...         print("entering FruitWithTaste.__str__")
...         return "%s tastes %s!" % (self.name, self.taste)

```
```
>>> apple = FruitWithTaste("Apple")
entering FruitWithTaste.__init__
entering Fruit.__init__
>>> print(apple)
entering FruitWithTaste.__str__
Apple tastes sweet!

```

---

### Methods that alter data

* It possible to alter data (object attributes) directly by assignment  (no private variables)
::
```
    apple.taste = "sour"
```

* o-o approach - only alter data through defined class methods 

```
>>> class FruitWithTaste(Fruit):
...     """ A fruit object class with taste"""
...     
...     def __init__(self, name, taste="sweet"):
...         print("entering FruitWithTaste.__init__")
...         Fruit.__init__(self, name) 
...         self.taste = taste
...
...     def __str__(self):
...         print("entering FruitWithTaste.__str__")
...         return "%s tastes %s!" % (self.name, self.taste)
...     def rot(self):
...         self.taste = "rotten"

```
```
>>> apple = FruitWithTaste("Apple")
entering FruitWithTaste.__init__
entering Fruit.__init__

```
>>> apple.rot()

```
```
>>> print(apple)
entering FruitWithTaste.__str__
Apple tastes rotten!

```
---

### Classical and new style classes


* in older code class definitions have the form
```
>>> class OldClass:
...
...     def __init__(self, indata):
...         self .data = indata

```

* in newer code top-level classes are written as a subclass of a general object class

```
>>> class NewClass(object):
...     def __init__(self, indata):
...         self .data = indata

```   
---

To illustrate the difference

```
>>> old = OldClass('some input')
>>> print(type(old)) #doctest: +SKIP
<type 'instance'>
>>> print(dir(old)) #doctest: +SKIP
['__doc__', '__init__', '__module__', 'data']

```
```
>>> new = NewClass('some input')
>>> print(type(new))
<class '__main__.NewClass'>
>>> print(dir(new))
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'data']


```

---

New style classes:


* a more modern implementation of the classes with more functionality
* in Python 3, all classes are of the modern kind.
* possible to subclass ``dict`` and ``list`` types

*Recommendation: always declare top-level classes with `object`*

---

### Exceptions

#### Error handling in python

* When there is an error, the program stops with a stack trace
* The error type is written
* When the error condition is detected the program is said to *raise an exception*
* The ``try..except`` statement provides the programmer with tools to handle potential errors

---

#### Common errors

Missing argument 

```    
    #exceptions.py
    import sys
    arg1 = sys.argv[1]
    ...

```
```
    $ python exceptions.py
    Traceback (most recent call last):
      File "exceptions.py", line 3, in <module>
        arg1 = sys.argv[1]
    IndexError: list index out of range

```
* The ``IndexError`` exception is raised because the code attempts to address a list element outside the range
    
---

#### File missing
```
    ...
    arg1 = sys.argv[1]
    inputfile  = open(arg1)

```
```bash
    $ python exceptions.py notafile
    Traceback (most recent call last):
      File "exceptions.py", line 4, in <module>
        inputfile = open(arg1)
    IOError: [Errno 2] No such file or directory: 'notafile'

```

* The ``IOError`` exception is raised because the code attempts to open a non-existing file
* By default, the ``open`` statement assumes open for reading.

---

### Handling errors

* Errors which can be predicted can be handled by a ``try-except`` block

```
    try:
        f = open(sys.argv[1])
    except(IndexError):
        print("Usage: %s <filename>" % sys.argv[0])
        sys.exit(1)
    except(IOError):
        print("%s: file %s not found" % (sys.argv[0], sys.argv[1]))
        sys.exit(1)

```
```
    $ python exceptions.py
    Usage: exceptions.py <filename>
    $ python exceptions.py notafile
    exceptions.py: file notafile not found
    $

```
