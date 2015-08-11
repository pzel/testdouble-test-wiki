Often overlooked—because it's simpler for teachers and tools to lock us into a single test suite or a fixed set of test suites—is that application teams have the freedom and capability to design arbitrarily many test suites of their application.

Most teams that aren't happy with their tests report a lack of:

* Focus on what the purpose of each test is
* Consistency in structure and approach between tests
* Feedback speed of the test

One way to overcome (or at least sidestep) each of these issues is to take a more intentional approach towards the test suites that cover the application. Some example motivations for breaking ground on a new test suite might include:

* A class of tests known to be slow or of incidental concern (e.g. an operation suite of tests that verify our assumptions about third-party APIs and services)
* A fresh start that represents a clean break from an existing, similarly-motivated suite (i.e. it's very difficult to  unilaterally introduce focus, consistency, or speed once it is lost)
* Tests that represent the interests of a particular stakeholder (e.g. a suite of browser-automation tests for admins and another one for regular users)
* Tests that target varying layers of an application (e.g. a [[Detroit-school|Detroit-school TDD]] suite for an application's HTTP controllers and a [[London-school|London-school TDD]] suite for its domain objects

Ideally, each test suite should define the following traits:

* Can be easily executed independently of all other test suites
* What behavior is and isn't covered by the suites
* What dependencies should be replaced with [[test doubles|test double]] and what should be left realistic
* The primary design benefit (if any) of these tests
* The primary regression protection (if any) provided by these tests
* What an example test should look like
* The maximum permissible elapsed time for a run of an individual test or for the full suite 

As with anything, there is such a thing as too much of a good thing. [[Redundant coverage]] is an obvious and ever-present risk to multiple test suites that cover the same code; even when that risk is well addressed, the cost of determining whether the blended coverage of the overall application is complete is made more difficult.