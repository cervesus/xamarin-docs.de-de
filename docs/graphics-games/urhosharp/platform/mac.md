---
title: "UrhoSharp Mac-Unterstützung"
description: "Bestimmte Mac-Setup und Funktionen für UrhoSharp."
ms.topic: article
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: fff96a19d5f5286f2c9483407fcaaab6d15ff2b5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-mac-support"></a>UrhoSharp Mac-Unterstützung

_Bestimmte Mac-Setup und Funktionen_

Während Urho eine portable Klassenbibliothek ist, und ermöglicht die gleiche API über die verschiedenen Plattform verwendet werden, für die Spiellogik Sie weiterhin Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Funktionen der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Unterklasse von der `Application` Klasse.

## <a name="macos"></a>macOS

**Unterstützte Architekturen:** X86/x86-64 für 32-Bit und 64-Bit.

## <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen eines Konsolenprojekts, verweisen die Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse, die das Datenverzeichnis enthält) zugreifen können.

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Beispiel

[Vollständiges Beispiel](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


