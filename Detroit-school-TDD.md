If you've only heard of one kind of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development), it was this one. 

## The Name
Named "Detroit-school" because it was popularized by the paragons of [Extreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) who developed a lot of the ideas while staffed on Chrysler's [C3 project](https://en.wikipedia.org/wiki/Chrysler_Comprehensive_Compensation_System) in Detroit in the late-1990s.

Some folks call it "Classical", "Traditional", or merely "TDD" because it laid the foundation of most of the core concepts of TDD and wields significant influence on the common wisdom, documentation, training, and tools that support TDD (and as a result, modern automated software testing).

## What is it?

A few things distinguish Detroit-school TDD from its contemporary [[London-school|London-school TDD]]

In Detroit-school TDD, a public API is first identified by writing a test against it, then each successive test is written as an examples of its use, which drives out additional requirements.  dee There are numerous intended benefits of this approach, including:

* Working in very small increments
* Having a regression safety net of the previous tests when implementing each requirement
* Freedom to aggressively refactor all the code


### Emphasis on Refactoring

The "Red-Green-Refactor" workflow in Detroit-school TDD necessitates a heavy refactor step, because the [[design pressure]] placed on the [[subject]] by its tests are limited to its public API. 
* Bottom up

### Minimizing Test Doubles

Testing the [[subject]] under sufficiently realistic conditions is considered paramount to maximize the resulting tests' regression value, and as a result, use of [[test doubles|test double]] is seen as an affordance to be minimized, often by reworking the broader design to obviate them.

### Bottom-up development

Practitioners of Detroit-school TDD tend to favor "bottom-up" development, in which smaller units are developed first (in anticipation of their being needed) and only later composed in use by higher-order units (e.g. a developer might test-drive a model and a particular sorting function before plugging both of them into the test of an HTTP controller that would utilize each).

The reason bottom-up development is common in this school is because working "outside-in" can result in far too large of units of work to be comfortable, which can result in analysis paralysis.