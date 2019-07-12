---
title: Von UrhoSharp Mac – Unterstützung
description: Dieses Dokument erläutert die Unterstützung von UrhoSharp MacOS. Es wird beschrieben, wie ein Projekt erstellt und enthält einen Link zum Beispielcode.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: ee0a03d168b6e628893b18a27d73b46d3fa2fbc2
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832661"
---
# <a name="urhosharp-mac-support"></a>Von UrhoSharp Mac – Unterstützung

_Bestimmte Mac-Setup und Features_

Obwohl Urho eine portable Klassenbibliothek ist und ermöglicht die gleiche API für die verschiedenen Plattformen verwendet werden, für die Spiele-Logik, Sie dennoch Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Features der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Unterklasse von der `Application` Klasse.

## <a name="macos"></a>macOS

**Unterstützte Architekturen:** X86/x86-64 für 32-Bit und 64-Bit.

## <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein Konsolenprojekt, verweisen Sie Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Beispiel

[Vollständiges Beispiel](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)
