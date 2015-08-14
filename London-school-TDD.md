Have you read [GOOS](http://www.growing-object-oriented-software.com)? This is basically GOOS.

## The Name

This school of TDD was derived from years of practice in the Extreme Programming community in London. It describes an entirely different approach to test-driven development from its predecessor, the more prevalent [[Detroit-school|Detroit-school TDD]]. Many of the concepts were first published (to my knowledge) in a [paper at XP 2000](http://www.ccs.neu.edu/research/demeter/related-work/extreme-programming/MockObjectsFinal.PDF). 

I can't speak for the GOOS's authors, [Nat Pryce](http://www.natpryce.com) and [Steve Freeman](http://higherorderlogic.com), so this article and other descriptions of "London" TDD only represent my derivative iterations on the concepts and themes they first published in their book.

## What is it?

## Comparison to Detroit-school TDD

### Emphasis on test doubles

### Design influence on implementations

An interesting consequence of practicing London-school TDD is that the design of each unit is not only heavily-influenced by its test and the overall testing workflow, but that the shape of each unit will typically resemble one of a handful of archetypes. Incidentally, each of these archetypical object patterns have some favorable design characteristics (e.g. adhering to [command-query separation](https://en.wikipedia.org/wiki/Command–query_separation), maximizing [pure functions](https://en.wikipedia.org/wiki/Pure_function), smaller units).

The reason that London-school TDD can influence the design of the implementation is because unit tests have far more awareness of the [[subject]]'s dependencies and their interactions. In [[Detroit-school|Detroit-school TDD]], if an "object is hard to test, then it's hard to use"; in London-school, if a "dependency is hard to mock, then it's definitely hard to use for the object that'll actually be using it." Put differently, Detroit-school TDD can only provide feedback about how comfortable it is to use an API in the sometimes contrived circumstance of a unit test, whereas London-school TDD routinely provides feedback about whether each unit's usage is awkward under real-world conditions.

* Collaborator
* Signaler
* Value
* Logic (leaf-nodes)

### De-emphasis on regression safety

Almost any application 

Any test of a unit that replaces dependencies with [[test doubles|test double]] cannot be trusted to provide confidence that the [[subject]] and the dependencies beneath it will work in a real-world context. Meanwhile, the tests of Value and Logic units can be counted on to fully cover their implemented behavior, since their tests will be under realistic conditions.

### Outside-in

(minimal implementations)