# Basics of software testing

## Introduction

A few decades ago the way developers tested their code was that they wrote a new piece of code, and manually tried whether it does what it should do. If yes, they were happy and integrated it to their software. So the software only contained tested and well functioning code. Right?

Well... not quite.

There are multiple problems with it:

* **Software is complex** - if you change something in your code, it will cause errors in totally other places - and multiple places in unexpected ways. So if you add code, you need to retest the other parts as well, which worked before, to make sure your new change did not break them. It means that all pieces of code need to be tested again and again.
* **Code is never done** - if you finish a piece of code, you know in the future you will extend, change, fix it. You will refactor it, find bugs, and the requirements will change. So it's not true that you test something once and it will work together.
* **No one knows what you did** - if you test something casually and manually, you will not document what requirements you validated. Did you check the edge cases? The happy and failing paths? The security issues? What are the exact requirements?

We could continue this list, but the point is that software testing became a separate profession with its own vocabulary, tools and best practices. This document tries to explain the basic concepts of software testing.

### What is testing

Testing for short is checking software against requirements.

You can consider writing tests as writing down the requirements in the language of the code. Like instead of having them in English in a doc, you have them in C++ (or whatever language you use), so the machine can check whether your code fulfills it or not.

So you write your own code (the *production code*) and parallelly you write your test code in the same language, which is just additional software. This test code uses your production code and decides whether it does what it should do. How exactly? This document is all about that.

Every test must be a requirement you should be able to explain it to someone in a human language as well.

