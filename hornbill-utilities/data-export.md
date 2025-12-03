# Hornbill Data Export
This utility provides a quick and easy method of running reports on your Hornbill instance and exporting the report output into a local database.

The tool will:

* Run pre-built reports on your Hornbill instance
* Wait for the reports to complete
* Retrieve the CSV file that was created as part of the report run (as defined within the report itself)
* Add and/or update the report records into the MySQL, MariaDB or Microsoft SQL Server database table of your choice

**IMPORTANT** \- Any Report being run by this tool needs to be configured to provide CSV as an additional data format, within the **Output Formats** tab of the Report builder. 

## Installation

### Windows

* Download the latest release ZIP archive that is relevant to your operating system and architecture.
* Extract the ZIP archive into a folder you would like the application to run from e.g. 'C:\\hornbillDataExport'.

## Configuration

Example JSON File:
```json
{
    "Database":{
        "Driver": "mysql",
        "Authentication": "",
        "KeysafeKeyID": 1,
        "Encrypt": true
    },
    "Reports":[
        {
            "ReportID":1,
            "ReportName":"Testing",
            "DeleteReportInstance": false,
            "DeleteReportLocalFile": false,
            "UseXLSX": false,
            "Table":{
                "TableName":"h_itsm_requests",
                "PrimaryKey":"h_pk_reference",
                "Mapping":{
                    "RequestID":"h_pk_reference",
                    "Language":"h_summary"
                }
            }
        }
    ]
}
```
  
* **Database** \- An object containing the details of the database that the records will be written to:  
    * **Driver** \- The database type:  
        * **mysql** \- MySQL (v4 or above)or MariaDB  
        * **mssql** \- Microsoft SQL Server (2005 or above)  
   * **keySafeID** \- The ID of the keysafe database object in Hornbill  
   * **Authentication** \- The type of authentication to use to connect to the SQL server, if the Driver is set to **mssql**. Can be either:  
        * **Windows** \- Windows Account authentication, uses the logged-in Windows account to authenticate  
        * **SQL** \- uses SQL Server authentication, and requires the Username and Password parameters (inside Keysafe) to be populated  
   * **Encrypt** \- Boolean value to specify wether the connection between the script and the database should be encrypted. _NOTE_: There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to true if your SQL Server has been patched accordingly.
* **Reports** \- An array containing objects defining the reports to be run and retrieved:  
   * **ReportID** \- The integer ID of the report to be run. This can be found in the report URL when viewing the report in Hornbill Administration.  
   * **ReportName** \- The name of the report to be run.  
   * **DeleteReportInstance** \- Boolean true or false. This governs whether the run of the report should be deleted from the report history (found in Hornbill Administration) once it has run.  
   * **DeleteReportLocalFile** \- Boolean true or false. This governs whether the report CSV file should be removed after the import has completed  
   * **UseXLSX** \- Boolean true or false. If false, the tool will download and extract data from the CSV file from the report, if true the tool will download and extract data from the XLSX file from the report  
   * **Table** \- The table details required to map the report data in to:  
        * **TableName** \- The name of the table to insert/update report records in to.  
        * **PrimaryKey** \- The name of the primary key column within the table above.    Only use when the Database > Driver is set to mssql. Supports column names that include spaces, as long as the column name is wrapped in square brackets, for example: `"Request Reference":"[Request Reference]"`  
        * **Mapping** \- Allows you to map the report output columns to the database table columns. In the format `"ReportColumnName":"DatabaseTableColumnName"`. The DatabaseTableColumnName supports column names that include spaces, as long as for MS SQL databases the column name is wrapped in square brackets, for example:   `"Request Reference":"[Request Reference]"`. For MySQL/MariaDB databases the column name is wrapped in back-ticks, for example: ``"Request Reference":"`Request Reference`"`` 
                  

