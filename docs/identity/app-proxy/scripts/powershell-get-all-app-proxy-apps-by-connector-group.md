---
title: List Microsoft Entra application proxy connector groups for apps
description: PowerShell example that lists all Microsoft Entra application proxy Connector groups with the assigned applications.

author: kenwith
manager: amycolannino
ms.service: entra-id
ms.subservice: app-proxy
ms.custom: has-azure-ad-ps-ref
ms.topic: sample
ms.date: 01/04/2024
ms.author: kenwith
ms.reviewer: ashishj
---

# Get all Application Proxy apps and list by connector group

This PowerShell script example lists information about all Microsoft Entra application proxy Connector groups with the assigned applications.

[!INCLUDE [quickstarts-free-trial-note](~/../azure-docs-pr/includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](~/../azure-docs-pr/includes/updated-for-az.md)]

This sample requires the [Microsoft Graph Beta PowerShell module](/powershell/microsoftgraph/installation) 2.10 or newer.

## Sample script

```powershell
# This sample script gets all Microsoft Entra application proxy connector groups with the assigned applications.
#
# Version 1.0
#
# This script requires PowerShell 5.1 (x64) or beyond and one of the following modules:
#
# Microsoft.Graph.Beta ver 2.10 or newer
#
# Before you begin:
#    
#    Required Microsoft Entra role: Global Administrator or Application Administrator or Application Developer 
#    or appropriate custom permissions as documented https://learn.microsoft.com/en-us/azure/active-directory/roles/custom-enterprise-app-permissions
#
# 

Import-Module Microsoft.Graph.Beta.Applications

Connect-MgGraph -Scope Directory.Read.All -NoWelcome

Write-Host "Reading service principals. This operation might take longer..." -BackgroundColor "Black" -ForegroundColor "Green" 

$aadapServPrinc = Get-MgBetaServicePrincipal -Top 100000 | where-object {$_.Tags -Contains "WindowsAzureActiveDirectoryOnPremApp"}

Write-Host "Reading Microsoft Entra applications. This operation might take longer..." -BackgroundColor "Black" -ForegroundColor "Green"

$allApps = Get-MgBetaApplication -Top 100000

Write-Host "Reading application. This operation might take longer..." -BackgroundColor "Black" -ForegroundColor "Green"

$aadapApp = $aadapServPrinc | ForEach-Object {$allApps.AppId -match $_.AppId}
 
Write-Host "Reading connector groups. This operation might take longer..." -BackgroundColor "Black" -ForegroundColor "Green"

$aadapConnectorGroups= Get-MgBetaOnPremisePublishingProfileConnectorGroup -OnPremisesPublishingProfileId "applicationProxy" -Top 100000 

Write-Host "Displaying connector groups and assigned applications..." -BackgroundColor "Black" -ForegroundColor "Green"
Write-Host " "

foreach ($item in $aadapConnectorGroups)
 {
  
   If ($item.ConnectorGroupType -eq "applicationProxy")
    {  
        "Connector group: " + $item.Name + " (Id: " + $item.Id+ ") - Region: " + $item.Region;
          
        $assignedApps= Get-MgBetaOnPremisePublishingProfileConnectorGroupApplication -ConnectorGroupId $item.Id -OnPremisesPublishingProfileId "applicationProxy";
    
        " "; 

        foreach ($item2 in $assignedApps)
         {
           
           $Item2.DisplayName + " (AppId: " + $item2.AppId+ ")"
         } 
    
    " ";
       
    }
           
 }   

Write-Host ("")
Write-Host ("Finished.") -BackgroundColor "Black" -ForegroundColor "Green"
Write-Host "To disconnect from Microsoft Graph, please use the Disconnect-MgGraph cmdlet." 
```

## Script explanation

| Command | Notes |
|---|---|
|[Connect-MgGraph](/powershell/module/microsoft.graph.authentication/connect-mggraph)| Connects to Microsoft Graph|
|[Get-MgBetaServicePrincipal](/powershell/module/microsoft.graph.applications/get-mgserviceprincipal)| Gets a service principal|
|[Get-MgBetaApplication](/powershell/module/microsoft.graph.beta.applications/get-mgbetaapplication)| Gets an Enterprise Application|
|[Get-MgBetaOnPremisePublishingProfileConnectorGroup](/powershell/module/microsoft.graph.beta.applications/get-mgbetaonpremisepublishingprofileconnectorgroup)| Gets a connector group|
|[Get-MgBetaOnPremisePublishingProfileConnectorGroupApplication](/powershell/module/microsoft.graph.beta.applications/get-mgbetaonpremisepublishingprofileconnectorgroupapplication)| Gets applications assigned to a connector group|

## Next steps

For more information on the Microsoft Graph PowerShell module, see [Microsoft Graph PowerShell overview](/powershell/microsoftgraph/overview).

For other PowerShell examples for Application Proxy, see [Microsoft Entra application proxy PowerShell examples](../application-proxy-powershell-samples.md).
