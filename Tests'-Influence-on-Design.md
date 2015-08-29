Many people will sometimes insist on saying "TDD" stands for "Test-Driven-*Design*" as opposed to merely "Development", because the perceived positive influence on the design of production code is for many a primary reason to bother with TDD at all.

There are several ways that TDD impacts design, but most of it can be be broken up into two categories: increased usage and  increased coupling.

## Increased Usage

This one's easy, and its benefits are evident in almost every type of test (TDD or not). 

### What increased usage means

In the absence of any automated testing that invokes a particular bit of code, most code will only ever have a *single invocation point* in a production system. This is a two-edged sword, because if the code being called isn't particularly robust—but works for that single caller using it—then no time was wasted ensuring the code would be unnecessarily robust under a variety of inputs and situations. However, in large and complex systems, having a function that's only ever been called under one context is often a liability: maybe it's not working as well as it seems to be, perhaps another developer will naively reuse it later in a context the code doesn't work as advertised, or maybe the code is successfully later reused in dozens of places but its initial API was awkward to use and that awkwardness spreads throughout the system as a sort of technical debt.

When code has a test, then that means it has at least twice as many usages as the average piece of code has, so that's a 2x improvement! Moreover, when the code was written with some school of TDD, then the code may not only have many callers, those callers may successfully exercise every conceivable context and set of inputs the code would ever be under. This is certainly more work up front, and it may improve the long-term durability of the system to do what people expect it to do, but it can have an additional benefit of leading the developers' to arrive at more *usable* designs for the code in the first place.

### Example

For instance, suppose I start with a Ruby method that takes an options hash:

``` ruby
def words_of_the_day(options)
  options[:words].select {|word| word.start_with?(options[:letter_of_the_day]) }
end
```

Perhaps the original author expected the number of options to be needed by the method to increase in the future, or perhaps they'd just been in the habit of defaulting to option-hash-style arguments lately because they're convenient when you have all the important keys in your head. Maybe the developer and others on his team would argue about whether this method was more usable than a two-argument version, `words_of_the_day(words, letter_of_the_day)`, but without the experience of using the method in a bunch of different contexts, it's unlikely either side would convert the other.

However, if that method had four or five test cases written against it like this:

``` ruby
def test_words_of_the_day
  words = ["banana", "apple"]
  letter_of_the_day = "b"

  results = @subject.words_of_the_day(:words => words, :letter_of_the_day => letter_of_the_day)

  assert_equal ["banana"], results
end
```

After a while, the test author would probably be annoyed with the long lines needed to invoke the method without breaking a new line. They'd also probably be tired of mistyping "letter_of_the_day", too. In response to the actual pain of using this method five times, the developer might decide to change its API to a two-argument method (and maybe rename `letter_of_the_day` while they were at it).

This is an (albeit, contrived) example of the benefit of having code that has been used a lot before its API has seeped throughout a system is that it's still quite cheap to improve the API before it escapes into use by other parts of an application.

## Increased Coupling

> Woah, increased coupling is always a bad thing, right!?
> 
> **—Everyone**

Coupling is not a four-letter-word. Any time you invoke a piece of code, you're coupling it to the caller—from that point forward, the called method can't change without considering the impact on the caller. All use is coupling. All reuse is additional coupling. A system with minimal coupling is a system without any abstraction or invocation. Deciding whether and how to couple two pieces of code is a fine art in software development, and as such, it's a point of never-ending tension as systems are designed and redesigned.

Simply by using the [[subject]] code under test, a test becomes coupled to that subject's public API and its behavior, if not the implementation itself. If a test uses a subject in a variety of contexts (e.g. testing error states or unconventional inputs), then there may be logical coupling between the test and the source that serve absolutely no benefit to the production system at all. (In fact, most tests I see against unexpected inputs produce added complexity in production code but are actually _unreachable_ in the production system, because any number of methods higher in the call stack will have already normalized the same inputs.)

