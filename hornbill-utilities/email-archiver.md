# Email Archiver Utility

## Overview
The utility provides a simple, safe and secure way to extract file attachments from the Hornbill platform. The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

This tool does two things:
- Emails, from specified mailboxes and/or folders, which are older than a specified amount of time, will be downloaded into individual .eml files.
- Delete those emails.

:::important
One of the optimizations within the Hornbill platform is that the same file (e.g. an image in a email footer) is only stored once. There is a counter which keeps track of how many times that file is used/referenced (within Emails). Only once the counter is zero (i.e. there is no email referencing that attachment), is the actual file removed. This utility '''deletes''' the email which in turn reduces the counter to the specific attachments by one - if that happens to make the reference number zero, then it will have the subsequent effect of actual file removal.
:::

## Installation Overview 

### Windows Installation

- Download the [ZIP archive](https://github.com/hornbill/goEmailArchiver/releases/latest) relevant to your OS and architecture
- Extract zip into a folder you would like the application to run from e.g. `C:\HornbillEmailArchive\`
- Open `conf.json` and add in the necessary configuration
- Open a Command Line Prompt as Administrator
- Change Directory to the folder containing the import files `C:\HornbillEmailArchive\`
- Run the command:

For Windows Systems:
```bat
goEmailAttachmentArchiver.exe -cutoff=26 -dryrun=true -file=conf.json
```

For Mac OSX and Linux Systems:
```bash
./goEmailAttachmentArchiver -cutoff=26 -dryrun=true -file=conf.json
```

To run this on a schedule, you might want to consider the following sample usage which locates the files to a local folder named for the current date:
```bat
setlocal
set M=%date:~3,2%
set Y=%date:~6,4%
set D=%date:~0,2%
goEmailAttachmentArchiver.exe -cutoff=52 -dryrun=true -output=%Y%%M%%D%
endlocal
```

## Configuration Overview

A demonstration configuration file is provided within the package. If a configuration file is not specified as a command line argument when executing the tool, then a default configuration file named conf.json, containing the correct JSON, must exist:

```javascript
{
	"InstanceID": ""
	, "APIKeys": [
		""
	]
	, "AttachmentFolder": "C:/Temp/"
	, "Mailboxes": [ ]
	, "Folders": [ ]
}
```

### Config
- **InstanceID** - the name of your Hornbill instance and can be found within the URL you use to navigate to it: live.hornbill.com/`instance name`/. E.g. if the URL you use to access your instance is live.hornbill.com/arescomputing/, then your instance id would be "arescomputing". '''This value is case sensitive'''.
- **APIKeys** - an array of API Keys. Hornbill API key for a user account with the correct permissions to carry out all of the required API calls. Details on how to create an API key can be found below.
- **AttachmentFolder** - The location where the files are going to be archived.
  - The format of the .eml file will be EMAILID_2015-11-06T14-26-13Z.zip - The email header, (optional) email text, (optional) html email text and each attachment that was found for that email will appear as attachments within the .eml file.
- **Mailboxes** - array to store the names of mailboxes eg `[ "servicedesk", "facilities" ]` - all folders in the given mailbox will be processed. Please note that the mailbox name is to be used and NOT the descriptive name of the mailbox. You can find them in Admin -> System -> Email -> Shared Mailboxes.
- **Folders** - array to store the numeric IDs of specific folders eg `[ 12, 37 ]`.
- **AttachmentDiscrepancyOverride** - boolean `true/false` - default `false` - removes emails for which the attachment count does not tally (default behavior keeps the email).

### Command Line Parameters

- *file* - Defaults to `conf.json` - Name of the Configuration file to load
- *dryrun* - Defaults to `false` - Set to True and the code for the REMOVAL of the attachments will not be called, and instead the generated XML for each asset will be dumped to the log file. This is to aid in debugging the initial connection information.
- *output* - Folder to store downloads in - overrides AttachmentFolder from the configuration file.
- *cutoff* - Defaults to `12`. Set the cut off date in weeks (12 or greater) - emails which have been in the mailfolder/box older than this amount of time will be picked up and removed.
- *nolocalkeep* - Defaults to `false` - Does NOT download the emails, only deletes them. This is useful if you already have email backups from your Mail Server and thus do not need a second archive.
- *forcedelete* - Defaults to `false` - removes emails for which the attachment count does not tally (default behavior keeps the email).
- *pagesize* - Defaults to `100` - Default Query Size (how many results per page).

### Testing Overview
There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities. 

#### Command Line Output
After each run of the utility, the command line will output a summary of the records that were processed. 

This output can also be found in the log files which should be examined to understand why records failed to archive. In the case of a failed archive, even if this is only due to a problem with one of the attributes, then the email will NOT be purged from the mailbox/folder.

:::important
If you are running the script for the **first time**, there is probably a lot of data to process.

It is recommended that you process this in a few steps.

For instance if you have 5 years (260 weeks) of accumulated emails, and wish to only keep emails no older than a year (52 weeks):

Instead of running the script with a cutoff of 52 (which you would do regularly AFTER this first exercise), run the script with a **cutoff** of *250*, and then reducing in **manageable** steps until you get to the 52 weeks *(eg: 225, 200, ..., 52)*
:::

[[INCLUDE /hornbill-utilities/_includes/api-key-rules.md]]

```cmd
data:queryExec
mail:deleteMessage
mail:folderGetList
mail:getMessage
system:pingCheck
```

### Minimum permissions
The account for which the API key is generated will need to be a: Full User

The minimum permissions necessary:

- sys.a.executeStoredQuery
- mail.deleteMessage
- mail.getFolder
- mail.getMessage

[[INCLUDE /hornbill-utilities/_includes/network.md]]

## Troubleshooting

### Logging Overview

All logging output is saved in the log directory, in the same directory as the executable. The file name contains the date and time the import was run '''''EAA_2015-11-06T14-26-13Z.log'''''

### Common Error Messages

Below are some common errors that you may encounter in the log file and what they mean:
- '' '''[ERROR] Error Decoding Configuration File:.....''' '' - this will be typically due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
- '' '''[ERROR] https:// ........invalid request :path "//xmlmc//apps/com.hornbill.servicemanager/?method=[''methodName'']"''' '' - If you identify errors stating an "invalid request path" for one or more API calls, this is typically due to a missing or incorrect instance name specified in the conf.json file. Check the instance id is correct. It also may be prudent to check you have added a valid API key too.

#### Error Codes
- `100` - Unable to create log File
- `101` - Unable to create log folder
- `102` - Unable to Load Configuration File

[[INCLUDE /hornbill-utilities/_includes/scheduling.md]]
