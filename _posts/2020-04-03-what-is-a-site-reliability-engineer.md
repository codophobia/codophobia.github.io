---
Layout: post
classes: wide
section-type: post
title: "What is a site reliability engineer and how they differ from sysadmin and devops engineer"
category: sre
description: "In this post, I discuss about what is a site reliability engineer and how a SRE differs from sysadmin and devops engineer"
tags: ['sre']
---
{% include article_ad.html %}

> "Software engineering has this in common with having children: the labor before the birth is painful and difficult, but the labor after the birth is where you actually spend most of your effort. Yet software engineering as a discipline spends much more time talking about the first period as opposed to the second, despite estimates that 40–90% of the total costs of a system are incurred after birth." - Site Reliability Engineering: How Google Runs Production Systems
 
* It is wrong to assume that code once deployed doesn't need much attention from the software engineers. If the services break down in production, it can cause huge loss to the company.
* Software engineers focus on designing and building softwares.
* But who will focus on keeping the service up most of the time? What happens when the service breaks down in production due to some code changes? Software engineers?
* The knowledge required for building softwares and operating it are different. Even if software engineers have the knowledge, the burden on them will increase.
 
Who then should be responsible for keeping the service up most of the time? Let's first look at the traditional approach of deploying services and operating them.
 
## Sysadmin Approach to Service management
 
Historically, companies have employed system administrators or sysadmin to run their systems and some companies do follow this practice even today.
 
![sysadmin approach to service management]({{site.baseurl}}/assets/images/sysadmin.png)
 
Let's now look at the advantages and disadvantages of the approach shown in the above diagram.

{% include article_ad.html %}
**Advantage**
 
* Easy to implement
* Talent pool of sysadmins widely available
* Tools to integrate are also easily available

**Disadvantage**
 
**Direct costs**
   * A manual intervention is required for change management or deploying code and also handling the events that happen after deployment.
   * This manual process becomes expensive as the traffic to the service increases. More traffic leads to more features being pushed out by developers regularly and additional load on the system which can lead to poor performance of the service.
   * The size of the operation teams need to be scaled with the additional load and increase in change management velocity.
 
**Indirect costs**
   * The two teams, development and operation team are quite different in background, skill set and incentives.
   * Developers are from computer science background vs operators from networking background
   * The development teams want to launch new features regularly and push it to the customers. If developers don't complete the jira tickets assigned to them, they may get fired.
   * The operations team wants to make sure that the service doesn't break. If the operations team fails to keep the site up regularly, they may get fired.
   * Since most outages are caused by some kind of change, the operations team resists making new changes to the stable system. Thus, both the teams end up in conflict.
 
What if we can follow a different approach to run the systems? What if we can come up with software solutions to the above problem?

{% include article_ad.html %}

## What is a Site Reliability Engineer or a SRE
 
> SRE is what happens when you ask a software engineer to design an operations team - Ben Treynor, founder of Google's Site Reliability Team
 
* SREs are engineers who have sound knowledge of the principles of computer science.
   * Write code along with our development counterparts
   * Write code for automating certain manual stuff - monitoring, deploying, failout etc.
   * Figuring out how to apply existing solutions to new problems - caching
   * Review the design of new products and features along with the development team - is the product resilient to failures?
* SREs focus on system reliability
   * If a service is not reliable, nobody is going to use the service.
   * A service that consistently performs as it is expected to do is considered a reliable service.
   * If some service is not reliable, it may lead to users moving to similar services provided by the competitors.
* SREs operate services built on top of distributed systems like storage, tools, networks etc.
 
One of the primary focuses of a SRE is towards making services reliable. In the next blog, we will go through different responsibilities of a SRE which are linked to keeping the service highly reliable.

{% include article_ad.html %}
 
 

