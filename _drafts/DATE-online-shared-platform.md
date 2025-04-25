---
layout: single
title:  "Draft Post"
categories: 
  - Azure
toc: true
show_date: true
---
Many environments in Azure follows the Enterprise Scale Landing Zones design where you have Platform Landing Zones that are shared through the Azure environment, and dividing the Application Landing Zones into Corp and Online. Corp applications are bound to utilizing private networks in Azure takes advantage of a hub to communicate between each other and to external services (i.e. on-premises or Internet). Online on the other hand are treated more like bubbles where application owners has a bigger responsibility to secure external traffic, and we minimize the blast radius if they are exposed by not letting them be connected to each other with virtual network peering. 

There are many reasons for why an applications needs to be in the Online archetype. One reason is cost, some PaaS services requires a higher SKU to be able to integrate them in a private network. Another reason I hear is that "private networking in Azure is hard". The last reason can be mitigate by elevating the team or bring in external competence in designing the solution, but the cost aspect might not be possible to mitigate and therefore the Online archetype is the right one for that solution. 

But being in the Online section of the Azure environment means that you can't take advantage of centrally managed shared services that are reached via the hub. One of them could be a centrally managed Web Application Firewall (WAF), and that will be the example in this post. The WAF can be complex to manage and needs configuration to make sure that its protecting the published applications. The WAF also comes with a cost. If we let the teams in the Online archetype managed this completely by them self we might end up with WAFs that are misconfigured and doesn't provide the protection that we want, or that teams publish the service directly to internet, relying only on the services level firewall on the PaaS service itself. 

# Solution 1: Shared Platform Landing Zones for Online
