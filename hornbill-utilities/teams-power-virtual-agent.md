# Teams Power Virtual Agent

![Microsoft Teams Logo](/_books/hornbill-utilities/images/teams-logo.png) 

The Hornbill Power Virtual Agent (PVA) for [Microsoft Teams](https://www.microsoft.com/en-gb/microsoft-teams/group-chat-software) allows your end users to interact with Hornbill.

The Hornbill PVA is supplied as a customisable Power Platform Solution, and contains several conversation topics, as well as supporting entities and Power Platform flows, that allow your end users to interact with your Service Desk entirely from within the Teams client(s).

The Hornbill PVA Power Platform Solution is to be installed and hosted within your Microsoft Power Platform & Teams environment - this is not hosted by Hornbill.

## Requirements

#### Required Microsoft subscriptions
To use the Hornbill PVA, you will need subscriptions for the following Microsoft services:

* Microsoft 365 Business Basic (minimum).
* Power Apps.

:::info
The above subscriptions will provide up to 2,000 Power Automate flow invocations per day. If your business requires more than this, or you wish to publish the bot to non-Teams channels, then you will require a full Power Virtual Agents subscription.
:::

#### Required configuration
For the Teams Power Virtual Agent to connect to and communicate with Hornbill, an existing Hornbill User Account will need to be used.

* **UPN**.  The PVA requires that the UPN of the session user is the _User ID_ of the user that you will be using. Make a note of the User ID as this will be required when building your PVA.
* **API Key**. An API Key needs to be created before your begin the creation of your Custom Solution. The API Key will be required when building your PVA.
    * ***User Type***. The user under which you create the API key must be of type *User*.
    * ***Roles***. The user under which you create the API key requires the following Roles: Collaboration Role, Hornbill Service Manager Integrations, Service Desk Admin.
* **Service and Request Catalog IDs**. During the creation of your Custom Solution you will be prompted for the IDs of both Services and Request Catalog Items which will link to the different Topics that you wish to use. You will want to make sure that you have these available before building your Custom Solution.

## Installation and deployment

### Step 1: Create a Custom Solution
The first stage in deploying your PVA is to create an instance-specific solution package. To do this:

1. Clone/download the contents of the [Hornbill PVA Github](https://github.com/hornbill/hornbillPowerVirtualAgent) repository into a local folder on your computer.
1. Open a PowerShell terminal from the folder that contains the downloaded files.
1. Run `.\BuildPVAPackage.ps1`
1. Follow all the prompts, entering relevant values when prompted. 
    :::note
    All text-type input properties below are **CASE SENSITIVE**.
    :::
    * **The ID of your Hornbill instance**. The ID of your Hornbill instance, example from the URL: `https://live.hornbill.com/**yourinstanceid**`
    * **The API key for your Teams PVA account on your Hornbill instance**. An API Key for the Hornbill user that will be used by the PVA to make API calls back into your Hornbill instance:
        * The User should be of type User
        * The User requires the following Roles:
            * Collaboration Role
            * Hornbill Service Manager Integrations
            * Service Desk Admin
    * **The field in your Hornbill instance user records that holds the UPN (userId / employeeId / loginId / attrib1 / ... / attrib8)**. The Hornbill user field that holds the Office 365 User Principal Name (UPN) to match your Teams users with your Hornbill users.
    * **Topic - I Need Help**. Information used when raising new Incident records through the **I Need Help** topic:
        * `Service ID` \- The ID of the Service that the Incident should be raised against. Enter **0** to raise against no Service
        * `Catalog Item ID` \- The ID of the Catalog Item that the Incident should be raised against. Enter **0** to raise against no Service

    * **Topic - New Starter**. Information used when raising new Service Request records through the **New Starter** topic:
        * `Enabled (yes/no)` \- Whether the topic should be enabled during the import. When set to **no**, the topic (and its dependencies) will still be imported, but in a disabled state
        * `Service ID` \- The ID of the Service that the Service Request should be raised against. Enter **0** to raise against no Service
        * `Catalog Item ID` \- The ID of the Catalog Item that the Service Request should be raised against. Enter **0** to raise against no Service
    * **Topic - Leaver**. Information used when raising new Service Request records through the **Leaver** topic:
        * `Enabled (yes/no)` \- Whether the topic should be enabled during the import. When set to **no**, the topic (and its dependencies) will still be imported, but in a disabled state
        * `Service ID` \- The ID of the Service that the Service Request should be raised against. Enter **0** to raise against no Service
        * `Catalog Item ID` \- The ID of the Catalog Item that the Service Request should be raised against. Enter **0** to raise against
    * **Topic - Password Reset**. Information used when raising new Service Request records through the **Password Reset** topic:
        * `Enabled (yes/no)` - Whether the topic should be enabled during the import. When set to no, the topic (and its dependencies) will still be imported, but in a disabled state.
        * `Service ID` - The ID of the Service that the Service Request should be raised against. Enter 0 to raise against no Service
        * `Catalog Item ID` - The ID of the Catalog Item that the Service Request should be raised against. Enter 0 to raise against no Catalog Item.

The script will then build the custom solution ZIP file that can be imported into Power Apps.

### Step 2: Import the Custom Solution
Once you have the ZIP file containing your custom solution:

1. Log into the Power Apps environment that contains your Teams solution:
    * The URL will be formatted as so: https://make.powerapps.com/environments/your-teams-environment-id/solutions
    * To retrieve your Power Apps Teams environment ID, you can:
        1. Visit https://admin.powerplatform.microsoft.com/
        2. Click on the relevant environment.
        3. Copy the environment ID from the URL.
1. From the Power Apps Solutions page, hit the Import button.
1. Click the Browse button, browse to and select your custom solution ZIP and click `Next`.
1. Choose an existing Connection for the Office 365 Users integration. This is used to pull details of the Teams session user. If none are available in the list, choose `New Connection` and create accordingly.
1. Click `Import`.
1. As soon as the import has completed, hit the `Publish all customisations` button - This is required, otherwise your bot will return errors.

### Step 3: Editing
Now that your PVA solution has been imported and published, you can now log into Teams and edit/publish/share the bot from there. To do this:

#### Finding the PVA
1. Log in to Teams.
1. Click the 3 dots in the sidebar, and open Power Virtual Agents from the apps menu.
1. Click the Chatbots tab at the top of the app window, and you should see the Hornbill Virtual Agent listed under **My chatbots** and/or the organization name.

#### Editing the PVA
PVA topics and entities can be edited as normal by selecting the relevant Topics and Entities side menu options.

:::important
Rather than directly editing the packaged topics, we recommend you make copies of these topics and edit/publish the copies ONLY. This will prevent you from losing any changes you make to the out of the box topics in the event you import new versions of the solution that we may release.
:::

We have provided the link in the **Escalate** topic as an example. This should be changed to either:
* A link to a Teams group conversation.
* Hand-off to Omnichannel or custom engagement hub. These can be defined in the **Manage > Agent Transfers** side menu from the PVA.

#### Testing
The PVA can be tested either from the **Test bot** pane in the PVA editor, or by publishing the PVA (as described below) and sharing to a select group of Teams users.

### Step 4: PVA Branding & Publishing
#### Branding
You can give the Virtual Agent a custom name and/or Icons that will appear when your users install the Power Virtual Agent into their Teams client. There are two places this is done:

##### Name & Chat Icon
From the PVA, click `Manage` then `Details` from the side menu. From there you can change the Name and Icon that will appear next to messages sent from the bot. 

##### App Icon, PVA Descriptions and Publishing
To set the App Icon and Bot Description(s), you need to first Publish the PVA. To do this, from the PVA click `Publish` from the side menu and click the `Publish` button on the page followed by the `Publish` button in the resulting dialog box. 

Once the bot is published, click the `Edit details` button to set the following fields:

* **App Icon**. We have provided a file in the Github repository that you can use as the App Icon: Hornbill\_corporate\_logo\_avatar\_2021.png
* **App Icon Color**. Custom color, hex value: 224187
* **Short description**. A short description of your PVA. Example: *Get help, make requests and check on the status of your Hornbill tickets*
* Long description - A longer description of your PVA. Example: *Hornbill Power Virtual Agent is a conversational bot that enables you to get help, see updates from your open Hornbill requests, and raise and resolve common requests all from within the Teams client.*
* **Developer name**. Hornbill
* **Website**. www.hornbill.com
* **Privacy statement**. www.hornbill.com/privacy-policy
* **Terms of use**. www.hornbill.com/terms-of-service

### Step 5: Sharing
Once your PVA is imported, published, tested and branded, you can now share the PVA with other Teams users in your organization.

To do this, from the **Publish** menu click on the `Make the bot available to others` button, then click the `Availability options` button. From here, you can choose from:
* **Share link**. This allows you to add the bot to members of specific security groups in your Office 365 environment
* **Show to my teammates and shared users**. The PVA will appear under the **Built by your colleagues** section of the Teams Apps list
* **Show to everyone in my org**. The PVA will appear under the **Built by your org** section of the Teams Apps list. Note - this will need to be authorized by your Teams admin in the Teams Admin Portal

## Hornbill Specific Topics
### I Need Help
A topic to allow users to ask for help. 

Triggered by phrases such as "I need help" and "I've got an issue", the topic will take a description of the issue from the user before searching Hornbill for relevant requests, known issues or FAQs. 

These are returned in a structured manner, in the hope that the user can help themselves to fix the issue themselves, or mark themselves as affected by a published known issue. If the found requests, known issues and FAQs are not helpful in fixing the issue, then the option is given to log a new incident.

### Get An Update
Allows users to see the status of and the latest customer-facing update of one of their requests. Returns the last few logged requests for the user to choose from in the event that they don't know the request reference.

Allows the user to add an update to the request timeline should they so wish.

### Search Knowledgebase
Allows users to provide a search string, that we then use to search FAQs that available to them, returning the top 5 results.

### New Starter
A topic that is provided as an example of how New Starter requests could be raised in a conversational manner.

The information requested is written into the following custom fields for automated processing by your Hornbill workflows:

* First Name - h\_custom\_a
* Surname - h\_custom\_b
* Site Name - h\_custom\_d
* Department - h\_custom\_e
* Role - h\_custom\_f
* Hardware - Computer - h\_custom\_g
* Hardware - Phone - h\_custom\_h
* Start Date - h\_custom\_21

### Leaver

A topic that is provided as an example of how Leaver requests could be raised in a conversational manner.

* User ID (UPN) - h\_custom\_a
* User Display Name - h\_custom\_b
* Last Working Date - h\_custom\_21

### Password Reset

A topic that is provided as an example of how Password Reset requests could be raised in a conversational manner.

* System - h\_custom\_a

### Bulletins

Returns news in the form of bulletins from the users subscribed services

### Cancel Request
Allows users to cancel in-flow requests
  
## API Key Rules
This utility uses (API keys):
* apps/com.hornbill.servicemanager:chatbotGetRecentRequests
* apps/com.hornbill.servicemanager:chatbotGetRequestStatus
* apps/com.hornbill.servicemanager:chatbotKnowledgeSearch
* apps/com.hornbill.servicemanager:chatbotLogRequest
* apps/com.hornbill.servicemanager:chatbotSetMeToo
* apps/com.hornbill.servicemanager:chatbotUpdateReqTimeline
* apps/com.hornbill.servicemanager:chatbotGetServiceBulletins
* apps/com.hornbill.servicemanager:chatbotCancelRequest
* apps/com.hornbill.servicemanager/Requests:update
* data:queryExec

## Troubleshooting

#### Importing a Solution Package into Power Apps

**Error Message:**
`"Hornbill Power Virtual Agent" failed to import: Error while importing workflow {5ce3d382-8935-ec11-8c64-000d3a0ae61d} type ModernFlow name Search Users: Workflow import: Xaml file is missing from import zip file: FileName: /Workflows/SearchUsers-5CE3D382-8935-EC11-8C64-000D3A0AE61D.json`

This error message is misleading as there is no Xaml file required. The issue is a result of the compression method for the creation of the Custom Solution Zip.

**Solution:** 

* extract the Custom Solution Zip.
* Open Powershell window.
* Change directory to the extracted Solution folder.
* Run the following batch script: tar -a -c -f "YourFileName.zip" \*

The tar utility is a built-in Windows utility that creates ZIP files that Power platform does not complain about.

## FAQs

#### Do we need a chatbot user account in Hornbill?
An API Key is used for the chatbot to interact with Hornbill in a similar way in which the import utilities work. This API Key is generated against a User. That User will need specific privileges within Hornbill Service Manager. It is not necessary to create a specific user for this, however if your chatbot uses an API Key generated against Joe Bloggs, then all calls (generated via chatbot) will be as if they were logged by Joe Bloggs (the customer of the call will be the person who interacted with the chatbot)

#### Does the chatbot appear in both desktop and online Teams apps?
Yes

#### How does the PVA recognize the Teams User?
In order to associate the Teams User as a User within Hornbill, we need to ensure that both systems (Teams & Hornbill) have data that can be matched. Teams uses the users' UserPrincipalName (UPN). As part of the configuration, the field within Hornbill which stores the UPN will need to be given. If you are currently not storing the UPN within Hornbill, then the use of PVA is not possible.

Authentication is done on the MS Teams side, so users can "live" within either AD or Azure. If the user's account is deactivated there (AD/Azure), then they won't have access to the chatbot.

#### Does each user have to install the chatbot app in Teams or can this be done centrally?
Access to the chatbot on the Teams side is controlled via Teams through App Setup Policies by your Teams Administrator.

#### Does the user/customer in Hornbill require specific privileges?
The same privileges as for logging into the Employee Portal.

#### How do we distinguish which requests are from the chatbot?
The SourceType field (_h\_itsm\_requests.h\_source\_type_) is populated with "Chatbot"

#### Can multiple users use the chatbot at the same time?
Yes, the chatbot runs within Teams.

#### What do the Hornbill FAQs need to contain to be found?
The chatbot uses the same free text search as the customer would when searching through the Employee Portal. It also uses the same restrictions for the user, so if a relevant FAQ is part of a service the customer does not have access to, then that FAQ will not be shown in the results.

#### How do I find Service and Catalog IDs?

The Service ID can be found in the URL bar when looking at the Service's details in the Service Portfolio

The Catalog ID can be found by hovering over the Catalog Item Name within the Request Type's Catalog Items section (within the Service details in the Service Portfolio)

#### What is in the github package?
The github package contains a PowerShell script and a vanilla PVA. Your Teams Administrator can run this and provide the script with some Serive Manager details (API key, field containin UPN, Service ID & Catalog IDs). This exercise creates a package which your Teams Administrator can then import into the PowerApps backend.

The package will enable your user to search the FAQs, request the status of their calls and update their calls.

One is also able to log calls using the chatbot. To this end some sample mechanisms have been provided:
* a mechanism to log a generic incident (requesting summary and further description),
* a password reset sample,
* a new starter sample,
* a leaver sample.

The samples provide some rudimentary data gathering and pushes that into custom fields within the Request in Service Manager. The Business Process that is connected to the Service/Catalog Item can then pick those details and utilize them within the process.

#### Can we have link from the Hornbill portal to the chatbot?
Provided Teams allows for deeplinking to a particular PowerApp, then it should be possible as one is able to set up links on the Hornbill portal. Your Teams administrator can assist with this.

#### Can more functionalirty be added?
* What if I want to capture more data for a new starter?
* What happens to the chat history? Can it be included?
* Can a user raise a request on behalf of another eg for new starters or leavers?
* Can attachments/images be uploaded to the chatbot/requests? 

It should be possible for your PowerApps developer to expand on the chatbot to include functionality to meet these requirements.