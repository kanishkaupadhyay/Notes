# Unit Testing

**Benefits of Automated test**
1. Test your code frequently.
2. Catch bugs before deploying.
3. Deploy with confidence.
4. Refactor with confidence.

**Types of Tests**
1. Unit tests 
   - Tests a unit of an application without its external dependencies.

2. Integration tests
   - Tests the application with external dependencies.

3. End-to-end tests
   - Drives an application through its UI.

**Test Pyramid**
   - E2E < Integration < Unit Tests

The actual ratio between unit, integration and end-to-end tests dependes on your project.

1. Favour unit tests to e2e tests.
2. Cover unit test gaps in integration tests.
3. Use e2e tests sparingly.

**Tools**
- NUnit, MSTest, XUnit 

**Install NUnit Package**  
1. Open NuGet package manager console
2. install-package NUnit -Version 3.8.1
3. install-package NUnit3TestAdapter -Version 3.8.0

**MSUnit decorators**
```
[TestClass]
[TestMethod]
```
**NUnit decorators**
```
[TestFixture]
[Test] 
```
**Test Driven Development**
1. With test driven development, you write your tests before writing the production code

**Process**
1. Write a failing test
2. Write the simplest cod eto make the test pass
3. Refactor the code

**Benefits**
1. Testable Source Code
2. Full Coverage by tests
3. Simpler Implementation

**Characteristics of a good unit test**
1. Simple and precise
2. Should not have logic ins the test like if(){}, else, foreach() {} etc.
3. Isolated 
4. Not be too specific/general

**What to test and what not to test?**
- Only test for your code's return value

**Naming and Organising tests**
```
DemoProject
    - DemoProject.UnitTests
        - DemoClassTests
            - DemoMethod_Scenario_ExpectedBehaviour()
```
**Code coverage tool**
- A code coverage tool scans the code and tell what part of the code are not tested. 
- e.g. Visual Studio Enterprise Edition or ReSharper Ultimate.


**Injecting a Dependency as a method parameter**
**Injecting a dependency using a property**
**Injecting dependencies using a constructor**

**Dependency Injection Frameworks**
In real enterprise app, we use a framework

- NInject
- StructureMap
- Spring.NET
- Autofac
- Unity

**Mocking Frameworks**
Dynamically create fake/mock objects

- Moq
- NSubstitute
- FakeItEasy
