---
title: Arbeiten mit WatchOS Symbole in Xamarin
description: Dieses Dokument beschreibt die verschiedenen Symbole für eine WatchOS-Anwendung und zum Einrichten einer Lösung für diese Symbole enthalten.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 150cca754de26edffcf97bb5d39b26166662c75b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790665"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Arbeiten mit WatchOS Symbole in Xamarin

Apple Watch-Lösungen erfordern zwei Sätze von Symbole:

* Die iOS-app-Symbole, die auf dem iPhone angezeigt werden.
* Apple Watch-Symbole, die in einem Kreis im Überwachungsfenster-Menü und Benachrichtigung Bildschirme gerendert werden. Das Symbol "Watch-app" wird auch angezeigt, der [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS-app.

## <a name="apple-watch-icons"></a>Apple Watch-Symbole

| | | |
|-|-|-|
|iOS-App-Symbol|Wird angezeigt, auf dem iPhone und wird die übergeordnete Anwendung gestartet|![](icons-images/icon-ios.png)|
|Symbol "Watch-App"|Wird auf der Apple Watch-Startbildschirm|![](icons-images/icon-home.png)|
||Überwachen von Benachrichtigungen wird angezeigt|![](icons-images/notification-icon.png)|
||Wird angezeigt, der [iOS Apple Watch-App](~/ios/watchos/app-fundamentals/settings.md)|![](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Konfigurieren die Projektmappe

Befolgen Sie diese Anweisungen für jedes Projekt, um sicherzustellen, dass Ihre iOS-app und Watch-app den richtigen Namen und das Symbol zeigen:

### <a name="ios-app"></a>iOS-App

Finden Sie in der [iOS Anwendungssymbolen geführt](~/ios/app-fundamentals/images-icons/app-icons.md) sicherzustellen, dass Ihre iOS-app-Symbole ordnungsgemäß konfiguriert sind.

#### <a name="infoplist"></a>Info.plist

Die Zeichenfolge, die neben der Watch-app in der [für die Apple Watch-app](~/ios/watchos/app-fundamentals/settings.md) konfiguriert ist, der **iOS-app "Info.plist"**.

Überprüfen Sie, ob Ihre **"Info.plist"** hat eine `CFBundleName` Schlüssel und Wert (Hinweis: Dies unterscheidet sich von der `CFBundleDisplayName`, dass beide):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch-App

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
> Symbole können nicht über einen alpha-Kanal verfügen (die app wird während der Übermittlung an den App Store abgelehnt, wenn ein alpha-Kanal vorhanden ist). Sie können überprüfen, ob ein alpha-Kanal vorhanden ist, und entfernen Sie ihn [mithilfe der Preview-app auf Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Verwandte Links

- [Symbol "& Bilder Apple Handbuch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
