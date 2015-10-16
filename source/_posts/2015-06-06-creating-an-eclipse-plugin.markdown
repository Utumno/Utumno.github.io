---
layout: post
title: "Creating an eclipse plugin"
date: 2015-06-06 00:52:00 +0200
comments: true
categories:
---
First download the [Eclipse IDE for Eclipse Committers][1] -
[used to be called][2] *Eclipse Standard* - see [this bug][3] for chit chat.

Run it, New Project > Plug-in Project. Apart from trivial choices why does it
not have eclipse version bigger than 3.5 ?

{% img center /images/eclipsePlugin/Target_platform.jpg 'image' 'images' %}

Question already asked and answered: [Eclipse RCP Plug-in Target Platform][4].

Next the plugin content - [official docs][5] - in particular *"Adding API
Analysis to the project will enable static analysis of API usage from the
new project"*.

Next you get at the project templates - [I had some difficulty finding them][6].
Choose and template and then - git (TBC)


###Links for later:

Testing:

- [How do I set up a test project for a Eclipse plugin project][7]
- [Automating unit tests (junit) for Eclipse Plugin development][8]

Tutorials - there are a gazillion out of date tutorials but for e4 see:

- [Eclipse4/Tutorials][9] (from the eclipse.org wiki)
- One recommended by VonC in his quoted answer [above][10] -
http://eclipsesource.com/blogs/2012/05/10/eclipse-4-final-sprint-part-1-the-e4-application-model/
- [Creating a Plug-in Project][11] from Luna docs
- Another [Eclipse Plugin Tutorial][12]
- [Top 10 Mistakes in Eclipse Plugin Development][13]

And messing with eclipse code (I will be blogging on this separately soon):

- [Patching your own Eclipse IDE][14]
- [Vogella Papercuts][15]

Bonus: [Install Eclipse Plugins - The Easy Way][16]

And some fun:
[How to become an Eclipse committer in 20 minutes and fork the IDE][17]


  [1]: http://www.eclipse.org/downloads/packages/eclipse-ide-eclipse-committers-442/lunasr2
  [2]: https://wagenknecht.org/blog/archives/2014/09/looking-for-eclipse-standard-download.html
  [3]: https://bugs.eclipse.org/bugs/show_bug.cgi?id=441957
  [4]: http://stackoverflow.com/q/28299931/281545
  [5]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.pde.doc.user%2Ftasks%2Fapi_tooling_setup.htm
  [6]: http://stackoverflow.com/questions/2612426/eclipse-plugin-development-for-eclipse
  [7]: http://stackoverflow.com/q/246130/281545
  [8]: http://stackoverflow.com/q/255370/281545
  [9]: http://wiki.eclipse.org/Eclipse4/Tutorials#EclipseSource_Eclipse_4_Tutorial
  [10]: http://stackoverflow.com/questions/2612426/eclipse-plugin-development-for-eclipse
  [11]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.rse.doc.isv%2Fguide%2Ftutorial%2FpdeProject.html
  [12]: http://www.wideskills.com/eclipse-plugin-tutorial
  [13]: http://eclipse.dzone.com/articles/top-10-mistakes-eclipse-plugin
  [14]: http://eclipsesource.com/blogs/2012/07/30/patching-your-own-eclipse-ide/
  [15]: http://blog.vogella.com/tag/papercut/
  [16]: http://www.venukb.com/2006/08/20/install-eclipse-plugins-the-easy-way/
  [17]: http://www.slideshare.net/LarsVogel/eclipse-committerin20minutes
