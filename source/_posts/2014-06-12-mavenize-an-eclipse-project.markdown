---
layout: post
title: "Mavenize an eclipse project"
date: 2014-06-11 18:13:15 +0100
comments: true
categories:  [java, eclipse, maven]
---

I had a simple eclipse java SE project and I wanted to start using maven ([in 5 Minutes][1]) for dependency management. What I did was (eclipse Luna, JEE pack - comes with m2e plugin [1.5.0.20140524-0005]):

- RClick on the project > Configure > Convert to Maven project

- **Maven Pom dialogue**: I had to choose group id, artifact id etc. Had a look at [Guide to naming conventions on groupId, artifactId and version][2], [Maven artifact and groupId naming][3], [Maven artifact and group naming conventions][4]. Ok - pick a groupId (should be unique), an artifact Id (will be the name of your jar minus the version) and a version (have to decide on a scheme for this - must read: [Understanding Maven Version Numbers][5]. Have also a look at [How do I tell Maven to use the latest version of a dependency?][6]). Enter a name and description for your project.

- **Maven dependencies dialogue**: Identify existing project references as Maven dependencies:

  {% img center /images/Maven_and_eclipse/Clipboard01.jpg 'image' 'images' %}

  well at a loss here... Glad it found Junit. I left the "Delete original references from project" unticked. It went on to create the POM. Now I cheated a bit here.
  <!-- more -->
  - I had already tried once and saw the Unidentified dependencies pictured above. Well - hamcrest was resolved but jgrapht not - resulting in build errors. What I had to do was [google "maven jgrapht 0.9.0"][7] and at long last end up [here][8]. Copied:

     ```
    <dependency>
    	<groupId>org.jgrapht</groupId>
    	<artifactId>jgrapht-core</artifactId>
    	<version>0.9.0</version>
    </dependency>
    ```
  in the pom and was good to go.
  - Now the second time I followed the same procedure (after `--hard` reseting the branch) apparently m2e kept the jgrapht jar in its cache. So despite the fact the jars showed as unidentified the second time round I had no built errors. Actually I don't know what the reason was but the diff from the first time up to the second was:

     ```
	 $ git diff 1stTry 2ndTry
	 diff --git a/.classpath b/.classpath
	 index a92a607..c5171d8 100644
	 --- a/.classpath
	 +++ b/.classpath
	 @@ -11,6 +11,8 @@
	 			<attribute name="maven.pomderived" value="true"/>
	 		</attributes>
	 	</classpathentry>
	 +	<classpathentry kind="con" path="org.eclipse.jdt.USER_LIBRARY/jgraph"/>
	 +	<classpathentry kind="con" path="org.eclipse.jdt.junit.JUNIT_CONTAINER/4"/>
	 	<classpathentry kind="con" path="org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER">
	 		<attributes>
	 			<attribute name="maven.pomderived" value="true"/>
	 diff --git a/.settings/CCM_RUN.launch b/.settings/CCM_RUN.launch
	 index 3cc26b2..632106d 100644
	 --- a/.settings/CCM_RUN.launch
	 +++ b/.settings/CCM_RUN.launch
	 @@ -6,8 +6,6 @@
	 <listAttribute key="org.eclipse.debug.core.MAPPED_RESOURCE_TYPES">
	 <listEntry value="1"/>
	 </listAttribute>
	 -<stringAttribute key="org.eclipse.jdt.launching.CLASSPATH_PROVIDER" value="org.eclipse.m2e.launchconfig.classpathProvider"/>
	 <stringAttribute key="org.eclipse.jdt.launching.MAIN_TYPE" value="gr.uoa.di.mde515.Main"/>
	 <stringAttribute key="org.eclipse.jdt.launching.PROJECT_ATTR" value="project"/>
	 -<stringAttribute key="org.eclipse.jdt.launching.SOURCE_PATH_PROVIDER" value="org.eclipse.m2e.launchconfig.sourcepathProvider"/>
	 </launchConfiguration>
	 diff --git a/pom.xml b/pom.xml
	 index 0f9e725..dffb4a5 100644
	 --- a/pom.xml
	 +++ b/pom.xml
	 @@ -1,20 +1,17 @@
	 <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	 	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	 	<modelVersion>4.0.0</modelVersion>
	 	#### UNIMPORTANT ###
	 	<dependencies>
	 		<dependency>
	 			<groupId>junit</groupId>
	 			<artifactId>junit</artifactId>
	 			<version>4.11</version>
	 		</dependency>
	 -		<dependency>
	 -			<groupId>org.jgrapht</groupId>
	 -			<artifactId>jgrapht-core</artifactId>
	 -			<version>0.9.0</version>
	 -		</dependency>
	 	</dependencies>
	 	<build>
	 		<sourceDirectory>src</sourceDirectory>
    ```
	It seems the second time it kept `jgrapht` as an eclipse lib. Dunno. Runs fine.

