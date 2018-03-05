---
title: Eingebettete frameworks
description: "Dieses Dokument beschreibt, wie Anwendungsentwickler Benutzer Frameworks in ihren apps einbetten können."
ms.topic: article
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62ddf665431a14ce7f4fe8db52cc6ee7a2c4635a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="embedded-frameworks"></a>Eingebettete frameworks

_Dieses Dokument beschreibt, wie Anwendungsentwickler Benutzer Frameworks in ihren apps einbetten können._

Mit iOS 8.0 Apple macht es, möglich, ein eingebettetes Framework zum Freigeben von Code für app-Erweiterungen und die Haupt-app in Xcode zu erstellen.

Xamarin.iOS 9.0 fügt Unterstützung für die Nutzung dieser eingebetteten Frameworks (erstellt mit Xcode) in Xamarin.iOS-apps. *Es wird **nicht** möglich sein, die eingebettete Frameworks aus jeder Art von Xamarin.iOS Projekte erstellen, die lediglich vorhandenen systemeigenen (Objective-C)-Frameworks zu nutzen.*

Es gibt zwei Möglichkeiten, Frameworks in Xamarin.iOS nutzen:

- Übergeben Sie das Framework für das Tool Mtouch durch Hinzufügen der folgenden Elemente auf die zusätzlichen Mtouch Argumente des Projekts **iOS-Build** Optionen:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Dies muss für jede Projektkonfiguration festgelegt werden.

- Fügen Sie im Kontextmenü der systemeigene Verweise hinzu

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Mit der rechten Maustaste auf Projekt "und" Durchsuchen ", um systemeigene Verweise hinzufügen

![](embedded-frameworks-images/xam-native-refs.png "Wählen Sie systemeigene Verweise hinzufügen in Visual Studio für Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Mit der rechten Maustaste auf Projekt "und" Durchsuchen ", um systemeigene Verweise hinzufügen

![](embedded-frameworks-images/vs-native-refs.png "Wählen Sie in Visual Studio systemeigene Verweise hinzufügen")

-----

  Dies funktioniert für alle Konfigurationen.

In zukünftigen Versionen von Visual Studio für Mac und die Xamarin-Tools für Visual Studio werden Frameworks aus, in der IDE (ohne die manuelle Bearbeitung der Projektdateien) genutzt.

Einige Beispielprojekte finden Sie in der [Github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Einschränkungen

- Eingebettete Frameworks sind nur in unterstützt [Unified](~/cross-platform/macios/unified/index.md) Projekte.
- Eingebettete Frameworks werden nur unterstützt, in-Projekten mit ein Bereitstellungsziel aus der mindestens iOS 8.0 oder höher.
- Wenn eine Erweiterung einer eingebetteten Framework erfordert, klicken Sie dann benötigen die Container-app auch einen Verweis auf das Framework enthalten ist, andernfalls, die das Framework nicht in der app-Bundles eingeschlossen wird.

## <a name="the-mono-runtime"></a>Mono-/ runtime

Intern nutzt Xamarin.iOS dieses Feature, mit der Mono / Runtime als Framework, anstatt eine Verknüpfung Mono / Runtime statisch in jede Erweiterung und die Container-app verknüpfen.

Dies erfolgt automatisch, wenn der Container-app eine einheitliche-app ist, sie Erweiterungen enthält und der zielbereitstellung iOS 8.0 oder höher.

Apps ohne Erweiterungen werden weiterhin mit der Mono / Runtime statisch, verknüpfen, weil es eine Größe Strafe wird für die Verwendung von ein Framework, wenn es ist nur eine app, die darauf verweisen.

Dieses Verhalten kann durch den app-Entwickler durch das Hinzufügen der folgenden als zusätzliche Mtouch Argument in das Projekt iOS Buildoptionen überschrieben werden:

- `--mono:static`: Statisch mit der Mono-Laufzeit verknüpft.
- `--mono:framework`: Mit der Mono / Runtime als Framework verknüpft.

Ein Szenario für die Verknüpfung mit der Mono / Runtime als auch für apps, ohne dass Erweiterungen Framework werden verringern die Größe ausführbaren um Größe Einschränkungen zu umgehen, die auf die ausführbare Datei Apple erzwingt. Zu Referenzzwecken fügt Mono / Runtime ungefähr 1.7MB pro Architektur (wie von Xamarin.iOS 8.12, jedoch seine zwischen Versionen und sogar zwischen apps variiert). Das Mono-Framework fügt ungefähr 2.3MB pro Architektur, dies bedeutet, für eine einzelnes-Architektur-app ohne Erweiterungen dass, machen die app-Verknüpfung mit der Mono / Runtime wie ein Framework wird die ausführbare Datei von ~1.7MB verkleinern, aber ein ~2.3MB Framework hinzufügen, wodurch in eine größere app-zusammen für ~0.6MB.

