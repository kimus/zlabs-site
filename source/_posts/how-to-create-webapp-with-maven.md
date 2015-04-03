layout: post
title: "How to create a WebApp project with Maven"
date: 2015-04-03 10:24:32
tags:
- maven
- tomcat
- eclipse
---

![](/images/maven_webapp.png)

In this article, we'll take a look at how you can use Apache Tomcat in conjunction with Apache Maven to speed up your development cycle.

We'll start with some Maven basics, including a look at the various versions of the Maven Tomcat plugin, and then move on to some more advanced concepts, such as archetypes, integrating Maven into your IDE, and continuous deployment.

<!-- more -->

## Create Web Project from Maven Template

The Maven "Archetype" plugin provides two key Maven features: generating a project from a standard template, and creating a new general template from an existing project. This allows projects to quickly be formatted into Maven's Standard Directory Layout.

There are many Maven archetypes, but the one most relevant to Apache Tomcat are the "webapp" and "wicket" archetypes. These generate basic frameworks for Java web applications and Apache Wicket framework projects.

This allows you to generate a basic project build format that documents all necessary dependencies and so on with minimum hassle. From these frameworks, you can build up your own idioms and create custom archetypes to match the kinds of applications you build most frequently.

You can create a quick start Java web application project by using the Maven maven-archetype-webapp template. In a terminal (Linux or Mac) or command prompt (Windows), navigate to the folder you want to create the project.

Type this command:

~~~bash
$ mvn archetype:generate -DgroupId={project-packaging} \
	-DartifactId={project-name} \
	-DarchetypeArtifactId=maven-archetype-webapp \
	-DinteractiveMode=false
~~~

For example:

~~~bash
$ pwd
/Users/kimus/Develop/maven

$ mvn archetype:generate -DgroupId=org.zlabs \
	-DartifactId=ZWebApp \
	-DarchetypeArtifactId=maven-archetype-webapp \
	-DinteractiveMode=false
 
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
[INFO]
(...)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.809 s
[INFO] Finished at: 2015-04-03T00:55:40+01:00
[INFO] Final Memory: 10M/81M
[INFO] ------------------------------------------------------------------------
~~~

A new web project named __ZWebApp__, and some of the standard web directory structure is created automatically.


## Project Directory Layout

Review the generated project layout:

```
ZWebApp
    |-- pom.xml
    `-- src
        `-- main
            |-- resources
            `-- webapp
                |-- WEB-INF
                |   `-- web.xml
                `-- index.jsp
```

Maven generated some folders, a deployment descriptor web.xml, pom.xml and index.jsp.

> Please check this official Maven standard directory layout guide to understand more.


## Eclipse Support

Almost sure you have already integrated your Tomcat development server into an IDE such as Eclipse for rapid development with graphical tools.

However, using Tomcat with Eclipse is only the beginning. Integrating Tomcat, Eclipse, and Maven allows for some really awesome development scenarios. Not only can you edit Maven POM files within Eclipse, using Eclipse's support features, you can also execute the builds you create from right within Eclipse, utilize Eclipse's repository interaction functionalities with the Maven plugin and dependency repositories, and more.

Eclipse and Tomcat can be integrated using the open-source m2eclipse plug-in developed by Sonatype, a company led by Jason van Zyl, the original creator of Maven.

Other IDEs that support Maven integration with their own plugins include Netbeans, IntelliJ, and JBuilder.


### Create Eclipse Project

To import this project into Eclipse, you need to generate some Eclipse project configuration files.

In terminal, navigate to “ZWebApp” folder, type this command:

~~~bash
$ mvn eclipse:eclipse -Dwtpversion=2.0
~~~

The option `-Dwtpversion=2.0` tells Maven to convert the project into an Eclipse web project (WAR), not the default Java project (JAR). For convenience, later we will show you how to configure this WTP option in pom.xml.


### Import Project in Eclipse

Imports it into Eclipse IDE – File -> Import... -> General -> Existing Projects into workspace.
project structure


### Debug in Eclipse

If you want to debug this project via Eclipse server plugin (Tomcat or other containers), type this again :

~~~bash
$ mvn eclipse:eclipse
~~~

### Maven Eclipse Plugin (m2e) Support

If you have a Maven based Eclipse project that you generated using mvn eclipse:eclipse, you’ll notice you won’t be able to use M2Eclipse (m2e) with it. If you look at the Eclipse .project file, you’ll see this comment.

NO_M2ECLIPSE_SUPPORT: Project files created with the maven-eclipse-plugin are not supported in M2Eclipse.

Here’s how you can convert your Maven Eclipse Plugin project to m2e. First add the M2Eclipse builder under the buildSpec tag, and the M2Eclipse nature under the natures tag, both inside the .project file.
	
~~~xml
<buildCommand>
  <name>org.eclipse.m2e.core.maven2Builder</name>
</buildCommand>
~~~

~~~xml
<nature>org.eclipse.m2e.core.maven2Nature</nature>
~~~

Next step is to replace all the M2_REPO libraries in the .classpath file with the M2Eclipse classpath container.

~~~	xml
<classpathentry kind="con" path="org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER" />
~~~


Refresh your Eclipse project. Right click on the project in Project Explorer View and you should now see the Maven option.



## The Mojo Tomcat Plugin

Like many of its functionalities, Maven provides Tomcat-specific goals through the use of independently developed plugins.

Mojo is a Codehaus-hosted project aimed at Maven plugin-in development.  The Tomcat plugin created under this project is the most commonly used tool for integrating Tomcat-specific goals into Maven build files.  

This plugin allows automated manipulation of WAR and exploded applications using the Tomcat Manager, to accomplish goals such as deployment, re-deployment, and start/stop.  

For example, the command "tomcat:deploy" would execute the entire build lifecycle as specified in the project's POM.xml file, and deploy it to the specified Tomcat server.


### Configuring the Maven Tomcat Plugin

pom.xml

~~~xml
<plugin>
	<groupId>org.apache.tomcat.maven</groupId>
	<artifactId>tomcat7-maven-plugin</artifactId>
	<version>2.2</version>
	<configuration>
		<path>/ZWebApp</path>
	</configuration>
</plugin>
~~~

Type this command:

~~~bash
$ mvn tomcat:run
~~~

It will start Tomcat and deploy your project default to port 8080.

`http://localhost:8080/ZWebApp/`


## Packaging

Here are few ways to deploy and test the web project.

To compile, test and package the project into a WAR file, type this:

~~~bash
$ mvn package
~~~

If everything is fine, the project dependencies will be attached to the project web deployment assembly.

A new WAR file will be generated at project/target/ZWebApp.war, just copy and deploy to your Tomcat.


