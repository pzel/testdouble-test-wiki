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

### Templates

For unit tests, there are probably two main types: those with mocks and those without. While Eclipse doesn't support custom file templates for tests (somehow), it does have a decent snippet templating feature found in `Java` -> `Editor` -> `Templates`.

To use a template, create a file, empty it out, and then start typing the name and hit `[Ctrl]-Space` to invoke the template via auto-complete.

#### Standard template

For tests that shouldn't use [[test doubles|test double]] (i.e. tests of [[value]] objects or [[logical]] methods when practicing [[London-school TDD]] or tests without side effects or complex setup when practicing [[Detroit-school TDD]]), the template shouldn't require Mockito at all.

Add a `New…` code template named "test-pure" with the following pattern:

``` java
package ${enclosing_package};

import org.junit.*;
import static org.junit.Assert.*;
import static org.hamcrest.Matchers.*;

public class ${primary_type_name} {

    ${testedType} subject = new ${testedType}();

    @Test
    public void test() {
      ${cursor}
    }
}
```

#### Mockito template

When practicing [[London-school TDD]], dependencies should be faked in [[collaborator]] and [[signaler]] objects. When practicing [[Detroit-school TDD]], use of test doubles should be minimized to whatever is necessary to write a reasonable test.

Add a `New…` code template named "test-mockito" with the following pattern:

``` java
package ${enclosing_package};

import org.junit.*;
import static org.junit.Assert.*;
import static org.hamcrest.Matchers.*;
import static org.mockito.Matchers.*;
import static org.mockito.Mockito.*;
import org.mockito.Mockito;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;
import org.junit.runner.RunWith;

@RunWith(MockitoJUnitRunner.class)
public class ${primary_type_name} {

    @InjectMocks ${testedType} subject;
    ${cursor}

    @Test
    public void test() {
    }
}
```

The above will require you to type `testedType` yourself, since there is no template variable for the [[subject]] type.

When the depended-on types are defined on the subject, MoreUnit can automatically generate the `@Mock TypeName typeName;` declarations via its "Mock Dependencies in Test Case" (`[Ctrl]-[Alt]-[Shift]-M`) wizard.
