
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
