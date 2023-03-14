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

To perform its tasks the Update Management Center (Preview) extension runs code localy on the virtual machine to inteact with the operating system. That allows it to perform the following tasks: 
- Retrive the assessment infromation about the status of the system updates specified by Windows Update client or Linux package manager.
- Initiate the download and installation of updates approved by Windows Update client or Linux package manager. 

Traffice flow to Windows Update, Linux package or WSUS