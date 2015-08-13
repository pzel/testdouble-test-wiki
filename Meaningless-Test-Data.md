In the interest of minimizing [[accidental creativity]], meaningless test data is a practice by which all test data is made to appear insignificant to the behavior of the [[subject]]. This practice also mitigates the [[realism impulse]] somewhat.

## Unit Tests

It's common to find unit tests in which the data passed to and expected from the [[subject]] appear like it might be special. For example, in:

``` ruby
describe AgeLimiter do
  it "limits ages" do
    expect(subject.limited?(18)).to eq(false)
  end
end
```

Suggests to the reader that the input might be significant in this case, as `18` is a typical majority age and might be considered a boundary case particular to this test. 

However, if it turns out the subject would behave exactly the same if it was passed `1337` as `18`, then this practice would recommend passing in `1337`, because it's far less likely that the reader will spend any time under the presumption that the input was meaningful to the subject under that test case.

## Integration Tests

For integration tests, test data will often need to be semi-realistic in order to pass any validations that a user might encounter. In cases like these, one approach to minimize the meaningfulness of the test data is to start with a consistent meme that might be understood by most of the team.

For example, all the fixtures in an app might be taken from fantastical pop culture references recognizable across the team and which couldn't be mistaken for the meaningful data of actual customers or systems.