---
layout: single
title:  "Don't cheap out on Subscriptions"
categories: 
  - Azure
toc: true
show_date: true
---
I have seen envrionments in Azure that are built around one or two subscriptions, usualy one for Production and one for Development/Testing. In these cases Resource Groups was used to provide some strucure in these Subscriptions. This is limiting the flexibility in how we can structure our environment and enforce a scalable governance framework. I am aware that this was the standard a couple of years back and that Microsoft preached this as the best practise when building your Azure environment. But as alot of other things in Azure the view on this has changed as well. 

**IMAGE ELS

In the Cloud Adoption Framework (CAF) we are introduced to Enterprise Scale Landing Zone architecture. (fig. 1) There are two things that clearly dfferentiates from the old model of gathering all resources in to two Subscriptions. 
  1. We are introducing the Management Group layer on top of the Subscriptions
  2. We see multiple single purpouse Subscriptions.
This allows you to enforce a governance framework on a more granular level. In this post we will take a look at how we can work with Management Groups and Subscriptions to help us with enforcing a flexible and scalable governance framework.

Frist of there are a few limits to know when we are working with Management Groups and Subscriptions. I one tenant you can deploy NUMBER of Management Groups and they can be 6 levles deep. Not including the Tennant Root or Subsciptions. There is also a limit on how many Subscriptions you can create, today that limit is NUMBER. So most organizations won't be effected by these limits.

*** IMAGE MGMT GRP & SUB LIMITS

Management Groups
Management Groups are used to hirarki and arease in the way we apply our governance framework. Everything you assign to a Management Group, wheter it's policies or RBAC role assignments they will be inherrited vertically down in the structure. They do not inherrit horizataly in the structure. As an example a RBAC role assignement added to the Management Group "Platform" will be inherrited to the Management Groups "Identity", "Management" and "Connectivity" and all the subscriptions that resides within them. How ever it will not be applied to the Management Group "LandingZones". 

The structure presented in the Enterprise Scale Landing Zone architecure is designed to scale with you through out your cloud journey. Think both one and two times before changing or adding two much to the design. I have seen cases where Management Groups are created to mimic the companys organizational structure or their country based footprint, that solution tends to run in to a scaling problem at some point. The guardrails that we set up with Azure Policies will in most cases span over multiple teams and countries since they are focues on what resource you are allowed to deploy or how the must be configured. If you take an application that is intendend to be connected to your central connectivity HUB, it doesen't matther what country the users are in. We must enforce our policies that applies for applications that whant to access our internal network. 

Subscriptions
Don't hold back when the organization ask for new subscriptions to build their applications in. If you have your Management Groups in order and the Azure Polices in place, adding extra subscriptions won't be a burden for you. Rather see them as container where your developers or other users can develop och try out new things. If the thing that they tried out works you already have it in a separe container that is organized with the correct governance rules, and it's a product that dosen't work you can just decommision the whole Subscription.

*** NOTE
Be very causious when handing out the RBAC role "Owner", even on Subscriptions. As an Owner you can migrate the Subscription to another Tenant. The effect would for your organization will be that you are still carrying the cost for this Subscription but you won't be able to see it. Even wors, all the guardails you implemented with RBAC and Azure Policies will not effect the Subscription once it's moved out.

So the recomendation I give to the companies that I work with are to use one subscription (or two, ptoduction and development) for each of the applications or workloads that are built or moved in to Azure. Give the application owners the freedom they need to build and test their application without worring that they will break something else in the environment. That sense of security goes both ways, for both the application team and the IT department responsiblr for the Azure environment. 

As of today you can give an application team the RBAC role Contributor without them beeing able to alter or remove any policies thet the subscription needs to be compliant to, and they can't move the subscription out from your tenant. When you have this structure in place along with a good RBAC strategy and policy framework, your IT department can breath a little easier whil still allow agility for the deveplopers. 
