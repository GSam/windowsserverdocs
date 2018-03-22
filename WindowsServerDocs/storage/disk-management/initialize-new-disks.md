---
title: Initialize new disks
description: How to initialize new disks with Disk Management, getting them ready to use. Also includes links to troubleshooting issues.
ms.date: 12/29/2017
ms.prod: windows-server-threshold 
ms.technology: storage 
ms.topic: article 
author: JasonGerend 
manager: brianlic 
ms.author: jgerend 
---
# Initialize new disks

> **Applies To:** Windows 10, Windows 8.1, Windows 7, Windows Server (Semi-Annual Channel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

If you add a brand new disk to your PC, you might need to initialize it before using it. Initializing a disk erases everything on it and prepares it for use by Windows, after which you can format it and then store files on it.

> [!WARNING]
> If your disk already has files on it that you care about, don't initialize it - you'll lose all the files. Instead we recommend troubleshooting the disk to see if you can read the files - see [A disk's status is Not Initialized or the disk is missing entirely](troubleshooting-disk-management.md#disk-not-initialized).

## To initialize new disks

Here's how to initialize a new disk using Disk Management. If you prefer using PowerShell, use the [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) cmdlet instead.

1. In Disk Management, right-click the disk you want to initialize, and then click **Initialize Disk** (shown here). If the disk is listed as *Offline*, first right-click it and select **Online**.

    ![](media\uninitialized-disk.PNG)
2. In the **Initialize Disk** dialog box (shown here), check to make sure that the correct disk is selected and then click **OK** to accept the default partition style. If you need to change the partition style (GPT or MBR) see [About partition style - GPT and MBR](#about-partition-styles-GPT-and-MBR).<br>The disk status briefly changes to **Initializing** and then to the **Online** status. If initializing fails for some reason, see [A disk's status is Not Initialized or the disk is missing entirely](troubleshooting-disk-management.md#disk-not-initialized).

    ![](media\initialize-disk.PNG)

## About partition styles - GPT and MBR

Disks can be divided up into multiple chunks called partitions. Each partition - even if you have only one - has to have a partition style - GPT or MBR. Windows uses the partition style to understand how to access the data on the disk.

As fascinating as this probably isn't, the bottom line is that these days, you don't usually have to worry about partition style - Windows automatically uses the appropriate disk type.

Most PCs use the GUID Partition Table (GPT) disk type for hard drives and SSDs. GPT is more robust and allows for volumes bigger than 2 TB. The older Master Boot Record (MBR) disk type is used by 32-bit PCs, older PCs, and removable drives such as memory cards.

To convert a disk from MBR to GPT or vice versa, you first have to delete all volumes from the disk, erasing everything on the disk. For more info, see [Convert an MBR disk into a GPT disk](change-an-mbr-disk-into-a-gpt-disk.md), or [Convert a GPT disk into an MBR disk](change-a-gpt-disk-into-an-mbr-disk.md).

## Additional considerations

- You must be a member of the **Backup Operators** or **Administrators** group, at minimum, to initialize a disk.