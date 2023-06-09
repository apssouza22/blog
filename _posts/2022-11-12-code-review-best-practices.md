---
title: Best Practices for Code Review — Quick Reference
---
Code review is a popular and one of the most critical software development processes used to improve code quality, consistency, share knowledge, and much more.

## **Mentoring**

Code review can have an important function of teaching developers something new about a language, a framework, or general software design principles. It’s always fine to leave comments that help a developer learn something new. Sharing knowledge is part of improving the code health of a system over time.

## **What To Look for in a Code Review**

In doing a code review, you should make sure that:

-   The code is well-designed.
-   The functionality is good for the users of the code.
-   Any UI changes are sensible and look good.
-   Any parallel programming is done safely.
-   The code isn’t more complex than it needs to be.
-   The developer isn’t implementing things they _might_ need in the future but doesn’t know they need it now.
-   The code has appropriate unit tests.
-   Tests are well-designed.
-   The developer used clear names for everything.
-   Comments are clear and useful and mostly explain _why_ instead of _what_.
-   The code is appropriately documented.
-   The code conforms to our style guides.

Make sure to review **every line** of code you’ve been asked to review, look at the **context**, make sure you’re **improving code health**, and compliment developers on **good things** that they do.

## **How Fast Should Code Reviews Be?**

If you are not in the middle of a focused task, **you should do a code review shortly after it comes in.**

**One business day is the maximum time it should take to respond** to a code review request (i.e. first thing the next morning).

## **Pull Request**

-   Create small PRs
-   Add the JIRA ticket to the PR title
-   Write descriptions with all information that will help the reviewers
-   Wait to merge the branch until continuous integration (TDDium, Travis CI, CircleCI, etc.) tells you the test suite is green in the branch.
-   Always delete your branch after merge it
-   If not sharing your branch use the squash option
-   One approval is required

## **Code Review Ethics**

When our code is being reviewed or when we review the work of others, it’s essential to have the correct mindset to make it a positive experience and reap all the benefits of this effort.

## **Everyone**

-   Accept that many programming decisions are opinions. Discuss tradeoffs, which you prefer, and resolve quickly.
-   Ask good questions; don’t make demands. (“What do you think about naming this :user_id?")
-   Good questions avoid judgment and avoid assumptions about the author’s perspective.
-   Ask for clarification. (“I didn’t understand. Can you clarify?”)
-   Avoid selective ownership of code. (“mine”, “not mine”, “yours”)
-   Avoid using terms that could be seen as referring to personal traits. (“dumb”, “stupid”). Assume everyone is intelligent and well-meaning.
-   Be explicit. Remember people don’t always understand your intentions online.
-   Be humble. (“I’m not sure — let’s look it up.”)
-   Don’t use hyperbole. (“always”, “never”, “endlessly”, “nothing”)
-   Don’t use sarcasm.
-   Keep it real. If emoji, animated gifs, or humor aren’t you, don’t force them. If they are, use them with aplomb.
-   Talk synchronously (e.g. chat, screen-sharing, in-person) if there are too many “I didn’t understand” or “Alternative solution:” comments. Post a follow-up comment summarizing the discussion.

## **Having Your Code Reviewed**

-   Be grateful for the reviewer’s suggestions. (“Good call. I’ll make that change.”)
-   A common axiom is “Don’t take it personally. The review is of the code, not you.” We used to include this, but now prefer to say what we mean: Be aware of [how hard it is to convey emotion online](https://www.fastcodesign.com/3036748/why-its-so-hard-to-detect-emotion-in-emails-and-texts) and how easy it is to misinterpret feedback. If a review seems aggressive or angry or otherwise personal, consider if it is intended to be read that way and ask the person for clarification of intent, in person if possible.
-   Keeping the previous point in mind: assume the best intention from the reviewer’s comments.
-   Explain why the code exists. (“It’s like that because of these reasons. Would it be more clear if I rename this class/file/method/variable?”)
-   Push commits based on earlier rounds of feedback as isolated commits to the branch. Do not squash until the branch is ready to merge. Reviewers should be able to read individual updates based on their earlier feedback.
-   Seek to understand the reviewer’s perspective.
-   Try to respond to every comment.
-   Merge once you feel confident in the code and its impact on the project.
-   Final editorial control rests with the pull request author.

## **Reviewing Code**

Understand why the change is necessary (fixes a bug, improves the user experience, refactors the existing code). Then:

-   Communicate which ideas you feel strongly about and those you don’t.
-   Identify ways to simplify the code while still solving the problem.
-   If discussions turn too philosophical or academic, move the discussion offline to a regular Friday afternoon technique discussion. In the meantime, let the author make the final decision on alternative implementations.
-   Offer alternative implementations, but assume the author already considered them. (“What do you think about using a custom validator here?”)
-   Seek to understand the author’s perspective.
-   Remember that you are here to provide feedback, not to be a gatekeeper.


**References**
https://github.com/thoughtbot/guides
https://google.github.io/eng-practices/review/
