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

To that ends, install [MoreUnit](http://moreunit.sourceforge.net/#download) (both `MoreUnit For Java` and `MoreUnit For Java: Mock Support`) and restart.