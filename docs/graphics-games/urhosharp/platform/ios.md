---
title: Von UrhoSharp iOS und TvOS-Unterstützung
description: In diesem Dokument wird erläutert, iOS und TvOS-Unterstützung für von UrhoSharp. Es beschreibt, wie ein Projekt erstellen, konfigurieren und starten Urho und führen Sie eine benutzerdefinierte Einbetten von Urho.
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: f15ae458c6bd613b59700908ad7c121315e377ab
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61302645"
---
# <a name="urhosharp-ios-and-tvos-support"></a>Von UrhoSharp iOS und TvOS-Unterstützung

Obwohl Urho eine portable Klassenbibliothek ist und ermöglicht die gleiche API für die verschiedenen Plattformen verwendet werden, für die Spiele-Logik, Sie dennoch Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Features der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Subclass von der `Application` Klasse.

## <a name="ios-and-tvos"></a>iOS und tvOS

**Unterstützte Architekturen:** armv7, arm64, i386

## <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein iOS-Projekt, und klicken Sie dann Daten in das Verzeichnis "Resources" und stellen sicher, dass alle Dateien **BundleResource** als die **Buildvorgang**.

![Setup Project](ios-images/image-4.png "Hinzufügen von Daten zu dem Verzeichnis \"Resources\"")

## <a name="configuring-and-launching-urho"></a>Konfigurieren und starten Urho

Hinzufügen von using-Anweisungen für die `Urho` und `Urho.iOS` Namespaces, und fügen Sie diesen Code für Urho initialisieren, sowie Ihre Anwendung zu starten:

```csharp
new MyGame().Run();
```

Beachten Sie, dass da iOS erwartet `FinishedLaunching` um abzuschließen, sollten Sie den Aufruf von Warteschlange `Run()` führen Sie nach Abschluss der Methode, die dies ist eine allgemeine Vorgehensweise:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

Es ist wichtig, dass Sie PNG-Optimierungen deaktiviert werden, weil der standardmäßige iOS PNG-Optimierer Images generiert wird, die Urho derzeit ordnungsgemäß nicht nutzen können

## <a name="custom-embedding-of-urho"></a>Benutzerdefinierte Einbetten von Urho

Sie können auch mit dem Urho verwendet werden, den Bildschirm für die gesamte Anwendung, und um sie als eine Komponente Ihrer Anwendung verwenden, können Sie erstellen eine `UrhoSurface` Dies ist eine `UIView` , die in einer vorhandenen Anwendung eingebettet werden können.

Dies ist die würde was Sie tun müssen:

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

Dies also Ihrer Urho-Klasse gehostet wird, dann würden Sie tun:

```csharp
new MyGame().Run ();
```

