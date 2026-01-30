---
layout: single
title: "Bicep or Terraform for Azure network management"
categories: 
  - Azure
toc: false
show_date: true
---
Infrastructure as code (IaC) not a new concept in the world of IT, but the usage differse from bussines. In the last years I have helped a customers to deploy new infrastructure with the help of IaC templates and my prefered flavour has been Bicep in these project. Both due to personal preferences and customer requirements. 
Deploying new environments with IaC templates is allways good for multiple reasons, i.e. you get a baseline for your environment that is redeployable and you can say that it's some what documented as well since all the infromation you need is there in the templates. But what happens after the deployment, when the new infrasturctire meets the reality? 
When the configuration of the infrastructure need to change to meet new requirments from the business it is easy to open up the Azure portal and kick on a few buttons or write a quick PowerShell scritp to meet these requirments, but then reality starts to drift from the template. This means that we no longer can use the template to redeploy the environment in a disaster recovery case or lean on it as documentation any more. To avoid this we can instead do all the configurations that the environment needs in the template instead and just keep deploying that with the new changes. Sounds simple, right? 

The reailty is that to reach the point where you can manage your infrastructure completley with code, your organisation needs to reach a certain level of maturity and you need the right skills in your infrastructure teams. To end this rant, I have now found a project where the end goal is that there's now human access in the the critcal areas of the infrastructure, and all deployments and configurations are made using pipelince and IaC templates. Yay! 

But this raised a question for me. When it comes to both deploy a new network infrastructure and manage it, can I still stick to Bicep or is Terraform better in this scenario? This is what i will try to figure out in this blog post. 

# The test
In reailty entire core network in Azure will be effected by this, but to test the IaC language I'll keep it simple. I'll be using Azure Virtual Network Manager as the base resource here, and with in that I'll create 2 network groups. Outside of the template I'll create 2 virtual networks and the the test will then be to:
- Add one virtual network in each of the network groups
- Move one virtual network form one network group to the other
- Remove one virtual network completley from the network groups

