---
layout: single
title:  "Draft Post"
categories: 
  - Azure
toc: true
show_date: true
---
If you are publishing your applications publicly, you are probably protecting them with a Web Application Firewall (WAF). In most cases I deploy an Application Gateway and attache a Web Application Firewall Policy to it to protect the applications that I want to publish to the internet.
When you want to reach your application over HTTPS you need to upload a SSL certificate to encrypt your traffic. This is then used in the Listener within the Application Gateway. These certificates can be stored in an Azure Key Vault so that they can be reached either from the portal or programmatically.
You can use the portal to browse through certificates that has been uploaded to your Azure Key Vault when you are configuring the Listener for your Application Gateway, but that only works if you have configured the Azure Key Vault to use access policies instead of Role-Based Access Control (RBAC), and using RBAC is the recommended way to manage Azure Key Vault access. Using RBAC requires that we have a Managed Identity assigned to the Application Gateway that we then assign a RBAC-role to that allows it to access the certificates in the Azure Key Vault. 
If we try to browse certificates in the Azure Key Vault when we have RBAC configured as the access model we run in to this message:

![](/assets/img/cert-upload-error-portal.png)

It tells us that we can't access the Azure Key Vault because it's using the RBAC permission model. But that is just through the portal, we can still reach our goal by using Powershell to upload the stored certificate to the Application Gateway as a Listener SSL certificate. 

Solution