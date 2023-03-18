---
layout: single
title:  "Update Management Center (Preview)"
categories: 
  - Azure
toc: false
---
As long as we use virtual machines patch management will allway be something that we need to be on top of. I resently came a cross a case where a customer wanted to update the way they managed patching for their virtual machines, both in Azure and on-prem. This led me to try out the service Update Managment Center (Preview). One thing that caught my attention right of the bat was the posibility to mangaed patching through the use of Azure Policies. That is a big upside since utilizing a policy drivenframework to govern Azure resources eases the administrative burden of IT departments and makes sure that you are compliant towards your own governance framework.
So what is Update Management Center (Preview) and how do you get started with it? 

# Update Managment Center (Preview)
One key difference from Automation Update Managment, the current update management service in Azure, is that you no longer need Azure Automation or Azure Monitor Logs. Update Management Center (Preview) instead relies on an extension that interacts with the operating system to provide the functionalitey it need to assess and preform updates to your virtual machines. This extension is automaticly installed on the virtual machine when an operation is initiated from Update Management Center (Preview), for example check for updates, install one-time update or periodic assessment on the virtual machine. The extenison is install by using one of the following agents: 
- Azure virtual machine Windows agent
- Azure virtual machine Linux agent
- Azure Arc-enabled server agent
This means that Update Management Center (Preview) can be used to manage updates on our Azure depolyed virtual machins and on-prem or other cloud providers that you have extended your Azure environment to using Azure Arc. 

To perform its tasks the Update Management Center (Preview) extension runs code locally on the virtual machine to inteact with the operating system. That allows it to perform the following tasks: 
- Retrive the assessment infromation about the status of the system updates specified by Windows Update client or Linux package manager.
- Initiate the download and installation of updates approved by Windows Update client or Linux package manager. 

Sincnce the Update Management Center (Preview) extension is looking at the servers configuration to pull the data for what is the latest updates, you need to make sure that the server can communicate to the source. Are you using a WSUS server to manage what updates you allow, each server needs to have a way to communicate with that WSUS server. Otherwise the assessment won't be able to fetch any data for the assessments. The same concept applies to public sources as well. If you are fetching updates from Microsoft Update the server needs to be able to connect to these endpoints:
- *.download.windowsupdate.com
- *.dl.delivery.mp.microsoft.com
- *.delivery.mp.microsoft.com

# Limitations
There are some limitations for Update Management Center (Preview). The service doesn't support driver updates and for Azure virtual machines Update Management Center (Preivew) is availiable in all regoins that supports Compute Virtual Machines. But for Azure Arc-enabled servers it's only supported in the following regions:
- South East Asia
- Australia East
- Canada Central
- North Europe
- West Europe
- France Central
- Japan East
- Korea Central
- UK South
- UK West
- East US
- East US 2
- North Central US
- South Central US
- West Central US
- West US
- West US 2
- West US 3

# Deploying with Azure Policies

