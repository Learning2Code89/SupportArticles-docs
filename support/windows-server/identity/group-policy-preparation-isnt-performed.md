---
title: Group Policy preparation isn't performed
description: Describes an issue that occurs when you automatically prepare an existing domain for Windows Server 2012 by using Windows Server 2012 Server Manager or the Install-AddsDomainController Windows PowerShell cmdlet.
ms.date: 09/23/2020
author: Deland-Han 
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika, nedpyle
ms.prod-support-area-path: DCPromo and the installation of domain controllers
ms.technology: ActiveDirectory
---
# Group Policy preparation isn't performed when you automatically prepare an existing domain for Windows Server 2012

This article describes an issue that Group Policy preparation isn't performed when you automatically prepare an existing domain.

_Original product version:_ &nbsp; Windows Server 2012 R2  
_Original KB number:_ &nbsp; 2737129

## Symptoms

When you automatically prepare an existing domain for Windows Server 2012 by using Windows Server 2012 Server Manager or the Install-AddsDomainController Windows PowerShell cmdlet, Group Policy preparation isn't performed. Additionally, the following output is displayed briefly when you use Install-AddsDomainController:

Adprep successfully updated the forest-wide information.

Adprep successfully updated the domain-wide information.

The new cross domain planning functionality for Group Policy, RSOP Planning Mode, requires file system and Active Directory Domain Services permissions to be updated for existing Group Policy Objects (GPOs).

> [!NOTE]
> This output is not visible when you install Active Directory Domain Services (AD DS) by using Server Manager, and the output is not logged.

## Cause

This behavior is by design. The automated remote domain preparation capabilities that are added in Windows Server 2012 don't alter the Group Policy preparation (Gpprep) behavior that occurred in earlier operating systems.

Gpprep adds cross-domain planning functionality for Group Policy and Resultant Set of Policy (RSoP) planning mode. This requires updating the file system in SYSVOL and Active Directory permissions for existing group policies. If the environment already contains custom or delegated permissions that were put in place manually by administrators, Gpprep triggers replication of all Group Policy files in SYSVOL and may deny RSOP functionality to delegated users until their permissions are re-created by administrators.

## Resolution

Domain administrators must run adprep.exe /domainprep /gpprep manually, just as they had to do in Windows Server 2003, Windows Server 2008, and Windows Server 2008 R2.

## More information

Administrators should run Gpprep only one time in the history of a domain. They shouldn't run Gpprep every time that an upgrade occurs. Gpprep was introduced in Windows Server 2003.

The Adprep.exe tool is included on the Windows Server 2012 media at \support\adprep. This version of Adprep.exe supports remote preparation and doesn't have to run on the infrastructure master operations master (also known as flexible single master operations or FSMO) role, as it did in previous server operating systems.

For more information about legacy Adprep.exe commands, go to the following Microsoft TechNet website:  
[Prepare Your Infrastructure for Upgrade](https://technet.microsoft.com/library/cc783495%28v=ws.10%29.aspx)