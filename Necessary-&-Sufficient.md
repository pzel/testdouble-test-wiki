Jim Weirich used this phrase to describe tests that were both complete specifications of their [[subject]] and contained nothing superfluous that would tie the hands of the implementation unnecessarily. It sounds obvious in hindsight, but is a handy concept to keep in mind when evaluating the quality of a test.

## Necessary

Only test the behaviors you know you need to care about. For example, if the desired behavior of a particular edge case doesn't truly matter yet or isn't fully understood, don't write a test for it yet. Doing so would restrict the freedom to refactor the implementation. Additionally, it will send the signal to future readers that this behavior is actually critical, when it very well might not be (perhaps a form of [[accidental creativity]].

## Sufficient

Each test should ensure all the behaviors the author wants to ensure about the subject. 

Examples of **insufficient tests** follow

### Insufficient assertions

``` ruby
def add(a,b)
  a + b
end

describe '#add' do
  When(:result) { subject.add(5, 3) }
  Then { expect(result).not_to be_nil }
end
```

The above will indeed test something gets returned, but not that adding is occurring.

### Liberal stubs

Here, for an [[isolation test]]:

``` ruby
describe '#call' do
  Given(:dependency1) { double.stub(:foo) { :value1 } }
  Given(:dependency2) { double.stub(:bar) { :value2 } }
  Given(:dependency3) { double.stub(:baz) { :value3 } }
  When(:result) { subject.call(dependency1, dependency2, dependency3) }
  Then { expect(result).to eq(:value3) }
end
```

Is almost certainly insufficient, because the stubs will unconditionally return their values instead of matching on their inputs. That means an implementation of `call` of simply `return dependency3.baz` will pass, even though `dependency1` and `dependency2` were never invoked. A sufficient test would instead use stubs that were conditional upon the inputs of the prior dependency in the chain:

``` ruby
describe '#call' do
  Given(:dependency1) { double.stub(:foo) { :value1 } }
  Given(:dependency2) { double.stub(:bar).with(:value1) { :value2 } }
  Given(:dependency3) { double.stub(:baz).with(:value2) { :value3 } }
  When(:result) { subject.call(dependency1, dependency2, dependency3) }
  Then { expect(result).to eq(:value3) }
end
```

Is sufficient, because it will require the implementation the author probably intended of `dependency3.baz(dependency2.bar(dependency1.foo))`.