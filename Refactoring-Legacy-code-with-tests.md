(This page is a collection of notes and references taken from discussions during a [[5 Day Training|5 Day Training Agenda]])

One definition of "legacy code" is "code without tests". Because plenty of hard-to-deal-with code has _some_ kind of testing around it, a more useful definition might be "code we don't understand and trust".

### Workflow

This flow is pretty common when a part of a legacy system needs to be changed:

1. Identify a seam that's as close to the code to be changed as is reasonable; if necessary, make minimally-risky changes (e.g. via refactoring tooling) to create an easier-to-test API by introducing indirection to it
2. Write a large number of characterization tests against the API exposed by the seam, covering as many interesting inputs and broader system states as possible. Observe the outputs or side effects for each arrangement and then lock them down with robust assertions, regardless of whether they make any sense.
3. Using the characterization tests as your safety net, feel free to aggressively refactor the code, frequently running the characterization test along the way to ensure the refactor (or rewrite) isn't changing observable behavior
4. Backfill the newly-refactored units with clean and idiomatic tests of their own that cover their behavior neatly
5. Test & implement the desired change
6. Ensure everything is working
7. Delete the characterization tests. They should be redundant if tests of the newly refactored code are complete, and [[redundant coverage]] is problematic. If it's still difficult to bring yourself to delete them, consider how murky and confusing tests in the middle of the testing pyramid tend to be to readers later.

### Concepts

* Trusting [Eclipse's Refactoring Support](http://help.eclipse.org/juno/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Fconcepts%2Fconcept-refactoring.htm) to help us find a testable "Seam" in the code
* [Sunk-cost fallacy](https://en.wikipedia.org/wiki/Escalation_of_commitment)
* "Respond to pain by changing the source code, not masking the pain in the test or with testing tools"
* "Never trust a test you haven't seen fail"
* [Primitive Obsession](http://c2.com/cgi/wiki?PrimitiveObsession)
* [Sandi Metz](http://www.sandimetz.com)'s Squint Test: changes in shape indicate high complexity (loops and branching), while changes in color suggest the code operates at multiple levels of abstraction simultaneously

### References

* [Michael Feathers](https://twitter.com/mfeathers)' book, "[Working Effectively with Legacy Code](http://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)"
* [Sandi Metz](http://www.sandimetz.com)'s RailsConf 2014 presentation: "[All the small things](https://www.youtube.com/watch?v=8bZh5LMaSmE)", in which she explains her approach to the [Gilded Rose Kata](https://github.com/testdouble/contributing-tests/wiki/Gilded-Rose-Kata)
