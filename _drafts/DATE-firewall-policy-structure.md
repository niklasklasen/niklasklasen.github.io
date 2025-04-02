---
layout: single
title:  "Structure Your Azure Firewall Policy Rules"
categories: 
  - Azure
  - Azure Firewall
toc: false
show_date: true
---
As with most things in life you need structure in your Azure Firewall Policy rules. Hanving a plan for creating your rules in Azure Firewall Policy can (and will) help you when it comes to creating new rules, claning up old ones, or in general connectivity troubleshooting. If you are about to create you first rule for your Azure Firewall take a minuite and design a structure, because redesigning the rule structure later will probably be more time consuming that getting it right the first time. There are ofcource multiple ways how you can structure the rules within your Azure Firewall Policy, the concept that I will show you is one that I have implemented and find easy to use when maintaining an Azure Firewall.

This blog post will focus on how to structure your rules within an Azure Firewall Police, not how to plan when using multiple Azure Firewall Policeis where you have a parent policy and multiple child policies.

# Scenario
A common network design in Azure is the hub-and-spoke. You have a central connectivity hub that contains shared services for the platform, like your Azure Firewall, and then the application teams deploy their solutions in different spokes. This means that your central firewall will manage both east-west and north-south traffic. This will most likely mean that your central firewall needs alot of rules. The central firewall will be one of the key pices that segments your network, and you only want to let traffic through the firewall that you actually need in your environment. 

<IMG HUB-AND-SPOKE>

