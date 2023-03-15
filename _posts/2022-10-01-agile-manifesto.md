---
title: AGILE - Effectiveness manifesto — practical tips
---
Modern and high-performing organizations employ agile concepts to develop better products faster than their competitors. They are constantly running experiments to discover innovative and efficient new ways to solve customer problems, and they build high-speed engineering capabilities to deliver value every day.

In this post, I will be showing practical actions that will help you to ensure your team focus on delivering values in an efficient way without to give up of quality.

The following tips are based on the agile manifesto, the lean software development methodology, researches and my own experience. I will not try to explain about Agile and Lean in this post but make sure you have those concepts clear in your mind.

## Team Cultural Manifesto

Most leaders agree that a “cultural manifesto” is the key to building strong engineering teams. A cultural manifesto is a document that lays out a team’s values, goals, and working methods. Companies values usually focus on external goals rather than internal ways of working. To shape an internal organizational atmosphere teams should have their own manifesto agreed within the team. Because teams are different, make sense to have different documents for the IT team and finance team. I don’t need to mention that it should be inline with the company’s values, right ;)?

Getting everyone in a team thinking in a way that it maximises the effectiveness of software delivery is a difficult task. But, a manifesto can help to influence team members towards common engineering principles and a culture of effectiveness and quality.

MIT scholar Edward Schein defines culture as a “pattern of basic assumptions”. Those basic assumptions, values, and problem-solving techniques become part of the organization’s “culture” as soon as they are codified into something that can be taught to new employees.

Having a “pattern of basic assumptions” will maximise the effectiveness of your team and I strongly believe every team should have your own manifesto. Below an example of an effective document.

## Effectiveness manifesto
- Team ownership and autonomy
- Daily work improvements
- Simplicity
- YAGNI
- Collaboration
- Don’t rush
- Be quick to admit mistakes
- Code is the primary source of documentation
- Developer-owned QA
- Consumer-Driven
- Feedback loop

## Team ownership and autonomy

Teams that do have end-to-end accountability are motivated by their freedom to creatively find the best solutions for problems with the highest business value. They need to see the value of their work and that they are part of the big picture.
Focus, Flow, and Joy

A developer’s daily work and the way that work feels directly impact how effective that developer is. When developers can focus on writing code without delays or dependencies, it creates a sense of Flow and therefore joy.

As Dan Pink says in his book:

> We have three innate psychological needs — competence, autonomy, and relatedness. When those needs are satisfied, we’re motivated, productive, and happy.

In the software development world developers who write code can also manage the infrastructure as today we can leverage from infrastructure-as-code and the process be totally accountable, this way we remove barriers, avoid delays and minimizing hand-offs.

In the highly regulated industries requires segregation of duties and adopt DevOps philosophy can be a challenge, however, it is always possible to improve the efficiency of this process as well.

## Simplicity
Skelton and Pais wrote about the importance of reducing cognitive load. Their argument is that regardless of whether you build a monolith or microservices, as long as you keep the cognitive load of your systems low, teams can be productive.

Whenever we are faced with a choice between something simple and something clever or complex, we should choose the simpler option. Kept things as simplistic as possible in its implementation and infrastructure. Do The Simplest Thing That Could Possibly Work!

## Daily work improvements
Always try to improve the day-to-day work of the team, do everything that helps the team to succeed, such as automation, process improvement, new tools, better equipment, address technical debt and etc. As a result, it will improve the quality, efficiency and joy of the team.

Microsoft is famous for a culture which says if a developer has a choice between working on a feature or improving developer productivity, the choice should always be developer productivity.

## YAGNI — you aren’t going to need it
It’s a statement that some capability we presume our software needs in the future should not be built now because “you aren’t gonna need it”.

we may now think we need this presumptive feature but it’s likely that we will be wrong and there’s an obvious cost of the presumptive feature — the cost of build: all the effort spent on analyzing, programming, and testing this now useless feature.

YAGNI is a way to refer to the XP practice of Simple Design and the notion of “incremental design”, build as you need it!
Always start with the questions: what is the problem you are trying to solve? who is experiencing the issue? is not the problem already solved somewhere?

