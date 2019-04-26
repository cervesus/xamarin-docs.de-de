---
title: Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 35efb00a721062ad3217300f7e3a5430b1bd1560
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357751"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert

Da Xamarin 3.5, Xamarin.iOS-Projekte automatisch bereitgestellt, wenn Sie drücken die **starten** Symbolleisten-Schaltfläche, oder wählen Sie die **Debuggen > Debuggen starten** Menüelement. Sie müssen weiterhin das gewünschte Xamarin.iOS-app-Projekt als Festlegen der **Startprojekt** vor dem Ausführen dieser Befehle.

Aus diesem Grund die **bereitstellen** Kontrollkästchen sind absichtlich im Visual Studio Konfigurations-Manager für Xamarin.iOS-Projekte deaktiviert:

![](deploy-checkboxes-images/configuration.png "Configuration Manager in Visual Studio mit der \"Bereitstellen\" das Kontrollkästchen für eine Xamarin.iOS-Projekt in Xamarin 3.5 deaktiviert")

Diese Änderung schließt einen Fehler, der in früheren Versionen von Xamarin (Version 3.3 und früher) angezeigt werden kann, wenn das Xamarin.iOS-app-Projekt nicht, zum Bereitstellen festgelegt wurde:

![](deploy-checkboxes-images/error.png "Dialogfeld \"Fehler: Das Projekt iPhoneApp1 muss bereitgestellt werden, bevor es gestartet werden kann. Stellen Sie sicher, dass das Projekt im Projektmappenkonfigurations-Manager bereitgestellt werden, ausgewählt ist.")
