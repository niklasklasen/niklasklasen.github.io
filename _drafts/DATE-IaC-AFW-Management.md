---
layout: single
title: "Bicep or Terraform for Azure network management"
categories: 
  - Azure
toc: false
show_date: true
---
![](/assets/img/avnm-with-code-header.png)
Infrastructure as code (IaC) not a new concept in the world of IT, but the usage differse from bussines. In the last years I have helped a customers to deploy new infrastructure with the help of IaC templates and my prefered flavour has been Bicep in these project. Both due to personal preferences and customer requirements. 
Deploying new environments with IaC templates is allways good for multiple reasons, i.e. you get a baseline for your environment that is redeployable and you can say that it's some what documented as well since all the infromation you need is there in the templates. But what happens after the deployment, when the new infrasturctire meets the reality? 
When the configuration of the infrastructure need to change to meet new requirments from the business it is easy to open up the Azure portal and kick on a few buttons or write a quick PowerShell scritp to meet these requirments, but then reality starts to drift from the template. This means that we no longer can use the template to redeploy the environment in a disaster recovery case or lean on it as documentation any more. To avoid this we can instead do all the configurations that the environment needs in the template instead and just keep deploying that with the new changes. Sounds simple, right? 

The reailty is that to reach the point where you can manage your infrastructure completley with code, your organisation needs to reach a certain level of maturity and you need the right skills in your infrastructure teams. To end this rant, I have now found a project where the end goal is that there's now human access in the the critcal areas of the infrastructure, and all deployments and configurations are made using pipelince and IaC templates. Yay! 

But this raised a question for me. When it comes to both deploy a new network infrastructure and manage it, can I still stick to Bicep or is Terraform better in this scenario? This is what i will try to figure out in this blog post. 

# The test
In reailty the entire core network in Azure will be effected by this, but to test the IaC language I'll keep it simple. I'll be using Azure Virtual Network Manager (AVNM) as the base resource here, and within that I'll create 2 network groups. Outside of the template I'll create 2 virtual networks and the the test will then be to:
- Add one virtual network in each of the network groups
- Move one virtual network form one network group to the other
- Remove one virtual network completley from the network groups

### IMAGE OF TEST SETUP ###

## In a box ## 
The template for the first test can be found on the bottom of this post if you want to try this yourself.
DISCLAIMER: These template doesn't follow any good template structure. The only serve the purpose to conduct these tests.

## Test 1 - creating AVNM and adding virtual networks to network groups
For this first step I created the AVNM using an IaC template and created the two network groups. The template will then add the two existing virtual networks to the network groups, one for each network group. 
### Terrafrom
Running this test in Terraform was no problem. AVNM and the network groups was created and one virtual network was added to each of the groups. As expected.
### Bicep

## Test 2 - Move one virtual network from one network group to the other.
By changing the parent for the virtual network "vnet-2" to point at the network group 1, this should move the virtual network from network group 2 to network group 1.
### Terraform
By chaning the parent id for the static member, terrafrom destroys the existing static member resource and recreates it under the new parent. The result is that the vnet is moved to the other network group. 

![](/assets/img/tf-2vnet-1networkgroup.png)

### Bicep

## Test 3 - Remove the virtual network from a network group
In this test I will simply remove the static member resource from the template and the expectation is that the network group loses the member.
### Terraform
With Terraform just removing the reference to the static member removes the member virtual network from the network group.

![](/assets/img/tf-1vnet-1networkgroup.png)

### Bicep 

# Conclusion 

## Test 1 tempaltes
### Terraform (code block)
```json
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0.2"
    }
    azapi = {
      source  = "azure/azapi"
      version = "~> 1.5.0"
    }
  }

  required_version = ">= 1.1.0"
}

provider "azurerm" {
  features {}
}

data "azurerm_client_config" "current" {
}

variable "vnet-1-id" {
  type        = string
  default     = "Add your own vnet resource ID"
}

variable "vnet-2-id" {
  type        = string
  default     = "Add your own vnet resource ID"
}

resource "azurerm_resource_group" "rg" {
  name     = "rg-avnm-demo"
  location = "swedencentral"
}

data "azapi_resource" "subscription" {
  type                   = "Microsoft.Resources/subscriptions@2021-01-01"
  resource_id            = "/subscriptions/${data.azurerm_client_config.current.subscription_id}"
  response_export_values = ["*"]
}

resource "azapi_resource" "networkManager" {
  type      = "Microsoft.Network/networkManagers@2024-10-01"
  parent_id = azurerm_resource_group.rg.id
  name      = "avnm-tf-demo"
  location  = "swedencentral"
  body = jsonencode({
    properties = {
      description = ""
      networkManagerScopeAccesses = [
        "Connectivity",
      ]
      networkManagerScopes = {
        managementGroups = [
        ]
        subscriptions = [
          data.azapi_resource.subscription.id,
        ]
      }
    }
  })
  schema_validation_enabled = false
  response_export_values    = ["*"]
}

resource "azapi_resource" "networkGroup-1" {
  type      = "Microsoft.Network/networkManagers/networkGroups@2024-10-01"
  parent_id = azapi_resource.networkManager.id
  name      = "TF-NetworkGroup-1"
  body = jsonencode({
    properties = {
    }
  })
  schema_validation_enabled = false
  response_export_values    = ["*"]
}

resource "azapi_resource" "networkGroup-2" {
  type      = "Microsoft.Network/networkManagers/networkGroups@2024-10-01"
  parent_id = azapi_resource.networkManager.id
  name      = "TF-NetworkGroup-2"
  body = jsonencode({
    properties = {
    }
  })
  schema_validation_enabled = false
  response_export_values    = ["*"]
}

resource "azapi_resource" "staticMember-1" {
  type      = "Microsoft.Network/networkManagers/networkGroups/staticMembers@2024-10-01"
  parent_id = azapi_resource.networkGroup-1.id
  name      = "static-member-vnet-1"
  body = jsonencode({
    properties = {
      resourceId = var.vnet-1-id
    }
  })
  schema_validation_enabled = false
  response_export_values    = ["*"]
}

resource "azapi_resource" "staticMember-2" {
  type      = "Microsoft.Network/networkManagers/networkGroups/staticMembers@2024-10-01"
  parent_id = azapi_resource.networkGroup-2.id
  name      = "static-member-vnet-2"
  body = jsonencode({
    properties = {
      resourceId = var.vnet-2-id
    }
  })
  schema_validation_enabled = false
  response_export_values    = ["*"]
}
```
### Bicep (code block)