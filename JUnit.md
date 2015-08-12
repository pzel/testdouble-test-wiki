What follows is an example setup for using JUnit in a Java project.

# Setup 

It's pretty typical to start with JUnit as the test library, Hamcrest for additional assertion matchers, and Mockito for its [[test doubles|test double]].

In a `pom.xml`, these can be specified as:

``` xml
<dependencies>
  	<dependency>
  		<groupId>junit</groupId>
  		<artifactId>junit</artifactId>
  		<scope>test</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.mockito</groupId>
  		<artifactId>mockito-all</artifactId>
  		<version>1.10.19</version>
  		<scope>test</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.hamcrest</groupId>
  		<artifactId>hamcrest-all</artifactId>
  		<version>1.3</version>
  		<scope>test</scope>
  	</dependency>
  </dependencies>
```