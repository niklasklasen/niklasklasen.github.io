---
layout: single
title:  "Azure Update Manager goes Generally Available and Dynamic Scopes is in Preview"
categories: 
  - Azure
toc: true
show_date: true
---
It's been half a year since I wrote my first post for this blog about the product "Update Management Cetner" which was in preview back then. A couple of days ago Microsoft released the product as Generally Available under the name "Azure Update Manager", read more in the release post on Microsoft Techcommunity here. #LÄNK# The release post mentions the new price for Azure Update Manager now when it moves in to Generally Avilability with out going in to any more details than Azure or Azure Stack HCI hosted Virtual Machines will remain free, and Azure Arc-enabled servers will cost up to $5/month per server. So lets shed some light on that. 

The cost is based on the amount of days that the Arc-enabled server is considerd as "managed by Azure Update Manager", and a server is considerd as managed if the folowing two conditions are met:
1. The Arc-enabled machine has the status "connected" when an operations takes place, see condition 2. 
2. An assess operation is triggerd no matter if it's manual or automatic, or a patch operation is triggerd either manual or by a schedule. 

The cost for each day that these two conditions are met are $0.167 per day. So if you have enabled "Periodic assesment", this will perform a check for new updates every 24h and if your machine is connected this will count your Arc-enabled server as "Managed by "Azure Update Manager" and trigger the a cost of $0.167. If this happens every day in the month it will land at around $5. But there are two scenarios where the cost for running Azure Update Manager operations on your Arc-enabled VMs are included. 

If you buy Extended Security Updates (ESUs) for your Arc-enabled servers, manage the patching with Azure Update Manager there will be no charge for Azure Update Manager. Also if you have your Arc-enabled servers onboarded to Defedner for Servers Plan 2 (Plan 1 won't cut it) ther will be no extra charge for utilizing Azure Update Manager. 

### SOME IMAGE ###

A key resource to get the Patches installed on your servers using Azure Update Manager is the Maintenance Configuration. I have explaind more on how to create Maintenance Configurations on a previous blog post. so I wont go in to details on that. If you would like to read more on creating Mainenance Configurations read this post instead ### LINK TO OLD POST ###.

For automatic onboarding of servers to Mainetnance Configurations I used Azure Policies and defined what parameters I needed for specific Schedules. If an organization had multiple windows and a global presence, there could be alot of Policies to deploy. In my opinion that leads to a very clutterd compliace view when looking at your policies. Luckily there is a feature in preview called Dynamic Scopes. This replaces the need for policies to dynamicly onboard servers to a Maintenance Configuration. 