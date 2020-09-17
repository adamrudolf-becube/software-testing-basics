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

### Why is testing important

Testing can slow down development process, therefore it is sometimes can be viewed as time-consuming and expensive activity.

But the experience shows that the price (even in time or in money or in complexity) of finding and fixing a bug grows exponentially during the life cycle of the product. This means that a bug costs the most, when found by the customer in the already released product, and it is significantly cheaper to fix it during testing phase. But it is even cheaper (way lot cheaper) if we can catch it during development time. To be short: the earlier we catch the problem, the better. And there is a huuuge difference.[1][2][3]

### Cost to Find and Fix Issues

So if you think that you can win an hour, a day, or even a few days by not testing something, keep in mind that it can cause weeks for you or someone else later.

And as we said, we will probably not find all of the bugs, we have to find as many as possible, and as soon as possible.

![Software testing ](https://martinfowler.com/articles/is-quality-worth-cost/both.png).

The above picture is from Martin Fowler's article [Is High Quality Software Worth the Cost?](https://martinfowler.com/articles/is-quality-worth-cost.html). It shows that if you don't test your code you start faster. In the beginning you create more functionality under a specified time. But later, when your code becomes more complex you will spend much more time by finding and fixing bugs then by actual development.

If you test your code well, you will start slower (because you invest time in writing tests as well, not only the production code), but later you won't slow down that much. After a time this will return, so it's worth to write tests, although it seems slower at the beginning.

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

If all of these are true, they can still fail as a system, for example because the lightbult and the battery are not designed for the same voltage. Module tests are to validate these kind of interactions. It validates that if you put together already tested units, they still work. After module tests pass, you can consider the "module" as a working block of your software.

### Other types

There are lot of other testing practices, developers don't have to work on a day-to-day basis. They often overlap. We still introduce the most important ones though.

#### Exploratory testing

Act of manual testing, when the tester tries to challenge the system in a quite unrestricted way. It is very intuitive, but needs a good knowledge of the system and an experienced and talented tester to be done well.

#### Functional testing

Done on a complete system, strictly based on the required functionality.

#### Integration testing

The act, when already tested small parts are put together, and tested on a higher level progressively. Starting with unit tests, and continuing with module (block) tests is an important part of a well-done integration testing. Integration testing is simply the idea of stepping upper and upper on this ladder. [Wikipedia](https://en.wikipedia.org/wiki/Integration_testing) says: "Integration testing is the phase in software testing in which individual software modules are combined and tested as a group."

#### System testing

System is tested thoroughly as a whole, usually by a specialized team. If unit tests are the first step on integration testing, system test is the last one.

#### Regression testing

It is a widely accepted fact that a change in a part of a software effects totally other parts of the software, often in an unexpected way. This means if you tested some parts (units, modules) and they passed, you cannot rest, because newly added changes can break them. Regression testing means re-running all of the previously passed tests when a new change is added, ensuring that the already working parts are not broken by the new feature.

This also needs very good (and fast) test automation machinery and thorough unit- and module tests.

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

For example if the happy path runs a function from the beginning to the end, your happy path test will provide you 100% coverage. But you can still have requirements for the cases your code throws an exception in the middle of that function.

Or if you have 10 independent if blocks after each other the execution can run them in 1024 different combinations. If you have a test where all 10 of the conditions are true, you will have 100% line coverage, while you tested 1 scenario and didn't test 1023.

Therefore line coverage should be used as a necessary, but not supplementary contidion. Not having good coverage is bad, but having good coverage is not enough.

#### Others

Exactly because of these problems they introduced other types of coverages. For example branch coverage measures how many of the possible paths have been tested, like the 1024 cases in the above example.

You can measure how many of the possible brances, functions, files, conditions have been tried out during run of unit tests.

Uncovered code means that you are unable to detect the breaking of this code when adding new functionality, and also that the requirements of these code parts are not defined in the test code, and they are maybe hard to understand or unneeded.

But as in the case of the line coverage none of them is omnipotent, and none of them should be used as the only goal or the only measurement.

There are automated tools to measure coverage, and usually they generate a nice html output where you can see color coded which parts of your code are not covered. Test coverages (most often line coverage) can be used in CI to block a pull request from merging if the coverage is below a certain limit.