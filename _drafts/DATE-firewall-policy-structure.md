---
layout: single
title:  "Structure Your Azure Firewall Policy Rules"
categories: 
  - Azure
  - Azure Firewall
toc: false
show_date: true
---
![](/assets/img/fw-rule-clean-up.png)
As with most things in life you need structure in your Azure Firewall Policy rules. Hanving a plan for creating your rules in Azure Firewall Policy can (and will) help you when it comes to creating new rules, claning up old ones, or in general connectivity troubleshooting. If you are about to create you first rule for your Azure Firewall take a minuite and design a structure, because redesigning the rule structure later will probably be more time consuming that getting it right the first time. There are ofcource multiple ways how you can structure the rules within your Azure Firewall Policy, the concept that I will show you is one that I have implemented and find easy to use when maintaining an Azure Firewall.

This blog post will focus on how to structure your rules within an Azure Firewall Police, not how to plan when using multiple Azure Firewall Policeis where you have a parent policy and multiple child policies.

# Scenario
A common network design in Azure is the hub-and-spoke. You have a central connectivity hub that contains shared services for the platform, like your Azure Firewall, and then the application teams deploy their solutions in different spokes. This means that your central firewall will manage both east-west and north-south traffic. This will most likely mean that your central firewall needs alot of rules. The central firewall will be one of the key pices that segments your network, and you only want to let traffic through the firewall that you actually need in your environment. 

<IMG HUB-AND-SPOKE>

# Rule Collection Groups
The structual hirearchy in an Azure Firewall Policy consists of three levels, where the top level is rule collection groups. Out of the box there are 3 included rule collection groups, they are not visible in the portal but will be as soon as you add rule collections (second level) to them. These are:
- DefaultDnatRuleCollectionGroup
- DefaultNetworkRuleCollectionGroup
- DefaultApplicationRuleCollectionGroup

I use these for global rules, rules that aren't bound to a specific teams need but a general need for the platform. As examples, allowing DNS request to or from on-premises DNS servers, or if you have a shared service deployed that you want the whole platform to be able to reach.

In addition to these default rule collection groups I create one for each department och application team that is running or building workloads on the platform. This gives you a nice and clead structure to work with when you are looking through the rules in your Azure Firewall Policy. 

<IMG RULE-COLLECTION-GROUP-STRUCTURE>

# Rule Collection
This is the second level in the structual hierachy with in Azure Firewall Policies, and these are grouped in to Rule Collection Groups. Rule Collections are defined to be either for Network Rules or Application Rules, and you specify if the rules in the Rule Collection is Allow or Deny rules. I tend to avoid using Allow rules since Azure Fiewall is explicit deny, meaning if no particular rule is created to allow the traffic through it is denied by default. 

>**NOTE:** If you want to make sure that speciffic IP addresses or ranges aren't allowed you could add speciffic deny rules for theses, just make sure that they are in a Rule Collection Group with a low priority number. This will make them the first rules that your firewall evaluates and if they triggers the traffic won't be evaluated by any other rules in the Azure Firewall Policy. The purpose of this is to make sure that these IP addresses will be blocked even if they are included in an allowed range by another rule with a higher priority number.

So, how do I structure Rule Collections? First of I create global Rule Collection under the corresponding Rule Collection Group, as an example:

*Global-NetworkRules  -->  DefaultNetworkRuleCollectionGroup*

Then for each of the team or department I create Rule Collections multiple Rule Collections for the different environments (production, development, etc.) and for each I create Rule Collections for network rules and application rules. The result can look something like this: 

<IMG RULE-COLLECTION-STRUCTURE>