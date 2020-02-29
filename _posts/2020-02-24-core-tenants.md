---
title: "A Building is Only Good as its Foundation"
date: 2020-02-28
tags: [Data Science, Team Foundation]
excerpt: "The guiding principles of our data science team and how we employ them."
---
When my team was first formed, I wanted to establish a series of guidelines on our operational philosophy.
I wanted to avoid a typical mission statement as often times they become buzzword bingo and ultimately
meaningless.  So, we developed a series of core tenets.  My goal was that when one of our customers
read the core tenets they understood how we would approach their problem.  Also, these tenets would
guide us and we began a project.  

![Core Tenet]({{site.url}}/images/blog/2020-02-25/core_tenants.PNG)

**Requirement Driven**
I have seen many development and data science projects spiral out of control due to scope creep
and the dev/data team losing sight of what the customer actually needs.  This can lead to the self
licking ice cream cone of data science, where the team is not building a project to satisfy a need,
but rather is building something because it is cool.

An emphasis of this tenet is the customer defines the requirement, the data science team works to understand
the requirement and how to build a solution for it.  The data science team works to understand this
requirement though a series of guided questions to gain a full understanding of what is needed.
The data science team will then back brief the customer to fully ensure all parties are working
towards the same goal.

**Enable Not Replace the Analyst**
This tenet is more focused to military customers.  As artificially intelligence become more prevalent
there is a fear among commanders that people will try to replace the analyst with an algorithm.  This
concern is largely due to the cost of failure.  For example, if a spam filter mislabels an email there
is little impact.  However, if a civilian vehicle is misclassified as a tank by an algorithm and a commander
calls for a precision strike on the vehicle an incent person could be killed.  Granted this is an extreme
example, but it does represent a mistrust in technology most commanders rightfully have.  The solutions
 provided will not replace the analyst in critical decisions, but rather will assist the analyst in handling
 the large amounts of data so the analyst can employ critical thinking.

 **Micro-Focused**
I have lost count of the projects I have seen that take so long to deliver the customer has moved on to another solution or the original problem no longer exists.  This happens even more in the military where a mission may only last a few months.  These were some of the reasons the team initially decided to be micro-focused.  We wanted to be able to develop and deploy a solution while the problem was still relevant to the user.  By keeping projects focused we also are able to avoid scope creep.  This also helps manage our resources to ensure we are not going over cost or over time.

**Simple Model Selection Focused**
We need to be able to inform our decision makers while the model provided the predictions it did.  A perfect model is no good if a decision maker does not have confidence in it.  This tenet mirrors the Enable not replace tenet, in that decision makers sometimes have limited trust in technology.  To ensure the people using our models have confidence in them we like to use simple models when possible.  

Another benefit of simple models is there ability to train and perform predictions quickly on computers that may not be top of the line.  If I am able to train a model on an older system or a smaller AWS instance for less cost and time and there is little to no difference between that model and a more complex model, well the simple model wins.  

**Open Source Solutions**
One thing I have learned doing data science for the Army is that everyone seems to have a product that will fix your every need, if you are willing to pay.  The problem is there is rarely enough money to do everything and the acquisitions process is slow.  Due to this it is important to use open source solutions as much as possible.  The are the right price and the acquisition process is generally pretty quick for free things.  It is also important that once the product is in use there are no hidden costs for the user.  

I fully understand these tenets are not perfect and not all encompassing, but they provide a solid foundation for us to plan and build our projects around.  
