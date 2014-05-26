---
layout: post
title: "JBoss on eclipse Luna"
date: 2014-05-25 20:12:58 +0100
comments: true
categories:
---

Windows - the [Installing and starting JBoss AS on Windows][1] page says I must have both maven and java installed - I really do not get why maven so I decided to proceed at my discretion.

- Downloaded <del>jboss-eap-6.2.0.zip</del> from http://jbossas.jboss.org/downloads. A hell of a difficult site to locate. Btw the user friendliness of the JBoss site(s) is minimal - take a peek at the [Getting Started Developing Applications Guide][2] it says "This guide has move (sic) to http://www.jboss.org/jdf/quickstarts/jboss-as-quickstart/guide/Introduction/" - the latter link being broken. Plus the community servers are down.
- Realized that this download is (probably) not what I wanted. The [Installing and starting JBoss AS on Windows][3] says something about jboss-7.x.x.x - scrolling further down in http://jbossas.jboss.org/downloads I located and downloaded JBoss AS 7.1.1.Final (jboss-as-7.1.1.Final.zip).
- Unzipped jboss-as-7.1.1.Final.zip to my c:\dev
- Fired eclipse up (Luna M7 Release (4.4.0M7) - EE pack) - located the update site of Jboss tools for Luna (installing them via the servers' view failed with dependency errors) -  http://download.jboss.org/jbosstools/updates/development/luna/  - downloaded only JBossAS Tools 3.0.0.Beta1-v20140408-1231-B31, accepted, restarted, and the fun part started
- Servers > New > JBoss AS 7.1 - profile Local (...), this server requires a runtime Next
- JBoss 7.1 Runtime: download and install runtime, duh. let's see
  - one of the runtimes (3) it offers to download is the jboss-as-7.1.1.Final.zip - great - go to Browse
  - c:\dev\jboss-as-7.1.1.Final, Execution environment JavaSE-1.6 (jdk 1.6.0.45) done
- import the project, make a new branch (jbossAS7)
- run - oops:
  >WARN  [org.jboss.modules] (MSC service thread 1-6) Failed to define class x.y.Z in Module "deployment.DataCollectionServlet.war:main" from Service Module Loader: java.lang.UnsupportedClassVersionError: x/y/Z : Unsupported major.minor version 51.0

 yep the project was 1.7 after all

- deleted the server, created it anew, pointed the runtime of Jboss server to jdk 7 and **it run** - logging needs to be info though to print anything in eclipse's console.
- yes but I couldn't hit the servlet - found the solution here: [Binding JBoss AS 7 to all interfaces][4]. In a nutshell I had to edit the server config at `C:\dev\jboss-as-7.1.1.Final\standalone\configuration\standalone.xml` - in there is the logging config too...

That was it - so basically zero modifications. Now to open shift...

<!-- more -->

###OpenShift

- Download the open shift JBoss tools from the same update site (http://download.jboss.org/jbosstools/updates/development/luna/) - JBoss OpenShift Tools (2.6.0.Beta1-v20140401-0009-B15)
- Hard part. New > Openshift application (use search), sign in to open shift (TODO: where does Eclipse store the passwords (bugzilla, openshift etc ?) - hope not in eclipse.core.runtime) > existing application (had created a jboss one) > Use existing project + create server for publishing + disable maven build > Use default remote name (untick, tick > openshift (?)) > Finish
- Scary warning about converting my project - back up original and have Beyond Compare handy
- Accept - cross fingers
- Starts downloading the repo from openshift - wish it just used github somehow... (edit: see [this blog post][7])
- OpenShift asks to publish - answer no and fire BC. .git/config only added a remote:

		[remote "openshift"]
			url = ssh://SHA@APP-DOMAIN.rhcloud.com/~/git/jbossas.git/

  good (`SHA` above stands for an hexadecimal number, the app's UUID apparently)
- .gitignore added

		target
		.settings/*
		!.settings/.jsdtscope
		.project
		.classpath
  still wander what will it do with build - notice .project .classpath removed - target added
- Added an `\.openshift\markers\` path containing only `skip_maven_build`, a `\.settings\org.jboss.tools.openshift.express.ui.prefs` file with deployment info and
- In .git it added a remote entry but also messed with HEAD and master's head - gitk: wow - a `Commit from JBoss Tools` - the changes detailed below + line endings diffs - noticed it reverts one previous line endings commit of mine - amending! _Why noone gets line endings right ever ?_
- Allright - backing the commit up (tag the original Commit from JBoss Tools)  amend, take in some unstaged line endings diffs (!) which effectively undo the reverting of my commit - finally:

        diff --git a/.gitignore b/.gitignore
        index 7f55666..3170c67 100644
        --- a/.gitignore
        +++ b/.gitignore
        // blah blah
        +target
        +.settings/*
        +!.settings/.jsdtscope
        +.project
        +.classpath
        diff --git a/.openshift/markers/skip_maven_build b/.openshift/markers/skip_maven
        new file mode 100644
        index 0000000..e69de29
  commit additional line endings diffs in a new branch to examine never
- more rebases etc and push (from git gui - allows forced updates) - <del>done</del> ! Well, not done - git reported success but the remote refs seen in gitk stayed put. I had to push from eclipse: Servers > APP at Openshift > Publish - "no local changes - publish by pushing ?" -> this now asks you if you want a forced update - yes:

		Stopping jbossas cartridge
		Sending SIGTERM to jboss:301473 ...
		Building git ref 'master', commit e48bedf
		Skipping Maven build due to absence of pom.xml
		Preparing build for deployment
		Deployment id is 72c749b6
		Activating deployment
		Deploying JBoss
		Starting jbossas cartridge
		Found 127.2.148.129:8080 listening port
		Found 127.2.148.129:9999 listening port
		/var/lib/openshift/SHA/jbossas/standalone/deployments /var/lib/openshift/SHA/jbossas
		/var/lib/openshift/SHA/jbossas
		Artifacts deployed: ./ROOT.war
		-------------------------
		Git Post-Receive Result: success
		Activation status: success
		Deployment completed with status: success

  Wow - but where is my app ? Hmmm: http://stackoverflow.com/questions/23873296/openshift-root-war-deployed-but-no-app

Producing manually the war (right click on the project then export as war) and then scp'ing straight from git Bash (msysgit, mingwin):

	$ scp "~/ROOT.war" SHA@APP-DOMAIN.rhcloud.com:/var/lib/\
	openshift/SHA/app-root/dependencies/jbossas/deployments

as detailed [here](https://www.openshift.com/kb/kb-e1088-how-to-deploy-pre-compiled-java-applications-war-and-ear-files-onto-your-openshift-gear) (detailed is an euphemism) solved it.

But now I am getting a class not found at my app's url (better than the default OpenShift page but still...):

>root cause

>java.lang.ClassNotFoundException: gr.uoa.di.monitoring.server.servlets.DataCollectionServlet from [Module "deployment.ROOT.war:main" from Service Module Loader]
	org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:190)
	org.jboss.modules.ConcurrentClassLoader.performLoadClassUnchecked(ConcurrentClassLoader.java:468)
	org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:456)
	org.jboss.modules.ConcurrentClassLoader.performLoadClass(ConcurrentClassLoader.java:398)
	org.jboss.modules.ConcurrentClassLoader.loadClass(ConcurrentClassLoader.java:120)
	org.jboss.as.web.deployment.WebInjectionContainer.newInstance(WebInjectionContainer.java:72)
	org.jboss.as.web.security.SecurityContextAssociationValve.invoke(SecurityContextAssociationValve.java:153)
	org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:102)
	org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:368)
	org.apache.coyote.http11.Http11Processor.process(Http11Processor.java:877)
	org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler.process(Http11Protocol.java:671)
	org.apache.tomcat.util.net.JIoEndpoint$Worker.run(JIoEndpoint.java:930)
	java.lang.Thread.run(Thread.java:701)

>note The full stack trace of the root cause is available in the JBoss Web/7.0.13.Final logs.
JBoss Web/7.0.13.Final

Let's see the logs

	$ ssh SHA@APP-DOMAIN.rhcloud.com
	[APP-DOMAIN.rhcloud.com SHA]\> cd $OPENSHIFT_LOG_DIR
	[APP-DOMAIN.rhcloud.com logs]\> pwd
	/var/lib/openshift/SHA/app-root/logs
	$ scp  SHA@APP-DOMAIN.rhcloud.com:/var/lib/openshift/SHA/app-root/logs/jbossas.log "~/upstream.jbossas.log"
	jbossas.log                                   100%  249KB 124.3KB/s   00:02
	$ npp "~/upstream.jbossas.log"

...

> 2014/05/26 15:57:26,679 WARN  [org.jboss.modules] (http-127.2.148.129-127.2.148.129-8080-1) Failed to define class\
gr.uoa.di.monitoring.server.servlets.DataCollectionServlet in Module "deployment.ROOT.war:main" from Service Module Loader:\
java.lang.UnsupportedClassVersionError: gr/uoa/di/monitoring/server/servlets/DataCollectionServlet : Unsupported major.minor version 51.0


oh well I have to enable java7 for the project - unfortunately this is done via a "marker" and trying to do it via rhc did not really help:

	$ rhc ssh jbossas
	[APP-DOMAIN.rhcloud.com SHA]\> export
	# ...
	declare -x JAVA_HOME="/etc/alternatives/java_sdk_1.6.0"
	# ...
	[APP-DOMAIN.rhcloud.com SHA]\> export JAVA_HOME=/etc/al
	ternatives/java_sdk_1.7.0
	[APP-DOMAIN.rhcloud.com SHA]\> ctl_app restart
	Cart to restart?
	1. jbossas-7
	?  1

Did not have an effect (found [this blog post][6] which explains some). I managed though - via the eclipse tools - OpenSHift Explorer > Right click on your app > Edit environmental variables:

{% img center /images/OpenShift/setting_up_java7.jpg 'image' 'images' %}

And that was it !


  [1]: https://docs.jboss.org/author/display/AS7/Installing+and+starting+JBoss+AS+on+Windows
  [2]: https://docs.jboss.org/author/display/AS7/Getting+Started+Developing+Applications+Guide
  [3]: https://docs.jboss.org/author/display/AS7/Installing+and+starting+JBoss+AS+on+Windows
  [4]: http://stackoverflow.com/questions/6853409/binding-jboss-as-7-to-all-interfaces
  [5]: https://www.openshift.com/kb/kb-e1088-how-to-deploy-pre-compiled-java-applications-war-and-ear-files-onto-your-openshift-gear
  [6]: http://blog.anthavio.net/2014/01/openshift-system-properties-and.html
  [7]: http://blog.anthavio.net/2014/01/deploy-to-openshift-from-github.html
