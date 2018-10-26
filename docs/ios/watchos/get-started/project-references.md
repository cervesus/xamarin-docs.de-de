---
title: WatchOS-Projekt-Verweise in Xamarin
description: Dieses Dokument beschreibt die Beziehung zwischen einer iOS-app, eine Watch-app und eine Watch-app-Erweiterung. Es wird erläutert, Projekt-Verweise und Paket Bezeichner.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: c900ab714fed2bb1e02367ba39ad3c5a0a76121e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106662"
---
# <a name="watchos-project-references-in-xamarin"></a>WatchOS-Projekt-Verweise in Xamarin

_Erläuterung der Beziehung zwischen der iOS-app, eine Watch-app und eine Watch-Erweiterung._

Die drei Projekte in einer WatchOS-Lösung sind *automatisch konfigurierte* , aufeinander verweisen, auf eine bestimmte Weise für WatchOS 3-apps erstellt und ordnungsgemäß gebündelt werden. Diese Projektverweise und Bundle-Bezeichner-Einstellungen werden zur Referenz nachfolgend beschrieben.

## <a name="project-references"></a>Projektverweise

Anzeigen der Verweise durch Doppelklicken auf den Knoten "Verweise" für jedes Projekt:

- **iPhone-app** Verweise **Watch-App**

![](project-references-images/catalog-reference1.png "iPhone-app verweist auf Watch-App")

- **Sehen Sie sich App** Verweise **Watch-App-Erweiterung**

![](project-references-images/catalog-reference2.png "iPhone-app verweist auf Watch-App")


 - Die **Watch-App-Erweiterung** verweist nicht auf eine der anderen Projekte

![](project-references-images/catalog-reference3.png "Watch-App-Erweiterung verweist nicht auf die anderen Projekte")



## <a name="bundle-identifiers"></a>Bundle-IDs

Sie müssen auch sicherstellen, Ihre **Bundle-IDs** richtig sind.
Alle drei Projekte müssen den *gleichen* ID-Präfix, mit den zwei Watch-Projekten mit Erweiterungen der vorab `watchkitextension` und `watchkitapp`wie folgt (für die **WatchKitCatalog** (Beispiel):

 - Vereinheitlichten Xamarin.iOS-Projekt- `com.xamarin.WatchKitCatalog`

 - WatchKit-Erweiterung-Projekt – `com.xamarin.WatchKitCatalog.watchkitextension`

 - Watch-App-Projekt: `com.xamarin.WatchKitCatalog.watchkitapp`

Stellen Sie außerdem sicher, dass diese **"Info.plist"** Einstellungen korrekt sind:

 - Die Watch-App-Projekts `WKCompanionAppBundleIdentifier` entspricht der über-/Container-app Bündel-ID (ie. derjenige, der auf dem iPhone ausgeführt wird);

 - Des Watch-Erweiterung, Kit-Projekts **WKApp-Bundle-ID** mit Paket-ID des Watch-App-Projekts übereinstimmt.

Sie können die IDs bearbeiten, indem Sie durch Doppelklicken auf die **"Info.plist"** Datei in jedem Projekt.

Dieser Screenshot zeigt die **Watch-Erweiterung** Info.plist-Datei mit den **Watch-App** auch Bezeichner:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
![](project-references-images/infoplist-extension.png "In diesem Screenshot ist der Watch-Erweiterung \"Info.plist\"-Datei")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
![](project-references-images/infoplist-extension-vs.png "In diesem Screenshot ist der Watch-Erweiterung \"Info.plist\"-Datei")

-----

Dieser Screenshot zeigt die **Watch-App** Datei "Info.plist".
Die aktuelle **Watch-OS** ist Version 8.2, sodass der **Bereitstellungsziel** für die Watch-App sollte **8.2**. Beachten Sie, dass wenn Sie Xcode 6.3 installiert haben, dieser Wert werden, bis 8.3 festgelegt kann-sie ändern sollten, 8.2.

![](project-references-images/infoplist-watchapp.png "Der Apple Watch-Datei \"Info.plist\"")

Das Bereitstellungsziel für die Watch-App möglich unterscheidet sich von der Watch-Erweiterung und der iOS-App.

