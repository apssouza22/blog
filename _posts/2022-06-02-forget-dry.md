
---
title: Are you starting with microservices? Forget the DRY principle and embrace redundancy!
---
###### I am always posting provoking post on my Linkedin, and I have decided to replicate those here. [Let's socialize!](https://www.linkedin.com/in/alexsandro-souza-dev)

Are you starting with microservices? Forget the DRY programming principle and embrace redundancy!

Extracting common code to a library seems to be the best practice. However, if two microservices share a library, they are coupled and not independent anymore.

That will increase the coordination effort, and a trivial change in the library will become very hard because it has to be coordinated among the microservice teams.

Someone may suggest adding versioning; then it will add restrictions and may lead to painful version conflicts.

Developers will fear adding improvements and refactoring the code because they will not know how their changes can affect other microservices.

Accept redundancy. It's ok to have redundancy to stay independent.  This way, we will avoid technical and organizational coupling, speed up the development and enable modernization.

You can find more [here](https://phauer.com/2016/dont-share-libraries-among-microservices/)

---

### Free Advanced Java Course
I am the author of the [Advanced Java for adults course](https://www.udemy.com/course/advanced-java-for-adults/?referralCode=8014CCF0A5A931ADED5F). This course contains advanced and not conventional lessons. In this course, you will learn to think differently from those who have a limited view of software development. I will provoke you to reflect on decisions that you take in your day to day job, which might not be the best ones. This course is for middle to senior developers and we will not teach Java language features but how to lead complex Java projects.

This course's lectures are based on a Trading system, an opensource project hosted on my [Github](https://github.com/apssouza22/trading-system).