There is a pretty boring tutorial generally about testing in case you want to read more details: [Software Testing Tutorial](https://www.tutorialspoint.com/software_testing/index.htm)

### No exhaustive testing

**Exhaustive testing** means *all* mathematically possible input combinations are tested, and *all* possible potential bugs are revealed. There is a theorem in [ISTQB](https://www.istqb.org/) (International Software Testing Qualifications Board) material which says that such an exhaustive test can *never* exist. This is not a mathematical theorem, it's more like a rule of thumb, but it is a really strong statement, and everyone who does software testing should be aware of the fact that exhaustive testing is practically impossible.

TODO example

### Why is testing important

Testing can slow down development process, therefore it is sometimes can be viewed as time-consuming and expensive activity.

But the experience shows that the price (even in time or in money or in complexity) of finding and fixing a bug grows exponentially during the life cycle of the product. This means that a bug costs the most, when found by the customer in the already released product, and it is significantly cheaper to fix it during testing phase. But it is even cheaper (waaay lot cheaper) if we can catch it during development time. To be short: the earlier we catch the problem, the better. And there is a huuuge difference.[1][2][3]

TODO safety net and part form wewlc and fear of making changes and what should be unchanged. Explain behaviour vs implementation

### Cost to Find and Fix Issues

So if you think that you can win an hour, a day, or even a few days by not testing something, keep in mind that it can cause weeks for you or someone else later.

And as we said, we will probably not find all of the bugs, we have to find as many as possible, and as soon as possible.

![Software testing ](https://martinfowler.com/articles/is-quality-worth-cost/both.png).

The above picture is from Martin Fowler's article [Is High Quality Software Worth the Cost?](https://martinfowler.com/articles/is-quality-worth-cost.html). It shows that if you don't test your code you start faster. In the beginning you create more functionality under a specified time. But later, when your code becomes more complex you will spend much more time by finding and fixing bugs then by actual development.

If you test your code well, you will start slower (because you invest time in writing tests as well, not only the production code), but later you won't slow down that much. After a time this will return, so it's worth to write tests, although it seems slower at the beginning. How fast it will return? According to Fowler (and I gighly agree with him) as fast as in a few weeks. So writing tests is actually not that long term investment.

The same applies to other quality practices, for example Clean Code.

It's only worth to skip testing if your software is developed for weeks only and is used only a few times. (Like to quickly calculate something.) These are called the quick-and-dirty programs.

### Tested is not equal to good

Although testing is extremely important, you should always know that no testing method is omnipotent. Even if you write test for every requirement, and your code fulfills them, it doesn't mean that your code is *good*.

* Keep in mind that probably you wrote the tests for the code which is also written by you. (You test your own code.) Maybe you misunderstood the requirements and requirements should have been written in a different way.
* Maybe the customer or whoever ordered the software or wrote the specification was unsure or wrong. 
* Maybe your code fulfills the requirements, but is unusable by the user, or unreadable and unextendable by other developers.

Different testing practices along with comprehensive review should be applied together, and even if code passes all, common sense must be used.

### Other things

A lot of other stuff can find - if you want - at [ISTQB Glossary](https://glossary.istqb.org/en/search/), like what is the definition of a bug, and a fault, and an incident, and what is the difference between them. And a lot more...

## Testing types

Testing is a whole separate profession, and there are a lot of different types of tests, and there are different approaches to categorize them. We tried to shortly collect and introduce the most important terms.

### Manual testing

This is when - what a surprise - the tester or the developer tests the system without the help of any automation tools. Usually this happens, when there is no automation framework, or when the situation is so unique that there is no standard way of testing it, like a very new feature or a mysterious bug. This usually does not happen regularly, it is one-time or few-times event.

The drawback of it is that it's really slow and expensive. The advantage is that it's much more flexible, and able to find tricky bugs as well. It is also closer to the end user's behavior.

### Automatic testing

Automatic tests are run by scripts, usually scheduled, triggered by time (e.g. daily) or an event (like commit or merge to a branch). Of course it needs more software, so the work of professional testers is much more similar to the developers.

### CI - Continuous Integration

Automatic testing makes the process faster. But this is not the only advantage. A common best practice is to shorten the development phases. For example if you develop a feature for half a year and then merge the whole thing all at once to the master branch, a lot of issues can emerge. If you release a big change to the customers, a lot of business and technicl assumptions are verified at once.

But if you develop little pieces of features and merge and release them more often (often daily of multiple times a day) you have much less opportunity to make mistakes and you also have a much faster feedback loop (about your test results and also about what the customers think). But this approach needs very good automated testing machinery, which runs tests, static checkers (formatting, linting, memory leak protections, etc.) fast and automated. The goal is to continuously integrate the newly developed features to the product. That's why this practice is called Continuous Integration.

This ensures that bugs can be caught early, and in case there is a problem, only a small part of code is suspected. The fixes will be cheaper and faster, and much less code will exist untested "under the radar" at any point of time.

This approach also goes well with the agile principles, like scrum, which says you should rather do a little of everything continuously instead of having long periods of only one thing.

CI helped a lot of companies to change business model, and it made it possible to have frequent updates on your mobile apps, and also to view software as a continuous living flow and not some monolits released in every two years.

### Black-Box vs. White-Box testing

#### Black-Box testing

Black-Box testing is when you don't know or don't care about the internal parts of the tested code. Technically saying, you test against the interface. This is good, because you test from the user's perspective, and it forces you to test the actual requirements and not how you imagine your code should work.

Of course, technically it is more complicated, because you have limited access to the code, and sometimes it is important or reasonable to see some detail of a piece of code.

An advantage of Black-Box testing is that you have the freedom to change the implementation details without ruining the already written testcases.

#### White-Box testing

You have full knowledge and access to the implementation details of the code. It gives you more control, and less worries about tricky test implementations. It can lead to too strict tests though, where you set requirements against implementation details, which could have been written any other way. In this case, you have less freedom to change the code.

DOTO explanation kepfeltoltes

### Grey-Box testing

Mixture of the two, when you have some knowledge (access) to the code in the test, but not everything.

## Testing levels

There are a lot of different levels (or types?) of tests. Developers meet only with unit tests and module tests 99% of the time.

### Unit testing

The most important test type for a developer. Unit testing[4] is when you test the smallest testable part (the unit) of the software. A unit does not have a strict definition. Usually this is a function, method, or sometimes a class. Since a unit should be the undividable "atom" of the code, no interactions are tested in unit tests. The purpose of the unit test is to ensure every building block is working as intended.

Although it is not set in stone, unit tests are the only tests, which are always written by the developer, simultaneously with the production code.

A unit test should

* Be as simple as possible, preferably not more than a few lines of code
* test only 1 well defined functionality - should have only 1 reason to fail
* be fast. There will be hundreds of unit tests, and in good case, you will run them very frequently, even in every few minutes. All tests should run in a few seconds.
* phrase a piece of requirement. If someone unfamiliar with the code reads through the unit tests (or preferably only the names of the tests) should understand the purpose of the tested unit

Note2: if you commit code, it should always be submitted together with the unit tests. They should never be separated or handled independently, and you don't have to emphasize for a commit that it also includes unit tests, because that is always mandatory part anyway.

TODO add "test is not unittest if" from the book

#### The F.I.R.S.T. principles

This abbreviation they often mention describes how unit test should be:

* **Fast** - it has to encourage the developer to run all the tests as often as possible, preferably after every few lines of code change, or every few minutes. Therefore they have to run in a few seconds.
* **Isolated/Independent** - the result of a test mustn't change the result of another test. Also their order shouldn't matter. If you need to run a test first in order to make another one pass, you are doing it wrong. All of them should set up and use their own variables.
* **Repeatable** - they must be deterministic. If a test passes/fails, it has to give the same result if you run it again.
* **Self-validating** - every test must have a true/false result. If you need to read through logs or manually check the result and decide whether it passes or fails, you are doing it wrong. Testing frameworks help with it and can understand what the results of a test is.
* **Thorough** - should cover all happy paths, edge cases (at the border of good/bad input), illegal arguments, security issues, too large values, etc.

#### Unit testing frameworks

There are either built in or 3rd party frameworks for practically all languages, such as Jtest for Java, unittest for Python. C++ has limited built in functionalities to help unit testing, but there is an open source 3rd party framework provided by Google, called Google Test[5]. PHP uses PHPUnit. Golang has a builtin unit testing framework which can be extended by libraries to be more useful.

These frameworks help to write tests easier, also automatically identify and group unit test, run them and evaluate their result and give feedback. In some less advanced frameworks you have a separate file where you list all your tests. But with some frameworks you don't need to do that. If you use the correct folder structure and naming conventions (e.g. prefixing your test functions with `test_`) the framework will find all of them in your project automatically. This is called automatic test discovery.

### Module testing

Module testing's definition can be more flexible. A module can be a library, or a set of classes. The point is that module test tests the interaction of a set of units, but it's still not the test of the whole product. This is the "next step" after unit testing. Similar, with a bit bigger scope.

Since all the units are already tested, and should work individually, block tests usually fail when the units don't interact the right way.

Imagine that you have to test a lamp of a car. Unit test are the following:
1) test that the lightbulb works when is under voltage (and vica versa)
2) the battery has votlage
3) the wires transfer electricity

If all of these are true, they can still fail as a system, for example because the lightbulb and the battery are not designed for the same voltage. Module tests are to validate these kind of interactions. It validates that if you put together already tested and workin units, they still work together as a system. After module tests pass, you can consider the "module" as a working block of your software.

### Other types

There are lot of other testing practices, developers don't have to work on a day-to-day basis. They often overlap. We still introduce the most important ones though.

#### Exploratory testing

Act of manual testing, when the tester tries to challenge the system in a quite unrestricted way. It is very intuitive, but needs a good knowledge of the system and an experienced and talented tester to be done well.

#### Functional testing

Done on a complete system, strictly based on the required functionality.

TODO more details

#### Integration testing

The act, when already tested small parts are put together, and tested on a higher level progressively. Starting with unit tests, and continuing with module (block) tests is an important part of a well-done integration testing. Integration testing is simply the idea of stepping upper and upper on this ladder. [Wikipedia](https://en.wikipedia.org/wiki/Integration_testing) says: "Integration testing is the phase in software testing in which individual software modules are combined and tested as a group."

TODO check this

#### System testing

System is tested thoroughly as a whole, usually by a specialized team. If unit tests are the first step on integration testing, system test is the last one.

#### Regression testing

It is a widely accepted fact that a change in a part of a software effects totally other parts of the software, often in an unexpected way. This means if you tested some parts (units, modules) and they passed, you cannot rest, because newly added changes can break them. Regression testing means re-running all of the previously passed tests when a new change is added, ensuring that the already working parts are not broken by the new feature.

This also needs very good (and fast) test automation machinery and thorough unit- and module tests.

TODO safety net again and the fear of change

#### Acceptance testing

This will decide whether the final product complies the original requirements (and a pre written scenarios).

#### Alpha testing

Acceptance testing of the "almost done" product on the developer's site by the company.

#### Beta testing

Acceptance test outside of the developer's site, by external parties, like a limited set of potential real end-users.

## Testing approaches

### TDD - Test Driven Develpment

Test Driven Development, or TDD for short [6] is related to Extreme Programming [7]. It takes the principle of testing as early as possible, and puts it to the extreme: write the test before the code itself.

It has 3 steps which you should repeat:

1) **Write a failing unit test**. Write only 1 test for only to one new small functionality. It will fail of course, because there is no code to fulfill it.
2) **Write to code to make the test pass**. ONLY write code to make the test pass. In TDD you write tests primarily, and you only write code because you have a failing test. Never write more or think in advance.
3) **Refactor**. Now you can refactor your code as long as you don't ruin any of your tests.

