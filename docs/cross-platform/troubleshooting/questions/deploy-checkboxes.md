---
title: Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: edf471f1d9a2ee4adc11f09e0c7b7ad3cf6f78f1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014256"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Kontrollkästchen für die Bereitstellung in Configuration Manager deaktiviert

Xamarin 3,5-Projekte, xamarin. IOS-Projekte, werden automatisch bereitgestellt, wenn Sie auf die Schaltfläche " **Start** " klicken, oder wählen Sie das Menü Element **Debuggen >** Sie müssen das gewünschte xamarin. IOS-App-Projekt weiterhin als **Startprojekt** festlegen, bevor Sie einen dieser Befehle ausführen.

Aus diesem Grund sind die **Kontrollkästchen** bereitstellen absichtlich in der Visual Studio-Configuration Manager für xamarin. IOS-Projekte deaktiviert:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager showing the 'Deploy' checkbox disabled for a Xamarin.iOS project in Xamarin 3.5")

Diese Änderung beseitigt einen Fehler, der in älteren Versionen von xamarin (Version 3,3 und früher) angezeigt werden kann, wenn das xamarin. IOS-App-Projekt nicht für die Bereitstellung festgelegt wurde:

![](deploy-checkboxes-images/error.png "Error dialog: The project iPhoneApp1 needs to be deployed before it can be started. Verify the project is selected to be deployed in the Solution Configuration Manager.")