So, if all test code creates (at least some sorts of) coupling to its [[subject], then how should we manage it? And what, after all, does this have to do with improving the code's design? To understand this, we'll work outside in with some examples.

### Minimally-coupled tests

A test that's minimally coupled to its [[subject]] will exercise it from a separate process, without knowledge of any of its names (e.g. class names, function names, even CSS selectors). A minimally-coupled test also can't know any details about how the production system manages its data, so it typically can't control its test data in a clean way. 

[Aside: because of how inconvenient it is to the developer, these sorts tests, whether you call them Full-Stack, End-to-End, Smoke, Feature, Acceptance (I just call them [[SAFE tests]] lately) almost always make concessions that introduce coupling to the system. Most Rails tests, for instance, will make data setup easier by maintaining access to the application's models, which requires the tests to run in-process and couples the tests to individual model names and methods.]

The benefit of minimizing coupling is clear: the implementation can be changed dramatically (Rails could be replaced by Phoenix; Angular could be replaced by Ember) and the tests could still provide regression safety without needing to be changed substantially. If all you value is catching bugs with an automated test suite before they reach production, you might opt for a broad suite of tests like this, because their only focus is on the extrinsic behavior of the system and they won't (if designed well) encounter churn due to course-of-business implementation changes.

Their are, of course, numerous downsides to leaning heavily on these sorts of tests, but when speaking specifically to the impact that tests can have on design, consider this: by having no coupling whatsoever to the implementation, writing a bunch of these tests provides no insight into how well the code is factored. The most these tests can tell you is how accessible the application is, since UI automation tools tend to share a lot of DNA with accessibility tools.

### Coupling to the Public API

Say your application provides a public API, or that there is a clear seam just "below" the UI by which you can invoke the top level of actual classes and methods easily. If you test at this layer, you'll only be marginally more coupled to the implementation (if those top-level units change their interface, the tests will break). 

However, no matter how many tests you write against this layer to drive out the extrinsic behavior, the feedback you receive about the design of the code will stop at the public API. Are the [[subject]]'s methods delegating to well-named abstractions that break up the problem in a logical way? Hopefully! But if they are, it's not because the test was influencing it.

I wrote the first iteration of [gimme](https://github.com/searls/gimme) driven nothing but top-level API examples specified by Cucumber. I was able to get through two-thirds of my desired features this way without issue. However, the robust test coverage gave me false confidence in my design, and when it came time to implement some next feature, I realized I couldn't without rewriting the whole thing. I'd created a beautiful public API as a thin veil over a rat's nest of half-baked abstractions and long methods. The benefit, though, was that even though I had to rewrite it, I at least could rely on those tests to tell me I hadn't inadvertently missed a requirement.

### Coupling to each "Unit"

[Note that the definitions to "unit" and "public API" are rarely rigorously defined consistently on a project.]

Many people who practice [[Detroit-school TDD]] will expect to write for every unit `Foo` in their system a unit test named `FooTest` that's symmetrically positioned under some `test/` directory for easy organization. These tests typically invoke the public API of the [[subject]] and call through to its actual dependencies (doing everything they can to minimize use of [[test doubles|test double]], preferring to test each unit under realistic conditions for maximum regression safety).

In this way, these tests aren't coupled to the "implementation" of any given unit, but they are almost certainly coupled to units that are only used internally by an application.

To illustrate the point, suppose a URL router invokes `FooController` when a user requests `/foos`, and `FooController` invokes a `FooRepository` to get some data. If we wrote a test of `FooRepository`, that test might only be coupled to the "public" behavior of `FooRepository`, but then again, `FooRepository` isn't very _public_ if it's only invoked by another object internal to the implementation of the application, `FooController`. In this way, discussing whether a test is "coupled to the implementation" is a sort of shell game, enabled by the very natural practice of introducing first-class abstractions in a system. Is `FooRepositoryTest` coupled to `FooRepository`'s implementation? Perhaps not. Is `FooRepositoryTest` coupled to the implementation of the broader app? You bet it is.