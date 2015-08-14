Have you read [GOOS](http://www.growing-object-oriented-software.com)? This is basically GOOS.

## The Name

This school of TDD was derived from years of practice in the Extreme Programming community in London. It describes an entirely different approach to test-driven development from its predecessor, the more prevalent [[Detroit-school|Detroit-school TDD]]. Many of the concepts were first published (to my knowledge) in a [paper at XP 2000](http://www.ccs.neu.edu/research/demeter/related-work/extreme-programming/MockObjectsFinal.PDF). 

I can't speak for the GOOS's authors, [Nat Pryce](http://www.natpryce.com) and [Steve Freeman](http://higherorderlogic.com), so this article and other descriptions of "London" TDD only represent my derivative iterations on the concepts and themes they first published in their book. Furthermore, so that I'm not putting too many words in their mouths, my approach to London-school testing will be documented in [[Discovery Testing]].

## What is it?

London-school TDD is an approach to using test-driven development to build systems that consistently arrive at clean, minimal designs of small, focused units of code. It shares many characteristics with [[Detroit-school TDD]], but differs significantly enough to warrant conceptualizing it as a separate methodology. Moreover, many terms and concepts familiar in [[Detroit-school|Detroit-school TDD]] are commonly overloaded when discussing London-school testing, which has been the source of endless confusion among developers.

At its most basic, the workflow aims to systematically reduce a problem's complexity into small and sensible component parts from the outside-in:

1. Identify the entry-point of the feature to-be-developed (e.g. an HTTP controller action), noting the inputs available and the desired output (or side effect)
2. Create a test for a yet-unwritten domain object with a method-signature that would satisfy the top-level needs identified at the entry-point (the top-level domain object acts as [[scar tissue]] between the domain and the framework or runtime)
3. Instead of attempting to immediately implement a solution, try to imagine 2-4 dependencies that could break the work up for the [[subject]] into logical sub-problems
4. Create [[test doubles|test double]] for each of these dependencies and provide them to the [[subject]] (dependency injection is common here, but the approach varies by language ecosystem)
5. Implement a test that specifies and enforces the appropriate interaction of the dependencies (often a chain of [[stubs||stub]] for each dependency, potentially [[mocking|mock]] or [[spying|spy]] the final one if the top-level desired behavior is a side-effect)
6. Once the test passes, recurse by repeating steps 2-5 for each dependency; once a dependency's work can no longer be reasonably broken down, implement it as you would a pure function with [[Detroit-school TDD]]
7. Once all of the dependencies specified in all of the tests have been implemented, invoke the initial top-level domain object from the entry-point and verify it works as intended

A few consistent themes emerge from practicing this workflow:
* A sense of surprise that everything works on the first attempt when it's finally invoked in production
* Working outside-in to recurse through each dependency results in a easy-to-conceptualize tree of units for every feature
* Minimizing the depth of the tree and maximizing the number of leaf nodes is desirable, because pure functions are easier to understand, test, and reuse

## Comparison to Detroit-school TDD

### Emphasis on test doubles

### Design influence on implementations

An interesting consequence of practicing London-school TDD is that the design of each unit is not only heavily-influenced by its test and the overall testing workflow, but that the shape of each unit will typically resemble one of a handful of archetypes. Incidentally, each of these archetypical object patterns have some favorable design characteristics (e.g. adhering to [command-query separation](https://en.wikipedia.org/wiki/Command–query_separation), maximizing [pure functions](https://en.wikipedia.org/wiki/Pure_function), smaller units).

The reason that London-school TDD can influence the design of the implementation is because unit tests have far more awareness of the [[subject]]'s dependencies and their interactions. In [[Detroit-school|Detroit-school TDD]], if an "object is hard to test, then it's hard to use"; in London-school, if a "dependency is hard to mock, then it's definitely hard to use for the object that'll actually be using it." Put differently, Detroit-school TDD can only provide feedback about how comfortable it is to use an API in the sometimes contrived circumstance of a unit test, whereas London-school TDD routinely provides feedback about whether each unit's usage is awkward under real-world conditions.

(Note: separate article or inline for discussion of discovery testing trees and unit types??)

* Collaborator
* Signaler
* Value
* Logic (leaf-nodes)

### Increased refactoring cost

A common criticism of London-school test suites is that the cost to refactor implementations of a unit is increased. 

While unit types whose tests don't use [[test doubles|test double]] (like Value and Logic units) are no different to refactor than Detroit-school units, types with mocked-out dependencies (like Collaborator and Signaler) require significantly more effort to refactor significantly. In general, attempting to refactor the implementation without changing the test first 

Workflows derived from the London-school, like [[Discovery Testing]], compensate for this added cost by de-emphasizing refactoring; whenever a change needs to be made that will impact the contract between a unit and its dependencies, Discovery Testing suggests deleting the unit and its entire sub-tree of dependencies (along with their tests) and test-driving a fresh implementation with the new requirements in mind.

### De-emphasis on regression safety

Almost any application 

Any test of a unit that replaces dependencies with [[test doubles|test double]] cannot be trusted to provide confidence that the [[subject]] and the dependencies beneath it will work in a real-world context. Meanwhile, the tests of Value and Logic units can be counted on to fully cover their implemented behavior, since their tests will be under realistic conditions.

### Outside-in

(minimal implementations)


### Wrapping third-party dependencies

The phrase "don't mock what you don't own"