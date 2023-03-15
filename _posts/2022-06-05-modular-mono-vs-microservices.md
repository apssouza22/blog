---
title: Microservices vs modular monolithic
---

###### I am always posting provoking post on my Linkedin, and I have decided to replicate those here. [Let's socialize!](https://www.linkedin.com/in/alexsandro-souza-dev/)

You heard about Microservices, and after searching about it, you realised the benefits of modularity principles?

Now, if you search for modularity, you will learn that you don't need to move to a microservices architecture to leverage modularity

Microservices architecture has some benefits, but the main ones are:
- Teams can work and scale independently
- Microservices are small and focused, reducing complexity
- Services can be internally changed or replaced without global impact

If your company is not the size of Google or Netflix, you can achieve everything from above in a modular monolithic system

What's essential is that we can effectively draw and enforce boundaries during development. Of course, that means embracing any help we can get from the programming language to enforce modularity principles

Different teams can work on different parts, where only the well-defined interfaces are touchpoints between the teams

## Enforcing modularity principles with Java
Effectively draw microservice boundaries is a hard job. Let's be honest, how often do you get your boundaries right the first time or even the second?

Redrawing microservice boundaries involves a lot of interpersonal communication to not let things blow up at run-time.

Refactoring across modules is easier, and it is supported by the type system and the compiler.

In Java, for example, Since Java 9 native module system is part of the language. Java modules can express dependencies on other modules and publicly export interfaces while strongly encapsulating implementation classes. Other languages offer similar mechanisms. For instance, JavaScript got a module system as of ES2015.


Transform a module into a microservice is always one option. Just add a communication layer on top of it(gRPC or REST)

I have created a project where I designed and implemented a monolithic application that was built using modularity principles. [Check it out!](https://github.com/apssouza22/trading-system)

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0pckz5qydlf0prcs31yc.png)

