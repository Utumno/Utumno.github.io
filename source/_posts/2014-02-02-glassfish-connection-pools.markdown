---
layout: post
title: "Glassfish MySQL Connection Pools & Resources"
date: 2014-02-02 12:39:27 +0000
comments: true
categories:
---

Fire up the glassfish admin console at http://localhost:4848. You will see something like:

{% img center /images/Glassfish_Connection_Pools/Clipboard01.bmp 'image' 'images' %}

Select the JDBC Connection Pools item and then new:

{% img center /images/Glassfish_Connection_Pools/Clipboard02.bmp 'image' 'images' %}

Enter the values below: <!-- more -->

{% img center /images/Glassfish_Connection_Pools/Clipboard05.bmp 'image' 'images' %}

Press next and face a table with 213 additional properties, all together. Morons. Anyway:

{% img center /images/Glassfish_Connection_Pools/Clipboard08.bmp 'image' 'images' %}

and:

{% img center /images/Glassfish_Connection_Pools/Clipboard07a.bmp 'image' 'images' %}

and:

{% img center /images/Glassfish_Connection_Pools/Clipboard07b.bmp 'image' 'images' %}

and:

{% img center /images/Glassfish_Connection_Pools/Clipboard09.bmp 'image' 'images' %}

and:

{% img center /images/Glassfish_Connection_Pools/Clipboard10.bmp 'image' 'images' %}

Done!

{% img center /images/Glassfish_Connection_Pools/Clipboard11.bmp 'image' 'images' %}

Now for the resource:

{% img center /images/Glassfish_Connection_Pools/Clipboard12.bmp 'image' 'images' %}

and:

{% img center /images/Glassfish_Connection_Pools/Clipboard13.bmp 'image' 'images' %}

Hit OK.

See also:

[Java EE 7 GlassFish 4.0 Restful Webservices using and Netbean and deploying on Glassfish 4.0](http://stackoverflow.com/questions/18339081/java-ee-7-glassfish-4-0-restful-webservices-using-and-netbean-and-deploying-on-g/18340894#18340894)
