---
title: Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: c3d0b8f2da971238b98253405f3b8fe08699ab56
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997214"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert

Xamarin 3,5-Projekte, xamarin. IOS-Projekte, werden automatisch bereitgestellt, wenn Sie auf die Schaltfläche " **Start** " klicken, oder wählen Sie das Menü Element **Debuggen >** Sie müssen das gewünschte xamarin. IOS-App-Projekt weiterhin als **Startprojekt** festlegen, bevor Sie einen dieser Befehle ausführen.

Aus diesem Grund sind die **Kontrollkästchen** bereitstellen absichtlich in der Visual Studio-Configuration Manager für xamarin. IOS-Projekte deaktiviert:

![Visual Studio Configuration Manager zeigt das Kontrollkästchen "Bereitstellen" für ein xamarin. IOS-Projekt in xamarin 3,5 an](deploy-checkboxes-images/configuration.png)

Diese Änderung beseitigt einen Fehler, der in älteren Versionen von xamarin (Version 3,3 und früher) angezeigt werden kann, wenn das xamarin. IOS-App-Projekt nicht für die Bereitstellung festgelegt wurde:

![Fehler Dialogfeld: das Projekt iPhoneApp1 muss bereitgestellt werden, bevor es gestartet werden kann. Vergewissern Sie sich, dass das Projekt für die Bereitstellung in der Projekt Mappe Configuration Manager ausgewählt ist.](deploy-checkboxes-images/error.png)