![tdd-picture](https://3.bp.blogspot.com/-Wh4E-E-fLbg/WIMc5S50NHI/AAAAAAAAINk/10csazaVSsAlkqzeuowJrjrOGKCeLT6WgCLcB/s400/image.png)

It can be very strange at first, but it has a lot of supporters. The arguments for it:

* It is guaranteed that you will have 100% (or very close) coverage.
* You will have working and tested code in every few minutes. You don't fly blind.
* You will never have unnecessary code. You don't try to write a good code and later check for the requirements. You write code directly against the requirements.
* A very good (and maybe overly idealized) example of TDD is the bowling game example. It is written down [here](http://butunclebob.com/ArticleS.UncleBob.TheBowlingGameKata). This is maybe long and boring, but there are a lot of blogposts and video captures about it. We encourage you to google around with the term "bowling game" or "bowling game TDD"!

### BDD - Behaviour Driven Develpment

After TDD, a lot of "X"DD ("'something' Driven Development") emerged. The most well-known is BDD, the Behaviour Driven Development.[8]

BDD is not an alternative to TDD or unit testing, it rather completes them, and gives advices how to write the tests.

The general principle of BDD is that you should phrase your requirements (== tests) in higher level, and you should set expectations against the behavior, and not the implementation.

#### The given-when-then approach

The main practical advice of BDD is the Given-When-Then principle[9].

It says that when you design a test scenario, you should build it around these 3 keypoints:

1) **Given**. The given part describes the state of the world before you begin the behavior you're specifying in this scenario. You can think of it as the pre-conditions to the test.
2) **When**. The when section is that behavior that you're specifying. This is the input, or the trigger.
3) **Then**. Finally the then section describes the changes you expect due to the specified behavior. This is the output, or expected result, the triggered event.

