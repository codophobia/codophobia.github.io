---
Layout: post
classes: wide
section-type: post
title: "Difference and Similarities Between Devops and SRE"
category: sre
description: "In this post, I will discuss about similarities and difference between devops and sre"
tags: ['sre']
---
{% include article_ad.html %}
 
In my [previous](https://shivammitra.com/sre/what-is-a-site-reliability-engineer/) post, I discussed about what is site reliability engineering, how SRE differs from sysadmins and what are the responsibilities of a SRE.
In this post, I will focus on highlighting similarities and differences between DevOps and SRE. Is DevOps the same as SRE? You will hear a lot of people asking this question. I hope that this post helps you in finding the answer for it.
 
## What is DevOps
 
DevOps was formed by combining words "development" and "operations". As the name suggests, it aims at bridging the gap between the development team and the operations team.
In my [last](https://shivammitra.com/sre/what-is-a-site-reliability-engineer/) post, I discussed the sysadmin approach to service management and what are the disadvantages of using such approach.
The incentives and goals for the development team and operations team are quite different. Developers want to push new features quickly to the customers whereas an operation engineer is responsible for keeping the services healthy and stable.
Since around 70% outages are caused by some change in code or config, their goals start conflicting.
 
DevOps is more of a culture or a philosophy which aims at better collaboration between the developers and the operations team, automation of manual tasks and implementation of continuous delivery and continuous deployment.

{% include article_ad.html %}

## Key Pillars of DevOps Philosophy
 
1. **Reduce organizational silos**
   * Conflicting goals for developers and operators
   * Can be very bad for the organization
   * Can be reduced by increasing collaboration between development team and operations team
2. **Accept failure as normal**
   * Operation team focused on keeping the systems stable
   * Failure of a service in production was seen as negative by the organization
   * Find the operator/developer who made the mistake and punish them
   * Focus on reliable testing, fast detection and recoveries from failures
3. **Implement gradual changes**
   * Remember, 70% outages are caused by a code or a config change
   * Pushing 1000 new lines of code at once vs pushing it in 10 separate deployments. Which one is better?
   * If failures happen, very tough to detect the cause using first approach
   * Make gradual change, test it and have rollback mechanism
   * Use change management approaches like continuous integration(CI) and continuous deployment(CD)
4. **Leverage tooling and automation**
   * Developers should be able to deploy changes directly to production.
   * In traditional approach, this was being handled by the operators
   * Change management approaches like CI/CD require specific tools(git, jenkins, kubernetes etc.)
   * Toil reduction should be an important goal for DevOps
   * Installing a specific package manually on 100+ servers vs using a tool to automate it?
   * Watching services graphs manually vs setting up alerts on top of graphs?
5. **Measure everything**
   * How do you know whether the first four pillars were implemented correctly in your organization?
   * Without data, the efforts will go into waste.
   * How long does it take from submitting code changes to deployment?
   * How often do the failures happen for different services?
   * How long does it take to recover from failures?
   * How many people are using your service? Are your users increasing or decreasing?

{% include article_ad.html %}

## What is SRE
 
Site Reliability Engineering (SRE) is a term (and associated job role) coined by Ben Treynor Sloss, a VP of engineering at Google.
> SRE is what happens when you ask a software engineer to design an operations team - Ben Treynor, founder of Google’s Site Reliability Team
 
If you think of DevOps as a philosophy and an approach to working, you can argue that SRE implements some of the philosophy that DevOps describes.
SRE follows a set of practices to implement the principles of DevOps.

{% include article_ad.html %}

## How SRE implements DevOps Principles
 
1. **Reduce organizational silos**
   * A SRE uses software engineering approaches to solve operations problem
   * A SRE should at least do 50% engineering work
   * Sharing ownership of services along with the developers
   * SREs use the same tools that developers use and vice versa
   * SREs are also from computer science background
   * Collaboration between SREs and developers
2. **Accept failure as normal**
   * SREs do not aim for 100% availability for a service
   * Product development team and SREs select an availability target for the service
   * This is called Service Level Objective(SLO).
   * Availability target - 99.99% implies Error budget - 0.01%
   * Error budget can be used to push new features quickly
   * If failure happens, conduct blameless postmortem and ensure that the issue doesn't happen again.
3. **Implement gradual changes**
   * Reducing cost of failures
   * Deployment approach : Staging -> Canary -> Production
   * Canary is pushing new code changes to a few production instances
   * Rollback mechanism to minimize the impact of failures
   * Quickly detect problems with the help of monitoring
4. **Leverage tooling and automation**
   * Eliminating toil is one of important responsibilities of a SRE
   * if a machine can perform a desired operation, then a machine often should.
   * A SRE should spend less than 50% of their time on operational work.
5. **Measure everything**
   * Monitoring the stability and health of the service using dashboards
   * Setting up and tuning alerts to know about failures and recover fast
   * Measuring the toil
   * Measuring SLOs

{% include article_ad.html %}

## Similarities between DevOps and SREs
 
* Both try to bridge gap between development team and the operations team
* Both share ownership of service with the developers
* Both believe in implementing gradual changes and follow change management approaches like CI/CD
* Automation is an integral part of job for both
* Measurement is absolutely key to how both DevOps and SRE work.
* Both accept failures as normal and practice blameless postmortem

## Difference between DevOps and SREs
 
* Philosophy vs Implementation
* What needs to be done vs How it needs to be done
* Business oriented vs Service oriented
* Delivering infrastructure for services vs performance and reliability of services
* SRE believes in the same things as DevOps but for slightly different reasons
* CI/CD - supporting because of business use case vs supporting for increasing reliability
 
{% include article_ad.html %}
    
 

