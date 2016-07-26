Attendees of this course may use either Java & Eclipse or C# & Visual Studio. Depending on the language you want to use, the tools you'll want to install will vary.

## Java

### Source code

There are two preconfigured projects that we will work from: one for unit tests and one for browser tests. Pull down both and verify they work on your machine by following the instructions in their README files:

* [a simple JUnit project](https://github.com/testdouble/java-junit-example) 
* [a Cucumber and Selenium project](https://github.com/testdouble/java-cucumber-example)

### IDE Plugins

There are a couple IDE plugins that we will use to reduce the amount of friction when running tests inside Eclipse.

* [MoreUnit](http://moreunit.sourceforge.net/#download)
* [EclEmma](http://www.eclemma.org/installation.html)

If either plugin's update site is inaccessible, download the source and manually install the plugin by placing the expanded plugin into the `dropins` folder in your Eclipse installation directory and then restarting Eclipse.

Verify MoreUnit is installed by right-clicking anywhere in a Java source listing and verifying a "MoreUnit" item appears in the context menu.

Verify EclEmma is installed by right-clicking a JUnit test and seeing a "Coverage As…" item in the context menu.

### IDE Settings

* Maven's Eclipse integration to fetch not only the jars your project depends on, but also their source & documentation. This can be done from `Preferences` -> `Maven` and checking `Download Artifact Sources` and `Download Artifact JavaDoc`
* Click the downward-facing triangle in the explorer view, and select `Package Presentation` -> `Hierarchical` to get a better view at organizing packages into logical sub-groups

## C&#35;

