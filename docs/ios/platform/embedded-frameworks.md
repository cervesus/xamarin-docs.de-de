---
title: Eingebettete Frameworks in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie Code mit eingebetteten Frameworks in einer xamarin. IOS-Anwendung freigeben. Dies kann entweder mit dem mtouchscreen-Tool oder mit nativen verweisen erfolgen.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 6287dca8660c1147455beb22304b7f8637ac7fa5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292776"
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

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Klicken Sie mit der rechten Maustaste auf Projekt, und navigieren Sie zu systemeigenen verweisen

![](embedded-frameworks-images/xam-native-refs.png "Wählen Sie systemeigene Verweise hinzufügen in Visual Studio für Mac")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Klicken Sie mit der rechten Maustaste auf Projekt, und navigieren Sie zu systemeigenen verweisen

![](embedded-frameworks-images/vs-native-refs.png "Wählen Sie systemeigene Verweise hinzufügen in Visual Studio aus.")

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

- `--mono:static`: Links mit der Mono-Laufzeit statisch.
- `--mono:framework`: Verknüpft mit der Mono-Laufzeit als Framework.

Ein Szenario für die Verknüpfung mit der Mono-Laufzeit als Framework auch für apps ohne Erweiterungen besteht darin, die Größe der ausführbaren Datei zu verringern, um alle Größen Einschränkungen zu überwinden, die Apple für die ausführbare Datei erzwingt Zur Referenz addiert die Mono-Laufzeit ungefähr 1,7 MB pro Architektur (ab xamarin. IOS 8,12, aber die variiert zwischen Releases und sogar zwischen Apps). Das Mono-Framework addiert ungefähr 2,3 MB pro Architektur. Dies bedeutet, dass für eine einzelne Architektur-App ohne Erweiterungen die APP-Verknüpfung mit der Mono-Laufzeit als Framework verringert und die ausführbare Datei um ~ 1.7 MB verkleinert wird. in einer größeren App mit einer Größe von ca. 0,6 MB.

