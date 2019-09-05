---
title: Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 82ff1a684ffad75a301f0db6b0f8e3116be6746d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285065"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert

Xamarin 3,5-Projekte, xamarin. IOS-Projekte, werden automatisch bereitgestellt, wenn Sie auf die Schaltfläche " **Start** " klicken, oder wählen Sie das Menü Element **Debuggen >** Sie müssen das gewünschte xamarin. IOS-App-Projekt weiterhin als **Startprojekt** festlegen, bevor Sie einen dieser Befehle ausführen.

Aus diesem Grund sind die **Kontrollkästchen** bereitstellen absichtlich in der Visual Studio-Configuration Manager für xamarin. IOS-Projekte deaktiviert:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager zeigt das Kontrollkästchen \"Bereitstellen\" für ein xamarin. IOS-Projekt in xamarin 3,5 an")

Diese Änderung beseitigt einen Fehler, der in älteren Versionen von xamarin (Version 3,3 und früher) angezeigt werden kann, wenn das xamarin. IOS-App-Projekt nicht für die Bereitstellung festgelegt wurde:

![](deploy-checkboxes-images/error.png "Fehler Dialogfeld: Das Projekt iPhoneApp1 muss bereitgestellt werden, bevor es gestartet werden kann. Vergewissern Sie sich, dass das Projekt für die Bereitstellung in der Projekt Mappe Configuration Manager ausgewählt ist.")
