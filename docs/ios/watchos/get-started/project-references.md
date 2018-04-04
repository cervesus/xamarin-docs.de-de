---
title: Projektverweise
description: Erläuterung der Beziehung zwischen der iOS-app, eine Watch-app und eine Watch-Erweiterung.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: f3573e8b578ca567ea9d7360eb132aead4c24f37
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="project-references"></a>Projektverweise

_Erläuterung der Beziehung zwischen der iOS-app, eine Watch-app und eine Watch-Erweiterung._

Sind die drei Projekte in einer Projektmappe WatchOS *automatisch konfigurierte* um aufeinander verweisen, auf eine bestimmte Weise für WatchOS 3 apps erstellt und ordnungsgemäß gebündelt werden. Diese Projektverweise und Paket-ID-Einstellungen werden nachfolgend beschrieben, für den Verweis.

## <a name="project-references"></a>Projektverweise

Zeigen Sie Verweise durch Doppelklicken auf den Knoten Verweise für jedes Projekt an:

- **iPhone-app** Verweise **Watch-App**

![](project-references-images/catalog-reference1.png "iPhone-app verweist Watch-App")

- **Überwachen der App** Verweise **Watch-App-Erweiterung**

![](project-references-images/catalog-reference2.png "iPhone-app verweist Watch-App")


 - Die **Watch-App-Erweiterung** verweist nicht auf eine der anderen Projekte

![](project-references-images/catalog-reference3.png "Sehen Sie sich, dass die App-Erweiterung nicht auf andere Projekte verweist")



## <a name="bundle-identifiers"></a>Paket-IDs

Sie müssen auch sicherstellen, Ihre **Bündel-IDs** richtig sind.
Alle drei Projekte müssen die *gleichen* ID-Präfix mit den Erweiterungen der vordefinierte müssen zwei überwachen-Projekten `watchkitextension` und `watchkitapp`wie folgt (für die **WatchKitCatalog** Beispiel):

 - Xamarin.iOS Unified Projekt- `com.xamarin.WatchKitCatalog`

 - WatchKit Erweiterungsprojekt- `com.xamarin.WatchKitCatalog.watchkitextension`

 - Watch-App-Projekt- `com.xamarin.WatchKitCatalog.watchkitapp`

Stellen Sie außerdem sicher, dass diese **"Info.plist"** Einstellungen korrekt sind:

 - Des Watch-App-Projekts `WKCompanionAppBundleIdentifier` entspricht der übergeordnete Container/app-Paket-ID (ie. das Projekt, das auf dem iPhone ausgeführt wird);

 - Des Kit Watch-Erweiterung, Projekts **WKApp-Paket-ID** entspricht der Watch-App-Projekt-Paket-ID.

Sie können die IDs bearbeiten, durch Doppelklicken auf die **"Info.plist"** Datei in jedem Projekt angelegt.

Diese Abbildung ist die **Watch-Erweiterung** Datei "Info.plist", mit der **Watch-App** Bezeichner als auch:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "Diesem Screenshot ist die Datei von der Watch-Erweiterung "Info.plist"")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "Diesem Screenshot ist die Datei von der Watch-Erweiterung "Info.plist"")

-----

Diese Abbildung ist die **Watch-App** Datei "Info.plist".
Die aktuelle **überwachen OS** Version ist 8.2, damit die **Bereitstellungsziel** für die Watch-App sollte **8.2**. Beachten Sie, dass wenn Sie Xcode 6.3 installiert haben, dieser Wert werden, bis 8.3 festgelegt kann-sie ändern sollten, 8.2.

![](project-references-images/infoplist-watchapp.png "Der Apple Watch-Datei "Info.plist"")

Das Bereitstellungsziel entweder für die Watch-App kann sich von der Watch-Erweiterung und iOS-App unterscheiden.

