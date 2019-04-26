---
title: Alternative App-Symbole in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie alternative app-Symbole in Xamarin.iOS verwendet wird. Es wird erläutert, wie Sie diese Symbole ein Xamarin.iOS-Projekt hinzufügen, ändern Sie die Datei "Info.plist" und Informationen zur programmgesteuerten Verwaltung von app Symbols.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: cc5052c8988a27605cf7680a3853f80e7afd38b7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61171009"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Alternative App-Symbole in Xamarin.iOS

_Dieser Artikel befasst sich mit anderen app-Symbole in Xamarin.iOS._

Apple hat mehrere Erweiterungen auf iOS 10.3 hinzugefügt, mit denen eine app zum Verwalten von das entsprechende Symbols:

 - `ApplicationIconBadgeNumber` -Ruft ab oder legt den Badge, der das app-Symbol in der Springboard-Reihe.
 - `SupportsAlternateIcons` -If `true` die app verfügt über eine Alternative Gruppe von Symbolen.
 - `AlternateIconName` -Gibt den Namen des aktuell ausgewählten alternativen Symbols zurück oder `null` Wenn das primäre Symbol verwenden.
 - `SetAlternameIconName` – Verwenden Sie diese Methode, um das Symbol der app in der angegebenen alternativen Symbol wechseln.

![](alternate-app-icons-images/icons04.png "Eine beispielwarnung, wenn eine app auf das Symbol ändert")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Hinzufügen von anderen Symbolen zu einer Xamarin.iOS-Projekt

Um eine app So wechseln Sie zu einem anderen Symbol zu ermöglichen, müssen eine Sammlung von Symbolbilder im Xamarin.iOS-app-Projekt aufgenommen werden. Diese Images können nicht hinzugefügt werden, um das Projekt mit der normalen `Assets.xcassets` -Methode, sie müssen hinzugefügt werden die **Ressourcen** Ordner direkt.

Führen Sie folgende Schritte aus:

1. Wählen Sie die erforderlichen Symbolbilder in einem Ordner, wählen Sie alle aus, und ziehen Sie sie in der **Ressourcen** Ordner in der **Projektmappen-Explorer**:

    ![](alternate-app-icons-images/icons00.png "Wählen Sie die Symbole-Images aus einem Ordner")

2. Wählen Sie bei Aufforderung **Kopie**, **verwenden die gleiche Aktion für alle ausgewählten Dateien** , und klicken Sie auf die **OK** Schaltfläche:

    ![](alternate-app-icons-images/icons02.png "Die Datei hinzufügen, um das Dialogfeld Ordner")

3. Die **Ressourcen** Ordner sollte nach Abschluss folgendermaßen aussehen:

    ![](alternate-app-icons-images/icons01.png "Der Ordner \"Resources\" sollte wie folgt aussehen.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Ändern die Datei "Info.plist"

Mit den erforderlichen Bilder hinzugefügt, die **Ressourcen** Ordner die [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) Schlüssel des Projekts hinzugefügt werden müssen **"Info.plist"** Datei. Dieser Schlüssel wird definiert, den Namen des dem Symbol "Neu" und die Images, die Anwendung besteht.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie zu der **Quelle** anzeigen.
3. Hinzufügen einer **bündeln Symbole** gedrückt, und lassen Sie die **Typ** festgelegt **Wörterbuch**.
4. Hinzufügen einer `CFBundleAlternateIcons` gedrückt, und legen Sie die **Typ** zu **Wörterbuch**.
5. Hinzufügen einer `AppIcon2` gedrückt, und legen Sie die **Typ** zu **Wörterbuch**. Dadurch werden der Name der neuen alternative app-Symbol.
6. Hinzufügen einer `CFBundleIconFiles` gedrückt, und legen Sie die **Typ** zu **Array**
7. Fügen Sie eine neue Zeichenfolge, die `CFBundleIconFiles` Array für jede Symboldatei, die die Erweiterung auslassen und die `@2x`, `@3x`, usw. Suffixe (Beispiel `100_icon`). Wiederholen Sie diesen Schritt für jede Datei, die den Satz von alternativen Symbol bildet.
8. Hinzufügen einer `UIPrerenderedIcon` um einen der `AppIcon2` Wörterbuch vorhanden ist, legen die **Typ** zu **booleschen** und der Wert, der **keine**.
9. Speichern Sie die Änderungen in der Datei.

Die resultierende **"Info.plist"** Datei sollte aussehen wie folgt nach Abschluss:

![](alternate-app-icons-images/icons03.png "Die fertige Datei \"Info.plist\"")

Oder wie diesen, wenn, die in einem Text-Editor geöffnet:

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

## <a name="managing-the-apps-icon"></a>Verwalten von App Symbols 

Mit der Symbolbilder enthalten im Xamarin.iOS-Projekt und die **"Info.plist"** Datei ordnungsgemäß konfiguriert ist, der Entwickler mithilfe einer der vielen neuen Features in iOS 10.3 auf das Symbol der app steuern.

Die `SupportsAlternateIcons` Eigenschaft der `UIApplication` -Klasse kann der Entwickler, um festzustellen, ob eine app alternativen-Symbole unterstützt. Zum Beispiel:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

Die `ApplicationIconBadgeNumber` Eigenschaft der `UIApplication` Klasse kann der Entwickler zum Abrufen oder Festlegen der aktuellen Badge-Anzahl von app-Symbol in der Springboard-Reihe. Der Standardwert ist 0 (null). Zum Beispiel:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

Die `AlternateIconName` Eigenschaft der `UIApplication` Klasse ermöglicht dem Entwickler, die den Namen des Symbols, das aktuell ausgewählte alternative app abzurufen, oder sie gibt `null` , wenn die app das erste Symbol verwendet. Zum Beispiel:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

Die `SetAlternameIconName` Eigenschaft der `UIApplication` Klasse ermöglicht dem Entwickler, die das Symbol der app zu ändern. Übergeben Sie den Namen des Symbols auswählen oder `null` auf das Symbol "primäre" zurückgegeben. Zum Beispiel:

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

Wenn die app ausgeführt wird, und der Benutzer durch Auswählen eines alternativen Symbols, wird eine Warnung wie folgt angezeigt:

![](alternate-app-icons-images/icons04.png "Eine beispielwarnung, wenn eine app auf das Symbol ändert")

Wenn der Benutzer wieder auf das Symbol "primäre" wechselt, wird eine Warnung wie folgt angezeigt:

![](alternate-app-icons-images/icons05.png "Eine beispielwarnung, wenn eine app auf das Symbol \"primäre\" geändert")

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, alternative app-Symbole zu einem Xamarin.iOS-Projekt hinzugefügt, und verwenden diese innerhalb der app.



## <a name="related-links"></a>Verwandte Links

- [iOSTenThree-Beispiel](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