A good practice is to reflect these in the name of the test. For example `given`

Often the *given*

In the tests, these are mapped to: given == pre-conditions, when == action, then == expectations. In most testing frameworks, they come exactly in this order, but it can be different as well.

### Others
There other <X>DD "something" Driven Development techniques, but we don't need them. You can use Google if you are courious.

## Measurements

### Coverage

You can measure how well did you write your tests in some ways, one is the different types of coverages.[10]

#### Line coverage

Means how many percent of the lines of the production code have been executed when you run all of the unit tests. Reaching 100% is the dream, but it is usually unreasonable. In our automated testcases, there are requirements, which can change from software unit to software unit. Uncovered code can mean missing unit test, but - if the tests are written well - it can also detect unneeded dead code as well.

It's important that even if you have 100% test coverage you can have untested scenarios.

TODO clarify and emphasize missing requirement vs too much code

TODO nontrivial branches, like exception handling

For example if the happy path runs a function from the beginning to the end, your happy path test will provide you 100% coverage. But you can still have requirements for the cases your code throws an exception in the middle of that function.

Or if you have 10 independent if blocks after each other the execution can run them in 1024 different combinations. If you have a test where all 10 of the conditions are true, you will have 100% line coverage, while you tested 1 scenario and didn't test 1023.

Therefore line coverage should be used as a necessary, but not supplementary contidion. Not having good coverage is bad, but having good coverage is not enough.

#### Others

Exactly because of these problems they introduced other types of coverages. For example branch coverage measures how many of the possible paths have been tested, like the 1024 cases in the above example.

You can measure how many of the possible brances, functions, files, conditions have been tried out during run of unit tests.

Uncovered code means that you are unable to detect the breaking of this code when adding new functionality, and also that the requirements of these code parts are not defined in the test code, and they are maybe hard to understand or unneeded.

But as in the case of the line coverage none of them is omnipotent, and none of them should be used as the only goal or the only measurement.

There are automated tools to measure coverage, and usually they generate a nice html output where you can see color coded which parts of your code are not covered. Test coverages (most often line coverage) can be used in CI to block a pull request from merging if the coverage is below a certain limit.

## Practical stuff

### Separate the SUT (System under test) from the dependencies (DOC)

TODO clarify that it applies only to specific parts of code

We would like to test a specific part of code, called **System Under Test** (SUT). It can be a function or a class in case of unit tests, or modules, libraries, software blocks in case of module tests. The SUT is what you are testing right now.

Every entity has inputs and outputs. During testing, we need to ensure that the entity gives the right output for a specific input.

In the code, there is always a set of parts (functions, classes, interfaces, software units, blocks, etc.) interacting with each other. How can we pull out the entity (like a class) from the surroundings, when it relies on a lot of other classes, and those other classes also need a lot of other classes t work? The classes, or whatever pieces of software the SUT is dependent on are called **Depended-On Component** (DOC). (Definition from xUnit.) 

TODO insert picture

