# Hornbill Clean Utility

## About the Hornbill Clean Utility
The utility provides a quick and easy method of removing requests, assets or users from a specified Hornbill instance.

:::warning
This utility permanently deletes request, asset or user records from a specified Hornbill instance, and records of entities that are associated to the deleted requests/assets. It is primarily intended to be used only by an administrator of a Hornbill instance at the appropriate stage of the switch-on process, to remove demonstration and test data prior to go-live.
:::

## Open Source
The Hornbill Cleaner Utility is provided open source under the [Hornbill Community Licence](https://github.com/hornbill/goHornbillCleaner/blob/master/LICENSE.md) and can be found [Here](https://github.com/hornbill/goHornbillCleaner) on GitHub.

## Installation Overview
### Windows Installation
* [Download the ZIP archive](https://github.com/hornbill/goHornbillCleaner/releases/latest) relevant to your OS and architecture. This contains the cleaner executable, configuration file and license.
* Extract the ZIP archive into a folder you would like the application to run from e.g. 'C:\hornbill_cleaner\'.

## Configuration Overview
The configuration of this utility is managed through a JSON file (conf.json), which is supplied with each release:

        "CleanRequests": true,
        "RequestServices":[
                1,
                2,
                3
        ],
        "RequestStatuses":[
                "status.open",
                "status.cancelled",
                "status.closed",
                "status.resolved"
        ],
        "RequestTypes":[
                "Incident",
                "Service Request"
        ],
        "RequestLogDateFrom":"2016-01-01 00:00:00",
        "RequestLogDateTo":"-P1D2H3M4S",
        "RequestClosedDateFrom":"2016-01-01 00:00:00",
        "RequestClosedDateTo":"2018-01-01 00:00:00",
        "RequestReferences":[
                "CHR00000021",
                "INC00000003"
        ],
        "CleanAssets": false,
        "AssetClassID": "",
        "AssetFilters": [
                        {
                                "ColumnName": "h_name"
                                "ColumnValue": "YourAssetName",
                                "Operator": "Equals",
                                "IsGeneralProperty": true
                        }
        ],
        "CleanUsers": true,
        "Users":[
                "UserIDOne",
                "UserIDTwo"
        ],
        "CleanServiceAvailabilityHistory": false,
        "ServiceAvailabilityServiceIDs": [],
        "CleanContacts": true,
        "ContactIDs": [
                5,
                8,
                13,
                21
        ],
        "CleanOrganisations": true,
        "OrganisationIDs": [
                34,
                55
        ],
        "CleanSuppliers": true,
        "SupplierIDs": [
                89,
                144
        ],
        "CleanSupplierContracts": true,
        "SupplierContractIDs": [
                "C20210700233",
                "C20210700377",
        ],
        "CleanEmails": false,
        "EmailFilters": {
                "FolderIDs": [
                        1,
                        7,
                        18
                ],
                "RecipientAddress": "",
                "RecipientClass": "",
                "ReceivedFrom": "2016-01-01 00:00:00",
                "ReceivedTo": "2017-01-01 00:00:00",
                "Subject": "%out of office%"
        },
        "CleanReports": true,
        "ReportIDs": [
                1,
                2,
                3,
                5,
                8,
                13
        ],
        "CleanChatSessions": true,
        "ChatSessionIDs": [
                "CS00354",
                "CS00346"
        ],

### Clean Requests
* **CleanRequests** : Set to true to remove all Service Manager Requests (and related entity data) from a Hornbill instance. Filter the requests to be deleted using the following parameters.
* **RequestServices** : An array containing Service ID Integers to filter the requests for deletion against. An empty array will remove the Service filter, meaning requests with any or no service associated will be deleted.
* **RequestStatuses** : An array containing Status strings to filter the requests for deletion against. An empty array will remove the Status filter, meaning requests at any status will be deleted.
* **RequestTypes** : An array containing Request Type strings to filter the requests for deletion against. An empty array will remove the Type filter, meaning requests of any Type will be deleted.
* **RequestLogDateFrom** : A date to filter requests against log date (requests logged after or equal to this date/time). Can take one of the following values:
An empty string will remove the Logged From filter.
A date string in the format YYYY-MM-DD HH:MM:SS.
A duration string, to calculate a new datetime from the current datetime:
Example: -P1D2H3M4S This will subtract 1 day (1D), 2 hours (2H), 3 minutes (3H) and 4 seconds (4S) from the current date & time.
See the CalculateTimeDuration function documentation in https://github.com/hornbill/goHornbillHelpers for more details
* **RequestLogDateTo** : A date to filter requests against log date (requests logged before or equal to this date/time). Can take one of the following values:
An empty string will remove the Logged Before filter.
A date string in the format YYYY-MM-DD HH:MM:SS.
A duration string, to calculate a new datetime from the current datetime:
Example: -P1D2H3M4S This will subtract 1 day (1D), 2 hours (2H), 3 minutes (3H) and 4 seconds (4S) from the current date & time.
See the CalculateTimeDuration function documentation in https://github.com/hornbill/goHornbillHelpers for more details
* **RequestClosedDateFrom** : A date to filter requests against close date (requests closed after or equal to this date/time). Can take one of the following values:
An empty string will remove the closed From filter.
A date string in the format YYYY-MM-DD HH:MM:SS.
A duration string, to calculate a new datetime from the current datetime:
Example: -P1D2H3M4S - This will subtract 1 day (1D), 2 hours (2H), 3 minutes (3H) and 4 seconds (4S) from the current date & time.
See the CalculateTimeDuration function documentation in <https://github.com/hornbill/goHornbillHelpers> for more details
* **RequestClosedDateTo** : A date to filter requests against close date (requests closed before or equal to this date/time). Can take one of the following values:
An empty string will remove the Closed Before filter.
A date string in the format YYYY-MM-DD HH:MM:SS.
A duration string, to calculate a new datetime from the current datetime:
Example: -P1D2H3M4S - This will subtract 1 day (1D), 2 hours (2H), 3 minutes (3H) and 4 seconds (4S) from the current date & time.
See the CalculateTimeDuration function documentation in <https://github.com/hornbill/goHornbillHelpers> for more details
* **RequestReferences** : An array of Request References to delete. If requests are defined in this array, then ONLY these requests will be deleted. The other parameters above will be ignored. In the example above, requests with reference CHR00000021 and INC00000003 would be deleted, and no other requests would be removed.
* **KeepRequestsCancelBPTasks** : a boolean (defaulting to false) which if set to true will NOT actually delete the selected requests, but will cancel the Business Process Workflow and any Tasks connected to the request.

### Clean Assets
* **CleanAssets** : Set to true to remove all Assets (and related entity data) from a Hornbill instance
* **AssetClassID** : Filter assets for deletetion by a single asset class ID (basic, computer, computerPeripheral, mobileDevice, printer, software, telecoms)
* **AssetFilters** : Array of filters to apply to the query when returning assets to delete. Each object in the array should contain:
* **ColumnName** : The name of the column in the assets general or extended table;
* **ColumnValue** : The value to filter by;
* **Operator** : The operator to apply, can be one of:
    * Empty - column is null or an empty string;
    * Equals - column equals the provided value;
    * NotEquals - column does not equal the provided value;
    * Greater - column value is greater than the provided value;
    * Less - column value is less than than the provided value;
    * LastXDays - date columns, value in the last X days where X is provided in ColumnValue;
    * LastMonth - date columns, value in the last month;
    * PreviousMonth - date columns, value in the previous month;
    * ThisMonth - date columns, value this month;
    * LastWeek - date columns, value last week
    * Yesterday - date columns, value yesterday;
    * Today - date columns, value today;
    * Before - date columns, value before the datetime value provided in ColumnValue;
    * After - date columns, value after the datetime value provided in ColumnValue;
    * BeforeXDays - date columns, before the number of days provided in ColumnValue;
    * Regex - column matches the regular expression provided in ColumnValue;
    * NotRegex - column doesn't match the regular expression provided in ColumnValue;
* **IsGeneralProperty** : If true, the column exists in the general assets table. If false, the column is in the extended assets table;

### Clean Users
* **CleanUsers** : Set to true to remove all Users listed in the Users array
* **Users** : Array of strings, contains a list of User IDs to remove from your Hornbill instance
    * CleanServiceAvailabilityHistory - Set to true to remove the Service Availability History records for all services listed in the ServiceAvailabilityServiceIDs array
* **ServiceAvailabilityServiceIDs** : Array of integers, contains a list of all Service IDs whose Availability Historty records should be deleted
    * CleanContacts - Set to true to remove the Contacts listed in the ContactIDs array
* **ContactIDs** : Array of integers, contains a list of all Contact IDs that should be deleted
    * CleanOrganisations - Set to true to remove the Organisations listed in the OrganisationIDs array
* **OrganisationIDs** : Array of integers, contains a list of all Organisation IDs that should be deleted
    * CleanSuppliers - Set to true to remove the Suppliers listed in the SupplierIDs array, and all associated records
* **SupplierIDs** : Array of integers, contains a list of all Supplier IDs that should be deleted
    * CleanSupplierContracts - Set to true to remove the Supplier Contracts listed in the SupplierContractIDs array, and all associated records
* **SupplierContractIDs** : Array of strings, contains a list of all Supplier Contact IDs that should be deleted
    * CleanEmails - Set to true to remove any email records that match the supplied filters
        * EmailFilters - The list of conditions to filter the email records for deletion by:
            * FolderIDs - MANDATORY - Array of integers, to contain a list of Folder IDs to filter the email records by
            * RecipientAddress - Deletes emails from this recipient email address
            * RecipientClass - Deletes emails from this recipient email class. Can be one of:
                * unknown
                * to
                * cc
                * bcc
                * from
                * replyTo
                * returnReceiptTo
            * ReceivedFrom - Date/time field, delete all emails with a date/time stamp greater than or equal to this value
            * ReceivedTo - Date/time field, delete all emails with a date/time stamp less than or equal to this value
            * Subject - Filter emails by this subject. Supports % wildcard characters
* **CleanReports** : Set to true to delete any reports, as defined in ReportsIDs below:
    * ReportsIDs - Array of integers, contains a list of all Report IDs that you would like to delete.
* **CleanChatSessions** : Set to true to delete chat sessions from the target instance.
    * ChatSessionIDs - Array of strings - a list of chat session IDs to delete. If no chat session IDs are provided, then ALL chat sessions will be deleted.

## Running the utility
When you are ready to clear-down your request, asset or user records:

Open conf.json and add in the necessary configuration;
On Windows:
Open a Command Line Prompt as Administrator;
On OSX or Linux:
 Open a Terminal;
Change Directory to the folder containing the cleaner executable and configuration files E.g. 'C:\hornbill_cleaner\' or '/Users/YourUserID/hornbillCleaner/';
Run the appropriate command:
Windows: hornbillCleaner.exe -instance=yourinstancename -apikey=yourapikey
OSX or Linux Terminal: ./hornbillCleaner -instance=yourinstancename -apikey=yourapikey

PLEASE NOTE:

Follow all on-screen prompts, taking care to read all messages provided.
If any errors are experienced, first review the Trouble Shooting section below. If you are unable to find the answer there, please seek further advice on the Hornbill Forums

## Post Utility Actions
After the Hornbill Clean Utility has been run successfully, you can reset your request reference number or asset ID back to 0:

Resetting Application Auto Values
After the Hornbill Clean utility has been run successfully, you may like to reset the relevant application Auto Values such as the request reference number. This is not essential, more a personal preference.
The Application Auto Values can be reset via Hornbill Administration > Home > System > Data > Auto Values.

PLEASE NOTE: Only reset the auto values for the entities you have cleared down:

itsmRequestsAutoId - this auto value is responsible for generating request reference numbers (only reset this if you have deleted all your requests).
itsmAssetAutoId - this auto value generates the id for new asset records (only reset this if you have deleted all your asset records).
Click the "Reset Counter" button to reset the Auto Value

## Command Line Parameters
-instance - This should be the ID of your instance
-apikey - This should be an API of a user on your instance that has the correct rights to perform the search & deletion of the specified records
-file - This is the name of the Configuration file to load. If this parameter is not specified, by default the utility will look for `conf.json'
-BlockSize x - Where x is the number of records that should be retrieved and deleted as "blocks". If this parameter is not specified, the default is 3, and under normal circumstances this should not need to be overridden.
-dryrun - Requires Service Manager build >= 1392 to work with request data. This boolean flag allows a "dry run" to be performed - the tool identifies the primary key for all parent records that would have been deleted, and outputs them to the log file without deleting any records. Defaults to false.
-justrun - This boolean flag allows you to skip the confirmation prompts when the tool is run. This allows the tool to be scheduled, with the correct configuration defined to delete request records over a certain age for example. Defaults to false.

## API Key Rules
This utility uses (API keys):

admin:getApplicationList
admin:userDelete
bpm:processCancel
bpm:processDelete
data:entityBrowseRecords2
data:entityDeleteRecord
data:entityGetRecord
data:getRecordCount
data:queryExec
reporting:reportDelete
system:logMessage
task:taskCancel
task:taskDelete
time:timerDelete
time:timerEventDelete
apps/com.hornbill.boardmanager/Card:removeCard
apps/com.hornbill.core/Task:getEntityTasks

## Minimum permissions
The account for which the API key is generated will need to be a: Full User with an "Admin" level Role.

The minimum permissions necessary:

sys.a.canCancelProcess
sys.a.deleteTimer
sys.a.deleteUser
sys.a.executeStoredQuery
sys.a.manageTimers
sys.a.manageUsers
sys.b.deleteTask
sys.b.manageTasks
sys.b.updateTask
sys.c.manageBpm
sys.d.getRecord
sys.d.manageApplications
sys.f.deleteReport
sys.f.manageReports
boardmanager.a.canAddTo
boardmanager.b.boardPrivledgedUser
servicemanager.h.searchEntityRecords
If creating a Custom Role to include these permissions ensuring that the Privilege Level is set to Admin will meet the above requirements.

## Troubleshooting
### Logging Overview
All logging output is saved to the Hornbill instance, in a log called Hornbill_Clean, and can be accessed from within the Hornbill Administration tool.

### Common Error Messages
Below are some common errors that you may encounter in the log file and what they mean:

SQL Query was unsuccessful - this occurs when the utility is prevented from running the query to obtain the list of requests to delete. This happens when the system setting security.database.allowSqlQueryOperation is set to false (OFF). To successfully run the utility, this setting must be set to true (ON). It can be found in Hornbill Administration > System > Settings > Advanced, and filter on "security".
Error Codes
100 - Unable to create log File
101 - Unable to create log folder
102 - Unable to Load Configuration File

## HTTP Proxies
If you use a proxy for all of your internet traffic, the HTTP_PROXY and HTTPS_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:
set HTTP_PROXY=HOST:PORT

set HTTPS_PROXY=HOST:PORT
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number. IF you require a username and password to go through the proxy, the format for the setting is as follows:
set HTTP_PROXY=username:password@HOST:PORT

set HTTPS_PROXY=username:password@HOST:PORT

### URLs to White List
Occasionally on top of setting the HTTP_PROXY variable the following URLs need to be white listed to allow access out to our network

https://files.hornbill.com/instances/INSTANCENAME/zoneinfo - Allows access to lookup your Instance API Endpoint
https://files.hornbill.co/instances/INSTANCENAME/zoneinfo - Backup URL for when files.hornbill.com is unavailable
https://eurapi.hornbill.com/INSTANCENAME/xmlmc/ - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
https://api.github.com/repos/hornbill/asset-rel-import/tags - Allows the utility to self-update. Optional