---
title: How to structure your code effectively
---

It is time to stop structuring your code as beginners framework tutorials once taught you.

By now, you should be capturing the domain knowledge in working code and protecting that knowledge in your context from contamination and overreaching from other contexts.

What do ProductRepository and BasketRepository have in common? Nothing. Both address different problems, so why group them together?
Product, ProductService, and ProductRepository are highly coupled; why would not you group them? Cohesion -  the code that changes together, stays together, carefully encapsulated.

The more restricted your access level, the fewer chances to create unnecessary dependencies that will end up in a big ball of mud.

We need to resist the temptation to make everything public! That is why codebases turn into a mass of interconnected objects.

It's essential to organize your packages correctly. Packages should contain all classes of a specific domain/context and only provide access to other packages through a unique interface.

Then, instead of writing unit tests for every public method of every class, write unit tests focusing on the component's public interface, covering every behaviour of the unit. [Checkout my article about effective testing](https://dev.to/apssouza22/we-are-testing-software-wrongly-and-it-costs-a-lot-of-money-23o5)

Components with a single, well-defined purpose are easy to understand and much more readable. The name of such components clearly announces their purpose, and such a unit do just that.

The basic summary of the story is that things get much easier to understand and maintain if your architectural ideas map explicitly into the code. Your code should reflect the architecture diagrams that you draw.

## Cohesion vs Coupling

The code that changes together stays together.  The code that serves a common purpose stays together. Cohesion is a pretty simple concept, and I hope you have understood it already.

Coupling is the next important thing you need to learn correctly. It is about how much one component depends on or interacts with other components.

![Coupling vs Cohesion](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/abo20s23zvejo3jum7yk.jpg)

Ideally, we should aim for high cohesion and low coupling. Apply the Single Responsibility Principle (SRP) for high cohesion. Use well-defined interfaces and inversion of control for low coupling.

Low coupling means fewer interconnections across components. It means one component can be changed or even replaced without impacting other components.

When software is decomposed into smaller components, these components will inevitably interact with one another, and it will require clearly defined boundaries not to leak the domain. The result is a system that consists of cooperating but independent, bounded contexts.

The basic summary of the story is that things get much easier to understand and maintain if we can effectively draw and enforce boundaries during development.

## Example
I have created a project where I designed and implemented a monolithic application that was built using modularity principles. [Check it out!](https://github.com/apssouza22/trading-system)

I also recorded a video walking through the project highlighting what we have discoursed on this post.

{% include youtube.html id="KcrCyJzK09I" %}

