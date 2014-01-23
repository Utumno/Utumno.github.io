---
layout: post
title: "Python's super"
date: 2014-01-05 16:45:20 +0200
comments: true
categories: [Python, Java]
---
First thing that comes to mind : why do I need to name the superclass explicitly, instead of passing in `self.__class__` (or better still, `type(self)`)  ?
[Because](http://stackoverflow.com/questions/18208683/when-calling-super-in-a-derived-class-can-i-pass-in-self-class/18208725#18208725):

<!--``` linenos:false-->
	class Base(object):
		def method(self):
			print 'original'

	class Derived(Base):
		def method(self):
			print 'derived'
			super(type(self), self).method()

	class Subclass(Derived):
		def method(self):
			print 'subclass of derived'
			super(Subclass, self).method()
<!-- ``` -->

leads to:

    >>> Subclass().method()
    subclass of derived
    derived
    derived
    derived
	<...>
      File "<stdin>", line 4, in method
      File "<stdin>", line 4, in method
      File "<stdin>", line 4, in method
    RuntimeError: maximum recursion depth exceeded while calling a Python object

And this pretty much explains the second question I had. <!-- more --> In [Python's Super is nifty, but you can't use it](https://fuhm.net/super-harmful/)
(Previously: Python's Super Considered Harmful) we read:

> People omit calls to `super(...).__init__` if the only superclass is 'object', as, after all, `object.__init__` doesn't do anything! However, this is very incorrect. Doing so will cause other classes' `__init__` methods to not be called.

But it does not say why (insert rant here). Btw [not all of his examples compile](http://stackoverflow.com/questions/8972866/correct-way-to-use-super-argument-passing) (see this link also for a nice hack when you need to have arguments passed in the init method, and can't pop them one by one).

Well that's a pile of gibberish _if there were only single inheritance_. Calling `super(...).__init__` in a class whose only superclass is 'object' is then as useless as a used chewing gum. But the catch is that there is no such thing as single inheritance in python. So when we have MI, and our "penultimate" base class (that is the one that inherits directly from object) is used as a superclass in an unrelated MI class the call `super(...).__init__` will not delegate to object  init but in the next class in the MRO. [Example]():

    class Service(object):
        def __init__(self, host, binary, topic, manager, report_interval=None,
                 periodic_interval=None, *args, **kwargs):
            print 'Initializing Service'
            super(Service, self).__init__(*args, **kwargs)

    class Color(object):
        def __init__(self, color='red', **kwargs):
            print 'Initializing Color'
            self.color = color
            super(Color, self).__init__(**kwargs)

    class ColoredService(Service, Color):
        def __init__(self, *args, **kwds):
            print 'Initializing Colored Service'
            super(ColoredService, self).__init__(*args, **kwds)

    c = ColoredService('host', 'bin', 'top', 'mgr', 'ivl', color='blue')

gives:

	Initializing Colored Service
	Initializing Service
	Initializing Color

Why ? The call `super(Service, self).__init__(*args, **kwargs)` calls the next method in _self_'s MRO - _self_ being an instance of ColoredService. So the MRO is derived from the instance passed into super's _second argument_ (you may also pass a type, haven't gone into this) while _the starting point in the MRO_ is derived from super's first argument.

    super(startFromThisType, getTheMROFromThisInstance)

And that explains why one must always call super in a method meant to be overridden in a MI setting but when we need also the super class implementation.

Btw for Java people - [there is no automatic constructor call in  Python](http://stackoverflow.com/questions/2399307/python-invoke-super-constructor/2399332#2399332). You must call `super.__init__` manually and that's where this post comes from.

References:

- Things to Know About Python Super [[1 of 3]](http://www.artima.com/weblogs/viewpost.jsp?thread=236275), [[2 of 3]](http://www.artima.com/weblogs/viewpost.jsp?thread=236278), [[3 of 3]](http://www.artima.com/weblogs/viewpost.jsp?thread=237121)
- [The Python 2.3 Method Resolution Order](http://www.python.org/download/releases/2.3/mro/)
- [super(type[, object-or-type])](http://docs.python.org/2/library/functions.html#super)
- [Descriptors](http://users.rcn.com/python/download/Descriptor.htm) (in my toread)
- [Python's super() considered super!](http://rhettinger.wordpress.com/2011/05/26/super-considered-super/)
