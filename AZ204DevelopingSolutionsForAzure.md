- [Powershell commands](#powershell-commands)
- [What's an app service plan and hows it related to subscription and resource group](#whats-an-app-service-plan-and-hows-it-related-to-subscription-and-resource-group)
    - [App Service Plan](#app-service-plan)
    - [Relationship with Subscription and Resource Group](#relationship-with-subscription-and-resource-group)
    - [Example](#example)
- [Can we have a resource group with windows and linux resources and associate it with 2 app service plans: one for windows and one for linux](#can-we-have-a-resource-group-with-windows-and-linux-resources-and-associate-it-with-2-app-service-plans-one-for-windows-and-one-for-linux)
    - [Example](#example-1)
- [Deployment slots](#deployment-slots)
- [App service settings](#app-service-settings)
  - [Environment variables and connection strings](#environment-variables-and-connection-strings)
  - [](#)

# Powershell commands
```
Get-AzSubscription
```

output:
```
TenantId: <TenantId>

Name    Id     State
----    --     -----
<Name>  <Id>   Enabled
```
---

Get Azure Modules installed
```
Get-Module -Name Az -ListAvailable
```

output:
```
 Directory: <Directory>

ModuleType Version    PreRelease Name   PSEdition ExportedCommands
---------- -------    ---------- ----   --------- ----------------
Script     13.1.0                Az     Core,Desk
```
---

Install module
```
Install-Module -Name Az -Force
```

# What's an app service plan and hows it related to subscription and resource group
An App service plan has: `region, instance size, pricing tier, operating system` for your web apps

### App Service Plan
An App Service Plan in Azure defines the region, instance size, and pricing tier for your web apps. It determines how much you pay and what features are available to your app. Here are some key points:

- **Hosting Environment**: An App Service Plan provides the underlying infrastructure for your web apps, APIs, and other Azure App Services. 
- **Resource Allocation**: It determines the amount of resources (CPU, memory, etc.) allocated to your app.
- **Scaling**: You can scale your app up or out by changing the pricing tier or adding more instances within the App Service Plan.
- **Operating System**: An App Service Plan is configured for either Windows or Linux and cannot be changed later.

### Relationship with Subscription and Resource Group
- **Subscription**: A subscription is an Azure account that provides access to Azure services and resources. It has billing information and is associated with one or more resource groups. All costs incurred by resources, including those in your App Service Plan, are billed to your subscription.
  
- **Resource Group**: A resource group is a container that holds related resources for an Azure solution. It helps you manage and organize resources by grouping them logically. You can place an App Service Plan within a resource group along with other related resources (like web apps, databases, etc.).

### Example
Let's say you have a web application that consists of:
- A front-end web app
- A back-end API app
- A database

You can place the front-end and back-end apps into the same App Service Plan to manage them together and optimize resource usage. These apps, along with the database, can be organized within the same resource group for better management and lifecycle coordination. The entire setup will be billed under a single subscription.

# Can we have a resource group with windows and linux resources and associate it with 2 app service plans: one for windows and one for linux

Absolutely! You can have a resource group that contains both Windows and Linux resources, and you can associate each type of resource with its own App Service Plan.

Here's how you can do it:

1. **Create the Resource Group**: First, create a resource group that will contain your resources.

2. **Create Two App Service Plans**: 
   - Create one App Service Plan configured for Windows.
   - Create another App Service Plan configured for Linux.

3. **Deploy Windows and Linux Resources**: 
   - Deploy your Windows-based web apps or other resources to the Windows App Service Plan.
   - Deploy your Linux-based web apps or other resources to the Linux App Service Plan.

### Example
- **Resource Group**: `MyResourceGroup`
  - **Windows App Service Plan**: `WindowsAppServicePlan`
    - Windows Web App
    - Windows API App
  - **Linux App Service Plan**: `LinuxAppServicePlan`
    - Linux Web App
    - Linux API App

This way, both sets of resources can be organized under the same resource group while being managed under their respective App Service Plans.

If you need further assistance or have additional questions, feel free to ask!

# Deployment slots
* We can have different deployment slots for each app 
* You could create for example one slot each for: dev, test, staging, production
* or you could split traffic b/w different deployments slots. This can be used in A/B testing. 
* We can swap different slots. 
  * for ex: once we have tested staging, then we can swap production and staging.And if something goes wrong, we could swap back. 

# App service settings 

## Environment variables and connection strings
* On the app services page on the left: `Settings/Environment variables`
* In the app-services settings, you can store environment variables, and connection strings
There is a separate tab for connection strings. 

##  