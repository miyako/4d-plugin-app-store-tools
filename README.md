# 4d-plugin-app-store-tools

Set of cryptographic functions to help implement App Store Receipt Validation.

In particular, the plugin provides methods to validate receipts __locally__. To validate receipts __with the App Store__, you can simply use native commands like ``HTTP Request`` and ``JSON Parse``.

**Note**: In-app purchase receipts are not supported.

For details, see [Receipt Validation Programming Guide](https://developer.apple.com/library/content/releasenotes/General/ValidateAppStoreReceipt/Introduction.html).

###Platform

| carbon | cocoa | win32 | win64 |
|:------:|:-----:|:---------:|:---------:|
|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|||

###Version

<img src="https://cloud.githubusercontent.com/assets/1725068/18940649/21945000-8645-11e6-86ed-4a0f800e5a73.png" width="32" height="32" /> <img src="https://cloud.githubusercontent.com/assets/1725068/18940648/2192ddba-8645-11e6-864d-6d5692d55717.png" width="32" height="32" />

##Syntax

```
hash:=RECEIPT Get hash
```

Parameter|Type|Description
------------|------|----
hash|TEXT|The hash value; it should match the hash value retuned by ``GUID Compute hash``

This command reads the computed hash value recorded in the App store receipt.

```
identifier:=RECEIPT Get identifier
```

Parameter|Type|Description
------------|------|----
identifier |TEXT|The application identifier; it should match the running application's identifier returned by ``BUNDLE Get identifier``

This command reads the application identifier recorded in the App store receipt.

```
opaque:=RECEIPT Get opaque
```

Parameter|Type|Description
------------|------|----
opaque |TEXT|The opaque hash key; it can be used for validation with ``GUID Compute hash``

This command reads the opaque hash key recorded in the App store receipt.

```
version:=RECEIPT Get version
```

Parameter|Type|Description
------------|------|----
version  |TEXT|The application version; it should match the running application's version returned by ``BUNDLE Get version``

This command reads the application version recorded in the App store receipt.

```
version:=RECEIPT Get original version
```

Parameter|Type|Description
------------|------|----
version  |TEXT|The version of the app that was originally purchased

This command reads the original application version recorded in the App store receipt.

```
date:=RECEIPT Get creation date
```

Parameter|Type|Description
------------|------|----
date  |TEXT|The date when the app receipt was created

This command reads the date when the app receipt was created recorded in the App store receipt.

```
date:=RECEIPT Get expiration date
```

Parameter|Type|Description
------------|------|----
date  |TEXT|The date that the app receipt expires

This command reads the date that the app receipt expires recorded in the App store receipt.


```
identifier:=BUNDLE Get identifier
```

Parameter|Type|Description
------------|------|----
identifier   |TEXT|The application identifier; it should match the receipt application identifier returned by ``RECEIPT Get identifier``

This command returns the running application's bundle identifier.

Every application bundle should have its own unique identifier, in the form of reverse DNS. For example, 4D.app has the UID ``com.4d.4d``. When building a standalone application for disribution, you would want to give your final application its own ID.

```
version:=BUNDLE Get version
```

Parameter|Type|Description
------------|------|----
version    |TEXT|The application version; it should match the receipt application version returned by ``RECEIPT Get version``

This command returns the running application's version.

Every application bundle should have a version string defined in it's ``info.plist`` file.

```
hash:=GUID Compute hash (machine; opaque; application)
```

Parameter|Type|Description
------------|------|----
machine|TEXT|The computer's GUID, returned by ``GUID Get identifier``
opaque|TEXT|The receipt's opaque hash key, returned by ``RECEIPT Get opaque``
application|TEXT|The application's UTI, returned by ``BUNDLE Get identifier``
hash |TEXT|The computed hash value. This whould match the hash returned by ``RECEIPT Get hash``

This command computes the Hash using the application's ID and version, using the GUID and hash key. 

If the result matches the value in the receipt, the receipt is valid for the computer, application and version.

```
identifier:=GUID Get identifier
```

Parameter|Type|Description
------------|------|----
identifier |TEXT|The machine's unique identifier. It can be used for validation with ``GUID Compute hash``

This command returns the current machine's unique identifier.

```
EXIT 173
```

This command calls the UNIX command ``exit``, with status code ``173``. Code ``173`` is only recognized by OS X 10.6.6 or later. For that reason, the command will perform a version check at runtime, and do nothing unless (major>=10 & minor>=6 & bugfix>=6).

When the system receives an exit status ``173``, it will attempts to obtain a valid receipt for that application.

You should make sure all your 4D exit protocol have been followed properly, before throwing a direct exit code to the system.

###Example

```
  //functions to read *.app/Contents/Info.plist are named BUNDLE*
  //functions to read *.app/Contents/_MASReceipt/receipt are named RECEIPT*


  //decoding the receipt is expensive; check 1 step at a time!
  //you can test by putting an existing receipt (xcode, for example) inside 4D...

$B_identifier:=BUNDLE Get identifier 
$R_identifier:=RECEIPT Get identifier 

If ($B_identifier=$R_identifier)  //user didn't chuck in a receipt from a different app

  $B_version:=BUNDLE Get version 
  $R_version:=RECEIPT Get version 

  If ($B_version=$R_version)  //user didn't chuck in a receipt from an old version

      //function to get MAC address of en0
    $GUID:=GUID Get identifier 
    $R_opaque:=RECEIPT Get opaque 

    $R_hash:=RECEIPT Get hash 
    $G_hash:=GUID Compute hash ($GUID;$R_opaque;$R_identifier)

    If ($G_hash=$R_hash)
      TRACE  //the receipt hash matches the computed hash; true customer!

      If (False)
          //for more checks
        $R_creation_date:=RECEIPT Get creation date 
        $R_expiration_date:=RECEIPT Get expiration date 
        $R_original_version:=RECEIPT Get original version 
      End if 

    Else 
      TRACE
        //this is the offiial error code for bad app receipt
        //EXIT 173 
    End if 

  End if 
End if 
```