## Be quick to admit mistakes
As shown in the Google Aristotle Project a culture where members of the team feel safe to make mistakes and learn from them, and to take risks is important to the effectiveness of a team. This is because teams need to be able to talk about the problems they have and feel safe doing so because prevention is required to solve the problems, which requires honesty and no fear.

Teams must recognize that systems are responsible for the failure, not people and that people usually come to work intending to do a good job.

Failures are an opportunity to improve systems. More important than avoid errors is to learn from them!

## Collaboration
Team collaboration is all about team members acting together to achieve a common goal. There are many boiler points that we should apply to improve team collaboration but I would like to list some more practical actions:

- It’s essential that your team is encouraged to give feedback and ask any questions;
- Avoid selective ownership of code — “mine”, “not mine”, “yours”;
- Promote mentoring in the team — Sharing knowledge is part of improving the code health of a system over time;

- Tasks should be divided fair and square making sure the whole team is growing;
  -Promote individuals interactions synchronously(chat, call, screen sharing, in-person)
- Fast response within the team —Always try to unblock colleagues and if you are too busy to help, you can still send a quick response that lets the developer know when you will be available;
- Disagree and commit — It puts the delivery and the team above the individual preferences;
- Avoid unnecessary discursions - document what was already agreed within the team.

## Don’t rush
Don’t rush and don’t cut corners, doing so usually ends up damaging product quality and generating loads of technical debt. This type of debt has a very high-interest rate, and the project will have to pay it sooner or later. The worst-case scenario it can causes bugs in the production environment, which can be extremely costly.

Also, making people work overtime or making people work overstress conditions might lead to a range of health issues and other problems in life management

## Code is the primary source of documentation
There is no need to document your code if you code your documentation. This principle comes with an important requirement — that it’s important that programmers put in the effort to make sure that the code is clear and readable.

Agile encourages “just enough documentation”. The Agile Manifesto is clear about “working software over comprehensive documentation.” But what is just enough? Documentation is useful until you overdo it, then it will not be updated and therefore useless. Worse than not having documentation is have outdated documentation. Plus gathering any information not useful is a total waste of time.

Sometimes, there is a need for further documentation to act as a supplement to the code, for example, details about external systems which your application is integrating to.

## Developer-owned QA
Developer-owned QA means developers write, execute, and maintain tests for the code they produce, it encourages everyone on the team to be engaged in delivering high-quality products to the customer quickly.

Without a dedicated QA team, there’s no dedicated QA step in the development process, so quality testing is integrated into the workflow. Developers are responsible for writing tests and maintaining them with the support of the product team. This way, the whole team is aware of the testing efforts that need to be completed for their commitments. The only way to this approach to succeed is by having testing as automated as possible.

## Consumer-Driven development
Demands and solutions evolve through the collaborative effort of self-organizing and cross-functional teams and their customers. Always strive to maintain strong relationships with your customer in order to continuously receive feedback that influences the development and be ready to accommodate changing requirements throughout the development process

A consumer in this context can be any client of your software, including other teams in the company as well as other systems within the team.

Engineers should never take final decisions for the consumer, it should be easy to get feedback/validation from who is responsible for the product. The wrong assumption could result in delays and rework.

## Feedback loop
Through constant feedback about their previous efforts, teams can identify areas for improvement and revise their practices. Your team builds something, gathers feedback on your design, implementation and process, and then adjust your product going forward.

- Take note of bad decisions
- Show real appreciation for proactivity
- Periodically discuss bugs found and how it could be avoided
- Collect metrics (bugs found, features released, deployment frequency, rollbacks)

## Conclusion
Developers on agile teams are more engaged, write better code, and have more fun, therefore a better outcome. Engineers are problem solvers. They love autonomy in their work, and they want to be creative and flexible.

Agile is about teamwork and we have to make sure everyone feel a sense of ownership and investment in the projects they work on.

There’s no one Agile approach that works for every organization or situation and as such, it seems foolish to advertise or prescribe one. What I have written above are my personal reflections about being Agile and effectiveness. I hope to inspire some of my readers to adopt effectiveness in their teams and deliver more value to all.

If you liked this article, please follow me on readme to read my future content.

https://medium.com/@alexsandrosouza


