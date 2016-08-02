```csharp
using Xunit;
using Moq;

namespace Example.Test {
    public class GetsProblemTest {

        private readonly Mock<IGenerate> _generateMock = new Mock<IGenerate>();
        private readonly Mock<ISave> _saveMock = new Mock<ISave>();
        private readonly Mock<IPresent> _presentMock = new Mock<IPresent>();

        [Fact]
        public void Test() {
            var problem = new Problem();
            var savedProblem = new SavedProblem();
            var problemJson = new ProblemJson();
            _generateMock.Setup(m => m.Generate()).Returns(problem);
            _saveMock.Setup(m => m.Save(problem)).Returns(savedProblem);
            _presentMock.Setup(m => m.Present(savedProblem)).Returns(problemJson);
            var subject = new GetsProblem(_generateMock.Object, _saveMock.Object, _presentMock.Object);

            var result = subject.Get();

            Assert.Equal(problemJson, result);
        }
    }
}
```

```csharp
namespace Example {
    public class GetsProblem {
        private IGenerate _generate;
        private ISave _save;
        private IPresent _present;

        public GetsProblem(IGenerate generate, ISave save, IPresent present) {
            _generate = generate;
            _save = save;
            _present = present;
        }

        public ProblemJson Get() {
            return null;
        }
    }
}
```

## Moq

See the [Moq Guide](https://github.com/Moq/moq4/wiki/Quickstart) for more information on mock setup and verification.