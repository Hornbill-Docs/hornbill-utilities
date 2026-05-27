# Hornbill Task Canceller

## Overview

The utility provides a simple way to either cancel tasks from within the Hornbill Collaboration Tool.  The tool is designed to run behind your corporate firewall, and requires access to your Hornbill instance.

The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

When the tool is executed, depending on configuration, a list of tasks will be canceled.

## Installation Overview

### Windows Installation

* Download the OS and architecture specific [ZIP archive](https://github.com/hornbill/goHornbillTaskCanceller/releases/latest)
* Extract zip into a folder you would like the application to run from e.g. ```C:\data\```
* Open a Command Line Prompt as Administrator
* Change Directory to the folder containing the utility ```C:\data\```
* Determine the appropriate executable and possibly rename it to remove confusion.
* Run the command relevant to the OS of the machine you are running this on:

Windows:

```bat
taskCanceller.exe -instance=testinstance -api=abc...def -listfile=C:\data\tasks.csv
```

```bat
taskCanceller.exe -instance=testinstance -api=abc...def -taskref=TSK1234567
```

## Command Line Arguments

- *instance* **string** : ID of the instance to connect to
- *api **string** : The API Key to use to authenticate against your Hornbill instance
- *taskref* **string** : Single Task Reference (format: TSK###)
- *listfile* **string** : File name of file containing list of task references (one per line)
- *delete* **boolean** : Set to true to delete the task(s), defaults to false and the cancelation of the task(s)

[[INCLUDE /hornbill-utilities/_includes/api-key-rules.md]]

```cmd
task:taskCancel
task:taskDelete
```

[[INCLUDE /hornbill-utilities/_includes/network.md]]

## Troubleshooting

### Logging Overview

No logging apart from a summary at the end and errors/issues will be reflected in the command terminal

## Extras

Bundled with the app is a file **open-tasks-on-cancelled-requests.report.txt** which is a Service Manager report export which can be imported to identify orphaned tasks (i.e. live tasks which identifies all Task IDs of tasks which are not completed/canceled to canceled requests). 
