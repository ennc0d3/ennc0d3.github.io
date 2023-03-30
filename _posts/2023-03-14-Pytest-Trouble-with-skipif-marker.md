# Trouble with pytest.mark.skipif mark decorator

The description on pytest skipif [documentation](https://docs.pytest.org/en/7.1.x/reference/reference.html#pytest-mark-skipif) states this,

```
pytest.mark.skipif(condition, *, reason=None)
Parameters
condition (bool or str) – True/False if the condition should be skipped or a condition string.

reason (str) – Reason why the test function is being skipped.
```
This expects a boolean or str value, here we can ignore the `str` condition as they are not
typically used now, because it uses `eval` to evaluate the str condition it can be useful
and risky at the same time.

What it means by bool? I understand this must an expression that returns a `bool` at the
evaluation of skipif marker.

When is the skipif marker evaluated?
This happens during the collection phase, i.e, almost close to import time and at this time
it doesn't have access to fixtures or we can say the fixtures are evaluated in later stage which
means we can't use pytest arguments to turn off the tests.

For example,
conftest.py
```
import pytest
import os

def pytest_addoption(parser):
    parser.addoption(
        "--skip-slow",
        action="store_true",
        default=False,
        help="Skip slow tests"
    )

@pytest.fixture(scope='session', autouse=True)
def skip(request):
    if request.config.getoption('--skip-slow'):
        os.environ['SKIP_SLOW'] = "1"
        print("Skip slow  is set to True")
        return
    os.environ['SKIP_SLOW'] = "0"
```

and the a test module,
```
import pytest
import os

def is_skip_slow_set():
    print("Checking for skip slow")
    value = os.environ.get('SKIP_SLOW') == "1"
    print(f"Checking for skip slow = {value}")

# Wont work, is_skip_slow_set() condition evaluates at collection and at
# that point os.environ is not set.
@pytest.mark.skipif(is_skip_slow_set(), reason="--skip-slow is set in args")
def test_skip_if_slow_set_in_pytest_args():
    assert True

def test_quick():
    # some slow test code here
    assert True
```

Here the test wont be skipped even if we invoke with **--skip-slow** argument,
```
pytest --skip-slow -rs -v test-skipif.py
```
The reason **is is_skip_slow_set** is evaluated at collection time and the session
scoped fixture even the ones with autouse are not executed resulting in **SKIP_SLOW**
not being set and finally the condition is set to **False** therefore the test is
not skipped as i wanted.

Now, if i use skipif marker like the one below,
```
@pytest.mark.skipif(is_skip_slow_set, reason="--skip-slow is set in args")
```
Will it skip? This wouldn't call **is_skip_slow_set**, why? because it is the reference to
reference to the function which is pythonic **True** so the test would be skipped
independent of the **--skip-slow** flag with is incorrect and confusing. Beware of this
pitfall.

Lesson is that, skipif needs an bool condition and that is evaluated at the import/collection
time only way is to use the **is_skip_slow_set()** which is also limiting as we can't
use the values from fixtures like *Config*. For this to work as intended the the environment
variable must to set outside the pytest invocation which defeats the purpose of the pytest
argument.

The cleaner solution is to use of the **pytest_collection_modifyitems**,

The corrected conftest.py would be,
```
import pytest
import os

def pytest_addoption(parser):
    parser.addoption(
        "--skip-slow",
        action="store_true",
        default=False,
        help="Skip slow tests"
    )

def pytest_collection_modifyitems(config, items):
    for item in items:
        if "slow" in item.keywords and config.getoption("--skip-slow"):
            item.add_marker(pytest.mark.skip(reason="skipping slow test run"))

```
And the test module with skip containing slow marker,
```
import pytest
import os

@pytest.mark.slow
def test_skip_if_slow_set_in_pytest_args():
    assert True

def test_quick():
    # some slow test code here
    assert True
```

and it is clean and works.



