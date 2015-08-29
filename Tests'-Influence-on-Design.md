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
