Here's an example [[Collaboration Test]] in Java written in JUnit & [Mockito](http://mockito.org). It takes advantage of Mockito's custom JUnit runner, which allows it to automatically replace the [[subject]]'s dependencies via [field injection](http://site.mockito.org/mockito/docs/current/org/mockito/InjectMocks.html).

``` java
package math;

import static org.hamcrest.Matchers.*;
import static org.junit.Assert.*;
import static org.mockito.Mockito.*;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

import math.values.Problem;
import math.values.ProblemJson;
import math.values.SavedProblem;

@RunWith(MockitoJUnitRunner.class)
public class GetsProblemTest {

	@InjectMocks
	GetsProblem subject;

	@Mock
	Generate generate;

	@Mock
	Save save;

	@Mock
	Present present;

	@Test
	public void test() {
		Problem problem = new Problem();
		SavedProblem savedProblem = new SavedProblem();
		ProblemJson someProblemJson = new ProblemJson();
		when(generate.generate()).thenReturn(problem);
		when(save.save(problem)).thenReturn(savedProblem);
		when(present.present(savedProblem)).thenReturn(someProblemJson);

		ProblemJson result = subject.get();

		assertThat(result, is(someProblemJson));
	}
}
```

And here's the implemented subject:

``` java
package math;

import math.values.Problem;
import math.values.ProblemJson;
import math.values.SavedProblem;

public class GetsProblem {

	Generate generate = new Generate();
	Save save = new Save();
	Present present = new Present();

	public ProblemJson get() {
		Problem problem = generate.generate();
		SavedProblem savedProblem = save.save(problem);
		return present.present(savedProblem);
	}

}
```