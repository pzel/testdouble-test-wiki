The term "Mock" has almost become an anachronistic term, carrying the baggage of multiple conflicting definitions related to [[test doubles|test double]].

## Colloquial use

When the term "mock" or phrase "mock out" is used today, it's typically meant to describe any kind of [[test double]]. This conflation occurred because the concept of test doubles emerged when mocking libraries began emerging in the early 2000s. 

Moreover, the general-purpose popularity of mocking libraries as a way to ease test pain quickly outpaced the growth of the [[London-school TDD]] practices which had been the reason for their invention in the first place. Even today, use of the term "mock" is more likely to refer to using a library to poke holes in reality than to refer to their use as design feedback mechanisms in [[London-school TDD]].

## Precise use

The term "mock" has a precise definition as well, as a specific sub-type of test double. A mock object asserts that certain invocations are made on itself, and will raise exception as soon as any unexpected interactions take place. Often, mocking libraries will adopt a record-playback metaphor in their API to facilitate this configuration, like EasyMock for Java shows below:

```java
import static org.easymock.classextension.EasyMock.*;

List mock = createMock(List.class);

expect(mock.get(0)).andStubReturn("one");
expect(mock.get(1)).andStubReturn("two");
mock.clear();

replay(mock);

someCodeThatInteractsWithMock();

verify(mock); 
```

There are several structural critiques one could make of this style of test:

* The `clear` and `replay` steps require every user to carry extra water for the sake of pleasing the test framework
* The test must configure its expectations before invoking the [[subject]] under test, which necessarily violates [[Arrange-Act-Assert]]
* It conflates assertion (via `expect`) with stubbing (via `andStubReturn`), which explicitly verifies the implementation of the subject and not its ultimate value (i.e. if the subject can magically figure out a way to solve the problem without needing to invoke a configured stubbing, it shouldn't be punished for doing so by triggering a failure). This is an indication that the assertion is [[unnecessary|Necessary-&-Sufficient]]

It was because of these issues that [[Test Spies|Spy]] became a more common alternative to mock objects, as they silently record each invocation made against themselves without requiring a record/playback metaphor, violating [[Arrange-Act-Assert]], or raising the fear of over-asserting the subject's implementation.