---
title: Questions/answers about software testing
---

#### Follow me on Linkedin if you like provoking posts about software development. [Let's socialize!](https://www.linkedin.com/in/alexsandro-souza-dev/detail/recent-activity/shares/)

Here are some essential questions/answers when defining your system testing approach.

This post is a follow up to my article [We are testing wrongly.](https://dev.to/apssouza22/we-are-testing-software-wrongly-and-it-costs-a-lot-of-money-23o5)

## Why do we write automated tests?
The main reason to write automated tests is to bring confidence that our system still working as expected after adding new changes to the code.

## What is unit testing?
Many engineers believe that for every public method of every class, they must create a corresponding "unit test.". That is wrong. That wasn't what many meant by "unit" when the term "unit testing" was first used.

Instead of writing unit tests for every public method of every class, write unit tests for every component (i.e., user, product, order, etc.), covering every behaviour of each component, focusing on the public interface of the unit.


## What is the right approach when testing software?
We learned that testing should begin "in the small" and progress toward testing "in the large"; Because that, we should focus more on the Unit testing - the aim is to test each part of the software by separating it. It checks that the component matches the expected behaviour. Remember, the UNIT here is not the method or class as most people think.

Next comes the module testing; the aim is to test different packages to assess if they work correctly together. By testing the units in groups, any faults in the way they interact together can be identified.

And finally, End-to-end testing; in this testing phase, different software modules are combined and tested as a group to ensure that the integrated system is working correctly.

E2E test plans generally cover user-level stories like:
"A user can log in."
"A user can make a deposit."
"A user can see its balance."

## Can you test too much?
"Can you test too much? Sure you can. You are testing too much if you can remove tests while still having enough." - Martin Fowler

Kent Beck (Author of the book TDD By Example) answered the question how much unit tests a system should have

> I get paid for code that works, not for tests, so my philosophy is to test as little as possible to reach a given level of confidence...

Optimising for confidence is the key to effective testing!  Don’t over-testing and be careful to not cover the  same things across the different tests(Unit, module, E2E)

## More
You can find more about testing [here](https://dev.to/apssouza22/we-are-testing-software-wrongly-and-it-costs-a-lot-of-money-23o5)