---
title: watchos-Projekt Verweise in xamarin
description: In diesem Dokument wird die Beziehung zwischen einer IOS-APP, einer Watch-APP und einer Watch-App-Erweiterung beschrieben. Er erläutert Projekt Verweise und Bündel Bezeichner.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: 72a1fe3541eb4ece67dd137ebcd33f8e81ae9553
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292266"
---
# <a name="watchos-project-references-in-xamarin"></a>watchos-Projekt Verweise in xamarin

_Erläuterung der Beziehung zwischen der IOS-APP, der Watch-APP und der Watch-Erweiterung._

Die drei Projekte in einer watchos-Lösung werden *automatisch so konfiguriert* , dass Sie auf eine bestimmte Weise darauf verweisen, dass watchos 3-apps ordnungsgemäß erstellt und gebündelt werden. Diese Projekt Verweise und Bündel Bezeichner-Einstellungen werden im folgenden als Referenz beschrieben.

## <a name="project-references"></a>Projektverweise

Zeigen Sie die Verweise durch Doppelklicken auf die Knoten Verweise für jedes Projekt an:

- **App-Referenz für** **iPhone-App**

  ![](project-references-images/catalog-reference1.png "App-Referenz für iPhone-App")

- **App-Verweise ansehen** APP- **Erweiterung** ansehen

  ![](project-references-images/catalog-reference2.png "App-Referenz für iPhone-App")


- Die **Watch-App-Erweiterung** verweist nicht auf eines der anderen Projekte.

  ![](project-references-images/catalog-reference3.png "Watch-App-Erweiterung verweist nicht auf andere Projekte")



## <a name="bundle-identifiers"></a>Bündel Bezeichner

Außerdem müssen Sie sicherstellen, dass die **Bündel** Bezeichner korrekt sind.
Alle drei Projekte sollten das *gleiche* bezeichnerpräfix haben, wobei die beiden überwachungsprojekte über vordefinierte `watchkitapp`Erweiterungen von `watchkitextension` und verfügen, wie im folgenden dargestellt (für das **watchkitcatalog** -Beispiel):

- Einheitliches xamarin. IOS-Projekt:`com.xamarin.WatchKitCatalog`

- Watchkit-Erweiterungsprojekt-`com.xamarin.WatchKitCatalog.watchkitextension`

- App-Projekt überwachen:`com.xamarin.WatchKitCatalog.watchkitapp`

Stellen Sie außerdem sicher, dass diese Einstellungen für " **Info. plist** " korrekt sind:

- Das Watch-APP- `WKCompanionAppBundleIdentifier` Projekt stimmt mit der Bündel-ID der übergeordneten/Container-App überein (d.h. der APP, die auf dem iPhone ausgeführt wird).

- Die **wkapp-Bundle-ID** des Watch Kit-Erweiterungsprojekts stimmt mit der Bündel-ID des Watch-App-Projekts überein.

Sie können die Bezeichner bearbeiten, indem Sie in jedem Projekt auf die **Info. plist** -Datei doppelklicken.

Dieser Screenshot zeigt die Info. plist-Datei der **Watch-Erweiterung** , in der auch der **Bezeichner der Watch-App** angezeigt wird:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "Dieser Screenshot ist die Datei \"Info. plist\" der Watch-Erweiterung.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "Dieser Screenshot ist die Datei \"Info. plist\" der Watch-Erweiterung.")

-----

Dieser Screenshot ist die Datei "Info. plist" der **Watch-App** .
Die aktuelle **Überwachungs-Betriebssystem** Version ist 8,2, daher sollte das **Bereitstellungs Ziel** für die Überwachungs-APP **8,2**lauten. Beachten Sie Folgendes: Wenn Sie Xcode 6,3 installiert haben, ist dieser Wert möglicherweise auf 8,3 festgelegt. ändern Sie den Wert 8,2.

![](project-references-images/infoplist-watchapp.png "Die Datei \"Watch Info. plist\"")

Das Bereitstellungs Ziel für die Watch-App kann sich von der Watch-Erweiterung und der IOS-App unterscheiden.

