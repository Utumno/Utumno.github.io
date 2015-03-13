---
layout: post
title: "Python notes"
date: 2014-07-06 17:38:03 +0100
comments: true
categories:
---

#####Python launcher for windows

Truly, you double click on a script and opens with specified (by shebang)  python version. Easiest to install: install python 3.3+ (see [this][1] for installer options). [Gentle introduction][2] and the [PEP][3].

#####Links

Basics:

- [*args and **kwargs][4] - pythonisms too
- [Printing tuple with string formatting in Python][5]
- [Python - print tuple elements with no brackets][6] - join and map
- [Why calling file.read in python fills my file with garbage?][7] - why oh why
- [Yield][9]

argparse:

- [Python argparse command line flags without arguments][8]

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

[as](
https://github.com/wrye-bash/wrye-bash/blob/aaa90f4f14f1879bb1c1bcb3c4e59bfe1424ea9f/Mopy/bash/balt.py#L912-L915)
 in the program I am maintaining.


  [1]: http://stackoverflow.com/a/13297878/281545
  [2]: http://www.rmi.net/~lutz/py33-windows-launcher.html
  [3]: http://legacy.python.org/dev/peps/pep-0397/
  [4]: http://stackoverflow.com/questions/3394835/args-and-kwargs
  [5]: http://stackoverflow.com/questions/1455602/printing-tuple-with-string-formatting-in-python
  [6]: http://stackoverflow.com/questions/19112735/python-print-tuple-elements-with-no-brackets#comment38914099_19112784
  [7]: http://stackoverflow.com/questions/24973284/why-calling-file-read-in-python-fills-my-file-with-garbage
  [8]: http://stackoverflow.com/questions/8259001/python-argparse-command-line-flags-without-arguments
  [9]: http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-python
