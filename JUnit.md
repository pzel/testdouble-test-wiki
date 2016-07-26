What follows is an example setup for using JUnit in a Java project.

# Setup 

## Dependencies 

It's pretty typical to start with JUnit as the test library, Hamcrest for additional assertion matchers, and Mockito for its [[test doubles|test double]].

In a `pom.xml`, these can be specified as:

``` xml
<dependency>
	<groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>RELEASE</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>org.mockito</groupId>
	<artifactId>mockito-all</artifactId>
<version>RELEASE</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>org.hamcrest</groupId>
	<artifactId>hamcrest-all</artifactId>
	<version>RELEASE</version>
	<scope>test</scope>
</dependency>
```

## IDE

If you're using Eclipse, a helper that makes it easier to create, run, and switch between production sources and their tests can help tighten [[feedback loops|feedback loop]].

### Plugins

* Install [MoreUnit](http://moreunit.sourceforge.net/#download) (both `MoreUnit For Java` and `MoreUnit For Java: Mock Support`)
* Install [EclEmma](http://www.eclemma.org) for code coverage

### Templates

Consider setting up [[code snippet templates for defining new tests|New Test Templates]]

### Settings

Also consider making these setting changes to Eclipse:

* Maven's Eclipse integration to fetch not only the jars your project depends on, but also their source & documentation. This can be done from `Preferences` -> `Maven` and checking `Download Artifact Sources` and `Download Artifact JavaDoc`
* Click the downward-facing triangle in the explorer view, and select `Package Presentation` -> `Hierarchical` to get a better view at organizing packages in logical pods