# Restore objects

Before you access Archive or Cold Archive objects, you must call RestoreObject to restore these objects. This topic describes how to restore objects.

**Note:** For more information about the operation used to restore objects, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md).

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Restore objects.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/restore.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/Restore objects.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/Restore objects.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Manage objects/Restore objects.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/Restore objects.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Manage objects/Restore an archive object.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Manage objects/Restore objects.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Manage objects/Restore an archive object.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Manage objects/Restore an Archive object.md)|

## Restore processes

Status changes throughout the process of restoring an Archive object:

1.  By default, the Archive object is in the frozen state.
2.  After you submit a restore request, the object enters the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state, and you can read the object. By default, the restored state lasts 24 hours, and can be extended by another 24 hours after you call RestoreObject again. During one restore process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

Status changes throughout the process of restoring a Cold Archive object:

1.  By default, the Cold Archive object is in the frozen state.
2.  After you submit a restore request, the object enters the restoring state. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object as follows:
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours. If the JobParameters element is not passed in, the default restore mode is Standard.
    -   Bulk: The object is restored within five to twelve hours.
3.  After the server completes the restore task, the object enters the restored state, and you can read the object. You can specify the duration for which the object can remain in the restored state, which ranges from one to seven days.
4.  After the restored state expires, the object returns to the frozen state.

## Usage notes

-   Data retrieval fees are generated when you restore Archive and Cold Archive objects. For more information, see [Data processing fees](/intl.en-US/Pricing/Billing items and methods/Data processing fees.md).
-   The restored state can be extended to up to seven days. During this period, no data retrieval fees are generated.
-   After the restored state expires, the object returns to the frozen state. Data retrieval fees are generated if you perform the restore operation on the object again.
-   When you restore a Cold Archive object, a Standard replica is generated for temporary access. OSS charges the temporary storage fees of the replica for the duration during which the replica is available based on Standard storage.

