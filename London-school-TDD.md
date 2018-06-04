## The Name

This school of TDD was derived from years of practice in the Extreme Programming community in London. It describes an entirely different approach to test-driven development from its predecessor, the more prevalent [[Detroit-school|Detroit-school TDD]]. Many of the concepts were first published (to my knowledge) in a [paper at XP 2000](http://www.ccs.neu.edu/research/demeter/related-work/extreme-programming/MockObjectsFinal.PDF). 

I can't speak for the [GOOS](http://www.growing-object-oriented-software.com)'s authors, [Nat Pryce](http://www.natpryce.com) and [Steve Freeman](http://www.higherorderlogic.com), so this article and other descriptions of "London" TDD only represent my derivative iterations on the concepts and themes they first published in their book. Furthermore, so that I'm not putting too many words in their mouths, my approach to London-school testing will be documented in [[Discovery Testing]].

## What is it?

London-school TDD is a name given to an approach to using test-driven development to build systems that consistently arrive at clean, minimal designs of small, focused units of code (for more, see [[Tests' Influence on Design]]). It shares many characteristics with [[Detroit-school TDD]], but differs significantly enough to warrant conceptualizing it as a separate methodology. Moreover, many terms and concepts familiar in [[Detroit-school|Detroit-school TDD]] are commonly overloaded when discussing London-school testing, which has been the source of endless confusion among developers. 

Because a London-school unit test suite only provides regression safety of each unit in isolation, it's typically necessary to have a second, minimal suite of [[end-to-end tests|SAFE test]] that validate the system works when everything is wired up and integrated. It's that integrated test which buys the developers the luxury of writing tightly focused isolation tests of each unit.

## Comparison to Detroit-school TDD

Many comparisons between the two schools of thinking can be summarized as "top-down" versus "bottom-up". Where London-school TDD encourages programmers to use external constraints as a starting point (an API endpoint, an HTTP controller, etc.), Detroit-school TDD encourages programmers to first identify the core domain logic that exemplifies the work without concern for how it might be integrated elsewhere.

Typically, systems designed with Detroit-school TDD demonstrate greater emphasis on [design patterns](https://en.wikipedia.org/wiki/Software_design_pattern), [object-oriented design](https://en.wikipedia.org/wiki/Object-oriented_design), and [domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design). The greatest risk posed by this approach is [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) resulting from APIs that wind up being a poor fit for the integrated code that would actually call it (e.g. mismatched parameters, differing return types, etc.)

Whereas systems written with a Detroit-school TDD method might demonstrate a number of creative patterns of object design & organization, systems designed with London-school TDD tend to produce very homogeneous trees of objects and functions of a handful of types (collaborators which interact with subordinate dependencies, pure functions, wrappers of third-party code, and value types). London-school TDD is commonly referred to as a gateway drug to functional programming, because it encourages separating code with side effects and maximizing pure functions. 

### Design influence on implementations

An interesting consequence of practicing London-school TDD is that the design of each unit is not only heavily-influenced by its test and the overall testing workflow, but that the shape of each unit will typically resemble one of a handful of archetypes. Incidentally, each of these archetypical object patterns have some favorable design characteristics (e.g. adhering to [command-query separation](https://en.wikipedia.org/wiki/Command–query_separation), maximizing [pure functions](https://en.wikipedia.org/wiki/Pure_function), smaller units).

The reason that London-school TDD can influence the design of the implementation is because unit tests have far more awareness of the [[subject]]'s dependencies and their interactions. In [[Detroit-school|Detroit-school TDD]], if an "object is hard to test, then it's hard to use"; in London-school, if a "dependency is hard to mock, then it's definitely hard to use for the object that'll actually be using it." Put differently, Detroit-school TDD can only provide feedback about how comfortable it is to use an API in the sometimes contrived circumstance of a unit test, whereas London-school TDD routinely provides feedback about whether each unit's usage is awkward under real-world conditions.

### Increased refactoring cost

A common criticism of London-school test suites is that the cost to refactor implementations of a unit is increased. 

While unit types whose tests don't use [[test doubles|test double]] (like Value and Logic units) are no different to refactor than Detroit-school units, types with mocked-out dependencies (like Collaborator and Signaler) require significantly more effort to refactor significantly. In general, attempting to refactor the implementation without changing the test first 

Because many [[collaboration tests|Collaboration Test]] use [[test doubles|test double]] to specify the contract of a dependency, the cost of refactoring a design is sometimes dramatically increased (which is why [[Discovery Testing]] encourages targeted rewrites instead, with refactoring as an exceptional activity).

Related Topics:

* [[Don't Mock What You Don't Own]]