---
title: Update CSP
description: Update CSP
ms.assetid: F1627B57-0749-47F6-A066-677FDD3D7359
ms.reviewer: 
manager: dansimp
ms.author: dansimp
ms.topic: article
ms.prod: w10
ms.technology: windows
author: manikadhiman
ms.date: 02/23/2018
---

# Update CSP

The Update configuration service provider enables IT administrators to manage and control the rollout of new updates.

The following diagram shows the Update configuration service provider in tree format.

![update csp diagram](images/provisioning-csp-update.png)

## Update
The root node.

Supported operation is Get.

### ApprovedUpdates
Node for update approvals and EULA acceptance on behalf of the end-user.

> [!NOTE]
> When the RequireUpdateApproval policy is set, the MDM uses the ApprovedUpdates list to pass the approved GUIDs. These GUIDs should be a subset of the InstallableUpdates list.

The MDM must first present the EULA to IT and have them accept it before the update is approved. Failure to do this is a breach of legal or contractual obligations. The EULAs can be obtained from the update metadata and have their own EULA ID. It&#39;s possible for multiple updates to share the same EULA. It is only necessary to approve the EULA once per EULA ID, not one per update.

The update approval list enables IT to approve individual updates and update classifications. Auto-approval by update classifications allows IT to automatically approve Definition Updates (i.e., updates to the virus and spyware definitions on devices) and Security Updates (i.e., product-specific updates for security-related vulnerability). The update approval list does not support the uninstallation of updates by revoking approval of already installed updates. Updates are approved based on UpdateID, and an UpdateID only needs to be approved once. An update UpdateID and RevisionNumber are part of the UpdateIdentity type. An UpdateID can be associated to several UpdateIdentity GUIDs due to changes to the RevisionNumber setting. MDM services must synchronize the UpdateIdentity of an UpdateID based on the latest RevisionNumber to get the latest metadata for an update. However, update approval is based on UpdateID.

> [!NOTE]
> For the Windows 10 build, the client may need to reboot after additional updates are added.

Supported operations are Get and Add.

#### <a id="approvedupdates-approved-update-guid"></a> Approved Update Guid
Specifies the update GUID.

To auto-approve a class of updates, you can specify the <a href="https://go.microsoft.com/fwlink/p/?LinkId=526723" data-raw-source="[Update Classifications](https://go.microsoft.com/fwlink/p/?LinkId=526723)">Update Classifications</a> GUIDs. We strongly recommend to always specify the DefinitionsUpdates classification (E0789628-CE08-4437-BE74-2495B842F43B), which are used for anti-malware signatures. There are released periodically (several times a day). Some businesses may also want to auto-approve security updates to get them deployed quickly.

Supported operations are Get and Add.

Sample syncml:

```
<LocURI>./Vendor/MSFT/Update/ApprovedUpdates/%7ba317dafe-baf4-453f-b232-a7075efae36e%7d</LocURI>
```

##### <a id="approvedupdates-approved-update-guid-approvedtime"></a> ApprovedTime
Specifies the time the update gets approved.

Supported operations are Get and Add.

### <a id="failedupdates"></a> FailedUpdates
Specifies the approved updates that failed to install on a device.

Supported operation is Get.

#### <a id="failedupdates-failed-update-guid"></a> Failed Update Guid
Update identifier field of the UpdateIdentity GUID that represent an update that failed to download or install.

Supported operation is Get.

##### <a id="failedupdates-failed-update-guid-hresult"></a> HResult
The update failure error code.

Supported operation is Get.

##### <a id="failedupdates-failed-update-guid-status"></a> Status
Specifies the failed update status (for example, download, install).

Supported operation is Get.

##### <a id="failedupdates-failed-update-guid-revisionnumber"></a> RevisionNumber
Added in Windows 10, version 1703. The revision number for the update that must be passed in server to server sync to get the metadata for the update.

Supported operation is Get.

### <a id="installedupdates"></a> InstalledUpdates
The updates that are installed on the device.

Supported operation is Get.

#### <a id="installedupdates-installed-update-guid"></a> Installed Update Guid
UpdateIDs that represent the updates installed on a device.

Supported operation is Get.

##### <a id="installedupdates-installed-update-guid-revisionnumber"></a> RevisionNumber
Added in Windows 10, version 1703. The revision number for the update that must be passed in server to server sync to get the metadata for the update.

Supported operation is Get.

### <a id="installableupdates"></a> InstallableUpdates
The updates that are applicable and not yet installed on the device. This includes updates that are not yet approved.

Supported operation is Get.

#### <a id="installableupdates-installable-update-guid"></a> Installable Update Guid
Update identifiers that represent the updates applicable and not installed on a device.

Supported operation is Get.

##### <a id="installableupdates-installable-update-guid-type"></a> Type
The UpdateClassification value of the update. Valid values are:

-   0 - None
-   1 - Security
-   2 = Critical

Supported operation is Get.

##### <a id="installableupdates-installable-update-guid-revisionnumber"></a> RevisionNumber
The revision number for the update that must be passed in server to server sync to get the metadata for the update.

Supported operation is Get.

### <a id="pendingrebootupdates"></a> PendingRebootUpdates
The updates that require a reboot to complete the update session.

Supported operation is Get.

#### <a id="pendingrebootupdates-pending-reboot-update-guid"></a> Pending Reboot Update Guid
Update identifiers for the pending reboot state.

Supported operation is Get.

##### <a id="pendingrebootupdates-pending-reboot-update-guid-installedtime"></a> InstalledTime
The time the update is installed.

Supported operation is Get.

##### <a id="pendingrebootupdates-pending-reboot-update-guid-revisionnumber"></a> RevisionNumber
Added in Windows 10, version 1703. The revision number for the update that must be passed in server to server sync to get the metadata for the update.

Supported operation is Get.

### <a id="lastsuccessfulscantime"></a> LastSuccessfulScanTime
The last successful scan time.

Supported operation is Get.

### <a id="deferupgrade"></a> DeferUpgrade
Upgrades deferred until the next period.

Supported operation is Get.

### <a id="rollback"></a> Rollback
Added in Windows 10, version 1803. Node for the rollback operations.

#### <a id="rollback-qualityupdate"></a> QualityUpdate
Added in Windows 10, version 1803. Roll back latest Quality Update, if the machine meets the following conditions:

-  Condition 1: Device must be Windows Update for Business Connected
-  Condition 2: Device must be in a Paused State
-  Condition 3: Device must have the Latest Quality Update installed on the device (Current State)

If the conditions are not true, the device will not Roll Back the Latest Quality Update.

#### <a id="rollback-featureupdate"></a> FeatureUpdate
Added in Windows 10, version 1803. Roll Back Latest Feature Update, if the machine meets the following conditions:

-  Condition 1: Device must be Windows Update for Business Connnected
-  Condition 2: Device must be in Paused State
-  Condition 3: Device must have the Latest Feature Update Installed on the device (Current State)
-  Condition 4: Machine should be within the uninstall period

> [!Note]
> This only works for Semi Annual Channel Targeted devices.

If the conditions are not true, the device will not Roll Back the Latest Feature Update.

#### <a id="rollback-qualityupdatestatus"></a> QualityUpdateStatus
Added in Windows 10, version 1803. Returns the result of last RollBack QualityUpdate operation.

#### <a id="rollback-featureupdatestatus"></a> FeatureUpdateStatus
Added in Windows 10, version 1803. Returns the result of last RollBack FeatureUpdate operation.

## Related topics

[Configuration service provider reference](configuration-service-provider-reference.md)
