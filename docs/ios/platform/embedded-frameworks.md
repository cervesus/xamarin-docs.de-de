---
title: Eingebettete Frameworks in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie Code mit eingebetteten Frameworks in einer xamarin. IOS-Anwendung freigeben. Dies kann entweder mit dem mtouchscreen-Tool oder mit nativen verweisen erfolgen.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 690aaf81ee2600bd792a36f14b81df3d15e2d21b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930272"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Eingebettete Frameworks in xamarin. IOS

_In diesem Dokument wird beschrieben, wie Anwendungsentwickler Benutzer-Frameworks in Ihre apps einbetten können._

Mit IOS 8,0 hat Apple das Erstellen eines eingebetteten Frameworks für die gemeinsame Nutzung von Code zwischen App-Erweiterungen und der Haupt-app in Xcode ermöglicht.

Xamarin. IOS 9,0 bietet Unterstützung für die Nutzung dieser eingebetteten Frameworks (erstellt mit Xcode) in xamarin. IOS-apps. *Es ist **nicht** möglich, eingebettete Frameworks aus beliebigen Typen von xamarin. IOS-Projekten zu erstellen, sondern nur vorhandene Native (Ziel-C)-Frameworks zu nutzen.*

Es gibt zwei Möglichkeiten, Frameworks in xamarin. IOS zu nutzen:

- Übergeben Sie das Framework an das mtouchscreen-Tool, indem Sie Folgendes zu den zusätzlichen mberührungs-Argumenten in den **IOS** -Buildoptionen des Projekts hinzufügen:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Dies muss für jede Projekt Konfiguration festgelegt werden.

- Systemeigene Verweise aus dem Kontextmenü hinzufügen

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Klicken Sie mit der rechten Maustaste auf Projekt, und navigieren Sie zu systemeigenen verweisen

![Wählen Sie systemeigene Verweise hinzufügen in Visual Studio für Mac](embedded-frameworks-images/xam-native-refs.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Klicken Sie mit der rechten Maustaste auf Projekt, und navigieren Sie zu systemeigenen verweisen

![Wählen Sie systemeigene Verweise hinzufügen in Visual Studio aus.](embedded-frameworks-images/vs-native-refs.png)

-----

  Dies funktioniert für alle Konfigurationen.

In zukünftigen Versionen von Visual Studio für Mac und den xamarin-Tools für Visual Studio ist es möglich, Frameworks innerhalb der IDE zu nutzen (ohne manuelle Bearbeitung von Projektdateien).

Einige Beispiel Projekte finden Sie auf [GitHub](https://github.com/rolfbjarne/embedded-frameworks) .

## <a name="limitations"></a>Einschränkungen

- Eingebettete Frameworks werden nur in [vereinheitlichten](~/cross-platform/macios/unified/index.md) Projekten unterstützt.
- Eingebettete Frameworks werden nur in Projekten mit einem Bereitstellungs Ziel von mindestens IOS 8,0 unterstützt.
- Wenn eine Erweiterung ein eingebettetes Framework erfordert, muss die Container-APP auch einen Verweis auf das Framework aufweisen, andernfalls ist das Framework nicht in der APP Bundle enthalten.

## <a name="the-mono-runtime"></a>Die Mono-Laufzeit

Intern nutzt xamarin. IOS diese Funktion, um eine Verknüpfung mit der Mono-Laufzeit als Framework zu erhalten, anstatt die Mono-Laufzeit statisch in jede Erweiterung und die Container-APP zu verknüpfen.

Dies erfolgt automatisch, wenn die Container-App eine einheitliche APP ist, Sie Erweiterungen enthält und die Ziel Bereitstellung IOS 8,0 oder höher ist.

Apps ohne Erweiterungen werden weiterhin statisch mit der Mono-Laufzeit verknüpft, da es eine Größen Einbuße für die Verwendung eines Frameworks gibt, wenn nur eine APP auf diese verweist.

Dieses Verhalten kann vom App-Entwickler überschrieben werden, indem Folgendes als zusätzliches mberührungs-Argument in den IOS-Buildoptionen des Projekts hinzugefügt wird:

- `--mono:static`: Verknüpft eine statische Verbindung mit der Mono-Laufzeit.
- `--mono:framework`: Verknüpft mit der Mono-Laufzeit als Framework.

Ein Szenario für die Verknüpfung mit der Mono-Laufzeit als Framework auch für apps ohne Erweiterungen besteht darin, die Größe der ausführbaren Datei zu verringern, um alle Größen Einschränkungen zu überwinden, die Apple für die ausführbare Datei erzwingt Zur Referenz addiert die Mono-Laufzeit ungefähr 1,7 MB pro Architektur (ab xamarin. IOS 8,12, aber die variiert zwischen Releases und sogar zwischen Apps). Das Mono-Framework addiert ungefähr 2,3 MB pro Architektur, was bedeutet, dass für eine Einzel Architektur-App ohne Erweiterungen die APP-Verknüpfung mit der Mono-Laufzeit als Framework verringert wird, indem Sie die ausführbare Datei um ~ 1.7 MB verkleinert, aber ein ~ 2.3 MB-Framework hinzugefügt wird, was zu einer größeren 64-MB-App führt.
