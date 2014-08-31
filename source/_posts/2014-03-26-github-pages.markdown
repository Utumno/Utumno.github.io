---
layout: post
title: "Github pages"
date: 2014-03-26 16:54:20 +0000
comments: true
categories:
---

Another 15 minutes fiddling with google so:

Sites:

	http://wrye-bash.github.io/docs/Wrye%20Bash%20General%20Readme.html
	http://msysgit.github.io/
	http://utumno.github.io/blog/2013/12/21/set-up-octopress-in-windows-7/

Code:

	https://github.com/wrye-bash/wrye-bash.github.io/blob/master/docs/Wrye%20Bash%20General%20Readme.html
	https://github.com/msysgit/msysgit.github.com/blob/master/index.html # com
	https://github.com/Utumno/Utumno.github.io/blob/master/blog/2013/12/21/set-up-octopress-in-windows-7/index.html

Notice that	`https://wrye-bash.github.io/` gives a 404 - there should be an `https://github.com/wrye-bash/wrye-bash.github.io/blob/master/index.html` apparently.

Ah, yes - [How do I clone a github wiki?](http://stackoverflow.com/questions/15080848/how-do-i-clone-a-github-wiki):

> Append *.wiki.git* to the repo name, i.e. if your repo name was foobar
> `git clone git@github.com:myusername/foobar.git` would be the path to clone your repo
> and `git clone git@github.com:myusername/foobar.wiki.git` would be the path to clone its wiki.

I 'm sure this post will grow.
