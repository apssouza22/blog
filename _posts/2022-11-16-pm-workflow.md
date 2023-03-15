---
title: Project management workflow
---

###### I am always posting provoking posts on my Linkedin, and I have decided to replicate those here. [Let's socialize!](https://www.linkedin.com/in/alexsandro-souza-dev)

In this post, I describe the workflow management I have been using with my teams. Establishing it as a workflow–makes it clearly structured and repeatable, which, in turn, makes it scalable.

## Tickets
It is essential to have each type of ticket well defined to improve clarity and consequently help with communication.

### Initiative

An initiative captures a large body of work, and it may take several sprints to be completed.

EX: "New QA environment", "new provider integration"


### Feature

Features are any functionality for the product and should contain all high-level details from the user perspective (Including visual mockups).

The Product Owner should initiate it and further refined by the team.


### User Story

Stories should be used for work that directly affects the end-user experience. Usually, it will be part of a Feature.

The User Story description should start with the following template.

"As a (persona) I want to (do something) so that I can (make something else possible)."

### SubTasks

Subtasks should be used for tech work related to user stories, and it should declare what must be done.

Ex. Implement search product by name

### Spikes

Spikes are tasks to gain the knowledge necessary to refine a User Story. A Spike should be linked to a Feature/User Story, and the output of a spike should be the Feature/User Story refined.

## Tech tasks

Tech tasks are those that are not part of any User Story. Usually refers to tech debts. It must be distinguished from User stories and planned appropriately.

## Definition of done

Features and User stories are considered done when there are pieces of evidence (screenshots) of the Feature in the QA environment  and approved by the PO

A user story can be closed even if there is a related open TECH task as long as it doesn't affect the functionality. Ex. Improve test coverage

## Product Backlog
Make sure the project backlog is well-prioritized this way it will help to broadcasts all the things your team intends to spend time on—including internal work that the customer will never notice.

### Now, Next and Later

The team Stories is divided into three buckets(Now, Next and Later). This way, it will be clear to everyone the priorities.

If not sure where to place new stories, they should be placed in the **NEXT** bucket. It will be moved accordingly during backlog refinement.

## Tickets workflow

-   The PO creates the Feature ticket, labels it as user-stories/feature and adds it to the **NOW/NEXT/LATER** bucket

-   **When refining the backlog**, the team will decide if any stories from **NEXT** should be prioritized(Bring to **NOW**), deprioritized(send to **Later**) or left to be addressed in the next phase. Stories in **NOW** should be already refined or contain a refinement(spike) ticket to do so.

-   **When refining a feature**, if the Feature is too big to fit in a sprint, the team should break it into small User Stories.

-   **When refining a User story**, if the User Story requires changes in different repositories(Services), the team should break the User Story into small tasks(subtasks)

-   **When implementing a User Story,** once started implementing a Feature/User Story, it should progress until its end. We should avoid multiple WIP Features/User stories.

-   **When planning a new PHASE**, the team will decide what issues from **LATER** should be included in the **NEXT** phase. The team roadmap should be divided into multiple small, meaningful and FLEXIBLE phases. Ex: Prototype, Integrate X third-party, Add new payment provider, Extra Features, etc.

## Roadmap

Below you can see how we visualize and communicate the strategic plan.

### Public version

Our public roadmap is divided into three PHASES: Now, Next and Later.

EX:
- Foundation and discoveries (NOW)
- Basic functionalities (NEXT)
- Custom rules(LATER)

### Private version

Our team roadmap will be divided into multiple, small and FLEXIBLE phases.
- Prototype
- MVP
- Third-party integration
- User experience improvements
- Support multiple payment options
