A Spy is a [[test double]] that records every invocation made against it and can verify certain interactions took place after the fact. They are most often used when a [[subject]] under test triggers a side effect that can't be asserted by the value it returns. Spies are generally seen as an iteration on [[Mock]] objects, but they aren't universally preferred.

A hand-rolled spy could be as simple as a fake function that remembers it was invoked:

```js
var saved = false
function save () { saved = true }
```

If `save` is passed to the [[subject]] and invoked, the test can invoke the [[subject]] and assert that `saved` is true. Of course, this verification will be "unconditional", and disregard whatever arguments are passed to `save`. Since, in most cases, what you pass a function matters quite a lot, test spies typically record the arguments of each invocation. Because hand-rolling a sophisticated test spy would be needlessly noisy, most test double libraries implement this behavior via a custom verification method or integration to the broader test framework's assertion mechanisms.

Here's an example using [testdouble.js](https://github.com/testdouble/testdouble.js):

```js
var save = td.function('save')
var subject = new Thing(save)

subject.doStuff('Jill')

td.verify(save('Jill'))
```

In the above example, `td.verify` will throw an error unless `save` was called at least once with the arguments `['Jill']`. Because verifications are often more nuanced (e.g. when some arguments don't matter, or an invocation is expected to be made several times), most test double libraries implement advanced configuration options for customizing a verification. To get an idea of what they typically support, see [our testdouble.js documentation on verifying](https://github.com/testdouble/testdouble.js/blob/master/docs/6-verifying-invocations.md)