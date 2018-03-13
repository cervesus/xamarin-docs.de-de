---
title: Arbeiten mit Symbolen
ms.topic: article
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 98cd780a29abdbeaab02483e4b6ed01a218f88e5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-icons"></a>Arbeiten mit Symbolen

Apple Watch-Lösungen erfordern zwei Sätze von Symbole:

* Die iOS-app-Symbole, die auf dem iPhone angezeigt werden.
* Apple Watch-Symbole, die in einem Kreis im Überwachungsfenster-Menü und Benachrichtigung Bildschirme gerendert werden. Das Symbol "Watch-app" wird auch angezeigt, der [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS-app.

## <a name="apple-watch-icons"></a>Apple Watch-Symbole

<table align="center" border="1" cellpadding="1" cellspacing="1">
    <tr>
      <td valign="top">
        <b>iOS-App-Symbol</b>
      </td>
      <td valign="top">
Wird angezeigt, auf dem iPhone und wird die übergeordnete Anwendung gestartet </td>
      <td>
        <img src="icons-images/icon-ios.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top" rowspan="3">
        <b>Symbol "Watch-App"</b>
      </td>
      <td valign="top">
Wird auf der Apple Watch-Startbildschirm </td>
      <td>
        <img src="icons-images/icon-home.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
Überwachen von Benachrichtigungen wird angezeigt </td>
      <td>
        <img src="icons-images/notification-icon.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
Wird angezeigt, der <a href="~/ios/watchos/app-fundamentals/settings.md">iOS Apple Watch-App</a>
      </td>
      <td>
        <a href="icons-images/watch-app.png">
          <img src="icons-images/watch-app-sml.png" class="tableimg">
        </a>
      </td>
    </tr>
    <tbody>
</table>



## <a name="configuring-your-solution"></a>Konfigurieren die Projektmappe

Befolgen Sie diese Anweisungen für jedes Projekt, um sicherzustellen, dass Ihre iOS-app und Watch-app den richtigen Namen und das Symbol zeigen:

### <a name="ios-app"></a>iOS App

Finden Sie in der [iOS Anwendungssymbolen geführt](~/ios/app-fundamentals/images-icons/app-icons.md) sicherzustellen, dass Ihre iOS-app-Symbole ordnungsgemäß konfiguriert sind.

#### <a name="infoplist"></a>Info.plist

Die Zeichenfolge, die neben der Watch-app in der [für die Apple Watch-app](~/ios/watchos/app-fundamentals/settings.md) konfiguriert ist, der **iOS-app "Info.plist"**.

Überprüfen Sie, ob Ihre **"Info.plist"** hat eine `CFBundleName` Schlüssel und Wert (Hinweis: Dies unterscheidet sich von der `CFBundleDisplayName`, dass beide):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch App

Einmal Ihre [übergeordnete app](~/ios/watchos/app-fundamentals/parent-app.md) ihm zugehörigen Symbole wurde konfiguriert werden, müssen Sie die Watch-app eine Anwendung Symbol Asset-Katalog hinzu.

1. Mit der rechten Maustaste auf das Watch-App-Projekt, und wählen Sie **Datei > Hinzufügen > neuen Datei... > iOS > Asset-Katalog** Asset-Katalog zum Projekt hinzufügen.

 ![](icons-images/newasset.png "Fügen Sie dem Projekt eine Asset-Katalog hinzu")

2. Doppelklicken Sie auf die **AppIcons.appiconset/Contents.json** Datei

  ![](icons-images/xcassets-iconset-sml.png "Der Inhalt AppIcons")

3. Fügen Sie alle WatchOS Images hinzu, wie in diesem Screenshot gezeigt:

  [![](icons-images/appicons-sml.png "Fügen Sie alle WatchOS Bilder hinzu, wie in diesem Screenshot dargestellt.")](icons-images/appicons.png#lightbox)

  Verweisen auf [Richtlinien von Apple Symbol](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) für die erforderliche Größen (Dimensionen werden auch auf dem Bildschirm angezeigt). Denken Sie daran, dass diese Symbole automatisch abgeschnitten werden werden, um in einem Kreis zu rendern.

  Die Symbolliste sollte etwa wie folgt aussehen:

  ![](icons-images/xcassets-complete-sml.png "Die Symbolliste im Projektmappen-Explorer")

4. Um sicherzustellen, dass das Asset-Katalog in der app enthalten ist, fügen Sie die folgenden Schlüssel und Wert der **Watch-App "Info.plist"**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Sie können überprüfen, ob die Symbole sind richtig konfiguriert, indem Sie überprüfen die [Apple Watch-einstellungs-app](~/ios/watchos/app-fundamentals/settings.md) in der iPhone-Simulator oder Generieren einer [Benachrichtigung](~/ios/watchos/platform/notifications.md) und bestätigen das Symbol auf die Benachrichtigung angezeigt werden Bildschirm.

> [!NOTE]
> **Hinweis**: Symbole sind keinen alpha-Kanal (die app wird abgelehnt werden während der Übermittlung an den Store-App ein alpha-Kanal vorhanden ist). Sie können überprüfen, ob ein alpha-Kanal vorhanden ist, und entfernen Sie ihn [mithilfe der Preview-app auf Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Verwandte Links

- [Symbol "& Bilder Apple Handbuch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
