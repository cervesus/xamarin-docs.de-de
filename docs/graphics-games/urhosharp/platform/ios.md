---
title: UrhoSharp IOS- und tvos. außerdem wurden Unterstützung
description: iOS und tvos. außerdem wurden bestimmte Setup- und Funktionen für UrhoSharp.
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 57dbd1e9f65361c1bcc8fe3959cded72922c3496
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp IOS- und tvos. außerdem wurden Unterstützung

_iOS und tvos. außerdem wurden bestimmte Setup- und Funktionen_

Während Urho eine portable Klassenbibliothek ist, und ermöglicht die gleiche API über die verschiedenen Plattform verwendet werden, für die Spiellogik Sie weiterhin Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Funktionen der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Subclass von der `Application` Klasse.

## <a name="ios-and-tvos"></a>iOS und tvos. außerdem wurden

**Unterstützte Architekturen:** armv7, arm64, i386

## <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein iOS-Projekt, und klicken Sie dann Ressourcenverzeichnis Daten hinzufügen und stellen Sie sicher, dass alle Dateien **BundleResource** als die **Buildvorgang**.

![Projekt Setup](ios-images/image-4.png "Daten hinzufügen, um das Ressourcenverzeichnis")

## <a name="configuring-and-launching-urho"></a>Konfigurieren und starten Urho

Hinzufügen von using-Anweisungen für die `Urho` und `Urho.iOS` Namespaces, und fügen Sie diesen Code für das Initialisieren von Urho sowie das Starten der Anwendungsstatus hinzu:

```csharp
new MyGame().Run();
```

Beachten Sie, dass seit iOS erwartet `FinishedLaunching` um abgeschlossen haben, sollten Sie den Aufruf von Warteschlange `Run()` führen Sie nach Abschluss der Methode, die dies ist eine allgemeine Vorgehensweise:

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

Es ist wichtig, dass Sie PNG Optimierungen deaktiviert werden, da der standardmäßige iOS PNG-Optimierer Images generiert, die Urho ohne derzeit ordnungsgemäß verarbeiten kann

## <a name="custom-embedding-of-urho"></a>Benutzerdefinierte Einbetten von Urho

Sie können auch mit dem Fehlen eines Urho werden über den Bildschirm für die gesamte Anwendung, und um es als eine Komponente der Anwendung verwenden, erstellen Sie eine `UrhoSurface` also eine `UIView` , die Sie in einer vorhandenen Anwendung einbetten können.

Was benötigen würde sieht folgendermaßen aus:

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

Dies also die Klasse Urho gehostet wird, dann wäre, wenn Sie:

```csharp
new MyGame().Run ();
```

