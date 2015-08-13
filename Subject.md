The "Subject" or "Subject Under Test" or even "SUT" is a term used for "the production code being tested by this test". It's not a particularly meaningful or descriptive name, but it's consistent, and it's what the [xUnit Patterns book](http://xunitpatterns.com/SUT.html) defined, so it's the one I suggest using—both in conversation and in naming.

Put simply, I recommend naming the variable of whatever is being tested `subject` in all cases. This yields two immediate benefits:

* Readers of the test familiar with the "subject" meme will be able to identify the thing being tested, even if the test is otherwise long, unwieldy, and hard to navigate
* Extract refactors of code from the subject into new first-class units will be less disruptive, as the identifiers won't need to be changed for the tests to be excised and placed in a new listing

## Support by tools

Some frameworks have even codified `subject` as a built-in feature, like RSpec:

``` ruby
class Foo
  def bar
    "HI"
  end
end

describe Foo
  it "works" do
    expect(subject.bar).to eq("HI")
  end
end
```

Where `subject` is an instance of `Foo` created by RSpec automatically, as if the user had defined `let(:subject) { Foo.new }`. Note that the instantiation of `subject` can be customized by passing a block like `subject { Foo.new(1234) }`.

## Criticism

Interestingly, RSpec's maintainer at the time [wrote that](http://blog.davidchelimsky.net/blog/2012/05/13/spec-smell-explicit-use-of-subject/) he believed using `subject` was a [code smell](https://en.wikipedia.org/wiki/Code_smell), preferring that each test arrive at its own expressive name for the thing being tested.

It's my opinion that, expressive as that approach may be, every name in a system represents an additional concept and an indirection to be mentally unfurled. Using `subject` is an opportunity to slightly reduce the cognitive load required to understand a given test, and since that's of greater concern in most test suites, it's worth the trade-off in expressiveness. As a result, providing a custom name to the subject smells like an instance of [[accidental creativity]].