---
layout: post
title: "Python notes"
date: 2014-07-06 17:38:03 +0100
comments: true
categories:
---
#####Random

[```    mylist = ['x', 3, 'b']
   print '[%s]' % ', '.join(map(str, mylist))
```](http://stackoverflow.com/a/5445983/281545)

[Infinite generator][1]: `iter(int, 1)` - try `li=filter(None, iter(int, 1))`

    >>> def ii(): yield filter(None, iter(int, 1))
    >>> ii
    <function ii at 0x025FBA70>
    >>> ii()
    <generator object ii at 0x0262B788>
    >>> ii()
    <generator object ii at 0x0262BFD0>
    >>> ii()[0]
    Traceback (most recent call last):
      File "<input>", line 1, in <module>
    TypeError: 'generator' object has no attribute '__getitem__'
    >>> ii().next()
    # Infinite loop

[I want to have a set of lists][2] (as opposed to adding the list items to
the set: [`s |= set(li)`][3])

Random refactoring puzzle:

	# before - indexes better be some set (although just _micro_ optimization)
    def SetChecked(self, indexes):
        for i in indexes:
            assert 0 <= i < self.Count, "Index (%s) out of range" % i
        for i in range(self.Count):
            self.Check(i, i in indexes)
    # after ?
    def SetChecked(self, indexes):
        for i in indexes: # will pick a random index from the set - may check
        # some items before assertion as opposed to before - important ?
            assert 0 <= i < self.Count, "Index (%s) out of range" % i
            self.Check(i, True)

[Thou Shalt Not Modify A List During Iteration][4] - [for sets just use pop][5]

- Scopes:

	- [Python nested classes scope][6]
	- [Nested Python class needs to access variable in enclosing class][7]

 My deleted: [Override python list operators dynamically][8] - see
https://wiki.python.org/moin/FromFunctionToMethod - and non deleted
[Override list's builtins dynamically in class scope][9] and
[Access base class attribute in derived class - in 'class scope'][10]

- Regexes:

  - [Regular Expression HOWTO - Python 2.7.10rc1 documentation][11]
  - [Python re Module - Use Regular Expressions with Python - Regex Support][12]

 To be expanded:

- [Can I get to the exception from the finally block ?][13]
- [Python 2: should I implement __cmp__() or __eq__()?][14]

#####Interpreter facts

	>>> def thatsSoUnlikeJava():
	...     try:
	...         raise Exception
	...     finally:return True
	...     
	>>> print thatsSoUnlikeJava()
	True
    >>> class C(object):
    ...     def __m(self): return 0
    ...     def m(self): return self.__m()
    ...
    >>> class D(C):
    ...     def __m(self): return 1
    ...
    >>> D().__m()
    Traceback (most recent call last):
      File "<input>", line 1, in <module>
    AttributeError: 'D' object has no attribute '__m'
    >>> D().m()
    0
    >>> def wrap():
    ...     m=0
    ...     def wrapped():
    ...         m+=1
    ...         return m
    ...     return wrapped
    ...
    >>> print wrap()()
    Traceback (most recent call last):
      File "<input>", line 1, in <module>
      File "<input>", line 4, in wrapped
    UnboundLocalError: local variable 'm' referenced before assignment


#####Links

- Basics:

	- [*args and **kwargs][15] - pythonisms too
	- [Printing tuple with string formatting in Python][16]
	- [Python - print tuple elements with no brackets][17]
	`', ,'.join([str(i[0]) for i in mytuple])`
	- [Why calling file.read in python fills my file with garbage?][18]
	- [Yield][19]

argparse:

- [Python argparse command line flags without arguments][20]

Import never ceases to surprise me. For instance in the code I have:

    try:
        from win32com.shell import shell, shellcon

In `Python27\Lib\site-packages\win32com` there was no shell to be seen - then I
noticed in `Python27\Lib\site-packages\win32com\__init__.py`:

    __path__.append( win32api.GetFullPathName( __path__[0] + "\\..\\win32comext") )

And `Python27\Lib\site-packages\win32comext\shell\__init__.py`:

	import win32com
	win32com.__PackageSupportBuildPath__(__path__)

And sure enough `C:\_\Python27\Lib\site-packages\win32comext\shell\shell.pyd`.
What's in there ? Centipydes probably.
There's shecllcon.py too. One can read there:

	FO_MOVE = 1
	FO_COPY = 2
	FO_DELETE = 3
	FO_RENAME = 4

almost [as](https://github.com/wrye-bash/wrye-bash/blob/aaa90f4f14f1879bb1c1bcb3c4e59bfe1424ea9f/Mopy/bash/balt.py#L912-L915)
 in the program I am maintaining.


#####Python launcher for windows

Double click on a script will open with specified (by shebang) python version.
Easiest to install: install python 3.3+ (see [this][21] for installer options).
[Gentle introduction][22] and the [PEP][23].


  [1]: http://stackoverflow.com/a/5739258/281545
  [2]: http://stackoverflow.com/questions/1306631/python-add-list-to-set
  [3]: http://stackoverflow.com/a/1306663/281545
  [4]: https://unspecified.wordpress.com/2009/02/12/thou-shalt-not-modify-a-list-during-iteration/
  [5]: http://stackoverflow.com/q/16551334/281545
  [6]: http://stackoverflow.com/questions/1765677/python-nested-classes-scope
  [7]: http://stackoverflow.com/q/6391645/281545
  [8]: http://stackoverflow.com/q/28685679/281545
  [9]: http://stackoverflow.com/q/29858405/281545
  [10]: http://stackoverflow.com/q/28734919/281545
  [11]: https://docs.python.org/2/howto/regex.html
  [12]: http://www.regular-expressions.info/python.html
  [13]: http://stackoverflow.com/a/1611572/281545
  [14]: http://gerg.ca/blog/post/2012/python-comparison/
  [15]: http://stackoverflow.com/questions/3394835/args-and-kwargs
  [16]: http://stackoverflow.com/questions/1455602/printing-tuple-with-string-formatting-in-python
  [17]: http://stackoverflow.com/questions/19112735/python-print-tuple-elements-with-no-brackets#comment38914099_19112784
  [18]: http://stackoverflow.com/questions/24973284/why-calling-file-read-in-python-fills-my-file-with-garbage
  [19]: http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-python
  [20]: http://stackoverflow.com/questions/8259001/python-argparse-command-line-flags-without-arguments
  [21]: http://stackoverflow.com/a/13297878/281545
  [22]: http://www.rmi.net/~lutz/py33-windows-launcher.html
  [23]: http://legacy.python.org/dev/peps/pep-0397/
