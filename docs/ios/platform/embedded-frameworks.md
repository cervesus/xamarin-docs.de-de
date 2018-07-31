---
title: Eingebettete Frameworks in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Code für eingebettete Frameworks in einer Xamarin.iOS-Anwendung freigeben. Dies kann mit dem Mtouch-Tools oder native Verweise erfolgen.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: cce5356fd1d3d9a5cf16370a4843c3541b00a7c0
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351433"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Eingebettete Frameworks in Xamarin.iOS

_Dieses Dokument beschreibt, wie Entwickler von Anwendungen die Benutzer-Frameworks in ihren apps einbetten können._

Mit iOS wurde 8.0 Apple es ermöglicht, ein eingebettetes Framework zum Teilen von Code zwischen app-Erweiterungen und die Haupt-app in Xcode zu erstellen.

Xamarin.iOS 9.0 fügt Unterstützung für die Nutzung dieser eingebettete Frameworks (erstellt mit Xcode) in Xamarin.iOS-apps hinzu. *Es wird **nicht** möglich sein, erstellen eingebettete Frameworks von jeder Art von Xamarin.iOS-Projekte, die nur vorhandene native (Objective-C)-Frameworks nutzen.*

Es gibt zwei Möglichkeiten, um die Frameworks in Xamarin.iOS nutzen:

- Übergeben Sie das Framework für das Mtouch-Tool, durch das Hinzufügen der folgenden auf die weitere Mtouch-Argumente des Projekts **iOS-Build** Optionen:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Dies hat für jede Projektkonfiguration festgelegt werden.

- Native Verweise aus dem Kontextmenü hinzufügen

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Mit der rechten Maustaste auf Projekt "und" Durchsuchen ", um Native Verweise hinzufügen

![](embedded-frameworks-images/xam-native-refs.png "Auswählen von systemeigenen Hinzufügen von Verweisen in Visual Studio für Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Mit der rechten Maustaste auf Projekt "und" Durchsuchen ", um Native Verweise hinzufügen

![](embedded-frameworks-images/vs-native-refs.png "Wählen Sie in Visual Studio native Verweise hinzufügen")

-----

  Dies funktioniert für alle Konfigurationen.

In zukünftigen Versionen von Visual Studio für Mac und die Xamarin-Tools für Visual Studio werden Frameworks, die von der IDE (ohne das Bearbeiten von Projektdateien manuell) genutzt.

Einige Beispielprojekte finden Sie unter [Github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Einschränkungen

- Eingebettete Frameworks werden nur in unterstützt [Unified](~/cross-platform/macios/unified/index.md) Projekte.
- Eingebettete Frameworks werden nur unterstützt, in-Projekten mit einem Bereitstellungsziel von mindestens iOS 8.0 oder höher.
- Wenn eine Erweiterung ein eingebetteten Frameworks erforderlich ist, klicken Sie dann benötigen die Container-app auch einen Verweis auf das Framework enthalten ist, andernfalls, die das Framework nicht in das app Bundle eingeschlossen wird.

## <a name="the-mono-runtime"></a>Die Mono-Laufzeit

Intern nutzt Xamarin.iOS dieses Feature mit dem Mono-Laufzeit als Framework, anstatt die Mono-Laufzeit statisch in jede Erweiterung und der Container-app verknüpfen.

Dies erfolgt automatisch, wenn die Container-app eine einheitliche-app ist, er Erweiterungen enthält und die zielbereitstellung, iOS 8.0 oder höher ist.

Apps ohne Erweiterungen werden weiterhin mit dem Mono-Laufzeit statisch verknüpfen, da es Größe ist für die Verwendung von ein Framework, es ist nur eine app, die auf Sie verwiesen wird.

Dieses Verhalten kann vom app-Entwickler, überschrieben werden, indem Sie Folgendes als eine weitere Mtouch-Argument in iOS-Buildoptionen des Projekts hinzufügen:

- `--mono:static`: Mit dem Mono-Laufzeit verknüpft statisch.
- `--mono:framework`: Links mit dem Mono-Laufzeit als Framework.

Ein Szenario für die Verknüpfung mit der Mono-Laufzeit als auch für apps ohne Erweiterungen Framework ist, verringern die Größe der ausführbaren Datei, um alle größeneinschränkungen zu meistern, die von die Apple für die ausführbare Datei erzwingt. Zu Referenzzwecken fügt die Mono-Laufzeit ungefähr 1,7 MB pro Architektur (da der Xamarin.iOS-8.12, jedoch seinen zwischen Versionen und auch zwischen apps unterschiedlich ist). Das Mono-Framework fügt ungefähr 2.3MB pro Architektur, die bedeutet, für eine einzelne-Architecture-app ohne Erweiterungen dass, sodass den app-Link ein Framework wird die ausführbare Datei von ~1.7MB verkleinern, aber ein ~2.3MB Framework hinzufügen, was Sie mit der Mono-Laufzeit in einer größeren app-zusammen für ~0.6MB.

