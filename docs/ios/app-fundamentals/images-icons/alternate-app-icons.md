---
title: Alternative App-Symbole
description: Dieser Artikel umfasst alternativen app Symbole in Xamarin.iOS verwendet.
ms.topic: article
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: ff24a1411a7ddf2ca78c7997f1dc37744013ece4
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="alternate-app-icons"></a>Alternative App-Symbole

_Dieser Artikel umfasst alternativen app Symbole in Xamarin.iOS verwendet._

Apple hat mehrere Erweiterungen für iOS-10.3 hinzugefügt, mit denen eine app, deren Symbol verwalten können:

 - `ApplicationIconBadgeNumber` -Ruft ab oder legt das Signal für das Symbol "app" in der Springboard fest.
 - `SupportsAlternateIcons` -If `true` die app hat eine Alternative Gruppe von Symbolen.
 - `AlternateIconName` -Gibt den Namen des aktuell ausgewählten alternativen Symbols oder `null` Wenn das Symbol "primary" verwenden.
 - `SetAlternameIconName` -Verwenden Sie diese Methode, um das Symbol für die app des angegebenen alternativen Symbols zu wechseln.

![](alternate-app-icons-images/icons04.png "Eine Beispiel-Warnung, wenn eine app sein Symbol ändert sich")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Hinzufügen von alternativen Symbolen zu einem Xamarin.iOS-Projekt

Um eine app So wechseln Sie zu einem anderen Symbol zu ermöglichen, wird eine Auflistung von Symbolbilder im Xamarin.iOS app-Projekt aufgenommen werden müssen. Diese Images können nicht hinzugefügt werden, zum Projekt mit den typischen `Assets.xcassets` -Methode, sie müssen hinzugefügt werden die **Ressourcen** Ordner direkt.

Führen Sie folgende Schritte aus:

1. Wählen Sie das Symbol "erforderlich" Bilder in einem Ordner, wählen Sie alle aus, und ziehen Sie sie auf die **Ressourcen** Ordner in der **Projektmappen-Explorer**:

    ![](alternate-app-icons-images/icons00.png "Wählen Sie die Bilder Symbole aus einem Ordner")

2. Wählen Sie bei Aufforderung **Kopie**, **verwenden die gleiche Aktion für alle ausgewählten Dateien** , und klicken Sie auf die **OK** Schaltfläche:

    ![](alternate-app-icons-images/icons02.png "Die Datei hinzufügen (Dialogfeld)")

3. Die **Ressourcen** Ordner sollte wie folgt nach Abschluss aussehen:

    ![](alternate-app-icons-images/icons01.png "Ressourcenordner sollte wie folgt aussehen.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Ändern die Datei "Info.plist"

Mit den erforderlichen Bildern hinzugefügt, die **Ressourcen** Ordner, der [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) Schlüssel muss dem Projekt hinzugefügt werden **"Info.plist"** Datei. Dieser Schlüssel wird definiert, den Namen des dem Symbol "Neu" und die Bilder, die sie erstellen.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie zu der **Quelle** anzeigen.
3. Hinzufügen einer **bündeln Symbole** Taste, und lassen Sie die **Typ** festgelegt **Wörterbuch**.
4. Hinzufügen einer `CFBundleAlternateIcons` Taste, und legen Sie die **Typ** auf **Wörterbuch**.
5. Hinzufügen einer `AppIcons2` Taste, und legen Sie die **Typ** auf **Wörterbuch**. Dadurch werden der Name des neuen alternativen app Symbolsatz.
6. Hinzufügen einer `CFBundleIconFiles` Taste, und legen Sie die **Typ** auf **Array**
7. Fügen Sie eine neue Zeichenfolge, die `CFBundleIconFiles` Array für jede Erweiterung eingepasst Icon-Datei und die `@2x`, `@3x`, usw. Suffixe (Beispiel `100_icon`). Wiederholen Sie diesen Schritt für jede Datei, aus der der alternativen Symbolsatz besteht.
8. Hinzufügen einer `UIPrerenderedIcon` um einen der `AppIcons2` Wörterbuch vorhanden ist, legen die **Typ** auf **booleschen** und den Wert auf **keine**.
9. Speichern Sie die Änderungen in der Datei.

Das resultierende **"Info.plist"** Datei sollte folgendermaßen aussehen Abschluss:

![](alternate-app-icons-images/icons03.png "Die vollständige Datei "Info.plist"")

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

## <a name="managing-the-apps-icon"></a>Verwalten des App Symbol 

Mit der Symbolbilder im Projekt Xamarin.iOS enthalten und die **"Info.plist"** Datei ordnungsgemäß konfiguriert, der Entwickler mithilfe eines viele neue Funktionen, die von iOS 10.3 hinzugefügt auf um das Symbol für die app zu steuern.

Die `SupportsAlternateIcons` Eigenschaft von der `UIApplication` -Klasse kann der Entwickler, um festzustellen, ob eine app alternativen-Symbole unterstützt. Zum Beispiel:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

Die `ApplicationIconBadgeNumber` Eigenschaft von der `UIApplication` -Klasse kann der Entwickler zum Abrufen oder Festlegen der aktuellen Badge Anzahl das Symbol "app" in der Springboard. Der Standardwert ist 0 (null). Zum Beispiel:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

Die `AlternateIconName` Eigenschaft von der `UIApplication` -Klasse ermöglicht es den Entwickler, die den Namen des aktuell ausgewählten alternativen app Symbols abzurufen, oder gibt `null` , wenn das Symbol "Primary" von der app verwendet wird. Zum Beispiel:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

Die `SetAlternameIconName` Eigenschaft von der `UIApplication` -Klasse kann der Entwickler so ändern Sie das Symbol "app". Übergeben Sie den Namen des Symbols auswählen oder `null` auf das Symbol "primary" zurückgegeben. Zum Beispiel:

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

Wenn die app ausgeführt wird, und die Benutzer wählen Sie ein alternatives Symbol, wird eine Warnung wie folgt angezeigt:

![](alternate-app-icons-images/icons04.png "Eine Beispiel-Warnung, wenn eine app sein Symbol ändert sich")

Wenn der Benutzer wieder auf das Symbol "primary" wechselt, wird eine Warnung wie folgt angezeigt:

![](alternate-app-icons-images/icons05.png "Eine Beispiel-Warnung, wenn eine app auf das Symbol "primary" geändert")

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, alternative app-Symbole mit einem Xamarin.iOS-Projekt hinzufügt und verwendet diese innerhalb der app.



## <a name="related-links"></a>Verwandte Links

- [iOSTenThree-Beispiel](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
