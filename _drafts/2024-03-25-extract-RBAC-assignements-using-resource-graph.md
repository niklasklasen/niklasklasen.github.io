---
layout: single
title:  "Extract RBAC role-assignments using Azure Resource Graph"
categories: 
  - Azure
toc: true
show_date: true
---
Identities play a central role when it comes to adopting Zero-Trust, so keeping the Entra ID former Azure AD) clean and under control is critical. But this post will focus on Azure and not Entra ID, and the way Azure and Entra ID are linked together is that Azure uses identities in Entra ID to grant permissions to perform different operations on specific parts or resources in Azure via Role Based Access Control (RBAC). RBAC role assignments can not only be given to Entra ID users, but also to Groups and Managed Identities.

> **Note** Managed Identities are used to assign a RBAC role to a resource in Azure, it can as an example be a Policy, Data Factory or a Virtual Machine.

RBAC roles describe what operations can be performed and on what resource type. There are a lot of built in RBAC roles that are divided in to two groups:
- **Job Function Roles** - These are more specific in what operations the allow a user to perform. As an example you have the role *Data Factory Contributor* that can create and manage data factories, as well as child resources within them but nothing else. These are good for granting least privileged permissions.
- **Privileged administrator roles** - These are roles allows a user to do much more and should only be granted to administrators and protected using Privileged Identity Management (PIM) that forces a user to elevate themselves before being granted the permissions of this role. An example of this type of role is the *Owner* that basically hands you the keys to the kingdom. 

To allow more granularity to what the user or identity can perform the operations approved by the RBAC role these roles are assigned to scope. These scopes are *Management Groups*, *Subscriptions*, *Resource Groups* or specific *Resources*. To read more on how to use this in an Enterprise Scale Landing Zones setup, check out this previous blog post: **[Don’t cheap out on Subscriptions](https://klasen.cloud/azure/dont-cheap-out-on-subscriptions/)**



Now that the basics of what RBAC is and how it is used, lets dig in to how we can extract this information from Azure Resource Graph using REST.