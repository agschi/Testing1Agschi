---
title: "Unterstützte Plattformen in Azure Security Center | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Liste mit Windows- und Linux-Betriebssystemen, die in Azure Security Center unterstützt werden."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2017
ms.author: terrylan
ms.openlocfilehash: fd238f0b2d877f7f57a27ce495dae8de1ab9c066
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2017
---
# <a name="supported-platforms-in-azure-security-center"></a>Unterstützte Plattformen in Azure Security Center
Die Überwachung des Sicherheitsstatus und entsprechende Empfehlungen sind für virtuelle Computer (VMs) verfügbar, die mit dem klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell erstellt wurden.

> [!NOTE]
> Erfahren Sie mehr über das [klassische und das Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen.
>
>

## <a name="supported-platforms-for-windows-vms"></a>Unterstützte Plattformen für Windows-VMs
Unterstützte Windows-Betriebssysteme:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


## <a name="supported-platforms-for-linux-vms"></a>Unterstützte Plattformen für Linux-VMs
Unterstützte Linux-Betriebssysteme:

* Ubuntu-Versionen 12.04, 14.04, 16.04, 16.10
* Debian-Versionen 7, 8
* CentOS-Versionen 6.\*, 7.\*
* Red Hat Enterprise Linux-Versionen (RHEL) 6.\*, 7.\*
* SUSE Linux Enterprise Server-Versionen (SLES) 11 SP4+, 12.\*
* Oracle Linux-Versionen 6.\*, 7.\*

> [!NOTE]
> Verhaltensanalysen für virtuelle Computer sind für Linux-Betriebssysteme noch nicht verfügbar.
>
>

## <a name="vms-and-cloud-services"></a>VMs und Clouddienste
VMs, die in einem Clouddienst ausgeführt werden, werden ebenfalls unterstützt. Es werden nur Clouddienst-Webrollen und -Workerrollen überwacht, die in Produktionsslots ausgeführt werden. Weitere Informationen zum Clouddienst finden Sie unter [Sollte ich mich für Clouddienste oder für eine andere Lösung entscheiden?](../cloud-services/cloud-services-choose-me.md).

## <a name="next-steps"></a>Nächste Schritte

- [Planungs- und Betriebshandbuch für Azure Security Center:](security-center-planning-and-operations-guide.md) Hier erfahren Sie, wie Sie die Einführung von Azure Security Center planen, und Sie erhalten grundlegende Informationen zu Entwurfsaspekten.
- [Security alerts by type in Azure Security Center (Sicherheitswarnungen nach Typ in Azure Security Center):](https://docs.microsoft.com/azure/security-center/security-center-alerts-type.md#virtual-machine-behavioral-analysis) Hier finden Sie Informationen zur Verhaltensanalyse bei virtuellen Computern und zur Absturzabbild-Speicheranalyse in Security Center.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog (Blog zur Azure-Sicherheit):](http://blogs.msdn.com/b/azuresecurity/) Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
