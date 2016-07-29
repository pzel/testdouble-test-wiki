In [[Discovery Testing]], a [[Collaborator Object]] describes any unit of code whose responsibility is to orchestrate the invocation of other units, as opposed to containing non-trivial enumeration or branching logic.

When practicing [[Discovery TDD|Discovery Testing]], a developer typically starts with a new collaboration test and asks "if several components could be composed to solve this problem, what would those components be?"

Let's look at an example. Suppose we're building a treehouse. We might start with a test that invokes this function under test:

``` javascript
var subject = new TreeHouseBuilder()

var result = subject.build()

assert(result instanceof TreeHouseWithRailings)
```

It would be foolish to try to implement everything to do with building a treehouse inside of a single method. A collaboration test differs doesn't presume we know _how_ to best do the job yet, but helps us focus on _what_ building a treehouse would entail. Brainstorming (and reading up [on treehouses](http://www.wikihow.com/Build-a-Treehouse)), we break it up into a few categories:

```js
var chooseTree
var buildPlatform
var addRailings
```

Mercifully, it's not our job at this point to answer how we'll choose a tree, design & build a platform, or install railings. All we need is a reasonable confidence that these three dependencies, if implemented well, would accomplish the task.

Of course, that won't work, because the three dependencies don't exist yet, so neither our test nor its [[subject]] have a reference to them. One option would be to pause our work on this test here, go implement those dependencies, and then finally implement the test. However, in [[Discovery Testing]] we'd prefer to prove out the subject's interaction with these dependencies before investing time in them. So, what do we do? This is where [[test doubles or "mocks"|test double]] come into play.

Here's how that test might look if we used test doubles in place of real functions, then passed them to our subject (using [testdouble.js](https://github.com/testdouble/testdouble.js) to illustrate:

```js
var chooseTree = td.function('chooseTree')
var buildPlatform = td.function('buildPlatform')
var addRailings = td.function('addRailings')
var subject = new TreeHouseBuilder(chooseTree, buildPlatform, addRailings)

var result = subject.build()

assert(result instanceof TreeHouseWithRailings)
```

_[Of course, passing function references around like this seems a bit ham-fisted, which is why testdouble.js provides less obtrusive ways to [replace dependencies with test doubles](https://github.com/testdouble/testdouble.js/blob/master/docs/7-replacing-dependencies.md#replacing-real-dependencies-with-test-doubles).]_

If we ran this test, even in its incomplete state, we'd get several rounds of initial feedback via messages like `TreeHouseBuilder is not defined`, then (after defining it) `subject.build is not a function`, then (after defining _it_) `TreeHouseWithRailings is not defined`. Once clearing these messages, we get to what might be called the "logical" or "intended" failure of `AssertionError: false == true`.

