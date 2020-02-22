# Testing using pytest
The pytest framework seems to simplify testing in python, by providing
simpler assertion, enhanced reports, extenstion via pytest plugins, easier ways 
to change and enrich behaviour by implementing the pytest hooks, together with 
wide range of community plugins.


## Why did I chose pytest?

Our testframework was based on `unittest` and was reasonably well written with
good number of testcases, primarily for function test of our application and But
we were creating/porting the services to be cloud-native which meant we need to
extend our test scope, together with he framework.

As we were extending the framwework, we were constantly facing issues with the current
framework getting complex, inflexible and maintainability isssues. That was the time,
we wanted to find a more suitable framework and we began with the following requirements,

 * Enhanced reporting
 * Enhanced test discovery and test selection
 * Flexible test execution
    - Stop at first failure
    - Repeatability of tests ( pytest repeat plugin)
    - Run the last failed ones, etc
 * Enhanced the log collection
    - Capture log feature
    - Ability to add instrument points/data collection using the pytest hooks at
      appropriate locations


Initially I was browsing over the documentation and got a feeling of the framework
being cool, extensible, flexible and together with a plethora of community plugins
it seemed to be an obvious choice. Also I always enjoy experimenting and learning
new stuff, so it was easier to pick it up.

## How did I migrate to pytest?

I didn't want a complete rewrite, as pytest was offering support of xunit style
test compability, without much refactoring it was possible to get the things to
run quickly. Then i had to adopt some testbase class, added some pytest hooks
and some fixtures with autouse option. It was not hard and felt very nice as it
was easier to extend and improve the test framework easily.

But problem was most of us were not familar with the concept of fixture, together with
some `autouse` fixture and the hooks it felt unnatural as the flow is not something easier
to follow. I was arguing that it due to lack of understanding of the framework, and on the
other why should this be complicated.

The basic documentation seems to be fine but the examples are pretty basic and
it couldn't find a good reference project. I can't agree more, but its a powerful, flexible
framework. The learning curve is slightly steep and grasping the plugins/hooks idea
took some time with the exercise, but its a worth investment.
Its going be to my choice until something more fanastic comes along and am enjoy using
it other projects.
