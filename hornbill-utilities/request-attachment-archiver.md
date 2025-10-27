# Request Attachment Archiver Utility

## Overview
The utility provides a simple, safe and secure way to extract file attachments from the Hornbill platform. The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

This tool does two things:
- Attachments to requests which have not been updated for x amount of weeks (x > 12) will be wrapped together in a .zip file
- remove links to those attachments from the Hornbill instance

:::info
**Important:** One of the optimizations within the Hornbill platform is that the same file (e.g. an image in a email footer) is only stored once. There is a counter which keeps track of how many times that file is used/referenced (within Service Manager). Only once the counter is zero (i.e. there is no request referencing that attachment), is the actual file removed. The "removal" in this utility only reduces the counter by one - if that happens to make the reference number zero, then it will have the subsequent effect of actual file removal. Within the affected requests, you WILL still see a reference to the file inteh "Attachments"-section (i.e. so you still have an overview of the files attached), BUT a download attempt will fail. At that point, you will need to refer to the backed up/archived .zip file - which is easily identified by the call reference.
:::


## Installation Overview

### Windows Installation

- Download the [ZIP archive](https://github.com/hornbill/goRequestAttachmentArchiver/releases/latest) relevant to your OS and architecture
- Extract zip into a folder you would like the application to run from e.g. `C:\HornbillRequestArchive\`
- Open `conf.json` and add in the necessary configuration
- Open a Command Line Prompt as Administrator
- Change Directory to the folder containing the import files `C:\HornbillRequestArchive\`
- Run the command:

For Windows Systems:
```bat
goRequestAttachmentArchiver.exe -cutoff=26 -dryrun=true -file=conf.json
```

For Mac OSX and Linux Systems:
```bash
./goRequestAttachmentArchiver -cutoff=26 -dryrun=true -file=conf.json
```

To run this on a schedule, you might want to consider the following sample usage which locates the files to a local folder named for the current date:
```bat
setlocal
set M=%date:~3,2%
set Y=%date:~6,4%
set D=%date:~0,2%
goRequestAttachmentArchiver.exe -cutoff=52 -dryrun=true -output=%Y%%M%%D%
endlocal
```

## Configuration Overview

A demonstration configuration file is provided within the package. If a configuration file is not specified as a command line argument when executing the tool, then a default configuration file named conf.json, containing the correct JSON, must exist:

```json
{
	"InstanceID": ""
	, "APIKeys": [
		""
	]
	, "AttachmentFolder": "C:/Temp/"
}
```

### Config
- `InstanceID` - the name of your Hornbill instance and can be found within the URL you use to navigate to it: live.hornbill.com/[instance name]/. E.g. if the URL you use to access your instance is live.hornbill.com/arescomputing/, then your instance id would be "arescomputing". '''This value is case sensitive'''.
- `APIKeys` - an array of API Keys. Hornbill API key for a user account with the correct permissions to carry out all of the required API calls. Details on how to create an API key can be found below.
- `AttachmentFolder` - The location where the files are going to be archived.
  - The format of the .zip file will be REQUESTID_2015-11-06T14-26-13Z.zip - each attachment that was found for that request will appear in the .zip file.

### Command Line Parameters

- *file* - Defaults to `conf.json` - Name of the Configuration file to load
- *dryrun* - Defaults to `false` - Set to True and the code for the REMOVAL of the attachments will not be called, and instead the generated XML for each asset will be dumped to the log file. This is to aid in debugging the initial connection information.
- *output* - Folder to store downloads in - overrides AttachmentFolder from the configuration file.
- *cutoff* - Defaults to `12`. Set the cut off date in weeks (12 or greater) - requests which haven't been touched for longer than this amount of time will be picked up.
- *pagesize* - Defaults to `100` - Default Query Size (how many results per page).
- *call* - IF a specific Request ID is given, then that request will be archived.
- *DoNotArchiveFiles* - Set this to true to and the files will NOT be archived, BUT the files WILL be removed (if not dryrun)
- *updateRequest* - Set this to true to update the request with the timeline entry of siphoned off files (Team Visibility). The additional **API Key Rule** required: `apps/com.hornbill.servicemanager/Requests:updateReqTimeline`

### Testing Overview
There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities.

```bat
goRequestAttachmentArchiver.exe -call=IN01234567 -cutoff=26 -dryrun=true -file=conf.json
```

This should create a IN01234567_2015-11-06T14-26-13Z.zip file in the *AttachmentFolder* configured in the .json. You should be able to open the file as usual and view compare the files in the .zip with those attached to the call.

#### Command Line Output
After each run of the utility, the command line will output a summary of the records that were processed. 

This output can also be found in the log files which should be examined to understand why records failed to archive. In the case of a failed archive, even if this is only due to a problem with one of the attributes, then the attachments will NOT be purged from the request.

:::info
**Important:** IF you are running the script for the **first time**, there is probably a lot of data to process.

It is recommended that you process this in a few steps.

For instance if you have 5 years (260 weeks) of accumulated requests, and wish to only remove the attachments of requests which have not been of updated for longer than a year (52 weeks):

Instead of running the script with a cutoff of 52 (which you would do regularly AFTER this first exercise), run the script with a **cutoff** of *250*, and then reducing in **manageable** steps until you get to the 52 weeks *(eg: 225, 200, ..., 52)*
:::

[[INCLUDE /hornbill-utilities/_includes/api-key-rules.md]]

```cmd
data:queryExec
data:entityAttachBrowse
data:entityAttachFile
data:entityAttachRemove
system:pingCheck
apps/com.hornbill.servicemanager/Requests:updateReqTimeline (for use with -updateRequest Command Line Parameter)
```

[[INCLUDE /hornbill-utilities/_includes/network.md]]

## Troubleshooting

### Logging Overview

All logging output is saved in the log directory, in the same directory as the executable. The file name contains the date and time the import was run '''''RAA_2015-11-06T14-26-13Z.log'''''

### Common Error Messages

Below are some common errors that you may encounter in the log file and what they mean:
- '' '''[ERROR] Error Decoding Configuration File:.....''' '' - this will be typically due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
- '' '''[ERROR] https:// ........invalid request :path "//xmlmc//apps/com.hornbill.servicemanager/?method=[''methodName'']"''' '' - If you identify errors stating an "invalid request path" for one or more API calls, this is typically due to a missing or incorrect instance name specified in the conf.json file. Check the instance id is correct. It also may be prudent to check you have added a valid API key too.

### Error Codes

- `100` - Unable to create log File
- `101` - Unable to create log folder
- `102` - Unable to Load Configuration File

[[INCLUDE /hornbill-utilities/_includes/scheduling.md]]
