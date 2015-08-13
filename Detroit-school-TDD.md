If you've only heard of one kind of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development), it was this one. 

## The Name
Named "Detroit-school" because it was popularized by the paragons of [Extreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) who developed a lot of the ideas while staffed on Chrysler's [C3 project](https://en.wikipedia.org/wiki/Chrysler_Comprehensive_Compensation_System) in Detroit in the late-1990s.

Some folks call it "Classical", "Traditional", or merely "TDD" because it laid the foundation of most of the core concepts of TDD and wields significant influence on the common wisdom, documentation, training, and tools that support TDD (and as a result, modern automated software testing).

## What is it?

In Detroit-school TDD, a public API is first identified by writing a test against it, then each successive test is written as an example of its use, which drives out additional requirements. 

There are numerous intended benefits of this approach, including:

* Working in very small increments
* Having the regression safety net of the previous tests when adding each requirement
* Freedom to aggressively refactor the implementation, since (ideally) the tests will have very little coupling to implementation details

## Comparison to London-school TDD

A few things distinguish Detroit-school TDD from its chief contemporary, [[London-school|London-school TDD]].

### Emphasis on Refactoring

The "Red-Green-Refactor" workflow in Detroit-school TDD necessitates a heavy refactor step, because the [[design pressure]] placed on the [[subject]] by its tests are limited to its public API. As a result, the tests tend to have little direct influence on the structure of the production code. The consideration of attributes of each unit (e.g. its size, dependencies, and [purity](https://en.wikipedia.org/wiki/Pure_function)) is almost entirely left up to the diligence of the developer.

A common pattern to emerge when practicing Detroit-school TDD is that an author will write some number of examples against a single public API, which in turn necessitates the creation of numerous private methods, which leads to relatively large and unwieldy units. This places all the design pressure on the author to answer for themselves questions like:

* "I have a large unit, what proper Design Patterns™ can I refactor this into?" ([Blank-slate syndrome](http://blog.codinghorror.com/avoiding-blank-page-syndrome/))
* "How many test examples are enough for the unit at this level of granularity?"
* "At what point are my implementation's private methods sufficiently complex that it warrants the creation of a separately-tested public API?"
* "What degree of redundant test coverage between the original public API and any subsequently-extracted public APIs is acceptable? Should related examples be culled or should the newly-extracted API be replaced by a [[test double]]?"

In practice, emphasizing refactoring as a task to be completed _after_ arriving at a working implementation has resulted in a point of contention for teams using TDD often enough that it warrants valid criticism of the methodology. Typically, if a team is under pressure to deliver software quickly, and they have passing tests to  indicate that their perhaps-not-very-well-factored solution does actually work, many teams choose to defer the task of aggressive refactoring. This action is popularly referred to as assuming "technical debt" (though that term's technical definition differs substantially).

This tension has resulted in advocates of Detroit-school TDD to exhort developers to work more slowly and insist on refactoring working solutions before delivering otherwise working code. This line of argument has frequently led to claims that TDD advocates are promoting dogma, where if the methodology doesn't work for someone they're clearly just "not doing it hard enough".

### Minimizing Test Doubles

Testing the [[subject]] under sufficiently realistic conditions is considered paramount to maximize the resulting tests' regression value, and as a result, use of [[test doubles|test double]] is seen as an affordance to be minimized, often by reworking the broader design to obviate them.

### Bottom-up development

Practitioners of Detroit-school TDD tend to favor "bottom-up" development, in which smaller units are developed first (in anticipation of their being needed) and only later composed in use by higher-order units (e.g. a developer might test-drive a model and a particular sorting function before plugging both of them into the test of an HTTP controller that would utilize each).

The reason bottom-up development is common in this school is because working "outside-in" can result in far too large of units of work to be comfortable, which can result in analysis paralysis.