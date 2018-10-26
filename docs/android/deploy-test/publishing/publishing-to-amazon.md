---
title: Veröffentlichen im Amazon App Store
ms.prod: xamarin
ms.assetid: A3E9EAC7-2968-8891-CDF2-B73FC0013EC9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: e65030092b1f59b1111bc521a8613cfe8a9160ee
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118694"
---
# <a name="publishing-to-the-amazon-app-store"></a>Veröffentlichen im Amazon App Store

Mit dem Verteilungsprogramm für mobile Apps von Amazon können Entwickler mobiler Apps ihre Anwendungen auf Amazon veröffentlichen. In diesem Abschnitt wird kurz der Amazon Appstore für Android behandelt. 

[![Amazon Appstore-Bildschirm](publishing-to-amazon-images/amazon-app-store.png)](publishing-to-amazon-images/amazon-app-store.png#lightbox)

Amazon schränkt die Größe von APKs nicht ein. Wenn ein APK jedoch größer als 30 MB ist, wird FTP statt des Amazon Mobile App-Distributionsportals zur Verteilung verwendet.


## <a name="submitting-apps-binary-info"></a>Übermitteln von Apps: Binärdaten

Das Verfahren zum Übermitteln von Anwendungen an den Amazon Appstore entspricht dem Verfahren zum Übermitteln von Anwendungen an Google Play. Für Anwendungen, die von Amazon verteilt werden, sind die folgenden Ressourcen erforderlich: 

-   **Symbol**: Dies ist eine 114 x 114 PNG-Datei mit transparentem Hintergrund. Erforderlich.
-   **Miniaturansicht**: Dies ist eine größere Version des Symbols oben. Sie weist 512 x 512 Pixel und einen transparenten Hintergrund auf. Dieses Symbol ist ebenfalls obligatorisch.
-   **Screenshots**: Bei Amazon sind mindestens drei und maximal 10 Screenshots erforderlich. Die Screenshots müssen 1024 x 600 Pixel oder 800 x 480 Pixel aufweisen. Es sind sowohl PNG- als auch JPG-Formate zulässig.
-   **Werbebild**: Es kann optional ein Werbebild übermittelt werden, damit eine Anwendung bei Werbeplatzierungen z.B. auf der Startseite vorgestellt werden kann. Dabei muss es sich um eine PNG- oder eine JPG-Datei im Querformat mit 1024 x 500 Pixeln handeln. Animationen sind nicht zulässig.
-  Es können Updates auf fünf Videos bereitgestellt werden.



## <a name="approval-process"></a>Genehmigungsprozess

Wenn eine Anwendung übermittelt wurde, durchläuft sie einen Genehmigungsprozess.
Amazon überprüft die Anwendung, um sicherzustellen, dass sie funktioniert wie in der Produktbeschreibung angegeben, keine Kundendaten gefährdet und den Gerätebetrieb nicht beeinträchtigt. Nach Abschluss des Genehmigungsprozesses versendet Amazon eine Benachrichtigung und verteilt die Anwendung.
