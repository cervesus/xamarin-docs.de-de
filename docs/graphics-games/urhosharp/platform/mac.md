---
title: Von UrhoSharp Mac – Unterstützung
description: Dieses Dokument erläutert die Unterstützung von UrhoSharp MacOS. Es wird beschrieben, wie ein Projekt erstellt und enthält einen Link zum Beispielcode.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6d0a048020284319682c1bee0f9a1d7f9af00977
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386363"
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