- I added the jgrapht as a maven dependency - as well as logging:

     ```
    +		<dependency>
    +			<groupId>ch.qos.logback</groupId>
    +			<artifactId>logback-classic</artifactId>
    +			<version>1.1.2</version>
    +		</dependency>
    +		<dependency>
    +			<groupId>org.jgrapht</groupId>
    +			<artifactId>jgrapht-core</artifactId>
    +			<version>0.9.0</version>
    +		</dependency>
	```
  yep - that's all (see [Dependency management for SLF4J and Logback][9]). Wow !

- But now I wanted to add the logging configuration (must read:  [How to setup SLF4J and LOGBack in a web app - fast][10]). In this article (from where I downloaded the `logback[-test].xml`) it is said that these files belong in the `src/[main|test]/resources` folder. Great. I went ahead and manually moved the `src` to `src/main/java`, deleted `src` in Build path > source folders and added `src/main/java`. Classpath:

     ```
    -	<classpathentry kind="src" output="target/classes" path="src">
    -		<attributes>
    -			<attribute name="optional" value="true"/>
    -			<attribute name="maven.pomderived" value="true"/>
    -		</attributes>
    -	</classpathentry>
    +	<classpathentry kind="src" path="src/main/java"/>
    ```

  Then tried to Alt+F5 (RClick > Maven > Update Project...) but I had the "nesting errors":

 > MESSAGE Cannot nest 'project/src/main/java' inside 'project/src'. To enable the nesting exclude 'main/' from 'project/src'

  Gibberish. Solved in [Eclipse Build Path Nesting Errors][11]. Basically there was a line in the pom that caused the problem - hitting then Alt+F5 modified the classpath:

    ---------------------------------- .classpath ----------------------------------
    index 37a3013..982ad3d 100644
    @@ -1,6 +1,11 @@
     <?xml version="1.0" encoding="UTF-8"?>
     <classpath>
    -	<classpathentry kind="src" path="src/main/java"/>
    +	<classpathentry kind="src" output="target/classes" path="src/main/java">
    +		<attributes>
    +			<attribute name="optional" value="true"/>
    +			<attribute name="maven.pomderived" value="true"/>
    +		</attributes>
    +	</classpathentry>
     	<classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.7">
     		<attributes>
     			<attribute name="maven.pomderived" value="true"/>
    @@ -12,5 +17,11 @@
     			<attribute name="maven.pomderived" value="true"/>
     		</attributes>
     	</classpathentry>
    +	<classpathentry kind="src" output="target/test-classes" path="src/test/java">
    +		<attributes>
    +			<attribute name="optional" value="true"/>
    +			<attribute name="maven.pomderived" value="true"/>
    +		</attributes>
    +	</classpathentry>
     	<classpathentry kind="output" path="target/classes"/>
     </classpath>

    ----------------------------------- pom.xml -----------------------------------
    index 6b5f4af..8f37398 100644
    @@ -24,7 +24,6 @@
     		</dependency>
     	</dependencies>
     	<build>
    -		<sourceDirectory>src</sourceDirectory>
     		<plugins>
     			<plugin>
     				<artifactId>maven-compiler-plugin</artifactId>
 So m2e actually automatically added a test folder (which was not created resulting in an error in the build path - but not on the markers view - you need to open the build path to see it). So what about the resources folders ? Well just create two new __folders__ (__NOT__ source folders) and hit Alt+F5. Boom:

	6/12/14, 12:57:19 AM GMT+1: [INFO] Update started
	6/12/14, 12:57:19 AM GMT+1: [INFO] Using org.eclipse.m2e.jdt.JarLifecycleMapping lifecycle mapping for MavenProject: gr.uoa.di.mde515.ccm:ccm:0.0.1-SNAPSHOT @ C:\Dropbox\eclipse_workspaces\javaSE\hw2_db\pom.xml.
	6/12/14, 12:57:19 AM GMT+1: [INFO] Adding source folder /hw2_db/src/main/java
	6/12/14, 12:57:19 AM GMT+1: [INFO] Adding resource folder /hw2_db/src/main/resources
	6/12/14, 12:57:19 AM GMT+1: [INFO] Adding source folder /hw2_db/src/test/java
	6/12/14, 12:57:19 AM GMT+1: [INFO] Adding resource folder /hw2_db/src/test/resources
	6/12/14, 12:57:19 AM GMT+1: [ERROR] The resource tree is locked for modifications.
	6/12/14, 12:57:19 AM GMT+1: [INFO] Using org.eclipse.m2e.jdt.JarLifecycleMapping lifecycle mapping for MavenProject: gr.uoa.di.mde515.ccm:ccm:0.0.1-SNAPSHOT @ C:\Dropbox\eclipse_workspaces\javaSE\hw2_db\pom.xml.
	6/12/14, 12:57:19 AM GMT+1: [INFO] Update completed: 0 sec
	6/12/14, 12:57:20 AM GMT+1: [WARN] Using platform encoding (Cp1252 actually) to copy filtered resources, i.e. build is platform dependent!
	6/12/14, 12:57:20 AM GMT+1: [INFO] Copying 0 resource
	6/12/14, 12:57:20 AM GMT+1: [WARN] Using platform encoding (Cp1252 actually) to copy filtered resources, i.e. build is platform dependent!
	6/12/14, 12:57:20 AM GMT+1: [INFO] Copying 0 resource

  Dunno if I should worry about `[ERROR] The resource tree is locked for modifications.` Alt+F5 produces:

	---------------------------------- .classpath ----------------------------------
	index 982ad3d..8e85ec6 100644
	@@ -23,5 +23,15 @@
				<attribute name="maven.pomderived" value="true"/>
			</attributes>
		</classpathentry>
	+	<classpathentry excluding="**" kind="src" output="target/classes" path="src/main/resources">
	+		<attributes>
	+			<attribute name="maven.pomderived" value="true"/>
	+		</attributes>
	+	</classpathentry>
	+	<classpathentry excluding="**" kind="src" output="target/test-classes" path="src/test/resources">
	+		<attributes>
	+			<attribute name="maven.pomderived" value="true"/>
	+		</attributes>
	+	</classpathentry>
		<classpathentry kind="output" path="target/classes"/>
	 </classpath>

 Great. Now that I seem to get the hang of it I will turn to WTP project mavenizing.

