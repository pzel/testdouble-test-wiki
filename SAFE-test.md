There are numerous names used to describe integration tests that are intended to be the primary regression safety net for a given codebase:

* [Smoke tests](https://en.wikipedia.org/wiki/Smoke_testing_(software))
* [Acceptance tests](https://en.wikipedia.org/wiki/Acceptance_testing)
* Full-stack tests
* End-to-end tests

Each of these terms have some distinction, but those distinctions tend to lose meaning in practice, as most teams typically only ever choose one and then proceed to saddle that suite with all of their integration-testing motivations.

Instead, I mash them up into a cute catch-all acronym "SAFE", since the motivation of these suites is almost always to produce a build that tells the team whether their software works at a given commit.