# Microsoft Azure And OMS Integration

We have provided collection of Powershell Runbooks and Modules for Microsoft Azure Automation and Operations Management Suite, to demonstrate integration with the Hornbill Collaboration platform and Service Manager application.

Please see the description within each Runbook and/or Module for more information about functionality, input parameters, and the APIs that they use.

## Open Source

The Microsoft Azure and OMS Hornbill Integration is provided open source under the [Hornbill Community License](https://github.com/hornbill/AzureOMSHornbillIntegration?tab=License-1-ov-file#readme) and can be found on the [PowerShell Gallery](https://www.powershellgallery.com/packages?q=hornbill) and [GitHub](https://github.com/hornbill/powershellHornbillAzureRunbooks).

## Modules

IMPORTANT! Both the [HornbillAPI](https://www.powershellgallery.com/packages/HornbillAPI/) and [HornbillHelpers](https://www.powershellgallery.com/packages/HornbillHelpers/1.2.0) PowerShell Modules are required to be added to the Azure Automation account prior to making use of the Hornbill Runbooks. See the .psm1 Module files within the Modules for more information on the input and output parameters for each CMDLET.

**HornbillAPI** : This module contains the required CMDLETs to build and fire API calls from Azure Runbooks to your Hornbill instance:

* _Set-HB-Instance_ : MANDATORY - Allows your Powershell script to define the Hornbill instance to connect to, the zone in which it resides, and the API key to use for session generation.
* _Add-HB-Param_ : Add a parameter to the XMLMC request
* _Clear-HB-Params_ : Clears any existing XMLMC parameters that have been added
* _Open-HB-Element_ : Allows for the building of complex XML
* _Close-HB-Element_ : Allows for the building of complex XML
* _Invoke-HB-XMLMC_ : Invokes your API call
* _ConvertTo-HB-B64Decode_ : Returns a UTF8 string from a given Base64 encoded string
* _ConvertTo-HB-B64Encode_ : Returns a Base64 encoded string from a given UTF8 string
* _Get-HB-Params_ : Returns XML string of parameters that have been added by Add-HB-Params, Open-HB-Element and Close-HB-Element

**HornbillHelpers** : This module contains a number of helper CMDLETs to carry out common requests within Hornbill Runbooks:

* _Get-HB-CatalogID_ : Provide a Catalog Item Name, and this CMDLET will search your Hornbill instance for a matching record, returning the Primary Key
* _Get-HB-OrganisationID_ : Provide a Organization Name, and this CMDLET will search your Hornbill instance for a matching record, returning the Primary Key
* _Get-HB-PriorityID_ : Provide a Priority Name, and this CMDLET will search your Hornbill instance for a matching record, returning the Primary Key
* _Get-HB-Request_ : Retrieves details about a Request
* _Get-HB-ServiceID_ : Provide a Service Name, and this CMDLET will search your Hornbill instance for a matching record, returning the Primary Key
* _Get-HB-SiteID_ : Provide a Site Name, and this CMDLET will search your Hornbill instance for a matching record, returning the Primary Key
* _Get-HB-WorkspaceID_ : Provide a Workspace Name, and this CMDLET will search your Hornbill instance for a matching record, returning the Activity Stream ID

### Module Installation

#### Install via the Module Gallery

* Open Microsoft Azure in your browser and navigate to your Automation Account
* Click on `Modules Gallery` under `Shared Resources`
* Search the Modules for `Hornbill`
* Click in to each Hornbill module in turn, and click the `Import` button
* On the list of Modules, wait for the Module Status to become `Available` before attempting to run any Runbooks that use them.

#### Install from the PowerShell Gallery

* Download the Modules from the [PowerShell Gallery](https://www.powershellgallery.com/packages?q=hornbill), and place on your local machine
* Open Microsoft Azure in your browser and navigate to your Automation Account
* Click on `Modules` under `Shared Resources`
* Click the `Add a module` button
* Use the `Upload File` control to find and select your Module, then click the `OK` button
* On the list of Modules, wait for the Module Status to become `Available` before attempting to run any Runbooks that use them.

## Runbooks

We have provided a number of example Powershell Runbooks to allow your Azure Automation Account (and therefore the Microsoft Operations Management Suite) to interact with your Hornbill instance. Please see the Runbooks themselves for more detailed information regarding input parameters etc:

* **[HornbillAzureIntuneAssetImport](https://www.powershellgallery.com/packages/HornbillAzureIntuneAssetImport/)** : Allows the import of asset records from Intune into Hornbill
* **[HornbillContactArchiveWorkflow](https://www.powershellgallery.com/packages/HornbillContactArchiveWorkflow/)** : Archives a Contact
* **[HornbillContactCreate](https://www.powershellgallery.com/packages/HornbillContactCreateWorkflow/)** : Creates a Contact
* **[HornbillLogChangeRequest](https://www.powershellgallery.com/packages/HornbillLogChangeRequest/)** : Logs a Change Request within Service Manager - this is a Powershell Workflow, and can be called from other Azure Workflow Runbooks (Powershell or Graphical)
* **[HornbillLogChangeRequestWebhook](https://www.powershellgallery.com/packages/HornbillLogChangeRequestWebhook/)** : Logs a Change Request within Service Manager - this should be called with an Azure Webhook, and is useful when setting up Alerts in Operations Management Suite
* **[HornbillLogIncident](https://www.powershellgallery.com/packages/HornbillLogIncident/)** : Logs an Incident within Service Manager - this is a Powershell Workflow, and can be called from other Azure Workflow Runbooks (Powershell or Graphical)
* **[HornbillLogIncidentWebhook](https://www.powershellgallery.com/packages/HornbillLogIncidentWebhook/)** : Logs an Incident within Service Manager - this should be called with an Azure Webhook, and is useful when setting up Alerts in Operations Management Suite
* **[HornbillLogServiceRequest](https://www.powershellgallery.com/packages/HornbillLogServiceRequest/)** : Logs a Service Request within Service Manager - this is a Powershell Workflow, and can be called from other Azure Workflow Runbooks (Powershell or Graphical)
* **[HornbillLogServiceRequestWebhook](https://www.powershellgallery.com/packages/HornbillLogServiceRequestWebhook/)** : Logs a Service Request within Service Manager - this should be called with an Azure Webhook, and is useful when setting up Alerts in Operations Management Suite
* **[HornbillRequestClose](https://www.powershellgallery.com/packages/HornbillRequestClose/)** : Closes a Service Manager Request
* **[HornbillRequestResolve](https://www.powershellgallery.com/packages/HornbillRequestResolve/)** : Resolves a Service Manager Request
* **[HornbillRequestUpdateDetails](https://www.powershellgallery.com/packages/HornbillRequestUpdateDetails/)** : Updates the details of a Service Manager Request
* **[HornbillRequestUpdateTimeline](https://www.powershellgallery.com/packages/HornbillRequestUpdateTimeline/)** : Updates the timeline of a Service Manager Request
* **[HornbillUserAddGroup](https://www.powershellgallery.com/packages/HornbillUserAddGroup/)** : Adds a Hornbill User to a Hornbill Group
* **[HornbillUserAddRoles](https://www.powershellgallery.com/packages/HornbillUserAddRoles/)** : Adds one or more Roles to a Hornbill User
* **[HornbillUserAddWorkspace](https://www.powershellgallery.com/packages/HornbillUserAddWorkspace/)** : Adds a Hornbill User to a Hornbill Collaboration Workspace
* **[HornbillUserArchive](https://www.powershellgallery.com/packages/HornbillUserArchive/)** : Archives a Hornbill User account
* **[HornbillUserCreate](https://www.powershellgallery.com/packages/HornbillUserCreate/)** : Creates a Hornbill User account
* **[HornbillUserDelete](https://www.powershellgallery.com/packages/HornbillUserDelete/)**  : Deletes a Hornbill User account
* **[HornbillWorkspaceCreate](https://www.powershellgallery.com/packages/HornbillWorkspaceCreate/)** : Creates a Hornbill Collaboration Workspace
* **[HornbillWorkspacePost](https://www.powershellgallery.com/packages/HornbillWorkspacePost/)** : Adds a Post to a Hornbill Collaboration Workspace - this is a Powershell Workflow, and can be called from other Azure Workflow Runbooks (Powershell or Graphical)
* **[HornbillWorkspacePostWebhook](https://www.powershellgallery.com/packages/HornbillWorkspacePostWebhook/)** : Adds a Post to a Hornbill Collaboration Workspace - this should be called with an Azure Webhook, and is useful when setting up Alerts in Operations Management Suite
* **[HornbillWorkspacePostComment](https://www.powershellgallery.com/packages/HornbillWorkspacePostComment/)** : Adds a Comment to an existing Post on a Hornbill Collaboration Workspace

### Runbook Installation

#### Install via the Runbook Gallery

* Open Microsoft Azure in your browser and navigate to your Automation Account
* Click on `Runbooks Gallery` under `Shared Resources`
* Search the `PowerShell Gallery` for `Hornbill`
* Click in to each Hornbill Runbook that you want to install in turn, and click the `Import` button
* The Runbook will then be available for use/edit/publishing in your list of Automation Runbooks

* Download the relevant Runbook Powershell scripts from the [PowerShell Gallery](https://www.powershellgallery.com/packages?q=hornbill), and place on your local machine
* Open Microsoft Azure in your browser and navigate to your Automation Account
* Click on `Runbooks` under `Process Automation`
* Click the `Add a runbook` button
* Click `Import an existing runbook`
* Use the `Runbook File` control to find and select your Runbook, then click the `OK` button
* Give the Runbook a description should you so wish, then click the `Create` button to add the Runbook to your Runbook library
