When working with [[test doubles|Test Double]], you may hear the phrase "Don't Mock What You Don't Own". Let's explain what people mean by that phrase, because it's counter-intuitive.

## Why this phrase is off-putting

99% of all mocks in use today are not used for the purpose for which 99% of test double libraries were designed: to facilitate isolated test-driven development aimed at arriving at clean designs of small units that interact via pleasant-to-use interfaces.

Almost all usage of mocking you're likely to find in the wild, however, are used to expediently knock out a real thing that makes a test difficult to implement or run (maybe it's a dependency on an external service, or it's an object that's really painful to construct, or someone just really likes faking stuff out whenever they can). To people who are used to using test doubles in this way, this phrase will make absolutely no sense. In a sense, the phrase simply doesn't apply to this usage of test doubles. In another, that usage of test doubles is often harmful and enables tests with insufficiently clear boundaries between what's tested and what's controlled.

## Background of the phrase

The term originates in the [[London-school TDD community|London-school-TDD]], the thrust of which is that test doubles should be primarily used to facilitate TDD to invent focused, usable interfaces between the thing you're testing and the code it will depend on. The primary value a test double offers is to provide a safe, easy-to-change testbed for using that interface (e.g. _will the caller have access to the types it requires and be able to consume the things it returns? Is the method simple to use?_). Because the actual thing doesn't exist yet, if it turns out to be awkward to specify how the subject would interact with it, then it's cheap to throw it away and try again.

So when this phrase is used, it's spoken from the mindset that the primary value of test doubles is **design feedback**, wherein any pain in faking something should be responded to not with ever-more-clever test double libraries, but with a redesign of the interaction between the subject and its dependencies. 

## Mocking what you don't own

Given that background, replacing a third-party object or method with a test double doesn't make much sense. 

* **If faking the third-party dependency is painless:** it may result in a consistent-looking test, but also may represent the leakage of a third-party dependency into the internal of your application (thus mixing levels of abstraction, in a sense).
* **If faking the third-party dependency is painful:** then, even if you're ultimately able to salve that pain with clever test-scoped code, it was still _useless pain_. No good came from the exercise because the design feedback of specifying the interaction wasn't actionable.

## What you should do instead

The prescription implied by "don't mock what you don't own" is to introduce your own shim/wrapper/adapter around it. This effectively cordons off the dependency to a single place in your codebase and contextualizes it in the consistent and easy-to-use style you're trying to promote within your codebase. If there's anything awkward about how one needs to invoke the dependency (maybe a chaining API, multi-step invocation, repetitive default configuration, etc.), it can be swept under the rug into that common adapter.

For instance, if you depend on an HTTP library for which each use looks like:

```js
new HttpRequest('https://long.production.url/1', 
  {method: 'GET', crossOrigin: true, cookie: getCookie()}, function (error, response) {
  // handle request
}).send()
```

That would be really awkward to fake with a test double, and it would be _useless pain_ given that you aren't in a position to change it (short of sending a pull request to the maintainer of the library). Instead you might introduce your own wrapper:

``` js
function get (path, callback) {
  new HttpRequest(
    'https://long.production.url/' + path, 
    {method: 'GET', crossOrigin: true, cookie: getCookie()}, 
    callback
  ).send()
}
```

And then fake that wrapper instead. The above could be faked with [testdouble.js](https://github.com/testdouble/testdouble.js) really easily:

``` js
var get = td.function('.get')
td.when(get('/my/path')).thenCallback(null, 'some response!')

// … the rest of the test
```

There's an additional benefit to wrapping third-party dependencies in your own adapter: they can serve as a specification of what you're using in that dependency. For example, if the library in question provides a large surface area of dozens of functions, and your wrapper only exposes three of them, then you've increased the codebase's flexibility by making it easier to replace that dependency in the future. If an alternative dependency comes along, you know it only needs to do those three things (as opposed to dozens), and you can even see what configuration parameters you may need—all in one place!

Here's some more reading:

* This [post by Eric Smith on the topic](https://8thlight.com/blog/eric-smith/2011/10/27/thats-not-yours.html)
* The testdouble.js docs about the [purpose of test double libraries](https://github.com/testdouble/testdouble.js/blob/master/docs/2-howto-purpose.md)