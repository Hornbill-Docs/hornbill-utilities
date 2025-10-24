# Power BI Reporting

<iframe width="560" height="315" src="https://www.youtube.com/embed/_xybLWgm5A8?si=_WHOJUxMv5idRIgF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Using Python 3
Using [Python 3](https://www.python.org/), and the Hornbill reporting and trend engines as data sources in Power BI.

### Overview
A number of example Python 3 scripts have been provided to enable Power BI administrators to use the Hornbill Reporting and Trending Engine APIs as Data Sources within [Power BI](https://powerbi.microsoft.com/) reports and dashboards.

The scripts can be found on our public [Github Repository](https://github.com/hornbill/pythonPowerBIHornbillDataSources).

### Dependencies
The scripts have been written in Python 3, and were developed using the following:
* [Power BI Desktop build 2.75.5649.861 64-bit (November 2019)](https://powerbi.microsoft.com/)
* [Python 3.8](https://docs.python.org/3/whatsnew/3.8.html)

The following packages are required dependencies, and can be installed using the Python Package Installer (pip):
* [requests](https://pypi.org/project/requests/)
* [pandas](https://pypi.org/project/pandas/)
* [xlrd](https://pypi.org/project/xlrd/)

### Configuration used in all scripts
Each script requires the following variables to be set (all case-sensitive):
* `apiKey` - This is an API key generated against a user account on the Hornbill Administration Console, where the user account has sufficient access to run reports and access trending data.
* `instanceId` - This is the (case sensitive) name of the instance to connect to. Your instance name can be found at the end of the URL which you use to navigate to your Hornbill instance i.e. `https://live.hornbill.com/[instanceName]/`

### Scripts
#### PowerBIReport.py
This script will:

* Run a pre-defined report on the Hornbill instance;
* Wait for the report to complete;
* Retrieve the report CSV data and present back as a dataframe called df, which can then be consumed by Power BI.

Script Variables:

* `reportId` - The ID (Primary Key, INT) of the report to be run.
* `suspendSeconds` - The number of seconds the script should wait between checks to see if the report is complete.
* `deleteReportInstance` - A boolean value to determine if, once the report is run on Hornbill and the data has been pulled in to PowerBI, whether the historic report run instance should be removed from your Hornbill report.
* `useXLSX` - False = the script will use the CSV output from your report; True = the script will use the XLSX output from your report. NOTE: XLSX output will need to be enabled within the Output Formats > Additional Data Formats section of your report in Hornbill.

#### PowerBIHistoricReport.py
This script will:

* Retrieve a historic report CSV from your Hornbill instance.
* Present the report data back as a dataframe called df, which can then be consumed by Power BI.

Script Variables:

 * `reportId` - The ID (Primary Key, INT) of the report to be run.
* `reportRunId` - The Run ID (INT) of a historic run of the above report ID.
- useXLSX: False = the script will use the CSV output from your report; True = the script will use the XLSX output from your report. NOTE: XLSX output will need to be enabled within the Output Formats > Additional Data Formats section of your report in Hornbill.

#### PowerBITrendingData.py
This script will:

* Run the reporting::measureGetInfo API against your Hornbill instance, with a given measure ID (Primary Key, INT).
* Build a table containing all Trend Value entries for the selected measure.
* Present the trend data back as a dataframe called df, which can then be consumed by Power BI.

Script Variable:

* `measureId` - The ID (Primary Key, INT) of the measure to return trend data from.

Outputs: As the response parameters from the Trending Engine is fixed (unlike the Reporting engine, which has user-specified column outputs), the output for this report will always consist of the following columns:
* `value` - the value of the trend sample;
* `sampleId` - the ID of the sample;
* `sampleTime` - the time & date that the sample was taken;
* `dateRange.from` - the start date of the sample snapshot;
* `dateRange.to` the end date of the sample snapshot;

### Power BI with Python Notes
Please see the [Power BI Documentation](https://docs.microsoft.com/en-us/power-bi/desktop-python-scripts) for more information about using Python with Power BI.

These scripts have been designed to be used as data sources only, and not as the source of Python visuals within Power BI. Which is not to say they couldn't be used in your Python visuals, with a little extra code and the [matplotlib](https://pypi.org/project/matplotlib/) library.

## Using R Scripts
Using [R Scripts](https://cran.r-project.org/), and the Hornbill reporting and trend engines as data sources in Power BI.

### Overview
A number of example R scripts have been provided to enable Power BI administrators to use the Hornbill Reporting and Trending Engine APIs as Data Sources within Power BI reports and dashboards.

![R Script](/_books/hornbill-utilities/images/power-bi-r-script.png)

The scripts can be found on our public [Github Repository]https://github.com/hornbill/rPowerBIHornbillDataSources.

### Dependencies
The scripts have been written in R, and were tested using the following:

* [Power BI Desktop v2.121.644.0 64-bit (September 2023)](https://powerbi.microsoft.com/)
* [R Open R.4.3.1](https://www.r-project.org/)

The following packages are required dependencies and can be installed from the CRAN repositories:

* [readr](https://cran.r-project.org/web/packages/readr/)
* [httr](https://cran.r-project.org/web/packages/httr/)
* [readxl](https://cran.r-project.org/web/packages/readxl/) - Just for the Report and HistoricReport scripts, when useXLSX is set to TRUE
* [data.table](https://cran.r-project.org/web/packages/data.table/) - Just for the TrendingData script

To install package(s):

* Launch RGui, which is provided as part of your R Open installation.
* Click Packages > Install Packages.
* Select your CRAN mirror of choice and click `OK`.
* Select the package(s) you wish to install and click `OK`.

### Configuration used in all scripts
Each script requires the following variables to be set (all case-sensitive):

* `instanceName` - This is the name of the instance to connect to. Your instance name can be found at the end of the URL which you use to navigate to your Hornbill instance i.e. https: //live.hornbill.com/[instanceName]/
* `apiKey` - This is an API key generated against a user account on the Hornbill Administration Console, where the user account has sufficient access to run reports and access trending data. The following page will outline how to create an API Key: API Keys

Each script can be configured to use a proxy for access to your Hornbill instance. Set all of the below to NULL to not use a proxy. If using a proxy, the proxyAddress and proxyPort are the minimum required to be provided.

* `proxyAddress` - The hostname or IP address of the proxy
* `proxyPort` - The proxy port
* `proxyUsername` - The username to access the proxy, if required
* `proxyPassword` - The password for the above account
* `proxyAuth` - The type of HTTP authentication to use. Should be one of the following: basic, digest, digest_ie, gssnegotiate, ntlm, any.

### Scripts
#### PowerBIDataSource_Report.R
This script will:
* Run a pre-defined report on the Hornbill instance.
* Wait for the report to complete.
* Retrieve the report CSV data and present back as an R data frame called dataframe, which can then be retrieved and reported on by Power BI.

Script Variables:
* `reportID` - The ID (Primary Key, INT) of the report to be run. This can be found in the browser URL when viewing the report in Hornbill Administration.
* `reportComment` - A comment to write against the report run job.
* `deleteReportInstance` - A boolean value to determine if, once the report is run on Hornbill and the data has been pulled in to PowerBI, whether the historic report run instance should be removed from your Hornbill report.
* `useXLSX` - FALSE = the script will use the CSV output from your report; TRUE = the script will use the XLSX output from your report. NOTE: XLSX output will need to be enabled within the Output Formats > Additional Data Formats section of your report in Hornbill;
* `deleteLocalXLSX` - FALSE = the downloaded XLSX file will remain on disk once the extract is complete; TRUE = the local XLSX file is deleted upon completion;
* `xlsxLocalFolder` - The folder where to store the downloaded XLSX file. Can be left blank, or specify a local folder to store the downloaded XLSX file into. Requires the postfixed / or \ on the path, depending on your OS;
* `csvEncoding` - The character set to be used when decoding the CSV report data, or when converting the XLSX data into a Power BI friendly codepage. This will usually be "UTF-8", but if you have issues returning data with certain characters (the Windows E2 80* characters are the usual culprits) then choose a different character set to use, ie: "ISO-8859-1". Look out for an error that looks like this for character set issues: "Details: "Unable to translate bytes [E2][80] at index 1077 from specified code page to Unicode"";
* `suspendSeconds` - The number of seconds the script should wait between checks to see if the report is complete.

#### PowerBIDataSource_HistoricReport.R
This script will:
* Retrieve a historic report CSV from your Hornbill instance;
* Present the report data back as an R data frame called dataframe, which can then be retrieved and reported on by Power BI.

Script Variables:
* `reportID` - The ID (Primary Key, INT) of the report to be run. This can be found in the browser URL when viewing the report in Hornbill Administration.
* `runId` - The Run ID (INT) of a historic run of the above report ID. This can be found in the "History" tab of the report you've specified in the reportID variable.
* `useXLSX` - FALSE = the script will use the CSV output from your report; TRUE = the script will use the XLSX output from your report. NOTE: XLSX output will need to be enabled within the Output Formats > Additional Data Formats section of your report in Hornbill;
* `deleteLocalXLSX` - FALSE = the downloaded XLSX file will remain on disk once the extract is complete; TRUE = the local XLSX file is deleted upon completion;
* `xlsxLocalFolder` - The folder where to store the downloaded XLSX file. Can be left blank, or specify a local folder to store the downloaded XLSX file into. Requires the postfixed / or \ on the path, depending on your OS;
* `csvEncoding` - The character set to be used when decoding the CSV report data, or when converting the XLSX data into a Power BI friendly codepage. This will usually be "UTF-8", but if you have issues returning data with certain characters (the Windows E2 80* characters are the usual culprits) then choose a different character set to use, ie: "ISO-8859-1". Look out for an error that looks like this for character set issues: "Details: "Unable to translate bytes [E2][80] at index 1077 from specified code page to Unicode"".

#### PowerBIDataSource_TrendingData.R
This script will:
* Run the reporting::measureGetInfo API against your Hornbill instance, with a given measure ID (Primary Key, INT).
* Build a table containing all Trend Value entries for the selected measure.
* Present the trend data back as an R data frame called dataframe, which can then be retrieved and reported on by PowerBI.

Script Variable:

* `measureID` - The ID (Primary Key, INT) of the measure to return trend data from. This can be found in the browser URL when viewing the measure in Hornbill Administration.

Outputs: As the response parameters from the Trending Engine is fixed (unlike the Reporting engine, which has user-specified column outputs), the output for this report will always consist of the following columns:

* `value` - The value of the trend sample;
* `sampleId` - The ID of the sample;
* `sampleTime` - The time & date that the sample was taken;
* `dateRange.from` - The start date of the sample snapshot;
* `dateRange.to` - The end date of the sample snapshot;

### Power BI with R Notes
These scripts have been designed to be used as data sources only, and not as the source of R script visuals within Power BI. Which is not to say they couldn’t be used in your R script visuals, with extra code of your own!

## HTTP Proxies
If you use a proxy for all of your internet traffic, the HTTP_PROXY Environment variable needs to be set on the local machine running Power BI Desktop.

If you use a proxy for all of your internet traffic, the HTTP_PROXY and HTTPS_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:
* `set HTTP_PROXY=HOST:PORT`
* `set HTTPS_PROXY=HOST:PORT`

Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number. IF you require a username and password to go through the proxy, the format for the setting is as follows:
* `set HTTP_PROXY=username:password@HOST:PORT`
* `set HTTPS_PROXY=username:password@HOST:PORT`

### URLs to White List
Occasionally on top of setting the HTTP_PROXY variable the following URLs need to be white listed to allow access out to our network

* `https://files.hornbill.com/instances/INSTANCENAME/zoneinfo` - Allows access to lookup your Instance API Endpoint
* `https://files.hornbill.co/instances/INSTANCENAME/zoneinfo` - Backup URL for when files.hornbill.com is unavailable
* `https://eurapi.hornbill.com/INSTANCENAME/xmlmc/` - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
* https://api.github.com/repos/hornbill/asset-rel-import/tags - Allows the utility to self-update. Optional

## API Key Rules
This utility uses (API keys):
* reporting:reportRun
* reporting:reportRunGetStatus
* reporting:reportRunDelete
* reporting:measureGetInfo