---
title: Arbeiten mit WatchOS-Einstellungen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Einstellungen für WatchOS in Xamarin. Es wird erläutert, Hinzufügen von Einstellungen für eine Watch-app-Lösung, verwenden diese Einstellungen in der app, und der Apple Watch-app auf dem iPhone.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: a8fe2c2765676db52c23fd7c475f218f14697caf
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675227"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Arbeiten mit WatchOS-Einstellungen in Xamarin

Apple Watch-apps können die gleiche Einstellungen für Funktionen wie iOS-apps – die Settings-Benutzeroberfläche wird angezeigt, der **Apple Watch** iPhone-app, aber die Werte stehen in der iPhone-app und auch der Watch-Erweiterung.

![](settings-images/intro.png "Apple Watch-apps können die gleiche Funktionalität für die Einstellungen als iOS-apps verwenden.")

Die Einstellungen werden gespeichert werden, in einen freigegebenen Dateispeicherort, die für die iOS-app und der Watch-app-Erweiterung, durch definiert zugänglich ist ein **App-Gruppe**. Sie sollten [konfigurieren einen App-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) vor dem Hinzufügen der Einstellungen mithilfe der folgenden Anweisungen.

## <a name="add-settings-in-a-watch-solution"></a>Hinzufügen von Einstellungen in einer Projektmappe ansehen

In der **iPhone-app** in der Projektmappe (*nicht* der Watch-app oder die Erweiterung):

1. Mit der rechten Maustaste **hinzufügen > neue Datei...**  , und wählen Sie **"Settings.Bundle"** (Sie können nicht den Namen im Bearbeiten der **neue Datei** Dialogfeld):

   [![](settings-images/settings-add-sml.png "Hinzufügen eines neuen Einstellungsbundles")](settings-images/settings-add.png#lightbox)

2. Ändern Sie den Namen in **Einstellungen-Watch.bundle** (Wählen Sie aus, und geben Sie **Befehl + R** umbenannt):

   ![](settings-images/settings-rename.png "Benennen Sie das Paket")

3. Fügen Sie einen neuen Schlüssel `ApplicationGroupContainerIdentifier` auf die **"Root.plist"** mit dem Wert, legen Sie auf die app-Gruppe, die Sie, (z. b konfiguriert haben. `group.com.xamarin.WatchSettings` Im Beispiel):

   [![](settings-images/settings-appgroup-sml.png "Fügen Sie einen ApplicationGroupContainerIdentifier-Schlüssel in der \"Root.plist\"")](settings-images/settings-appgroup.png#lightbox)

4. Bearbeiten der **Settings-Watch.bundle/Root.plist** Optionen enthalten soll: die Vorlagendatei enthält eine Gruppe.
  TextField, Umschalter und Schieberegler in der Standardeinstellung (die Sie löschen und Ersetzen Sie dies durch Ihre eigenen Einstellungen können):

  [![](settings-images/rootplist-sml.png "Bearbeiten Sie die Settings-Watch.bundle/Root.plist")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>Verwenden Sie die Einstellungen in der Watch-App

Um die vom Benutzer ausgewählten Werte zuzugreifen, erstellen eine `NSUserDefaults` -Instanz unter Verwendung der app-Gruppe und Angeben von `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch-App

[![](settings-images/settings-app-sml.png "Die neue Apple Watch-app auf dem iPhone")](settings-images/settings-app.png#lightbox)

Benutzer interagieren mit den Einstellungen über das neue **Apple Watch** -app auf ihren iPhone. Diese app kann der Benutzer zum Ein-/Ausblenden von apps für die Überwachung, und auch bearbeiten, die mit die Einstellungen verfügbar gemacht werden die **Einstellungen-Watch.bundle**.

![](settings-images/applewatch-1.png "Beispiel für app-Einstellungen") ![](settings-images/applewatch-2.png "Beispiel-app-Einstellungen")



## <a name="related-links"></a>Verwandte Links

- [Einstellungen von Apple-Dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
