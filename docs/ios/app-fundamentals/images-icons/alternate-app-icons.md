---
title: Alternative App-Symbole in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie alternative App-Symbole in xamarin. IOS verwenden. Darin wird erläutert, wie Sie diese Symbole einem xamarin. IOS-Projekt hinzufügen, wie Sie die Datei "Info. plist" ändern und das Symbol der APP Programm gesteuert verwalten.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: ed31f1dca3f823ccd0374b4fcbac1bbd9e80e022
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004319"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Alternative App-Symbole in xamarin. IOS

_In diesem Artikel wird die Verwendung alternativer App-Symbole in xamarin. IOS behandelt._

Apple hat eine Reihe von Erweiterungen zu IOS 10,3 hinzugefügt, die es einer APP ermöglichen, Ihr Symbol zu verwalten:

- `ApplicationIconBadgeNumber`: Ruft das Badge des App-Symbols im Springboard ab oder legt es fest.
- `SupportsAlternateIcons`: Wenn `true` der APP ein alternativer Satz von Symbolen.
- `AlternateIconName`: gibt den Namen des alternativen Symbols zurück, das derzeit ausgewählt ist, oder `null`, wenn das primäre Symbol verwendet wird.
- `SetAlternameIconName`: Verwenden Sie diese Methode, um das Symbol der APP auf das angegebene Alternative Symbol zu wechseln.

![](alternate-app-icons-images/icons04.png "A sample alert when an app changes its icon")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Hinzufügen alternativer Symbole zu einem xamarin. IOS-Projekt

Damit eine APP auf ein alternatives Symbol umgestellt werden kann, muss eine Auflistung von Symbolbildern im xamarin. IOS-App-Projekt enthalten sein. Diese Images können dem Projekt nicht mithilfe der typischen `Assets.xcassets`-Methode hinzugefügt werden. Sie müssen direkt dem **Ressourcen** Ordner hinzugefügt werden.

Führen Sie folgende Schritte aus:

1. Wählen Sie die erforderlichen Symbolbilder in einem Ordner aus, wählen Sie alle aus, und ziehen Sie Sie in der **Projektmappen-Explorer**in den Ordner **Ressourcen** :

    ![](alternate-app-icons-images/icons00.png "Select the icons images from a folder")

2. Wenn Sie dazu aufgefordert werden, wählen Sie **Kopieren** **aus, verwenden Sie die gleiche Aktion für alle ausgewählten Dateien** , und klicken Sie auf **OK** :

    ![](alternate-app-icons-images/icons02.png "The Add File to Folder dialog box")

3. Der Ordner **Ressourcen** sollte wie folgt aussehen:

    ![](alternate-app-icons-images/icons01.png "The Resources folder should look like this")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Ändern der Datei "Info. plist"

Nachdem die erforderlichen Images dem **Ressourcen** Ordner hinzugefügt wurden, muss der Schlüssel " [cfbundlealternative Eicons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) " der Datei " **Info. plist** " des Projekts hinzugefügt werden. Mit diesem Schlüssel werden der Name des neuen Symbols und die Bilder definiert, aus denen es besteht.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie zur **Quell** Ansicht.
3. Fügen Sie einen Schlüssel für die **Bündel Symbole** hinzu, und belassen Sie den **Typ** " **Dictionary**".
4. Fügen Sie einen `CFBundleAlternateIcons` Schlüssel hinzu, und legen Sie den **Typ** auf **Dictionary**fest.
5. Fügen Sie einen `AppIcon2` Schlüssel hinzu, und legen Sie den **Typ** auf **Dictionary**fest. Dabei handelt es sich um den Namen des neuen alternativen App-Symbol Satzes.
6. `CFBundleIconFiles` Schlüssel hinzufügen und den **Typ** auf **Array** festlegen
7. Fügen Sie eine neue Zeichenfolge zum `CFBundleIconFiles` Array für jede Symbol Datei hinzu, die die Erweiterung und die `@2x`, `@3x`, usw. Suffixe auslässt (Beispiel `100_icon`). Wiederholen Sie diesen Schritt für jede Datei, aus der der Alternative Symbolsatz besteht.
8. Fügen Sie dem `AppIcon2` Wörterbuch einen `UIPrerenderedIcon` Schlüssel hinzu, legen Sie den **Typ** auf **Boolean** und den Wert auf **Nein**fest.
9. Speichern Sie die Änderungen in der Datei.

Die resultierende Datei " **Info. plist** " sollte wie folgt aussehen:

![](alternate-app-icons-images/icons03.png "The completed Info.plist file")

Oder wie folgt, wenn Sie in einem Text-Editor geöffnet ist:

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>Verwalten des App-Symbols 

Wenn die Symbolbilder im xamarin. IOS-Projekt enthalten sind und die Datei " **Info. plist** " ordnungsgemäß konfiguriert ist, kann der Entwickler eine der vielen neuen Features verwenden, die IOS 10,3 hinzugefügt wurden, um das Symbol der APP zu steuern.

Die `SupportsAlternateIcons`-Eigenschaft der `UIApplication`-Klasse ermöglicht es dem Entwickler zu überprüfen, ob eine APP Alternative Symbole unterstützt. Beispiel:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

Die `ApplicationIconBadgeNumber`-Eigenschaft der `UIApplication`-Klasse ermöglicht es dem Entwickler, die aktuelle Badge-Nummer des App-Symbols im Springboard zu erhalten oder festzulegen. Der Standardwert ist 0 (null). Beispiel:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

Die `AlternateIconName`-Eigenschaft der `UIApplication`-Klasse ermöglicht es dem Entwickler, den Namen des aktuell ausgewählten Alternativen App-Symbols zu erhalten, oder er gibt `null` zurück, wenn die APP das primäre Symbol verwendet. Beispiel:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

Die `SetAlternameIconName`-Eigenschaft der `UIApplication`-Klasse ermöglicht es dem Entwickler, das App-Symbol zu ändern. Übergeben Sie den Namen des Symbols, das ausgewählt oder `null` werden soll, um zum primären Symbol zurückzukehren. Beispiel:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

Wenn die app ausgeführt wird und der Benutzer ein alternatives Symbol auswählt, wird eine Warnung wie die folgende angezeigt:

![](alternate-app-icons-images/icons04.png "A sample alert when an app changes its icon")

Wenn der Benutzer wieder zum primären Symbol wechselt, wird eine Warnung wie die folgende angezeigt:

![](alternate-app-icons-images/icons05.png "A sample alert when an app changes to the primary icon")

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Hinzufügen alternativer App-Symbole zu einem xamarin. IOS-Projekt und deren Verwendung in der APP behandelt.

## <a name="related-links"></a>Verwandte Links

- [iostenthree-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
