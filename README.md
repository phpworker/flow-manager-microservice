# flow-manager-microservice
Flow Manager Microservice (FMM) is example microservice written using Symfony4

**The big picture**

This service is mainly created for learning purposes. I have had some experience with Symfony2 but none with Symfony 3 and 4. 

Imagine that you have multi-step e-commerce form that is splited into separate pages done in React or similar frontend framework. And these pages are able to talk to backend microservices to store and retrieve their data, etc. 

Now, let's say that business needs to A/B test the form, but without step 3, and then they need to check if it is better to switch step 2 and 3 with each other. See the point?

Usually, such requests would eventually lead you to another monolith application, full of config switchers, extensions, etc. My approach here is to have another microservice that would be able (when properly asked) tell what and where is your next (and prev) step. 

In other words, when user clicks "Next Step" link, then it will call that step related microservie. Next, this service will ask FMM for next steo that user should be redirected to and it will redirect to it.

That way we have flow separated from each step or page of the form. We just need to tell the first page for which flow we want to use it and pass that info further.

**What I want to learn**

- S4 (Symfony4) architecture, esp. configuration
- making S4 microservice up'n ready, creating own boilerplate
- Design Patterns and PHP7 features

**MVP Definition**
- MUST-HAVE: configuration architecture allowing to manage separate projects, with multiple flows (request params)
- MUST-HAVE: endpoint for responding with next/prev step link through POST protocol
- MUST-HAVE: REST architecture
- COULD-HAVE: JWT authentication

**Resources**

- Udemy courses: 

1. [Create the REST API in PHP Symfony Complete Class](https://www.udemy.com/creating-rest-api-in-symfony/learn/v4/content), 

2. [Learn PHP Symfony 4 Hands-On Creating Real World App](https://www.udemy.com/learn-symfony-4-hands-on-creating-a-real-world-application/learn/v4/content), 

**Learning Plan (each step for less than hour)**

- follow relevant materials from sections 1-4 from Udemy course [1]
- create microservice quickstart boilerplate and push it as separate repository
- add routing, handler, model for POST request
- find best way on how to describe flows of the client application in yaml config file
- add config file with a solution for reading from it
