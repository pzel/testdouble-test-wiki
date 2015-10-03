If a [[test double]] is a crash dummy, then a partial mock is a person wearing a [crash dummy Halloween costume](https://www.google.com/search?q=crash+dummy+halloween+costume&safe=off&client=safari&rls=en&source=lnms&tbm=isch&sa=X&ved=0CAcQ_AUoAWoVChMI0ef2l7amyAIVxY8NCh13ZwAA&biw=1240&bih=716): still a stand-in for the real thing, but whose purpose is definitely less straightforward.

Put simply, "partial mock" refers to any actual object which has been wrapped or changed to provide artificial responses or verify actual interactions by a test. Partial mocks are widely considered to be an anti-pattern of [[test double]] usage.

## Why are they used

First, partial mocks sate some developers' [[realism impulse]], because they are certainly "less fake" than a full-blown test double. However, when pressed to articulate how a "partially real, partially fake" object is superior to a wholly real or a wholly fake one, its proponents typically struggle to articulate the advantages.

Second, when writing tests with a [[Detroit-school|Detroit-school TDD]] mindset, _any_ fakeness is seen as a concession in the name of either convenience or testability. For instance, a partial mock might be configured to call through to the real object and behave exactly as it normally would, but only be used by the test to verify some interaction took place; this pattern is typical in tests of functions that don't return anything. It's the present author's opinion that in a sufficiently integrated/realistic test, any desired side effect ought to be measurable by a more straightforward means (e.g. observing a new `Post` in the database as opposed to asserting that `Post.save()` was invoked).

Third, when testing legacy (or otherwise hard-to-test) code, a partial mock may be the least invasive way to make the code at all testable. This is the rationale provided by [Mockito's documentation on its Partial Mock feature](http://site.mockito.org/mockito/docs/current/org/mockito/Mockito.html#16) (which it confusingly named a "Spy"):

> As usual you are going to read the partial mock warning: Object oriented programming is more less tackling complexity by dividing the complexity into separate, specific, SRPy objects. How does partial mock fit into this paradigm? Well, it just doesn't... Partial mock usually means that the complexity has been moved to a different method on the same object. In most cases, this is not the way you want to design your application.

Finally, several test double libraries make it so easy to create partial mocks that they're often used uncritically, as if by default. Because objects-as-namespaces-of-function-bags are so common in JavaScript, Jasmine has a `spyOn(someObject, 'functionName')` API which will replace `someObject.functionName` for the duration of the test; it even allows the original behavior to be retained with `someObject.functionName.and.callThrough()` configuration.

## Why are they problematic?

Labeling anything an "anti-pattern" is often seen as a blanket pejorative, but in my experience partial mocks are rarely helpful in the short-term and almost always confusing in the long-term.

The short-term impact of using a partial mock is typically to salve over pain in testing a hard-to-use-or-isolate subject. This can have the effect of numbing the developer to healthy design feedback by encouraging they remedy a testing pain with additional affordances in the test code, as opposed to reworking the design of the production code. Additionally, partial mocks can be pretty challenging to set up: they require instantiating a real thing, then altering or wrapping it, then providing it to the subject, often leading to verbose and [[accidentally creative|Accidental Creativity]] setup code.

The long-term impact of using a partial mock is mostly in decreasing the comprehensibility of the test; their use often raises questions like "what's the value of this test?", "what's real and what's fake?", and "can I trust that a passing test means it's working under real conditions?". These questions are often asked _whenever_ test doubles are used, but finding clear-cut answers to those questions in the face of partial mocking is often a challenge. Partial mocks can also lead to hard-to-maintain tests, in the event that one of an object's methods is changed to invoke another method which has been faked-out in some number of tests, the partial mock may begin *interacting with itself* in unexpected ways.

There is a special sub-type of a partial mock, which is when the [[subject]] itself is partially mocked in a test. This exacerbates all of the aforementioned issues.

## Advice

If you want my advice, avoid using partial mocks wherever possible. Whenever I might feel the urge to use a partial mock, I ask how I might refactor the solution to obviate this need, which would normally result in a cleaner separation of the subject and its dependencies. When this sort of change is not possible, then I would prefer to drop into a more integrated test which can manage to put the subject under test with a fully-real dependency; in that case, the coverage would be reliable enough to refactor away whatever made the partial mock an appealing idea in the first place.