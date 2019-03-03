Accidental creativity is a testing antipattern in which a test's meaningful novel attributes are confounded by arbitrary novel elements.

When a developer reads a test, every novel thing they encounter ought to be meaningful. Anything else is an instance of what might be called "accidental creativity", which upon reading must be parsed, acknowledged, and discarded when searching for the aspects of the test that say something meaningful about the behavior of the [[subject]].

Interpreting the author's intent when reading a test is often made unnecessarily challenging when the structure and form of each test varies arbitrarily. Teams that adopt a rigid and consistent structure to each test tend to more readily understand each test, because every deviation from the norm can be trusted to be meaningful and somehow specific to the nature of the [[subject]]. Examples of conventions that a team might agree to in order to decrease accidental creativity:

* Symmetrical unit testing (i.e. placing the test in a directory listing that mirrors the subject and with a predictable name like `SubjectTest`)
* Default to using [[meaningless test data]]
* Instantiating and naming the [[subject]] and injecting [[test doubles|test double]] in the exact same way across tests
* Using (or avoiding the use of) test utilities (e.g. [FactoryBot](https://github.com/thoughtbot/factory_bot) or [VCR](https://github.com/vcr/vcr)) in a consistent manner for a group of tests