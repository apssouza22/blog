---
title: How to test software effectively
---
Many engineers believe that for every public method of every class, they must create a corresponding "unit test."

That wasn't what many meant by "unit" when the term "unit testing" was first used.

The problem with that definition of unit testing is that you're no longer focused on testing behaviour but implementation.

The tests you write are tightly coupled to the underlying design of your code. Design is constantly evolving. You now not only have to refactor the designs of your production code—you have to change your tests too!

In other words, your tests should help you with the refactoring, giving confidence, but instead, it is only making work harder and giving no confidence of things still working correctly.

I will not even mention the mock hell for brevity(Please google about it).

But instead of abandoning refactoring, or unit tests, all you need to do is free yourself from the mistaken definition of "unit testing". Focus on testing behaviours!

Instead of writing unit tests for every public method of every class, write unit tests for every component (i.e., user, product, order, etc.), covering every behaviour of each component, focusing on the public interface of the unit.

To achieve that, you will need to learn how to structure your code properly. Please don't package your code by technical concerns(controllers, services, repositories...). Senior devs structure their code by domain. Check out my post [You are structuring your code wrongly](https://dev.to/apssouza22/we-are-structuring-software-incorrectly-4oab) to learn more.

## Code coverage
Lines of code covered doesn't mean it is ready for real life. It won't tell you that you were supposed to check that String for a leading slash and trim it off or ask you to check null values.

Code coverage reporting is dangerous. It tends to distract from the use cases that should drive the software development process.

If we are not allowed to ship when code coverage is below 80%, we will add more and more trivial tests without much value to ensure quality or bring confidence during refactoring.

Code coverage has nothing to do with code quality, which has been [proven statistically](https://www.researchgate.net/publication/317429288_On_the_Relation_Between_Unit_Testing_and_Code_Quality) and it has [insignificant correlation for system defects](https://hal.inria.fr/hal-01653728/document)


Kent Beck (Author of the book TDD By Example) answered the question [how much unit tests a system should have](https://stackoverflow.com/questions/153234/how-deep-are-your-unit-tests/153565#153565)

> I get paid for code that works, not for tests, so my philosophy is to test as little as possible to reach a given level of confidence...

Martin Fowler answered if a system can have too much tests

> Can you test too much? Sure you can. You are testing too much if you can remove tests while still having enough

That's not to say coverage doesn't have value, as [Martin Fowler points out](https://martinfowler.com/bliki/TestCoverage.html), it is a good way to identify untested code.

## Implementing a Test Automation Strategy
Testing in these short Agile iterations requires a "shift left" approach. This shift left in the agile development process means testing starts much earlier in the application lifecycle. As a result, developers should own QA, which means developers write, execute, and maintain tests for the code they produce; it encourages everyone on the team to be engaged in delivering high-quality products to the customer quickly.

Now that we know who should write tests let's look at my recommended test strategy.
Testing should begin "in the small" and progress toward testing "in the large"; Because that, we should focus more on the Unit testing - the aim is to test each part of the software by separating it. It checks that the component matches the expected behaviour. Remember, the unit here is not the class.

Next comes the module test; the aim is to test different system parts in combination to assess if they work correctly together. By testing the units in groups, any faults in the way they interact together can be identified.

And finally, End-to-end testing; in this testing phase, different software systems are combined and tested as a group to ensure that the integrated system is working correctly.

E2E test plans generally cover user-level stories like:
"A user can log in."
"A user can make a deposit."
"A user can see its balance."

## Conclusion

It is essential to understand that we should be writing tests to enable teams to move fast. Code is constantly evolving, and developers need to feel confident adding new features and improving the existing code continually.

Some ideas here can be controversial, and you don't need to agree with me, but leaders must challenge the status quo and look at new ways of doing things.

Don’t blindly accept fads, myths, authorities and established truths. Question everything, collect experience, judge for yourself.



# Example
If you want to learn more about how to write efficient tests, you can checkout my  [opensource project](https://github.com/apssouza22/trading-system) where I show how to structure the code enabling meaningful unit tests creation.

I also recorded a video walking through the project highlighting what we have discoursed in this post.

{% embed https://www.youtube.com/watch?v=cYef3KReEzo %}