Related:

- [Convert Existing Eclipse Project to Maven Project][12]: interesting links to m2e TODOs - yes it is still primitive...
  - [M2E project conversion enhancements][13], dreary eclipse wiki, the wiki of the dead roads etc
  - [eclipse-to-maven][14]: purports to do the thing (for eclipse kepler (?))



  [1]: http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
  [2]: http://maven.apache.org/guides/mini/guide-naming-conventions.html
  [3]: http://stackoverflow.com/questions/3724415/maven-artifact-and-groupid-naming
  [4]: http://stackoverflow.com/questions/23172586/maven-artifact-and-group-naming-conventions?lq=1
  [5]: http://docs.oracle.com/middleware/1212/core/MAVEN/maven_version.htm
  [6]: http://stackoverflow.com/q/30571/281545
  [7]: https://www.google.com/search?q=maven%20jgrapht%200.9.0&safe=off
  [8]: http://mvnrepository.com/artifact/org.jgrapht/jgrapht-core/0.9.0
  [9]: http://stackoverflow.com/questions/16660749/dependency-management-for-slf4j-and-logback/16661493#16661493
  [10]: https://wiki.base22.com/display/btg/How+to+setup+SLF4J+and+LOGBack+in+a+web+app+-+fast
  [11]: http://stackoverflow.com/questions/10838109/eclipse-build-path-nesting-errors/15411137#15411137
  [12]: http://stackoverflow.com/questions/2449461/convert-existing-eclipse-project-to-maven-project
  [13]: https://wiki.eclipse.org/M2E_project_conversion_enhancements
  [14]: https://github.com/vashishthask/eclipse-to-maven/tree/master/src/main/java
