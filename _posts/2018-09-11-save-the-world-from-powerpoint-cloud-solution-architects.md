---
layout: post
title: Save the world from Powerpoint Cloud Solution Architects
description: Cloud Solution Architects are responsible for transfering requirements and conditions into cloud optimized or cloud native architectures - we cannot stop before the level of implementation details
tags: architecture plantuml
published: true
---

I'm a Solution Architect, a Cloud Solution Architect - to be complete - a Microsoft Cloud Solution Architect for Azure Applications Development.

I'm developing solution architectures each week, with my customers, using Azure services, existing systems, partner offerings and common sense.

And unfortunately, in the past I've noticed more and more often that "Cloud Solution Architecture" stops before the level of implementation details.
<!--more-->

**Updated 12 Sep 2018**: Added [And what happens with Powerpoint?](#and-what-happens-with-powerpoint) and [Does a Cloud Solution Architect have to write code in order not to be a “Powerpoint Architect”?](#does-a-cloud-solution-architect-have-to-write-code-in-order-not-to-be-a-powerpoint-architect)

## Solution Architecture

> What does "solution architecture" actually mean? For some organisations “solution architect” is simply a synonym for "software architect", whereas others have a specific role that focusses on designing an overall "solution" to a problem, but stopping before the level at which implementation details are discussed.
>
> -- <cite>[Technical leadership and the balance with agility, Simon Brown](https://leanpub.com/software-architecture-for-developers)</cite>

Ok let's look into another definiton:

> A Solution Architecture typically applies to a single project or project release, assisting in the translation of requirements into a solution vision, high-level business and/or IT system specifications, and a portfolio of implementation tasks.
>
> -- <cite>The Open Group Standard. TOGAF™ Version 9.1 2009. p. 31</cite>

That's fine for me - so let us assume that *Cloud* infront means, we are responsible for translating the requirements into a cloud or hybrid architecture.

## Cloud Solution Architects (CSAs)

Let us take a look into my employer's - Microsoft's - definition of responsibilities for CSAs.  
I think it is very clear:

> You will own the Azure (Applications Development - for my specialization) technical customer engagements including:
>
> * Architectural Design Sessions (ADS)
> * Architectural Design Reviews (ADR)
> * Proof of Concepts (PoC)
> * Pilots
> * and specific implementation projects
>
> You will lead deep technical architecture discussions with senior customer executives, Enterprise Architects, IT Management and DevOps to drive Azure (Application Development & DevOps solutions - for my specialization)

In my heart I am a software architect, and I think that is exactly what stands behind a "Cloud Solution Architect for Azure Applications Development".

In my point of view it is the same situation with all the other specializations:

Cloud Solution Architect for Azure Infrastructure
  : IT/Infrastructure/Network Architect

Cloud Solution Architect for Azure Advanced Analytics & AI
  : Data Architect

We all have one thing in common: we are spezialized in Azure, and we are deep technical in our area.

## Powerpoint Cloud Solution Architects

I really like the phrase [Software Architects who don’t write code are “Powerpoint Architects”](https://accidentaltechnologist.com/software-architecture/architects-who-don%E2%80%99t-write-code-are-%E2%80%9Cpowerpoint-architects%E2%80%9D/) and would like to expand it.

If we as *Cloud Solution Architects* are responsible for transfering requirements and conditions into cloud optimized or cloud native architectures, and if we are already specialized in different technical areas, **we cannot stop before the level of implementation details**.

But the reality is different...

If someone wants to communicate or to discuss a *Cloud Solution Architecture* these days, you will not get a lot of implementation details.

You'll likely get a confused mess of nice symbols, boxes and lines.  
 Including inconsistent notations with colours, shapes, line styles, ambiguous naming, unlabelled relationships, generic terminology.
 
 And worst of all: missing technology choices all over the place and completely mixed abstractions.

 That is the current situation with

 * [Azure Solution Architectures](https://azure.microsoft.com/en-us/solutions/architecture/)
 * [Azure Reference Architectures](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/)
 * [AWS Solution Architectures](https://aws.amazon.com/blogs/architecture/tag/solutions-architecture/)
 * [AWS Reference Architectures](https://aws.amazon.com/architecture/#aws-ref-arch)

 and with 90% of other cloud providers, customers and partners I engage with.

 > **"Cloud Solution Architects" stopping before the level of implementation details are “Powerpoint Cloud Solution Architects”**

## C4 Model for Cloud Solution Architectures

If you want to design, communicate or discuss a *Cloud Solution Architecture*, you will need a visual representation of it.

You can use Unified Modeling Language (UML), ArchiMate and SysML for it.  
But the reality is, that many teams and architects have already thrown them out in favour of much simpler "boxes and lines" diagrams.

I personally feel the same way because I need to work with many different companies, partners and roles.  
I cannot expect that everyone has specific tools, software or even the same operating system.  
And I cannot expect that everyone in the room has the same level of experience and knowledge regarding UML as I have.

That is the reason I am promoting the [C4 model](https://c4model.com/). It defines a common set of abstractions to create an ubiquitous language that we can use to describe the static structure of a software system.  

The notation itself is not prescribed, but a recommendation for a [simple notation](https://c4model.com/#notation) that works well on whiteboards, paper, sticky notes, index cards and a variety of diagraming tools is defined.

## PlantUML, C4-PlantUML and Azure-PlantUML

[PlantUML](http://en.plantuml.com/) is an open source project that allows you to create UML diagrams.  
Diagrams are defined using a simple, intuitive human readable text description.

I really like it because it is free and OSS (versions with GPL, LGPL, Apache, Eclipse Public or MIT license available) and runs on any operating system.

To make it more accessible, I decided to develop two OSS projects:

* [C4-PlantUML](https://github.com/RicardoNiepel/C4-PlantUML) - combines the benefits of PlantUML and the C4 model for providing a simple way of describing and communicate software architectures
* [Azure-PlantUML](https://github.com/RicardoNiepel/Azure-PlantUML) - PlantUML sprites, macros, and other includes for Azure services

Something like [this](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/scalable-web-app) (nobody understanding the lines, no technology decisions except which Azure service is used, etc.):

![Azure Ref Arc - Highly scalable web application](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/images/scalable-web-app.png)

Could then be visualized like this in a C4 container diagram:

![Azure-PlantUML - Highly scalable web application](http://www.plantuml.com/plantuml/png/dLLDRziu4Bq7o7-OzG8BBU3ORThJdkh8GJusTLpBtc9F1YNHCZSKgVB3hlFhEwH4MKwS8DcBDHhDcna-lXb_qOOeOqj-Ud9wyXiJ6RSvjOh3sfXA_pKSAh8T5CoiRMerLPaKXWepo6GvdB6Cg5nE6Aqe7yQVpwQS2BFuUZSiYJPKeMiPOpnXWgf15MhaT8KE63rQHQVeV7SbZtnMB6VQ79wWhL0ZObcMKZHz78yR4qDIegck4JEAIMEIhUbxKB7KfTJZz3sOv0SFdbTXtsbQ0z0OLTNROkFnv6s12IKgMxyQcnYfI47h-0ikqyRnTj0tIoqzH9sUt8t-lL_CbzzlvjVBkykhoydOUQTKQIaYfuO-z1RuwFtf_Ase8bqH_98mKN1K16O4PAM7hf4eB2S8kqqSfiDPjFKFZC5t-0D6qviMb4m7ayivn59H-FfF3EU3Zts8VtVVnfIp3VO3PaqXcOw0Y1mg9JSifnfcbybyPJbWRfTGmtRDiZKGp8CwrhQaEQoKB0EwxGdl9ifoWGNGyZqya98mICxTI-Qqfw6oAMSPtm7P4CP9odrMDnsmaWgut5By9UZ8ThwlbHy9oEkUTo1N0L0J-FJs8bm2XcKTZsFL-o1kWEYveiWvuJ2rgfAQmkQy3zVIzE5kbdGITsyu-k5U4EkTBoLfy1qHHje_N1lUW1kHqv-zkYsHupQ0vADxRQ-W1e2sHK6rOSXw7rRsyN478PLZ1fT6OR6lCrtHZAtmVE6hkoV9MOuqQY2WlQP2LkQgrpTOMcvOXPym4iFmc5ItpUytHgHQqrskUnvq7vzt60ih8yeQmmbEPYWRlEBnXGT-ktFAwiFd3nzWjjYNxAoEIAETDi4Dh6NhKwVUVX9kPTfC0bgzsaHBGpB-6pQVWlgL5Hfr9eepj0HI0uRe1usyU4Md97iebBGYhulCA6-cjXvG75tjqbpFvrD_q_k0TXfVYQa7v5c03rbEA2lMgMnsmRDqjydz15NQIRgym6vVWeKnFvBVTt5iK6QKvDgBOt_wa338VsgDxj04_pcEcJ0od3G0mXNkathTRXLpkiHujoF6zsvFFOLsnFCQkb7IK_I1Nb_4F-qsJEaH3fQBLc21XIkAB0ZwqxZpjz67iUyde0F-NnHqy6ycuPYIziLRMYuiuxdkh8zGSY-_9ZGS-oW2_o39RZbkW-js_OSlLEJu9_eB "Azure-PlantUML - Highly scalable web application")

## And what happens with Powerpoint?

Or to reframe this question:
> Does a Cloud Solution Architect still need very simplified architecture visualizations?

**Yes - of course!**

Like every other architect in our and other industries, Cloud Solution Architects should be able to communicate on the same level as the their audience.  
Thus also any architectural visualization and documentation has an intended audience.

Something I am seeing a lot is the situation that Cloud Solution Architects have more meetings, discussions and presentations with senior executives as more traditional technical (software, IT, network, data) architects.  
Thus it is still essential to be able to have the right abstractions for them to be able to present, discuss and defend solution architectures on this level.

And in this situations it could be, that something like the [C4 model](https://c4model.com/) or the [“4+1” View Model of Software Architecture](http://www.cs.ubc.ca/~gregor/teaching/papers/4+1view-architecture.pdf) is not the right thing to use.  
But especially in this situations it is essential to have a clear understanding what these "boxes and lines" really means.

On the other hand - if our audience are technical people - it should show this technical level and all the technical decisions that have already been made or are yet to be done.

## Does a Cloud Solution Architect have to write code in order not to be a "Powerpoint Architect"?

That is a very dangerous discussion - which I had with many people after the first version of this article.

*Special thanks to all of them for the input they gave!*

And it is also not so easy as the heading shouts it out.

<dl>
  <dt>Cloud Solution Architect for Azure Infrastructure</dt>
  <dd>Does she/he writes code? Mmhhh...only if PowerShell scripts are code...</dd>
  <dd>But writing Infrastructure as Code, deployment automation, chaos testing, using all these tools like code ARM templates, Ansible playbooks, Docker manifests, Spinnaker deployment plans, etc. seems reasonable</dd>

  <dt>Cloud Solution Architect for Azure Advanced Analytics & AI</dt>
  <dd>Same question applies...</dd>
  <dd>But doing data engineering, automating data integration, automating data preparation, defining data models, conducting ML experiments, writing SQL/Gremlin queries and Spark streaming jobs, etc. seems reasonable  </dd>
</dl>

or for all

<dl>
  <dt>Cloud Solution Architects</dt>
  <dd>choosing the right technologies, tools, script and programming languages</dd>
  <dd>deciding the correct communication protocols and messaging patterns</dd>
  <dd>determine the right performance and feature SKUs and manual/scaling requirements</dd>
  <dd>gathering all requirements and defining business continuity and disaster recovery plans</dd>
</dl>

It's not really about writing source code - it's more about getting your hands dirty.

I really like that Microsoft has redefined the CSA role last year into a deep technical role.  
I also love to see this inside our personal Core Priorities we have:  
**Customer Focus, Customers' Trusted Advisor, Customer Technical Enablement, Deep Technical Expertise, Technical Thought Leadership**.

For our biggest and high potential customers, we as CSAs are - in each of our technical Azure spezializations - the **"Primary Local Tech Resource"**.

*I think there is nothing left to add.*

## Feedback

Please give me your feedback and share your progress.

* [@RicardoNiepel](https://twitter.com/RicardoNiepel)
* [#C4PlantUML](https://twitter.com/hashtag/C4PlantUML?src=hash)
* [#AzurePlantUML](https://twitter.com/hashtag/AzurePlantUML?src=hash)

## Acknowledgments

* [C4 Model](https://c4model.com/) / [Simon Brown](https://twitter.com/simonbrown) - for the hope that it's possible to improve architecture visualizations and documentations