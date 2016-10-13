---
layout: post
title: "Sort before you loop"
date: 2016-05-29 17:06:24 +0200
comments: true
categories:
---

[Why is it faster to process a sorted array than an unsorted array?][1]

And a python quirk:

<script src="https://gist.github.com/Utumno/f3d25e0fe4bd0f43ceb9178a60181a53.js"></script>

Prints:

	C:\_\Python27\python.exe C:/Users/MrD/.PyCharm2016.2/config/scratches/timeit_key_tuple_sorting.py
	***** STRINGS
	0.683437665535
	0.659069905351
	0.578510675866
	***** INTS
	0.116263496833
	0.200527380123
	0.160531444582
	***** OBJECTS
	9.64448849458
	7.22313209823
	6.99353018193
	***** BONUS range vs xrange and inlining length
	3.16139231018
	3.13278352095
	2.18729023897
	2.16974310625


For an explanation see
[Why is sorting a python list of tuples faster when I explicitly
provide the key as the first element?][2]. Note that for int keys the non
key way is still faster - apparently the overhead of lambda (or even
itemgetter) compared to int comparison is significant.


  [1]: http://stackoverflow.com/q/11227809/281545
  [2]: http://stackoverflow.com/q/34455594/281545
