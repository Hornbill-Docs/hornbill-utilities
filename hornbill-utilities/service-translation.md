# Service Translation Tool
The utility provides a simple, safe and secure way to bulk-translate your Services, and associated Catalog Items, FAQs, Bulletins, Customer Feedback Questions and Sub Statuses. Translation of Sub Statuses requires Service Manager build of 2144 or above.

The tool is designed to run behind your corporate firewall and connects to your Hornbill instance in the cloud over HTTPS/SSL. So as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

  
## Installation Overview

* Download the architecture-specific ZIP archive
* Extract zip into a folder you would like the application to run from e.g. **C:\\Users\\yourserviceaccount\\servicetranslate\\**
* Open a Command Line Prompt as Administrator
* Change Directory to the folder containing the utility **C:\\Users\\yourserviceaccount\\servicetranslate\\**
* Run the command:

  
`services_translation.exe -src="en-GB" -dst="it"` 

## Configuration

### Command Line Parameters

* `debug`: true/false, defaults to false - Log extended debug information;
* `src`: The source language code to translate the records from;
* `dst`: The destination language code to translate the records to;
* `getlangs`: Return a list of supported languages and their codes into a file in the tool folder called languages.txt, then exits;
* `version`: Output the tools version number, and exit;
* `creds`: Defaults to false. Set to true to be prompted to input your instance ID, and you will be presented with the stored API key.

## First Run

From version **2.0.0** of the Service Translation tool, when you first run the utility it will prompt you for the ID of your Hornbill instance (case-sensitive), and the API key (also case-sensitive) that will be used to authenticate the API calls back into Hornbill. This information will be encrypted, and stored locally on the client PC that will be running the tool. For each subsequent import run, the utility will decrypt your instance ID and API key, and will use those to make the API calls back into Hornbill.

NOTE - the encrypted instance ID and API key can only be decrypted on the computer, and by the user, that performed the encryption, so please keep this in mind when scheduling your imports.

Should you wish to use a different API key to what has been previously encrypted, just delete the **import.cfg** file from the folder where the import binary resides, and re-run your import from the command line inputting the Instance ID and new API Key as you would have on its first run.

## API Key Rules

This utility uses (API keys):

* data:entityBrowseRecords2
* system:getLanguageList
* session:getApplicationList
* apps/com.hornbill.servicemanager/Services:smGetServiceDetails
* apps/com.hornbill.servicemanager:translateData
* apps/com.hornbill.servicemanager:translateAddLanguage

## HTTP Proxies

If you use a proxy for all of your internet traffic, the HTTP\_PROXY Environment variable needs to be set. The https\_proxy environment variable holds the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:  
`set HTTP_PROXY=HOST:PORT set HTTPS_PROXY=HOST:PORT`   
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number.

## Troubleshooting

#### Logging Overview

All logging output is saved in the log directory, in the same directory as the executable. The file name contains the date and time the import was run _**Service\_Translation\_20210202142114.log**_ 