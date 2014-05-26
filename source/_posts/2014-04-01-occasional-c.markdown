---
layout: post
title: "Occasional C"
date: 2014-04-01 16:30:31 +0100
comments: true
categories: 
---

When I happen to revisit this slim dinosaur that is C I always have to look up a couple things - here they are for your convenience and mine.

First things first - wrong:

	int *ptr = malloc(10 * sizeof (int));
	
Right:

	int *ptr = malloc(10 * sizeof *ptr);
	
NO exceptions. A, yes, sizeof is a [unary operator](http://en.wikipedia.org/wiki/Sizeof).
 Are parentheses needed ? Well, I always add them when nobody's looking.
