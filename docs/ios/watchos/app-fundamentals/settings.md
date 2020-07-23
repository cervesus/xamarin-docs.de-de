---
title: Arbeiten mit watchos-Einstellungen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit den watchos-Einstellungen in xamarin arbeiten. Er erläutert das Hinzufügen von Einstellungen zu einer Watch-App-Lösung mithilfe dieser Einstellungen in der APP und der Apple Watch-App auf dem iPhone.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c5ad40c375320ed21acb3f0a75c5c66f2efc7824
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938642"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Arbeiten mit watchos-Einstellungen in xamarin

Apple Watch-Apps die gleichen Einstellungen wie IOS-Apps verwenden können, wird die Benutzeroberfläche "Einstellungen" in der **Apple Watch** iPhone-App angezeigt, aber die Werte sind sowohl in der iPhone-App als auch in der Watch-Erweiterung verfügbar.

![Apple Watch-Apps können die gleichen Einstellungs Funktionen wie IOS-Apps verwenden.](settings-images/intro.png)

Die Einstellungen werden an einem freigegebenen Speicherort gespeichert, auf den sowohl die IOS-App als auch die APP-Erweiterung "Watch", die durch eine **App-Gruppe**definiert ist, zugreifen können. Sie sollten [eine APP-Gruppe konfigurieren](~/ios/watchos/app-fundamentals/app-groups.md) , bevor Sie die Einstellungen mithilfe der folgenden Anweisungen hinzufügen.

## <a name="add-settings-in-a-watch-solution"></a>Hinzufügen von Einstellungen in einer Überwachungslösung

In der **iPhone-App** in ihrer Projekt Mappe (*nicht* in der Watch-APP oder-Erweiterung):

1. Klicken Sie mit der rechten Maustaste auf **> neue Datei hinzufügen...** , und wählen Sie **Settings. Bundle** (Sie können den Namen im Dialogfeld " **neue Datei** " nicht bearbeiten):

   [![Hinzufügen eines neuen Einstellungs Pakets](settings-images/settings-add-sml.png)](settings-images/settings-add.png#lightbox)

2. Ändern Sie den Namen in **Settings-Watch. Bundle** (Wählen Sie aus, und geben Sie **Command + R** zum Umbenennen ein):

   ![Umbenennen des Pakets](settings-images/settings-rename.png)

3. Fügen Sie der Datei " `ApplicationGroupContainerIdentifier` **root. plist** " einen neuen Schlüssel mit dem Wert hinzu, der auf die konfigurierte App-Gruppe festgelegt ist (z. b. `group.com.xamarin.WatchSettings`im Beispiel):

   [![Fügen Sie der Datei "root. plist" einen applicationgroupcontaineridentifier-Schlüssel hinzu.](settings-images/settings-appgroup-sml.png)](settings-images/settings-appgroup.png#lightbox)

4. Bearbeiten Sie die Datei " **Settings-Watch. Bundle/root. plist** " mit den Optionen, die Sie verwenden möchten. die Vorlagen Datei enthält eine Gruppe.
  TextField, Switch und Schieberegler standardmäßig umschalten (diese können gelöscht und durch ihre eigenen Einstellungen ersetzt werden):

  [![Bearbeiten Sie die Datei "Settings-Watch. Bundle/root. plist".](settings-images/rootplist-sml.png)](settings-images/rootplist.png#lightbox)

## <a name="use-settings-in-the-watch-app"></a>Verwenden von Einstellungen in der Watch-App

Erstellen Sie zum Zugreifen auf die vom Benutzer ausgewählten Werte eine `NSUserDefaults` Instanz mithilfe der APP-Gruppe, und geben Sie Folgendes an `NSUserDefaultsType.SuiteName` :

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch-App

[![Die neue Apple Watch-App auf dem iPhone](settings-images/settings-app-sml.png)](settings-images/settings-app.png#lightbox)

Benutzer interagieren mit den Einstellungen über die neue **Apple Watch** -App auf Ihrem iPhone. Diese APP ermöglicht dem Benutzer das Anzeigen/Ausblenden von apps auf der Überwachung und das Bearbeiten der Einstellungen, die mithilfe der **Settings-Watch. Bundle**verfügbar gemacht werden.

![Beispiel für App-Einstellungen](settings-images/applewatch-1.png) ![Beispiel für App-Einstellungen](settings-images/applewatch-2.png)

## <a name="related-links"></a>Ähnliche Themen

- [Dokument mit den Einstellungen von Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
