---
title: Bereitstellen von Kontrollkästchen in Configuration Manager deaktiviert
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: c0f116565a2741c62a00ed2a255cfde8c57b8569
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Bereitstellen von Kontrollkästchen in Configuration Manager deaktiviert

Seit Xamarin 3.5 Xamarin.iOS Projekte automatisch bereitgestellt, wenn Sie durch Drücken der **starten** Symbolleisten-Schaltfläche oder Auswählen der **Debuggen > Debuggen starten** Menüelement. Müssen Sie dennoch legen Sie das gewünschte Xamarin.iOS-app-Projekt als dem **Startprojekt** vor ihrer Ausführung Befehle einen der beiden vor.

Aus diesem Grund die **bereitstellen** Kontrollkästchen sind in der Visual Studio Configuration Manager für Projekte Xamarin.iOS absichtlich deaktiviert:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager mit der "Bereitstellung" das Kontrollkästchen für ein Xamarin.iOS-Projekt in Xamarin 3.5 deaktiviert")

Diese Änderung schließt einen Fehler, der bei der Xamarin.iOS-app-Projekt nicht, zum Bereitstellen festgelegt wurde in früheren Versionen von Xamarin (Version 3.3 und früher) angezeigt werden konnte:

![](deploy-checkboxes-images/error.png "Dialogfeld zu Fehler: das Projekt iPhoneApp1 muss bereitgestellt werden, bevor er gestartet werden kann. Stellen Sie sicher, dass das Projekt ausgewählt ist, in der Projektmappe Configuration Manager bereitgestellt werden.")