Example: you want to test a controller class, but it uses a model class inside, so to be able to run your controller you need to use the model as well. But the model uses underlying datastructures, which need to be included too, and a lot of helper classes for example to convert images. Then, and this is quite a big problem, your model is trying to communicate with the database. Not speaking about all of the classes your classes inherit from. So now that you wanted to test your controller separately suddenly you need to bring up kind of the whole system with a fully functioning database connection. Right?

Wrong! We just pick the SUT, and not any real classes around it. The point is that we need some simplified substitutes instead of the DOC-s, which have the same interface towards our SUT, but are not dependent on any other classes. With simpler words in the above example you use your real controller (because that's what you want to test), but a fake model which looks like the real one, but doesn't use other classes and a dabatase connection in the background. It just have the same interface towards the SUT, but returns predefined values for function calls instead of reading them from the DB.

These substitutes can be called **stubs**, **fakes** or **mocks**. We will return to these soon.

The interaction of classes in a real code can be propagated forever. The whole product is coherent in some way.

When testing a class (SUT), only its surrounding is simulated, without their dependencies.

How can we achieve this? Differently in every language.

The point is that you can dynamically provide which dependency your SUT uses. For example you have a `Room` and a `RoomMock` class. In the real code your `RoomController` uses the `Room`, but when testing it has to use the `RoomMock`.

If you use a strongly typed object oriented language the `Room` and `RoomMock` 

TODO finish

(In C++, it is quite simple: every class is included by its headers, or even just as an interface, and we can provide any implementation for it, using the build.spec files. If we use the same header, the SUT will think it communicates with the correct class, but we will use a simplified implementation, so we don't have to go further with the dependencies.

When writing a new class, which is public, and can be used by others, you should always provide stub or mock implementation to it as well in the test/export (or corresponding) folder.

#### What is a stub?

**Stub** is an overly simplified version of a class. It has the minimal number of methods, which are required by the header, and they usually have only one line: return a hard-coded value. A stub is usually the minimal code which satisfies the compiler check.

#### What is a fake?

**Fake** is a bit more complicated stub. It can implement some logic, but with shortcuts to not have dependencies. For example instead of communicating with a database, it uses a predefined data structure. Can behave different ways in different situations, but still a simplified version of the DOC class.

In reality we are usually sloppy, and use the word "stub" to stubs and fakes as well.

#### What is a mock?

In most cases we need to inspect how the SUT communicates with other classes. For example: our brand new class needs to raise an alarm when the destructor is called. How do we check it? We cannot raise a real alarm, and cannot use the framework for raising alarms, because it would bring a lot of dependencies and complexity to our simple test. So we have to have something similar to a stub for the "alarm raising framework". But we also need to check whether the raiseAlarm() function of it has been called or not. Our stub has to be intelligent enough to indicate to the testing framework that a specific function of it has been called, or not, and make the test fail if not.

There are a lot of similar stuff we can inspect. Testing frameworks provide these kind of "intelligent stubs", called mocks.

So a **mock** is a simplified substitution of a real class, which provides functionality to make expectations against it and check the satisfaction of them.

#### Where do mocks come from?

(Here and later sometimes I use the work "mock" for all mocks, stubs and fakes too. Also when I write "mocking" it can mean the act of creating a mock, a stub or a fake and use it to substitute a real class.)

So when you test your class, function, module it is usually surrounded by mocks, stubs and fakes. Of course it means extra work, because you need to create mock for practically every class in the system. It's one practice to create a mock whenever you create a new class, so it can be used later when testing *other* parts of the code.

For example if I create a new class called `FinanceReport` which has dependencies and talks to the database, I also create `FinanceReportMock` to the folder where we store mocks. `FinanceReportMock` will not use real classes from the system and will not talk to the database so it can be used in isolation and runs very fast, but implements the same interface to the outside world as `FinanceReport`.

Later, when someone else in the future writes a class, called `FinanceReportController` which *uses* `FinanceReport` they can just use `FinanceReportMock` in the test of `FinanceReportController`.

Always remember to update the interface of `FinanceReportMock` when you change the interface of the real `FinanceReport` so they always look the same. If the language supports interfaces, it's better if the real class and it's mock implements the interface (or have some common abstract parent) and everyone else uses the interface and not the concrete classes.

#### Other helpers

Apart from mocks, stubs and fakes there are often other things, just like helper functions, setups and teardowns, [drivers](https://www.tutorialspoint.com/software_testing_dictionary/test_driver.htm) which surround your systems under test and support testing. All these code together is called the **test harness**.

It sounds complicated, but trust me, it will make your life much simpler and development faster.

### Do I need to mock every dependency? Can't I just use real classes in tests?

No, you don't and yes, you can. Sometimes. You don't have to mock *everything*. There is no straight line, it is usually common sense, but you can keep some rules in mind:

* Mocking (and stubbing and faking) is to resolve dependency issues, and make expectations. If you don't have problems with dependencies and you have no expectations, (and it doesn't make your tests slower) it's OK to use the real one.
* Usually when the class is in the same abstraction level as the SUT, it is stubbed or mocked (they are "next to each other"). But if the class is contained by the SUT (is "inside it"), the real one often can be used, because it will not continue the dependency chain. Also, in this case this other class can be considered as the inside working of SUT (like a subcontractor). We often say, that you don't mock the builtin classes of the language like string, because you consider that already tested. If your own, already tested utility class is inside the SUT, just use the real one. But remember, nothing is set in stone! Follow your common sense!
* Only use real class if it has been tested in an earlier phase, so it can be considered trustable. Otherwise if the test fails, you don't know if your currently tested class broke it or the one you included. Never rely on "equally new" classes in your test. You should rather mock them.

### Who tests the tests?

TODO

mock and test is not tested simple

todo section about mocking example and breakingo ut parts

### Organizing tests

Tests are organized to groups so it's easier to find them, to understand what they do and importantly so you can execute a subset of them, which, for example are testing the same class or function. How you physically organize your test files can vary from framework to framework, from language to language or can be dependent on your own team's conventions. However some vocabulary is common and you can hear people say **test case**, **test suite**, **test fixture**, **test harness** and such things.

#### What is a test case?

The wide-spread terminology, (and official ISTQB terminology) accepts **test case** as one specific test: one single set of prerequisites, expectations and actions. In some frameworks the vocabulary can be different and confusing. Usually technically a test is a function or method in the test framework.

#### What is a test suite?

According to ISTQB a **test suite** is a *set of test cases* that belong together in some way and are usually executed together. Grouping can be any logical rule, like they are testing the same method, and/or they start from the same state of the system (same prerequisites, or same "given"s). Test suites have names as well and make tests easier to manage by creating a hierarchical system. (Sometimes suites are grouped to bigger suites and so on to form this hieararchical structure.)

For example TODO

*Nitpicking note*: "test suite" is often pronounced wrong. It should be pronounced similar to *sweet* and not like *suit*. (Note that "suit" and "suite" are not the same word.)

*Note 2*: this vocabulary can be different in different contexts. For example Google's C++ testing framework GTEST uses the term TestCase to the concept we described here as test suite and uses Test to the concept we described here as test case. Confusing. Always check what you should use in your context.

#### What is a test fixture?

[Test fixture](https://en.wikipedia.org/wiki/Test_fixture) is a test suite, which can have it's own state, by having own data members, and helper methods as well. Test fixture is often technically a class which contains test methods, and other helper stuff. These members are usually the SUT, the stubs and the mocks.

If multiple tests use the same set of mocks, they are grouped together in a test fixture, so you have to define them only once and concentrate on the expectations and actions in the tests.

Often test fixture and test suite is used interchangably, but actually there is difference. Sometimes I will call text fixtures test suite in this document as well. Please don't be confused.

TODO add example

##### Setup

Usually a set of tests have the same pre-conditions, so frameworks provide functionality to put this boilerplate code to a function, which is automatically executed before every test in the test suite (or fixture).

A setup is to set the stage, to create the starting point of the tests insite the test fixture. Speaking in BDD terms a setup is what provides that the points in "given" are actually given when the tests starts to execute.

Setup usually contains initialization of mocks, and set up the surroundings. If you define your tests well, it comes naturally.

You don't have to always put your pre-conditions to a setup. If it is simpler, setting up the "given" can be part of the test also.

Setups can be defined in different hierarchy levels, for example it is possible in some frameworks to have suite setups and suite teardowns. We don't really use them though.

##### Teardown

Teardown is the reflection of setup in the mirror. It is executed automatically after every test in the test suite. Usually it is used to clean up. Typically a badly written teardown can cause memory leaks. It is not mandatory to have setup or teardown.

It is important to know that the fixture classes object is created and deleted for every single test. It's **not** like setup runs once and then every testcase is run, and then teardown is run. Instea in every testcase a new object is created, setup is run, one testcase is run and teardown is run, then the object is destructed. Then a new test object is created, setup is run, the second testcase is run and teardown is run, and the object gets destroyed. And so on. So the tests are totally independent, the second test will not "remember" what happened in the first one.

#### What is a test harness?

As mentioned earlier, a test harness is the test environment of stubs, fakes, mocks, drivers and helpers. All the code you wrote for testing apart from the tests themselves.

### What is the scope of a test? An example with 2 possible solutions

You have to decide strictly what is the SUT, and where the boundaries are. If the control flow exits the boundaries, the test case ends. Any entry point to the system should be a new test case (or set of test cases). If this sounds too abstract, here is an example:

You have a cusomer who asks you do implement a new feature. You need to create a method in `AwesomeClass`, called `AwesomeClass.displayServerMessage()`. Your client says when it's called, it should send an request to a server, wait for the response, and when the response is received, display the message in it. Note that the server has been in place, you don't touch that, your task now is only to implement `AwesomeClass.displayServerMessage()`.

You realize that the server can respond by a correctly formatted message, or a successful message but with the wrongformat, by an error (like status `404` or any unsuccessful) or not respond at all for a given time.

After consultation with the customer you identify:

1. In case of a successful and well formatted message the content should be displayed in a UI. (Of yourse you have a definition of formatting.)
2. In case of a successful, but wrongly formatted message `"Unsupported message format"` should be displayed in the UI.
3. In case of an error message `"Server error"` should be displayed on the UI.
4. In case of no response comes after 5 seconds `"No response from server"` should be displayed on the IO.

How do you test it with the server and connection? There are different approaches, I will show you two. You should always understand which one is more suitable for your case. In the first approach you consider it as one series of events and one system. In the other case you strictly consider your class as the SUT and you split to separate cases to exclude the server from your tests.

![Approaches](approaches.png).

#### Consider it one series of events

You think of your class and the server as one system. Actually you think of `AwesomeClass` as an interface which hides the server and the responsibility of this system is to display the message when you call the `AwesomeClass.displayServerMessage()` method.

In your setup you create the server instance and the `AwesomeClass` instance, and create the connection between them. Probably you don't want to use a real server and a real connection so you use a mock for the server. Actually in this case the server should have a fake. This fake should be told what to respond when it gets a request.

You will have tests something like these:

In the setup you set up the `AwesomeClass` instance and the `ServerFake` instance. You make the `AwesomeClass` instance to use the `ServerFake` as it's server.

And the testcases:

1. You tell the `ServerFake` to respond by a `"hello"` message when it gets a request. Then you call `AwesomeClass.displayServerMessage()` and assert that `"hello"` is displayed.
2. You tell the `ServerFake` to respond by a wrongly formatted response when it gets a request. Then you call `AwesomeClass.displayServerMessage()` and assert that `"Unsupported message format"` is displayed.
3. You tell the `ServerFake` to respond by an error, you call `AwesomeClass.displayServerMessage()` and assert that `"Server error"` is displayed.
4. You tell the `ServerFake` to not respond at all, you call `AwesomeClass.displayServerMessage()` and assert that `"No response from server"` is displayed when the timput is passed. Note that in this case you want to set the timeout to milliseconds because you don't want the class to wait 5 seconds every time you execute this testcase.

Notes:
* This approach requires `ServerFake`, a little bit clever fake to respont what you want it to respond. It means you have to implement this fake class which has it's own variables and logic.
* This fake just responds and doesn't judge whether the `AwesomeClass` did it right or not.
* You had to write your production code in a way so you can test it. It means you need to be able to set which server it uses and also you need to be able to set the timeout. If you don't test yuor code, maybe these features are not needed. So keep in mind when writing code that you need to write it in a way it's testable.
* Your argument here is that you test the interface towards the user: you put something in at an end of a system and check what comes back at the same end.
* In this case your fake is part of the system. So you call the `displayServerMessage()` and the control flow goes the whole way to the server and back again. Your testcase can fail if something bad happens inside the server so it doens't only validate `AwesomeClass`. In more complicated cases more complicated fakes can be needed and this approach can be problematic. But in some cases it is the simpler choice.

#### Strictly test the SUT

In this approach you consider only `AwesomeClass` the system under test and the server as something separate. As soon as the control flow leaves the `AwesomeClass` the testcase ends.

In this case you can have 2 test fixtures:

In the fist one you create an `AwesomeClass` instance, a `ServerMock` instance and you connect them. There will be one testcase in it.

1. when `AwesomeClass.displayServerMessage()` is called, then a request is sent

That's it, `AwesomeClass`'s responsitilbity is only to send the request, it doesn't have control over what the response will be. So the second suite is to check what if we are already waiting for the response:

With pseudo code it's something like this:

```
class AwesomeClassExistsWithServerConnection extends TestFiture {
	function setUp() {
		AwesomeClass awesomeClassUnderTest = new AwesomeClass();
		Server server = new ServerMock();
		awesomeClassUnderTest.makeConnection(server);
	}
	
	function test_whenDisplayServerMessageIsCalledRequestIsSentToServer() {
		server.expectRequest({ /* request data */ });
		awesomeClassUnderTest.displayServerMessage();
	}
}
```

In the second suite's setup you create an `AwesomeClass` instance, a `ServerMock` instance and you connect them and call the `AwesomeClass.displayServerMessage()` so it sends the request. This is the starting point (the "given") of all testcases in this suite (given the request is sent). Note that this setup is the same as the first suite's setup, plus calling `AwesomeClass.displayServerMessage()`. In practice test fixtures are often classes which inhherit from each other. This second fixture can be the child of the first one where the setup just calls the setup of the previous fixture and calls `AwesomeClass.displayServerMessage()`.


```
class AwesomeHasSentRequestToServerConnection extends AwesomeClassExistsWithServerConnection {
	function setUp() {
		parent.setUp();
		awesomeClassUnderTest.displayServerMessage();
	}
	
	...
}
```

And the testcases are:

2. You make the server respond with a correctly formatted response containing `"Hello"` and assert that `"Hello"` is displayed.
2. You make the server respond with an incorrectly formatted response and assert that `"Unsupported message format"` is displayed.
3. You make the server respond with an error code response and assert that `"Server error"` is displayed.
4. You tell the `ServerFake` to not respond at all, you call `AwesomeClass.displayServerMessage()` and assert that `"No response from server"` is displayed when the timput is passed. Note that in this case you want to set the timeout to milliseconds because you don't want the class to wait 5 seconds every time you execute this testcase.

And of course others for the case when 3) reject comes back instead of CFM, and 4) nothing comes back and system times out.

Notes:
* If the server doesn't respond correctly that is not the fault of `AwesomeClass`. One advantage of this approach is to completely take behaviour of the server out of the tests and only tests what is the responsibility if the SUT. The approach above have assumptions coded inside the testcase about how the server works. If the real server does something we didn't expect that can cause a bug the testcase doesn't catch.
* Here you are not testing one interface: you put something in at one interface and check if something else comes out in the other end.
* This approach is more suitable for low level tests. It is stricter separation of elements, which, after tested separately can be put together and be tested as a whole system with a real `AwesomeClass` and a real server. That testcase is also valid, but it's high level while the ones I described are low level usittests.
* Whichevery you do you will end up higher level tests testing the interaction of the real `AwesomeClass` and real server. If both of `AwesomeClass` and server tests pass, but they fail as a system you will know there is a problem with how they interact. This is very good to localize your errors.

### Black-Box testing

Also, you should consider it as a black box. This is the requirement, and not *how* it is done. So the implementation can be changed in the future, but as long as the function does the same thing, all tests should still pass. If you write too strict requirements against "private parts", you will not have the freedom to change the implementation, although the details are not really required by anyone.

### Naming

There is no commonly used rule for naming, but we have our internal habits:

verify_<function under test>_<GivenWhenThenShortly>

For example: verify_connect_xReqIsSentUnconditionally, and verify_connect_messageIsReturnedWhenXCfmIsReceived

But again: use your common sense.

Keep in mind: usually hundreds of tests are run, and maybe the runner only sees the list of the names and that which one failed. It should be easy to understand, what failed, and how to locate it.

## References

1 CrossBrowserTesting: What’s the True Cost of a Software Bug? - https://crossbrowsertesting.com/blog/development/software-bug-cost/
2 synopsys: How much do bugs cost to fix during each phase of the SDLC? - https://www.synopsys.com/blogs/software-security/cost-to-fix-bugs-during-each-sdlc-phase/
3 The Celerity Blog: The True Cost of a Software Bug: Part One , http://blog.celerity.com/the-true-cost-of-a-software-bug
4 Unit testing, Wikipedia https://en.wikipedia.org/wiki/Unit_testing
5 Google Test on Github https://github.com/google/googletest
6 Test Driven Development https://en.wikipedia.org/wiki/Test-driven_development
7 Extreme Programming https://en.wikipedia.org/wiki/Extreme_programming
8 Behavoiur Driven Development https://en.wikipedia.org/wiki/Behavior-driven_development
9 Given When Then https://martinfowler.com/bliki/GivenWhenThen.html
10 Code coverage https://en.wikipedia.org/wiki/Code_coverage