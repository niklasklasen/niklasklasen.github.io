---
layout: single
title:  "Draft Post"
categories: 
  - Azure
toc: true
show_date: true
---
Today the 10th of October 2023 Windows server 2012 and 1012 R2 goes "End of Support". That means that they received their last Security and Critical update today. Are you behind on your upgrade project and still have a few old servers in your data center that hasn't been upgraded yet? Then this blog post will give you some guidance on how to prolong the Security updates for another 3 years for these servers. 

# Extended Security Updates
Extended Security Updates (ESUs) are available to servers hosted in Azure free of charge, but for servers hosted in other places than Azure (i.e. On-Premises) ESUs available for purchase. Then you need to onboard these servers with Azure Arc, but more on that later. 

Wether your Windows Server 2012 or 2012 R2 are hosted in Azure or elsewhere, ESU will extend your Security and Critical updates to the 13th of October 2026. That's three more years for you to complete your upgrade project. But I would still recommend hurrying up, since updates outside of the classifications Security and Critical won't be delivered to your 2012 and 2012 R2 servers.

For Azure hosted servers there's no extra steps to take, ESUs are included and you can move straight to the step of setting up automated patching with Azure Update Manager. 

> **_WARNING:_** After the period for Extended Security Updates ends on the 13th of October 2026 there will be no more updates for Windows server 2012 and 2012 R2. 

## Licensing


## Licensing Examples 
// Include link to MS Learn

# ESU with Azure Arc

## Prerequisites

## Buying through the Azure portal

## Get updates through Azure Update Manager
// Security & Critical Offset to Patch Tuesday