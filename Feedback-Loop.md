The term "feedback loop" describes the amount of time which elapses between someone doing something and learning whether it had the desired effect. Software developers often use the term as shorthand for "how long my tests take to run", but the concept can be applied to almost any oft-repeated process. 

As it pertains to software, the feedback is normally represented by "it worked and I expected it to work", "it worked and I don't know why it worked", "it didn't work for a reason I understand", and "it didn't work for a reason I didn't understand. Typically, the feedback is needed in order to continue work (or, continue confidently that everything is as-expected). If the feedback received isn't what the developer expected, the duration of the last iteration of the feedback loop could be viewed as waste, as they'll need to step back and correct for the unexpected outcome.

## Minimizing feedback loops

Generally speaking, minimizing feedback loops is seen as a way to optimize productivity. Time spent waiting for feedback is usually considered waste. Moreover, when feedback loops are long, it's easy to lose one's train of thought, become distracted, and lose even more productivity due to context switching before being able to respond to the feedback.

There are two main variables governing feedback loops: duration of the process and the frequency the process is repeated. For the purpose of developer productivity. 

If a test build is very slow, then a developer might look into why it's slow. Sometimes, implementation optimizations emerge (i.e. perhaps database setup is being repeated for every test when it doesn't need to be). Just as often, systemic causes for slowness exist that would require rewriting every test in order to mitigate (i.e. perhaps every test interacts with an app via a browser representing massively [[redundant coverage]] when nearly-as-robust coverage could have been achieved by invoking an API directly).

If a build is slow, it's only natural that most developers will compensate by decreasing the frequency with which they run the tests. If a build takes less than a second, it's trivial to run constantly upon every minor change. Meanwhile, a build takes more than a minute to run, well, there are only 480 minutes in the ideal 8-hour work day, and to run the build for every single change would place an upper bound on the number of decisions a developer can make to 480 per day.

Generally, as test suites get slower, they are run less frequently, and developers tackle larger, more speculative units of work without the benefit of feedback, and risking greater amounts of waste per feedback loop.