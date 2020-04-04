---
Layout: post
classes: wide
section-type: post
title: "What is a site reliability engineer and how they differ from sysadmin"
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

{% include article_ad.html %}
## Sysadmin Approach to Service management

Historically, companies have employed system administrators or sysadmin to run their systems and some companies do follow this practice even today.
![sysadmin approach to service management]({{site.baseurl}}/assets/images/sysadmin.png)
Let's now look at the advantages and disadvantages of the approach shown in the above diagram.

**Advantage**
* Easy to implement
* Talent pool of sysadmins widely available
* Tools to integrate are also easily available
 
**Disadvantage**

* **Direct costs**

  * A manual intervention is required for change management or deploying code and also handling the events that happen after deployment.
  * This manual process becomes expensive as the traffic to the service increases. More traffic leads to more features being pushed out by developers regularly and additional load on the system which can lead to poor performance of the service.
  * The size of the operation teams need to be scaled with the additional load and increase in change management velocity.

* **Indirect costs**

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
 
{% include article_ad.html %}
## Responsibilities of a Site Reliability Engineer
 
In general, an SRE team is responsible for the availability, latency, performance, efficiency, change management, monitoring, emergency response, and capacity planning of their service(s).
 
**Engineering Work**
 
A SRE should spend at least 50% of their time in engineering work.
* Writing code for the services they own along with the developers
* Writing tools to automate manual work
* Writing documentation for the work they have done
* Working along with the development team to discuss ongoing issues and how the issues can be tackled
 
**Helping developers push changes frequently without voilating a service's SLO**
 
* Developers main goal is to push their code changes as quickly as they can to customers.
* SREs main goal is to keep the service stable in production.
* As 70% of the outrages are caused by a change in code or configuration, goals of the developers and SREs conflict.
* This conflict can be resolved with the help of error budgets.
* SREs and developers sit together to decide Service Level Objectives(SLO) for their services. We will discuss SLO in detail in future blogs.
* One of the SLOs can be to maintain 99.9% availability for the service. A service that’s 99.99% available is 0.01% unavailable. That permitted 0.01% unavailability is the service’s error budget.
* Development team can use this error budget to push changes fast to the customers.
* SRE’s goal is no longer "zero outages"; rather, SREs and product developers aim to spend the error budget getting maximum feature velocity.
 
**Monitoring**
 
Monitoring is one of the primary means by which service owners keep track of a system’s health and availability.
* Responsible for setting up alerts on their services.
   * Alert if the percentage of errors start increasing on their services
   * Alert if the latency start increasing on their services
* Tweaking the thresholds of these alerts from time to time depending on various changes happening around the service
 
**Emergency Response**
 
A SRE is also responsible for handling the oncall for their team. In most teams, oncall lasts for a week. The frequency of oncall depends on the size of the team. On an average, one should be spending around a week on oncall each month.
When you are oncall, you are responsible for the following:
* Handle all the alerts for the services your team owns, debug why alerts have fired, restore normal service and conduct post mortem if necessary.
* Oncall work is a very operation heavy task.
* As an oncall, you act as a shield to your teammates(SRE). If there are any ad hoc tasks, you are responsible to pick it up and not disturb other SREs who are focussing on engineering work.
 
{% include article_ad.html %}
**Change Management**
 
SRE has found that roughly 70% of outages are due to changes in a live system. As a SRE, you are responsible for:
* Implementing progressive rollouts - testing -> canary -> production
* Quickly and accurately detecting problems with the help of monitoring
* Rolling back changes safely when problems arise.
 
The above practices help in minimizing the number of users or services impacted by the bad change in the code.
 
**Capacity Planning**
 
Capacity planning is the process of determining the production capacity or number of servers needed to meet changing demands for the products. What happens if the development team ramps a viral product feature  and the service is not able to handle the increased demand?
In worst cases, your service may become unavailable if you do not do capacity planning for your service. In other cases, you may see increase in latency/errors for your service. These all will bring the availability down for your service.
 
Capacity planning should take both organic growth (which stems from natural product adoption and usage by customers) and inorganic growth (which results from events like feature launches, marketing campaigns, or other business-driven changes) into account.
Regular load testing should be done to find out the maximum qps that your services can handle before breaking down.
 
**Efficiency and Performance**
 
Efficient use of resources is important any time a service cares about money. What happens when your service latency crosses the target latency that was set up? Do you directly increase the number of servers?
For handling the issue in the short term, it would be a good thing to do. In addition to capacity planning and provisioning the required capacity, a SRE should also work with the developers to improve the performance of the service. This may include gc tuning if your service uses java.
 
{% include article_ad.html %}
## Who Might be the First SRE?
 
[Margaret Hamilton](https://en.wikipedia.org/wiki/Margaret_Hamilton_(software_engineer)), working on the Apollo program on loan from MIT, might be the first SRE.
 
* The codebase consisted of a program named P01 which wiped out navigation data. If this program was run by mistake during a real mission, it would have caused major issues.
* With an SRE’s instincts, Margaret submitted a program change request to add special error checking code in the on­board flight software in case an astronaut should, by accident, happen to select P01 during flight.
* But this move was considered unnecessary by the "higher-ups" at NASA: of course, that could never happen! So instead of adding error checking code, Margaret updated the mission specifications documentation to say the equivalent of "Do not select P01 during flight."
* During midcourse on the fourth day of flight of Apollo 8 mission with the astronauts Jim Lovell, William Anders, and Frank Borman on board, Jim Lovell selected P01 by mistake—as it happens, on Christmas Day—creating much havoc for all involved. This was a critical problem, because in the absence of a workaround, no navigation data meant the astronauts were never coming home.
* Thankfully, the documentation update had explicitly called this possibility out, and was invaluable in figuring out how to upload usable data and recover the mission, with not much time to spare.
* The change request to add error detection and recovery software to the prelaunch program P01 was approved shortly afterwards.
 
{% include article_ad.html %}
 
 
 

