A "stub" is any [[test double]] that implements a so-called "stubbing", that is a preconfigured response to being invoked. A hand-rolled stub might be as simple as:

``` js
var fetchUser = function () { return {id: 42, name: 'Jane'} }
```

If `fetchUser` is then passed to a [[subject]] and invoked to facilitate some downstream behavior (like, say, printing the user's name), then that fake function could be called a stub. 

This is an "unconditional" stubbing, however, because it will always return the same thing regardless of the arguments you pass it. Using unconditional stubs generally leads to [[insufficient|Necessary-&-Sufficient]] tests, because the odds are that the _real_ `fetchUser` invoked in production will behave differently if passed the wrong arguments.

As a result, most test double libraries are a good deal more sophisticated, handing users test doubles that can be configured to respond a certain way only when a particular stubbing is "satisfied" with the appropriate arguments. In [testdouble.js](https://github.com/testdouble/testdouble.js), that would look like:

``` js
var fetchUser = td.function('fetchUser')
td.when(fetchUser(42)).thenReturn({id: 42, name: 'Jane'})
```

Now the subject will have to invoke the function `fetchUser` exactly as it should in production, because if it's called with different arguments, no stubbing will apply and `undefined` will be returned (which, presumably, would cause the test's assertion to fail).

For some more ideas on the sorts of configuration and features a stubbing API might enable, check out our [documentation on stubbing in testdouble.js](https://github.com/testdouble/testdouble.js/blob/master/docs/5-stubbing-results.md#stubbing-behavior).