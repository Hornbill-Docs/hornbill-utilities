# User Role Removal Utility

This utility functions through the command line parameters only - i.e. no configuration file.

The CSV file given as parameter *-file* **REQUIRES** a header with field name(s).

The column/field to use is given in the **-header=field** parameter, but defaults to **userid** (case-sensitive!).

```
  -api string
        API Key
  -debug
        Debug mode - additional logging (default true)
  -dryrun
        Dry Run
  -file string
        CSV File
  -header string
        The header/fieldname in the CSV to use (default "userid")
  -instance string
        Instance ID
  -version
        Return version and end
```

[User Role Removal Utility](https://github.com/hornbill/goHUserRoleRemover/releases/latest)