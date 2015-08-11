"Test Double" is the broadest term available to describe any fake thing introduced in place of a real thing for the purpose of writing an automated test. The name is derived from the term "[Stunt Double](https://en.wikipedia.org/wiki/Stunt_double)", to evoke the sense of an artificial, but suitable stand-in for the real thing.

There are several sub-types of test doubles, but because most tools do a poor job either following a conventional nomenclature or providing test double utilities that adhere to only one sub-type, it's more useful to think of the various types of test doubles as various _intentional behaviors_ that test doubles may exhibit. These include:

* Stubs
* Fakes
* Mocks
* Partial Mocks and Proxies
* Spies

The usage of test doubles tends to be convoluted and confused in practice because of the lack of popular understanding between [[Detroit-school TDD]] and [[London-school TDD]].

* In Detroit (or "classical") TDD, it is paramount that the implementation of the [[subject under test]] be as divorced as possible from the test itself as possible, because the artifact of the test is intended to provide maximal regression value. Therefore, when a test double is introduced during Detroit-school TDD, it is typically an affordance which acknowledges some shortcoming in the author's reasonable ability to test the subject under totally realistic conditions.
* In London (or "mockist") TDD, because the regression value of individual unit tests is de-emphasized, test doubles are routinely used to replace _every single dependency_ that interacts with the subject and iterating on the interaction between the subject and its collaborators is seen as the chief design activity inherent in TDD.

This is why complaints of "over-mocking" in tests are typically lobbed at Detroit-school tests that have sacrificed their regression value in the name of convenience or at London-school tests which are not well-understood by the person lodging the complaint.