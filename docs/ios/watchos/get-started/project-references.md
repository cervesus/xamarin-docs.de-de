---
title: watchos-Projekt Verweise in xamarin
description: In diesem Dokument wird die Beziehung zwischen einer IOS-APP, einer Watch-APP und einer Watch-App-Erweiterung beschrieben. Er erläutert Projekt Verweise und Bündel Bezeichner.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 3dcd5f17b35b9829831adcf997d8bde97c0572e7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030160"
---
# <a name="watchos-project-references-in-xamarin"></a>watchos-Projekt Verweise in xamarin

_Erläuterung der Beziehung zwischen der IOS-APP, der Watch-APP und der Watch-Erweiterung._

Die drei Projekte in einer watchos-Lösung werden *automatisch so konfiguriert* , dass Sie auf eine bestimmte Weise darauf verweisen, dass watchos 3-apps ordnungsgemäß erstellt und gebündelt werden. Diese Projekt Verweise und Bündel Bezeichner-Einstellungen werden im folgenden als Referenz beschrieben.

## <a name="project-references"></a>Projektverweise

Zeigen Sie die Verweise durch Doppelklicken auf die Knoten Verweise für jedes Projekt an:

- **App-Referenz für** **iPhone-App**

  ![](project-references-images/catalog-reference1.png "iPhone app references Watch App")

- **App-Verweise ansehen** APP- **Erweiterung** ansehen

  ![](project-references-images/catalog-reference2.png "iPhone app references Watch App")

- Die **Watch-App-Erweiterung** verweist nicht auf eines der anderen Projekte.

  ![](project-references-images/catalog-reference3.png "Watch App Extension does not reference the other projects")

## <a name="bundle-identifiers"></a>Bündel Bezeichner

Außerdem müssen Sie sicherstellen, dass die **Bündel** Bezeichner korrekt sind.
Alle drei Projekte sollten das *gleiche* bezeichnerpräfix haben, wobei die beiden überwachungsprojekte über vordefinierte Erweiterungen von `watchkitextension` und `watchkitapp`verfügen (für das **watchkitcatalog** -Beispiel):

- Einheitliches xamarin. IOS-Projekt `com.xamarin.WatchKitCatalog`

- Watchkit-Erweiterungsprojekt-`com.xamarin.WatchKitCatalog.watchkitextension`

- App-Projekt überwachen-`com.xamarin.WatchKitCatalog.watchkitapp`

Stellen Sie außerdem sicher, dass diese Einstellungen für " **Info. plist** " korrekt sind:

- Die `WKCompanionAppBundleIdentifier` des Watch-App-Projekts stimmt mit der Bündel-ID der übergeordneten/Container-App überein (d... die, die auf dem iPhone läuft);

- Die **wkapp-Bundle-ID** des Watch Kit-Erweiterungsprojekts stimmt mit der Bündel-ID des Watch-App-Projekts überein.

Sie können die Bezeichner bearbeiten, indem Sie in jedem Projekt auf die **Info. plist** -Datei doppelklicken.

Dieser Screenshot zeigt die Info. plist-Datei der **Watch-Erweiterung** , in der auch der **Bezeichner der Watch-App** angezeigt wird:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "This screenshot is the Watch Extension's Info.plist file")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "This screenshot is the Watch Extension's Info.plist file")

-----

Dieser Screenshot ist die Datei "Info. plist" der **Watch-App** .
Die aktuelle **Überwachungs-Betriebssystem** Version ist 8,2, daher sollte das **Bereitstellungs Ziel** für die Überwachungs-APP **8,2**lauten. Beachten Sie Folgendes: Wenn Sie Xcode 6,3 installiert haben, ist dieser Wert möglicherweise auf 8,3 festgelegt. ändern Sie den Wert 8,2.

![](project-references-images/infoplist-watchapp.png "The watch Info.plist file")

Das Bereitstellungs Ziel für die Watch-App kann sich von der Watch-Erweiterung und der IOS-App unterscheiden.
