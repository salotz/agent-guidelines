# Software Guidelines

These are guidelines specific to the authorship and maintenance of software projects.

## Generic

Requirements for generic coding tasks.

### Code Tags

Follow [salotz RFC 6](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.023_local-agent-context) ([summary](./summaries/salotz-rfc-023-local-agent-context.md)) when writing comments in source code.

### Git Commit Messages

Use the advice in this [blog post](https://chris.beams.io/git-commit) ([summary](./summaries/git-commit-messages-chris-beams.md))

### Git Branching Strategies

Unless otherwise specified in a project you should always assume that the repository follows the [trunk based development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development) ([summary](./summaries/trunk-based-development-atlassian.md)) pattern with short lived feature branches.

There should be no merges and instead use rebasing heavily.

Treat writing git commits like "layers" rather than simply a stream of consciousness.

While not required it is nice to rebase commits into logical chunks of understanding.

This aids in code review for multi-step changes.


### Test Driven Development (TDD)

Use test driven development (TDD) for software projects.

You should not write your own tests, as this is one of the main ways in which the operator will know that agents have done the job right.

The operator may delegate test writing to agents after defining the specifics of tests, in which case agents can write tests.

Agents should let the operator know that you need tests to progress in your work, rather than attempting the work without them.

Code should be written with an emphasis on testability of components, with good factoring to allow for fine grained units tests.

### Test Organization

There are multiple kinds of tests that should be created for software projects. Not all are necessary for all projects.

They are:

- unit: Tests for each individual software component
- functional: Tests code for specific unit correctness behaviors. For instance desired mathematical precision of a function.
- external: Tests behavior of outside systems. Typically for behavior that is depended on in your system.
- build: Tests of generated artifacts
- performance: Performance benchmarks
- end-to-end smoke: Simple tests of limited number that intend to probe whether an integration system is functional in a rough sense.
- regression: Tests maintained to protect against previously regressive behavior.
- preflight: Tests which run during integration system set up. For instance server readiness.
- integration: Tests of a system against a realistic test environment (the "integration system") without mocked I/O
- acceptance: Tests systems for specific correctness behavior. For instance canonicalization algorithms work when round-tripping to an server.

Only unit tests and functional tests can be colocated with code, the rest should live in separate testing directories.

In a project directory the tests directory should be named `tests`.

All subcategories of tests should be placed in the `tests` directory.

#### Unit Tests

The unit tests should be called `unit`.

Unit test requirements are:

- should be able to run in any context with isolated environment control.
- should not utilize any outside or parallel managed service, such as a local web server or database.
- should be able to all run in parallel in a reasonably short amount of time.
- should not use any resources not also part of the repository.
  - Staging of input data is an acceptable pre-requisite for unit tests however, but clear instructions for obtaining the data and tagging tests with this requirement is required.

Unit tests should be written such that there is a one-to-one mapping between a function and the test.


For example for the function in the file `src/module/things.py`:

```python
def foo():
    pass
```

The test should be either in the same module directory or in a separate test directory that mirrors the project structure `tests/module/test_things.py` with the contents:

```python
def test_foo():
    ....
```

The choice of inline or separate tests is a decision made by the operator on a project by project basis and other advice in this guide.

If you have multiple test functions for the same function you should add distinguishers to the test function name.

```python
def test_foo():
    ...
def test_foo_alt_behavior():
    """Test specific alternative behavior."""
    ...
```

If supported in your language details of what is under test is necessary for additional tests. This is not required for the original function.

#### Functional Tests

Functional tests directory is called `functional`.

Functional tests are similar to unit tests.

However, unit tests can and should only test whether the behavior of a function is not erroneous from the point of view of the software system.

Functional tests on the other hand test whether the behavior of units or systems of units produce results which are correct from some other point of view.

For instance suppose you have a function that estimates the volume of a shape:

```python
def area_of(shape: Shape) -> float:
    ...
```

The unit test would test whether the behavior of the code is correct e.g.:

- Does it raise unhandled errors?
- Does it return the correct type?
- Does it return an area unit value?
- Does it raise the expected errors?

In this case the are produced is an estimate and you may have separate
requirements on the accuracy of the answer. The unit test should not
test this, the functional test should.

Functional tests should be written to prefer testing of specific functions, but often final behaviors are dependent on multiple functions in which case they can be written to test multiple units together.

#### External Tests

External tests directory is called `external`.

Ideally all software is tested thoroughly and makes strong guarantees on its behavior and interfaces.

However, this is not always true. In this case authors should write their own tests against external software and services that assert behavior or correctness that they depend on.

In this way if behavior of dependencies change you catch this directly rather than obtusely through your other tests.

Not all external behavior needs to be tested. Only critical behaviors, undocumented behaviors, or relied upon internals.

There is no format to these tests except keeping them in their directories and organizing them by dependencies.

E.g. `tests/external/test_numpy.py` or `tests/external/test_numpy/test_integrate.py`

#### Build Tests

Build tests directory is called `build`.

Software projects create builds, packages, executables, containers, and many other kinds of artifacts.

These builds themselves can be erroneous. A common error is leaving out some files from a container or package tarball.

Build tests should run simple "smoke" tests on build artifacts to ensure that they were built correctly.

If possible the full test suites should be run on artifacts, but this is not possible in all cases as not all artifacts can bundle tests with them.

Build tests should be organized in their own directory with subdirectories for each artifact type (e.g. `package`, `executable`, `container`, etc.).

Each specific artifact of each type should have its own test file e.g. `tests/container/test_prod.py`.

#### Performance Tests

Performance tests directory is called `perf`.

When performance characteristics are of importance to a project you should keep tests which exercise and benchmark the performance characteristics.

These should be runnable in CI runners for tests on different kinds of hosts.

You may set alerts if performance parameters reach certain levels or simply log results into the repo to track improvements or regressions over time.


#### Regression Tests

Regression tests directory is called `regressions`.


#### Integration Tests

Integration tests are a class of tests which require an integration system.

Instantiations of the integration system is in different integration environments. Each environment may have different properties you want to test.

For example you may want to test against multiple database backends in different environments.

As such there is no specific set of tests that are only "integration tests". All integration tests are of the following types.

Integration tests do not have a specific directory as each subtype has a directory.

Integration systems have a wide variety of requirements and are not detailed here. Some examples are:

- database and API server running in a cluster
- sandboxed host system for testing CLIs
- a SLURM cluster for testing

##### End-to-End (E2E) Smoke Tests

E2E smoke tests directory is called `e2e`.

A smoke test is a simple test that doesn't test any specific behavior and only checks that the system can get up and running without error.

An end-to-end test is an integration test which attempts to test all aspects of a system. Testing all behaviors of complex systems is impossible and should be tested in units.

However, you may still want to get some indication of a system working with tests. Hence the smoke test.


##### User Story Tests (E2E)

E2E user story tests directory is called `stories`.

This is a test that takes a particular user story for behavior or feature of the system and runs it as a test. As such it requires the entire system in an integration environment.

These tests should not be considered comprehensive and they may be removed over time.

These are essentially acceptance tests over the integration system.

##### Acceptance Tests

Acceptance tests directory is called `acceptance`.


Similar to a functional test, acceptance tests test for correctness.

This is some internal terminology for the sake of distinguishing them.

##### Preflight Tests

Preflight tests directory is called `preflight`.

Preflight tests are used to check if the integration system is ready for real testing or load.

Preflight tests run during set up of the integration environment and may have different sets of tests run at different phaes of system bring-up.

Preflight tests are also typically run when setting up production environments.

### Unit Test Coverage

Code bases should have unit tests for all functions. We call this "unit coverage".

Within each unit test there is a goal of 100% branch coverage.

However, in some languages (like Python) this is not practical because of highly dynamic nature. In practice then other strategies for cutting down on valid function behaviors should be used like type annotations, type checking, and property testing (like Hypothesis).

Unit tests for projects using these add-on methods should focus then on only the possible behaviors given the guarantees those system provide.

For instance if you have a generic function:

```python
def foo(a):
    return a + 1
```

The unit test would need to test all possible types going into the function. Do not do this.

Instead provide types:

```python
def foo(a: int | float | Decimal) -> int | float | Decimal:
    return a + 1
```

And only test the specific input and output types provided.

If possible you should write test system utilities that report the current unit coverage to be used in QA.

In Python, for example, is [pytest-checklist](https://github.com/examol-corp/pytest-checklist). Other solutions are acceptable.

Branch coverage tools like [coverage](https://coverage.readthedocs.io/en/7.15.4/) can be used as a quality metric but should not be used as a gating mechanism for acceptance.

If 100% unit coverage is maintained then minimal integration tests are needed.
