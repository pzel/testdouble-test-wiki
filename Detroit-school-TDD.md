If you've only heard of one kind of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development), it was this one. 

## The Name
Named "Detroit-school" because it was popularized by the paragons of [Extreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) who developed a lot of the ideas while staffed on Chrysler's [C3 project](https://en.wikipedia.org/wiki/Chrysler_Comprehensive_Compensation_System) in Detroit in the late-1990s.

Some folks call it "Classical", "Traditional", or merely "TDD" because it laid the foundation of most of the core concepts of TDD and wields significant influence on the common wisdom, documentation, training, and tools that support TDD (and as a result, modern automated software testing).

## What is it?

In Detroit-school TDD, a public API is first identified by writing a test against it, then each successive test is written as an example of its use, which drives out additional requirements. At its most simple, its workflow is:

1. Write a failing test
2. Change the implementation to make it pass (or change the message)
3. Refactor
4. Go to Step 1

There are numerous intended benefits of this approach, including:

* Working in very small increments
* Having the regression safety net of the previous tests when adding each requirement
* Freedom to aggressively refactor the implementation, since (ideally) the tests will have very little coupling to implementation details
* Resulting tests provide a complete (if highly [[redundant|redundant coverage]]) regression test suite, if TDD is practiced universally

## Comparison to London-school TDD

A few things distinguish Detroit-school TDD from its chief contemporary, [[London-school|London-school TDD]].

### Emphasis on Refactoring

The "Red-Green-Refactor" workflow in Detroit-school TDD necessitates a heavy refactor step, because the [[design pressure]] placed on the [[subject]] by its tests are limited to its public API. As a result, the tests tend to have little direct influence on the structure of the production code. The consideration of most important attributes of each unit (e.g. its size, dependencies, and [purity](https://en.wikipedia.org/wiki/Pure_function)) is almost entirely left up to the diligence of the developer to refactor aggressively after getting the tests to pass.

A common pattern to emerge when practicing Detroit-school TDD is that an author will write some number of examples against a single public API, which in turn necessitates the creation of numerous private methods, which then leads to relatively large and unwieldy units. This places all the design pressure on the author to answer for themselves questions like:

* "I have a large unit, what proper Design Patterns™ can I refactor this into?" ([Blank-slate syndrome](http://blog.codinghorror.com/avoiding-blank-page-syndrome/))
* "How many examples are enough to test the unit at this level of granularity?"
* "At what point are my implementation's private methods sufficiently complex that they warrant the creation of a separately-tested public API?"
* "What degree of [[redundant test coverage|redundant coverage]] between the original public API and any subsequently-extracted public APIs is acceptable? Once that threshold is crossed, should redundant examples be culled or should the newly-extracted API be replaced by a [[test double]] in the original test?"

In practice, emphasizing refactoring as a task to be completed _after_ arriving at a working implementation has so often become a point of contention for teams using TDD that it warrants valid criticism of the methodology. Typically, if a team is under pressure to deliver software quickly, and they have passing tests to indicate that their perhaps-not-very-well-factored solution does actually work, many teams will choose to defer the task of refactoring to some later point in time. This action is popularly referred to as assuming "technical debt" (though that term's definition varies dramatically).

This tension has resulted in some advocates of Detroit-school TDD to exhort developers to work more slowly and insist on refactoring before delivering otherwise working code. This line of argument has frequently led to claims that TDD's chief advocates are promoting dogma, because that message—as perceived by people for whom TDD hasn't worked well—can be perceived as "you're not doing it hard enough" (i.e. the [No True Scotsman](https://en.wikipedia.org/wiki/No_true_Scotsman) fallacy).

[Aside: I documented a number of these concerns in [this blog post](http://blog.testdouble.com/posts/2014-01-25-the-failures-of-intro-to-tdd.html).]

### Minimizing Test Doubles

Every test written in a Detroit-school test suite is designed to maximize regression safety. As a result, testing the [[subject]] under sufficiently realistic conditions is considered paramount to maximize the resulting tests' regression value. Therefore, use of [[test doubles|test double]] is seen as an affordance to be minimized, often by reworking the broader design to obviate them. 

This introduces a surprisingly complex responsibility of Detroit-school TDD practitioners: to define what level of realism is acceptable for a test to be called a "unit test". Rules similar to the ones [published by Michael Feathers in 2005](http://www.artima.com/weblogs/viewpost.jsp?thread=126923) are common and often debated within teams as suites grow and become slower.

By using [[test doubles|test double]] as a convenience to work around slow dependencies or too-difficult-to-test situations (e.g. verifying a side-effect occurred), the unit tests in a Detroit-school suite tend to be heavily incidentally dependent on the behavior of the [[subject]]'s dependencies. This acts as a double-edged sword: on one hand, when the behavior of a dependency changes in an unanticipated way, tests of its users will helpfully fail; on the other hand, large test suites will exhibit the problems caused by highly [[redundant coverage]].

In contrast, [[London-school|London-school TDD]] practitioners tend to have very clearly defined rules for when to use mocks. Because very few people in the broader public understand that there are two camps—much less the nuanced differences between them—a developer brought up in the Detroit-school will often look at a London-school unit test suite and immediately draw the conclusion that it has succumbed to extreme [[over-mocking]].

### Bottom-up development

Practitioners of Detroit-school TDD often favor "bottom-up" development, in which smaller units are developed first (in anticipation of their being needed by the larger system) and only later composed in use by higher-order units. To illustrate, a developer might test-drive a model and a particular sorting function before plugging both of them into the test of an HTTP controller that would utilize each.

This tendency emerges because "outside-in" development would result in uncomfortably large units of work. From the previous example, attempting "TDD an HTTP controller" would first require the developer to implement both a model and a sorting algorithm in order to make realistic test cases of the controller's behavior pass. Working in such coarse steps causes numerous problems, from [blank-slate syndrome](http://blog.codinghorror.com/avoiding-blank-page-syndrome/), to [[accidental creativity]] of the depended-on types, to a much-too-heavy refactor step extracting those types.

A significant drawback of bottom-up development is the opportunity for waste, dead code, and rework to result: 

* Waste, because it's fairly common that the subordinate components the developer predicts they will need won't actually be needed when it comes time to plug them into the public-facing unit (a customer would be right to question time spent writing  a fancy sorting algorithm when the HTTP controller they asked for turns out not to need it)
* Dead code, because it's up to the developer to (often, arbitrarily) decide how many test cases are "enough" for a given unit; each test case the unit satisfies naturally results in a more complex unit, which may result in code paths that are unreachable by the higher-order components that will ultimately depend on them
* Rework, because the API (e.g. method signature) of the subordinate coordinates is predicted before the higher-order component which will depend on it; while developing the higher-order component, perhaps the data available doesn't match what the developer predicted, which would require a change to the subordinate unit's inputs

