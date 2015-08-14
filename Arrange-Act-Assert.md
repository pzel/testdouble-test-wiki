"Arrange, Act, Assert" is a testing pattern to describe the natural phases of most software tests. 
* **Arrange** describes whatever setup is needed
* **Act** describes the [[subject]]'s behavior that's under test (and typically only describes a single line needed to invoke that behavior)
* **Assert** describes the verification that the [[subject]]'s behavior had the desired effect by evaluating its return value or measuring a side-effect (with a [[spy]] or [[mock]])

Many people have identified that "Given", "When", and "Then" are more natural English descriptions of the three phrases, and as a result many [BDD](https://en.wikipedia.org/wiki/Behavior-driven_development) tools named their testing keywords accordingly.

What's most important about the concept is that the author of the test is aware that:
* Clearly separating the three phases increases the readability of the test
* Executing the three phrases in lexical order makes the test easier to understand

## Separating the phrases

When mindful of the three phases, it's trivially easy to separate. Some testing frameworks support this separation, and some don't. 

Most [[xUnit]] tools don't have any built-in awareness of the pattern, so it's not uncommon to see xUnit tests separate the phases with significant whitespace such as:

``` java
public void testSomething() {
  when(helloWorld.say()).thenReturn("Something Cool");

  String result = subject.say();

  assertThat(result, is("Something Cool"));
}
```

BDD tools like RSpec support the separation a little more formally through support functions like `before` and `let`:

``` ruby
describe "#say"
  before { when(helloWorld).say { "Something Cool" } } # <- Arrange
  let(:result) { subject.say } # <- Act
  it "proxies whatever HelloWorld says" do
    expect(result).to eq("Something Cool") # <- Assert
  end 
end
```

Some tools, like [RSpec-given](https://github.com/jimweirich/rspec-given), [jasmine-given](https://github.com/searls/jasmine-given), and [mocha-gwt](https://www.npmjs.com/package/mocha-gwt) were designed around emphasizing the three phases:

``` ruby
describe "#say"
  Given { when(helloWorld).say { "Something Cool" } }
  When(:result) { subject.say }
  Then { result == "Something Cool" }
end
```

## Maintaining lexical order

Typically it's natural to write tests in arrange-act-assert order, but some testing utilities make this unhelpfully difficult, in particular [[mock]] objects, which require their expectations be set (an Assert phase action) prior to invocation of the [[subject]] (the Act phase). Because tests with [[mock]] assertions can be awkward to read and understand, [[spies|spy]] are often favored as an alternative.