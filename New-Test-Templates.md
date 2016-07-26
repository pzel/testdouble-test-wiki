In this document, we'll show how to add code snippets to your IDE to create specific types of tests more quickly and consistently.

The duplication of import statements and dependency declaration in some language's tests increases the amount of friction required to add a new test. Because we want to encourage writing more units and more tests, reducing the amount of friction for creating new tests will be an important part of success. Moreover, inconsistencies across tests increase the cognitive load required to understand how they work, and if generating tests with a code snippet can help ward against inconsistency, then it's probably worth the cost of setting them up.

## Java (Eclipse)

To add a new Java code template, open preferences to Java -> Editor -> Templates:

<img width="727" alt="screen shot 2016-07-26 at 9 39 37 am" src="https://cloud.githubusercontent.com/assets/79303/17140266/de85a082-5315-11e6-9e5a-229836adebd8.png">

Next, give a name and paste in the template. The name will be picked up by Eclipse's auto complete feature (which defaults to the key-binding `ctrl-space`:

<img width="604" alt="screen shot 2016-07-26 at 9 40 07 am" src="https://cloud.githubusercontent.com/assets/79303/17140300/ffc17294-5315-11e6-80bc-6e6e462848ec.png">

### Collaboration tests

Collaboration tests specify how the subject under test interacts with its dependencies and are used to break the subject's work down into multiple, focused sub-tasks. The purpose of the test is to ensure the interfaces between the dependencies is sound and that the types of the values passed between those dependencies is sufficient to accomplish the broader task. 

Because collaboration tests are used to specify interactions between a subject and dependencies that typically don't exist yet, they typically use [[test doubles|test double]] to stand in for the actual dependencies. As a result, this template uses Mockito to inject test doubles for each dependency

Add a `New…` code template named "mockitoTest" with the following pattern:

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

Once you've added the snippet, you should be able to clear out an empty test, type `mockitoTest` and then expand the snippet with `ctrl-space` (or whatever you've set to auto-complete):

![snippet](https://cloud.githubusercontent.com/assets/79303/17140345/2845727e-5316-11e6-9fbd-9a3385edc7ed.gif)

The above will require you to enter in the `testedType` yourself, since there is no template variable for the [[subject]] type.

When the depended-on types are defined on the subject, MoreUnit can automatically generate the `@Mock TypeName typeName;` declarations via its "Mock Dependencies in Test Case" (`[Ctrl]-[Alt]-[Shift]-M`) wizard.

### Regression tests

For tests that shouldn't use [[test doubles|test double]] (i.e. tests of [[value]] objects and pure functions practicing [[London-school TDD]] or tests without side effects or complex setup when practicing [[Detroit-school TDD]]), the template shouldn't import Mockito at all.

Add a `New…` code template named "pureTest" with the following pattern:

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

This template is obviously much simpler, as it only pulls in JUnit & Hamcrest assertions.

## C#

### Collaboration tests

### Regression tests