**NOTE on Date Format:** For any date/time column included in the report output (such as date logged, date closed, etc), we recommend that the "Raw" box is checked. This option is found in the configuration of the report in Hornbill Administration, specifically in the "Select Columns" tab and is located next to each of the selected columns which will be included in the output. When "Raw" is checked, this ensures that the raw database value (and hence format) will be outputted and any other application setting or logic will be ignored. If "Raw" is NOT checked for a date/time column, the date/time format used by the export will be taken from the system setting "system.regionalSettings.dateTimeFormat". This behavior can be checked by running the report in Hornbill Administration and inspecting the output before performing the data export.

  
## API Key Rules

This utility uses (API keys):

* reporting:reportRun
* reporting:reportRunGetStatus
* reporting:reportRunDelete
* admin:keysafeGetKey

## Execute

Command Line Parameters:

* **file** \- This should point to your JSON configuration file and by default looks for a file in the current working directory called conf.json. If this is present you don't need to have the parameter.
* **debug** \- Defaults to **false**. If set to **true**, then the log file will contain extra debugging information.
* **timeout** \- Defaults to **30** \- the number of seconds that the HTTP request should be given when retrieving large CSV report files from your Hornbill instance.
* **skipdb** \- Defaults to **false**. If set to **true**, then the tool will not attempt to insert/update the report records into a database.
* **creds** \- Defaults to **false**, if set to **true** the tool will returned the stored APIKey and end.

'goHornbillDataExport.exe -file=conf.json'

## Preparing to run the tool

* Open **conf.json** and add in the necessary configration;
* Open Command Line Prompt as Administrator;
* Change Directory to the folder with goHornbillDataExport\*.\* executables 'C:\\hornbillDataExport  
   * On 32 bit Windows PCs: goHornbillDataExport.exe  
   * On 64 bit Windows PCs: goHornbillDataExport.exe
* Follow all on-screen prompts, taking careful note of all prompts and messages provided.

  
## On First Run

You will be prompted for two key pieces of information:

* **APIKey** \- A valid API key created against a Hornbill user account with enough rights to run and retrieve the report (case sensitive). Details on how to create an API key can be found **here**.
* **InstanceId** \- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access your Hornbill instance i.e. `https://live.hornbill.com/**instanceid**/` (case sensitive).

This will create an export.cfg file, this file is encrypted and contains your instance ID and API Key. The user who first ran the tool will be the only one able to run the tool until this file is deleted. 

## HTTP Proxies

If you use a proxy for all of your internet traffic, the HTTP\_PROXY and HTTPS\_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:  
`set HTTP_PROXY=HOST:PORT`

`set HTTPS_PROXY=HOST:PORT`   
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number. IF you require a username and password to go through the proxy, the format for the setting is as follows:  
`set HTTP_PROXY=username:password@HOST:PORT`

`set HTTPS_PROXY=username:password@HOST:PORT`   

### URLs to White List

Occasionally on top of setting the HTTP\_PROXY variable the following URLs need to be white listed to allow access out to our network

* `https://files.hornbill.com/instances/INSTANCENAME/zoneinfo` - Allows access to lookup your Instance API Endpoint
* `https://files.hornbill.co/instances/INSTANCENAME/zoneinfo` - Backup URL for when files.hornbill.com is unavailable
* `https://eurapi.hornbill.com/INSTANCENAME/xmlmc/` - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
* `https://api.github.com/repos/hornbill/asset-rel-import/tags` - Allows the utility to self-update. **Optional**

## Troubleshooting

### Logging Overview

All Logging output is saved in the "log" directory which can be found in the same location as the executable. The file name contains the date and time the import was run e.g. _**dataexport\_20181004162344.log'**_ 

### Common Error Messages

Below are some common errors that you may encounter in the log file and what they mean:

* _**\[ERROR\] Error Decoding Configuration File:.....**_  \- this will be typically due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.

### Error Codes

* **100** \- Unable to create log File
* **101** \- Unable to create log folder
* **102** \- Unable to Load Configuration File

## Scheduling Overview

### Windows

You can schedule goHornbillDataExport.exe to run with any optional command line argument from Windows Task Scheduler.

* Ensure the user account running the task has rights to goHornbillDataExport.exe and the containing folder.
* Make sure the Start In parameter contains the folder where goHornbillDataExport.exe resides in otherwise it will not be able to pick up the correct path.
