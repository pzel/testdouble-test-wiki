## Intro

Discovery Testing descends from [[London-school TDD]] to provide a very specific workflow for using test-driven development to arrive at working designs composed of small, well-named units. As a side effect of that workflow, it encourages factoring object-oriented code into pure functions, discourages code reuse, and promotes rewriting in-the-small.

Discovery Testing prescribes a recursive workflow for TDD, seeking to defining a tree of objects to implement a feature:

1. Start by identifying an entry point and writing a [[collaboration test]] of it
2. For each dependency the first collaboration test identifies:
  1. if it needs to be broken down further, write another [[collaboration test]] for it (e.g. `GOTO 1`)
  2. if its task is a straightforward data transformation, implement it as a pure-function [[leaf node]]
  3. if its task requires interaction with a third-party, implement a [[wrapper object]]

By recursing through the trees that emerge from following the above steps, most typical application development tasks can be accomplished.

All-the-while, any [[value objects|value object]] that are identified when specifying the contract between a [[collaborator object]] and its dependencies are tracked separately, aside from the tree (i.e. the values represent the fluid flowing through the pipes described by the tree of behavioral objects the tree represents).

## Tutorials

For a deeper dive, these videos & screencasts describe Discovery Testing in greater detail:

* [My favorite way to TDD](http://blog.testdouble.com/posts/2015-09-10-how-i-use-test-doubles.html) (3h30m in 4 parts), implementing [[Game of Life]] in Java
* [Happier TDD with testdouble.js](http://blog.testdouble.com/posts/2016-06-05-happier-tdd-with-testdouble-js.html) (20m), JavaScript-focused
* [Mock objects in Discovery Testing](http://blog.testdouble.com/posts/2014-05-14-mock-objects-in-discovery-tests.html)

## Implications of Discovery Testing

### De-emphasis on regression safety

Any test of a unit that replaces dependencies with [[test doubles|test double]] cannot be trusted to provide confidence that the [[subject]] and the dependencies beneath it will work in a real-world context. Meanwhile, the tests of Value and Logic units can be counted on to fully cover their implemented behavior, since their tests will be under realistic conditions.

### Encouraging pure functions

Quite a few functional programmers have been quick to realize that Discovery Testing implies a rejection of traditional Object-oriented programming. By splitting [[value objects|Value object]] from [[collaborator objects|Collaborator object]] and [[leaf nodes|leaf node]], units that describe data are generally not intertwined with units that encode application logic. When combined with the [reductionist](https://en.wikipedia.org/wiki/Reductionism) goal of shaking out as many pure function [[leaf nodes|leaf node]] as possible, [[collaborators|collaborator objects]] start to serve the same role as [higher-order functions](https://en.wikipedia.org/wiki/Higher-order_function) in functional programming.

Object-oriented programming languages, concepts, and jargon are still dominant in the industry. Discovery Testing seeks to provide a path for developers to take advantage of several of its benefits without leaving the comfort zone provided by their object-oriented ecosystem.

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