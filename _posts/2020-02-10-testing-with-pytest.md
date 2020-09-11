# Testing using pytest
The pytest framework seems to simplify testing in python, by providing
simpler assertion, enhanced reports, extenstions via pytest plugins, easier ways
to change and enrich behaviour by implementing the pytest hooks, together with
wide range of community plugins.


## Why did I chose pytest?
Our test framework was based on `unittest` and was reasonably well written with
good number of testcases, primarily for function test of our application. But
we were creating/porting the services to be cloud-native which meant we need to
extend our test scope, together with he framework.

As we were extending the framework, we were constantly facing issues with the
current framework getting complex, inflexible and maintainability isssues. That
was the time, we wanted to find a more suitable framework and we began with the
following requirements,

 * Enhanced reporting
 * Enhanced test discovery and test selection
 * Flexible test execution
    - Stop on first failure
    - Repeatability of tests (pytest repeat plugin)
    - Run the last failed ones, etc
 * Enhanced the log collection
    - Capture log feature
    - Ability to add instrument points/data collection using the pytest hooks
      at appropriate locations

Pytest seems an obvious choice, after browsing over the documentation and got a
feeling of the framework being cool, extensible, flexible and plethora of
community plugins and together with my eagerness to experiment a new tool it
seemed a good choice.

## How did I migrate to pytest?
I didn't want to do a complete rewrite, as pytest was offering support of xunit
style test compability, without much refactoring it was possible to get it up
and run quickly.

The next step was to do some adoption on the existing base class, adding
pytest hooks and adding some fixtures with `autouse` option.
It was not hard, was easier to extend and improve the test framework in a
short span of time.

But problem we were too familiar with the `xunit` style of writing test and
organizing, new fixture concept was not easy to get used to and there were
confusion with the flow of the execution as it was not clear.

Obviously the usage `autouse` fixture were the reason for lack of clarity,
but that is the thing gave us the flexibility to port the testcase to `pytest`
quickly.


## My learning

The basic documentation seems to be fine but the examples are pretty basic and
it couldn't find a good reference project which uses them for more complicated
test without `mock`.

The learning curve is slightly steep and grasping the plugins/hooks idea took
some time with the exercise, but its a worth investment.

Its going be to my choice until something more fanastic comes along and am enjoy
using it other projects.

I intent to add more information, issues, learning that i have/will face with my
usage of this framework as a reference for *myself*, because i can't guarantee
for correctness and quality.
