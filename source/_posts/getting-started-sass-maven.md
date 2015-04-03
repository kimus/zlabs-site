layout: post
title: "Getting started with Sass using Maven"
date: 2015-04-03 21:06:14
tags:
- maven
- sass
---

![](/images/maven_sass.png)

We already talked about how to {% post_link how-to-create-webapp-with-maven create a quick web project using Maven %}. Now let's just a simple way start using Sass in you web projects.

<!-- more -->

## Sass Maven Plugin

There's several plugins for using Sass in Maven. I've tried the _official_
but didn't work for me. So I'm using the `sass-maven-plugin` from jasig.

Add the maven sass plugin to your project:

~~~xml
<plugin>
  <groupId>org.jasig.maven</groupId>
  <artifactId>sass-maven-plugin</artifactId>
  <version>1.1.1</version>
  <configuration>
    <resources>
      <resource>
        <source>
          <directory>${basedir}/src/main/sass</directory>
        </source>
        <destination>${project.build.directory}/css</destination>
      </resource>
    </resources>
  </configuration>
</plugin>
~~~

Now we can start listening to changes on our scss files to auto generate:

~~~bash
$ mvn sass:watch
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building ZWebApp Webapp 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
(...)
[INFO]     >> /Users/kimus/Develop/maven/ZWebApp/src/main/sass/custom.scss => /Users/kimus/Develop/maven/ZWebApp/target/css/custom.css
[Listen warning]:
  Listen will be polling changes. Learn more at https://github.com/guard/listen#polling-fallback.
~~~

Alternatively we can generate on demand:

~~~bash
$ mvn sass:update-stylesheets

[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building ZWebApp Webapp 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
(...)
[INFO]     >> /Users/kimus/Develop/maven/ZWebApp/src/main/sass/custom.scss => /Users/kimus/Develop/maven/ZWebApp/target/css/custom.css
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.568 s
[INFO] Finished at: 2015-04-03T20:39:15+01:00
[INFO] Final Memory: 18M/302M
[INFO] ------------------------------------------------------------------------
~~~
