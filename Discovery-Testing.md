At its most basic, the workflow aims to systematically reduce a problem's complexity into small and sensible component parts from the outside-in:

1. Identify the entry-point of the feature to-be-developed (e.g. an HTTP controller action), noting the inputs available and the desired output (or side effect)
2. Create a test for a yet-unwritten domain object with a method-signature that would satisfy the top-level needs identified at the entry-point (the top-level domain object acts as [[scar tissue]] between the domain and the framework or runtime)
3. Instead of attempting to immediately implement a solution, try to imagine 2-4 dependencies that could break the work up for the [[subject]] into logical sub-problems
4. Create [[test doubles|test double]] for each of these dependencies and provide them to the [[subject]] (dependency injection is common here, but the approach varies by language ecosystem)
5. Implement a test that specifies and enforces the appropriate interaction of the dependencies (often a chain of [[stubs||stub]] for each dependency, potentially [[mocking|mock]] or [[spying|spy]] the final one if the top-level desired behavior is a side-effect)
6. Once the test passes, recurse by repeating steps 2-5 for each dependency; once a dependency's work can no longer be reasonably broken down, implement it as you would a pure function with [[Detroit-school TDD]]
7. Once all of the dependencies specified in all of the tests have been implemented, invoke the initial top-level domain object from the entry-point and verify it works as intended

A few consistent themes emerge from practicing this workflow:
* A [sense of surprise](http://michaelfeathers.typepad.com/michael_feathers_blog/2008/06/the-flawed-theo.html) that everything works on the first attempt when it's finally invoked in production
* Working outside-in to recurse through each dependency results in a easy-to-conceptualize tree of units for every feature
* Minimizing the depth of the tree and maximizing the number of leaf nodes is desirable, because pure functions are easier to understand, test, and reuse



Workflows derived from the London-school, like [[Discovery Testing]], compensate for this added cost by de-emphasizing refactoring; whenever a change needs to be made that will impact the contract between a unit and its dependencies, Discovery Testing suggests deleting the unit and its entire sub-tree of dependencies (along with their tests) and test-driving a fresh implementation with the new requirements in mind.

### De-emphasis on regression safety

Any test of a unit that replaces dependencies with [[test doubles|test double]] cannot be trusted to provide confidence that the [[subject]] and the dependencies beneath it will work in a real-world context. Meanwhile, the tests of Value and Logic units can be counted on to fully cover their implemented behavior, since their tests will be under realistic conditions.

