---
title: Alternative App-Symbole in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie alternative App-Symbole in xamarin. IOS verwenden. Darin wird erläutert, wie Sie diese Symbole einem xamarin. IOS-Projekt hinzufügen, wie Sie die Datei "Info. plist" ändern und das Symbol der APP Programm gesteuert verwalten.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 39a18a775946c2f139b4c032d2c360bc5680a0e7
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937916"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Alternative App-Symbole in xamarin. IOS

_In diesem Artikel wird die Verwendung alternativer App-Symbole in xamarin. IOS behandelt._

Apple hat eine Reihe von Erweiterungen zu IOS 10,3 hinzugefügt, die es einer APP ermöglichen, Ihr Symbol zu verwalten:

- `ApplicationIconBadgeNumber`: Ruft das Badge des App-Symbols im Springboard ab oder legt es fest.
- `SupportsAlternateIcons`: Wenn `true` die APP über eine alternative Gruppe von Symbolen verfügt.
- `AlternateIconName`-Gibt den Namen des alternativen Symbols zurück, das derzeit ausgewählt ist, oder, `null` Wenn das primäre Symbol verwendet wird.
- `SetAlternameIconName`-Verwenden Sie diese Methode, um das Symbol der APP auf das angegebene Alternative Symbol zu wechseln.

![Eine Beispiel Warnung, wenn eine APP Ihr Symbol ändert](alternate-app-icons-images/icons04.png)

<a name="Adding-Alternate-Icons"></a>

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Hinzufügen alternativer Symbole zu einem xamarin. IOS-Projekt

Damit eine APP auf ein alternatives Symbol umgestellt werden kann, muss eine Auflistung von Symbolbildern im xamarin. IOS-App-Projekt enthalten sein. Diese Bilder können dem Projekt nicht mithilfe der typischen Methode hinzugefügt werden `Assets.xcassets` . Sie müssen direkt dem **Ressourcen** Ordner hinzugefügt werden.

Gehen Sie wie folgt vor:

1. Wählen Sie die erforderlichen Symbolbilder in einem Ordner aus, wählen Sie alle aus, und ziehen Sie Sie in der **Projektmappen-Explorer**in den Ordner **Ressourcen** :

    ![Symbole aus einem Ordner auswählen](alternate-app-icons-images/icons00.png)

2. Wenn Sie dazu aufgefordert werden, wählen Sie **Kopieren** **aus, verwenden Sie die gleiche Aktion für alle ausgewählten Dateien** , und klicken Sie auf **OK** :

    ![Dialogfeld "Datei zum Ordner hinzufügen"](alternate-app-icons-images/icons02.png)

3. Der Ordner **Ressourcen** sollte wie folgt aussehen:

    ![Der Ordner "Resources" sollte wie folgt aussehen.](alternate-app-icons-images/icons01.png)

<a name="Modifying-the-Info.plist-File"></a>

## <a name="modifying-the-infoplist-file"></a>Ändern der Datei "Info. plist"

Nachdem die erforderlichen Images dem **Ressourcen** Ordner hinzugefügt wurden, muss der Schlüssel " [cfbundlealternative Eicons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) " der Datei " **Info. plist** " des Projekts hinzugefügt werden. Mit diesem Schlüssel werden der Name des neuen Symbols und die Bilder definiert, aus denen es besteht.

Gehen Sie wie folgt vor:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie zur Ansicht **Quelle**.
3. Fügen Sie einen Schlüssel für die **Bündel Symbole** hinzu, und belassen Sie den **Typ** " **Dictionary**".
4. Fügen Sie einen `CFBundleAlternateIcons` Schlüssel hinzu, und legen Sie den **Typ** auf **Dictionary**fest.
5. Fügen Sie einen `AppIcon2` Schlüssel hinzu, und legen Sie den **Typ** auf **Dictionary**fest. Dabei handelt es sich um den Namen des neuen alternativen App-Symbol Satzes.
6. Fügen Sie einen `CFBundleIconFiles` Schlüssel hinzu, und legen Sie den **Typ** auf **Array** fest
7. Fügen Sie dem Array für jede Symbol Datei eine neue Zeichenfolge mit der `CFBundleIconFiles` Erweiterung und den `@2x` `@3x` Suffixen,, usw. (z. b. `100_icon` ) hinzu. Wiederholen Sie diesen Schritt für jede Datei, aus der der Alternative Symbolsatz besteht.
8. Fügen Sie `UIPrerenderedIcon` dem Wörterbuch einen Schlüssel hinzu `AppIcon2` , legen Sie den **Typ** auf " **Boolean** " und den Wert auf " **No**" fest.
9. Speichern Sie die Änderungen in der Datei.

Die resultierende Datei " **Info. plist** " sollte wie folgt aussehen:

![Die fertige Datei "Info. plist"](alternate-app-icons-images/icons03.png)

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

<a name="Managing-the-Apps-Icon"></a>

## <a name="managing-the-apps-icon"></a>Verwalten des App-Symbols 

Wenn die Symbolbilder im xamarin. IOS-Projekt enthalten sind und die Datei " **Info. plist** " ordnungsgemäß konfiguriert ist, kann der Entwickler eine der vielen neuen Features verwenden, die IOS 10,3 hinzugefügt wurden, um das Symbol der APP zu steuern.

Die- `SupportsAlternateIcons` Eigenschaft der- `UIApplication` Klasse ermöglicht es dem Entwickler zu überprüfen, ob eine APP Alternative Symbole unterstützt. Beispiel:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

Die- `ApplicationIconBadgeNumber` Eigenschaft der- `UIApplication` Klasse ermöglicht es dem Entwickler, die aktuelle Badge-Nummer des App-Symbols im Springboard zu erhalten oder festzulegen. Der Standardwert ist 0 (null). Beispiel:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

Die- `AlternateIconName` Eigenschaft der- `UIApplication` Klasse ermöglicht es dem Entwickler, den Namen des aktuell ausgewählten Alternativen App-Symbols zu erhalten, oder es wird zurückgegeben, `null` Wenn die APP das primäre Symbol verwendet. Beispiel:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

Die- `SetAlternameIconName` Eigenschaft der- `UIApplication` Klasse ermöglicht es dem Entwickler, das App-Symbol zu ändern. Übergeben Sie den Namen des Symbols, das Sie auswählen möchten, oder, `null` um zum primären Symbol zurückzukehren. Beispiel:

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

![Eine Beispiel Warnung, wenn eine APP Ihr Symbol ändert](alternate-app-icons-images/icons04.png)

Wenn der Benutzer wieder zum primären Symbol wechselt, wird eine Warnung wie die folgende angezeigt:

![Eine Beispiel Warnung, wenn eine app in das primäre Symbol geändert wird](alternate-app-icons-images/icons05.png)

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Hinzufügen alternativer App-Symbole zu einem xamarin. IOS-Projekt und deren Verwendung in der APP behandelt.

## <a name="related-links"></a>Verwandte Links

- [iostenthree-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
