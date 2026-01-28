---
title: IDCAMS
description: Integrated Data Cluster Access Method Services
permalink:
aliases:
tags:
  - zOS
draft: false
date: 2025-12-27
cssclasses:
---
IDCAMS stands for **I**ntegrated **D**ata **C**luster **A**ccess **M**ethod **S**ervices. It can perform the following tasks:
- Define, delete, alter, copy, and list catalogs, data sets[^1], catalog entries, or tape library entries.
- Collect data set and Systems Managed Storage (SMS) information, detect catalog problems, detect VSAM data set problems, retrieve Direct Access Storage Device (DASD) information, set cache parameters for DASD devices, and communicate with SMS VSAM.
- Turn VSAM Record Level Sharing (RLS) on and off.

- Define VSAM data sets.
- Define and build alternate indexes.
- Back up and restore VSAM data sets.
- Copy data sets.
- Print the contents of data sets.
- Delete data sets.
- Collect information about data sets.
- Examine the structural consistency of VSAM key-sequenced data sets.
- Control DASD cache.
- List tape volume (VOLCAT) catalog entries.
- Diagnose catalog errors.
- Recover from catalog errors.
- Define system-managed libraries and volumes.
- Define extended addressability for an extended-format VSAM data set to support a data set size greater than 4 GB.
- Encrypt and decrypt data sets.

The commands in IDCAMS are passed in the `SYSIN`.
There are 9 IDCAMS commands -
* [[ALTER]] - used for changing file attributes
* [[DEFINE]] - used for file creations (Typically VSAMs)
* [[DELETE]] - used for file deletions (Typically VSAMs)
* [[EXPORT]]
* [[IMPORT]]
* [[LISTCAT]] - used for accessing catalog entries
* [[PRINT]] - used for printing the contents from a dataset to `SYSOUT`
* [[REPRO]] - used for copying data

## Sample JCL

```jcl
//STEP01   EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN *
  ALTER HLQ.DATA.OLDNAME -
  NEWNAME(HLQ.DATA.NEWNAME)
/*
```

[^1]: IDCAMS is widely used with VSAMs but it supports flat file and tape as well