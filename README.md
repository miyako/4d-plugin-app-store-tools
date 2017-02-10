# 4d-plugin-app-store-tools

###Platform

Set of functions to help implement receipt validation

| carbon | cocoa | win32 | win64 |
|:------:|:-----:|:---------:|:---------:|
|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|||

###Version

<img src="https://cloud.githubusercontent.com/assets/1725068/22371270/93e3661c-e4d9-11e6-9021-4a9754c70630.png" width="32" height="32" /> <img src="https://cloud.githubusercontent.com/assets/1725068/18940648/2192ddba-8645-11e6-864d-6d5692d55717.png" width="32" height="32" />

See [Receipt Validation Programming Guide](https://developer.apple.com/library/content/releasenotes/General/ValidateAppStoreReceipt/Introduction.html)

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



