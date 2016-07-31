## Intro

Discovery Testing descends from [[London-school TDD]] to provide a very specific workflow for using the test-driven development to arrive at working designs composed of small, well-named units. As a side effect of that workflow, it encourages factoring object-oriented code into pure functions, discourages code reuse, and promotes rewriting in-the-small.



## Implications of Discovery Testing

### De-emphasis on regression safety

Any test of a unit that replaces dependencies with [[test doubles|test double]] cannot be trusted to provide confidence that the [[subject]] and the dependencies beneath it will work in a real-world context. Meanwhile, the tests of Value and Logic units can be counted on to fully cover their implemented behavior, since their tests will be under realistic conditions.

### Encouraging pure functions

TODO

### Discouraging code reuse

Because the Discovery Testing workflow encourages developers to break problems down into one-off tree-shaped dependency graphs, cyclic dependencies and code reuse occur somewhat infrequently when practicing Discovery Testing. 

While code reuse has been commonly promoted as a universal good over most of the history of computer science, it comes at an increased cost of change & replacement. In order to rewrite a bit of code, one must ensure the rewrite will satisfy its contract with all of its callers. Code that's only used once is, therefore, the easiest to change or replace—it only needs to be tested for one use. 

Take, for example, an MVC framework that encourages model-behavior and model-persistence to both be defined in a single source listing to collectively define the model. While it may be convenient to have a single place to look for any behavior corresponding with that model, it also encourages those behaviors to be reused—often in unexpected, untested contexts. The tangles of reuse that result in this monolithic style can cripple a team's ability to change (much less rewrite) vast swaths of their codebase, leading many to adopt microservice architectures in response.

The goal of Discovery Testing is to encourage segregation of behavior by default, even when opportunities for reuse are clear, because reducing the number of callers of any piece of code makes it dramatically cheaper to change and rewrite. The chief benefit is to get the clarity of mind and disposability associated with microservices with none of the infrastructure, networking, or deployment complexities.

### De-emphasis on Refactoring 

By encouraging outside-in testing with [[test doubles|test double]], any [[collaboration tests|Collaboration Test]] will naturally be coupled to the implementation of their subjects. As a result, refactoring any [[collaboration object]] in a way that would change the contract between itself and one of its subordinate dependencies will require a change to the subject's test as well. This is in contrast to [[Detroit-school TDD]], where tests at a given layer will ideally provide safety to aggressively refactor the layers below. 

Discovery Testing breaks from the [[Detroit-school|Detroit-school TDD]] mantra of "red-green-refactor" by considering refactoring to be an exception case to the typical workflow. Whereas regular refactoring is generally needed to keep [[Detroit-school|Detroit-school TDD]] designs small and manageable, Discovery Testing encourages the developer to start with narrowly focused units from the outset, passing the buck to subordinate dependencies at the first sign of complexity. As a result of this, the impulse to refactor is typically less pronounced, in two ways:

* Refactoring to try to make sense of the intent of the code is less common, as each named unit is typically very small, low-complexity, and free of cyclic dependencies
* Refactoring the codebase to better accommodate a forthcoming change comes at an increased cost of changing tests; where [[Detroit-school TDD]] might call for refactoring, Discovery Testing encourages rewriting the smallest subtree affected by the change

### Small, regular rewrites

Nobody likes undertaking Big Epic Rewrites, but as organizations change and grow, the code's imbued knowledge of the domain it serves tends to become increasingly outdated. Some people try to address this tension with rigor: refactor names and designs continuously whenever making a change. Others try to guard against it with infrastructure: write micro-services that will be discarded and replaced when they no longer suit their purpose.

Discovery testing takes a different tack to modernizing our understanding of the code base: encouraging targeted rewrites (as opposed to merely refactors) when requirements change substantially. It represents a sort of [controlled burn](https://en.wikipedia.org/wiki/Controlled_burn) for monolithic system architectures.



---


At its most basic, the workflow aims to systematically reduce a problem's complexity into small and sensible component parts from the outside-in:

1. Identify the entry-point of the feature to-be-developed (e.g. an HTTP controller action), noting the inputs available and the desired output (or side effect)
2. Create a test for a yet-unwritten domain object with a method-signature that would satisfy the top-level needs identified at the entry-point (the top-level domain object acts as [[scar tissue]] between the domain and the framework or runtime)
3. Instead of attempting to immediately implement a solution, try to imagine 2-4 dependencies that could break the work up for the [[subject]] into logical sub-problems
4. Create [[test doubles|test double]] for each of these dependencies and provide them to the [[subject]] (dependency injection is common here, but the approach varies by language ecosystem)
5. Implement a test that specifies and enforces the appropriate interaction of the dependencies (often a chain of [[stubs||stub]] for each dependency, potentially [[mocking|mock]] or [[spying|spy]] the final one if the top-level desired behavior is a side-effect)
6. Once the test passes, recurse by repeating steps 2-5 for each dependency; once a dependency's work can no longer be reasonably broken down, implement it as you would a pure function with [[Detroit-school TDD]]
7. Once all of the dependencies specified in all of the tests have been implemented, invoke the initial top-level domain object from the entry-point and verify it works as intended

A few consistent themes emerge from practicing this workflow:
* A [sense of surprise](http://michaelfeathers.typepad.com/michael_feathers_blog/2008/06/the-flawed-theo.html) that everything works on the first attempt when it's finally invoked in production
* Working outside-in to recurse through each dependency results in a easy-to-conceptualize tree of units for every feature
* Minimizing the depth of the tree and maximizing the number of leaf nodes is desirable, because pure functions are easier to understand, test, and reuse



Workflows derived from the London-school, like [[Discovery Testing]], compensate for this added cost by de-emphasizing refactoring; whenever a change needs to be made that will impact the contract between a unit and its dependencies, Discovery Testing suggests deleting the unit and its entire sub-tree of dependencies (along with their tests) and test-driving a fresh implementation with the new requirements in mind.


## Resources

For a deeper dive, these videos & screencasts describe Discovery Testing in greater detail:

* [My favorite way to TDD](http://blog.testdouble.com/posts/2015-09-10-how-i-use-test-doubles.html) (3h30m in 4 parts), implementing [[Game of Life]] in Java
* [Happier TDD with testdouble.js](http://blog.testdouble.com/posts/2016-06-05-happier-tdd-with-testdouble-js.html) (20m), JavaScript-focused
* [Mock objects in Discovery Testing](http://blog.testdouble.com/posts/2014-05-14-mock-objects-in-discovery-tests.html)
