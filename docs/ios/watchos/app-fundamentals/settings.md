---
title: Arbeiten mit WatchOS Einstellungen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Einstellungen für WatchOS in Xamarin. Es wird erläutert, Hinzufügen von Einstellungen zu einer Projektmappe Watch-app verwenden diese Einstellungen in der app, und die Apple Watch-app auf dem iPhone.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6cfbcf3b4383588819490838c2a54cdb4faf9403
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790876"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Arbeiten mit WatchOS Einstellungen in Xamarin

Apple Watch-apps können die gleiche Einstellungen Funktionalität als iOS-apps – die Einstellungen-Benutzeroberfläche wird angezeigt, der **Apple Watch** iPhone-app aber die Werte in der iPhone-app und auch der Watch-Erweiterung verfügbar sind.

![](settings-images/intro.png "Apple Watch-apps können die gleiche Einstellungen Funktionalität als iOS-apps verwenden.")

Die Einstellungen werden in einen freigegebenen Dateispeicherort, die den iOS-app und Watch-app-Erweiterung durch definierten zugegriffen werden gespeichert ein **App-Gruppe**. Sie sollten [konfigurieren einen App-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) vor dem Hinzufügen der Einstellungen mithilfe der folgenden Anweisungen.

## <a name="add-settings-in-a-watch-solution"></a>Fügen Sie die Einstellung in einer Projektmappe Überwachungsfenster hinzu

In der **iPhone-app** in der Projektmappe (*nicht* der Watch-app oder die Erweiterung):

1. Mit der rechten Maustaste **hinzufügen > neuen Datei...**  , und wählen Sie **Settings.bundle** (Sie können nicht bearbeiten Sie den Namen in der **neue Datei** Dialogfeld):

   [![](settings-images/settings-add-sml.png "Hinzufügen einer neuen Settings Bundle")](settings-images/settings-add.png#lightbox)

2. Ändern Sie den Namen in **Einstellungen Watch.bundle** (Wählen Sie aus, und geben Sie **Befehl + R** umbenannt):

   ![](settings-images/settings-rename.png "Benennen Sie das Paket")

3. Fügen Sie einen neuen Schlüssel `ApplicationGroupContainerIdentifier` auf die **Root.plist** mit dem der Wert der app-Gruppe, die Sie, (z. b konfiguriert haben. `group.com.xamarin.WatchSettings` Im Beispiel):

   [ ![](settings-images/settings-appgroup-sml.png "Fügen Sie einen ApplicationGroupContainerIdentifier-Schlüssel, um die Root.plist")](settings-images/settings-appgroup.png#lightbox)

4. Bearbeiten der **Settings-Watch.bundle/Root.plist** Optionen enthält, Sie verwenden möchten – die Vorlagedatei enthält eine Gruppe.
  Textfeld, ein/aus-Schalter und Schieberegler in der Standardeinstellung (die Sie löschen und Ersetzen Sie durch Ihren eigenen Einstellungen können):

  [![](settings-images/rootplist-sml.png "Bearbeiten Sie die Settings-Watch.bundle/Root.plist")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>Verwenden Sie die Einstellungen in der Watch-App

Erstellen Sie die vom Benutzer ausgewählten Werte für den Zugriff auf eine `NSUserDefaults` Instanz unter Verwendung der app-Gruppe und Angeben von `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch-App

[![](settings-images/settings-app-sml.png "Die neue Apple Watch-app auf dem iPhone")](settings-images/settings-app.png#lightbox)

Benutzer interagieren mit den Einstellungen der neuen **Apple Watch** -app auf ihre iPhone. Diese app kann der Benutzer apps auf die Überwachungsfenster, und auch bearbeiten, die mit die Einstellungen verfügbar gemacht werden ein-/ausblenden der **Einstellungen Watch.bundle**.

![](settings-images/applewatch-1.png "Beispiel für app-Einstellungen") ![](settings-images/applewatch-2.png "Beispiel-app-Einstellungen")



## <a name="related-links"></a>Verwandte Links

- [Apple Einstellungen doc](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
