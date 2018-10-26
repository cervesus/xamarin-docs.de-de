---
title: Arbeiten mit WatchOS Symbole in Xamarin
description: Dieses Dokument beschreibt die verschiedenen Symbole für eine WatchOS-Anwendung und eine Projektmappe einrichten, um diese Symbole einzuschließen.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/26/2018
ms.openlocfilehash: 435af10484827826d53b767c2738e3945e0bae42
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121372"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Arbeiten mit WatchOS Symbole in Xamarin

Apple Watch-Lösungen erfordern zwei Sätze von Symbolen:

* Die iOS-app-Symbole, die auf dem iPhone angezeigt werden.
* Apple Watch-Symbole, die in einem Kreis auf das Menü "überwachen" und in der Benachrichtigung Bildschirme gerendert werden. Das Watch-app-Symbol wird auch im der [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS-app.

## <a name="apple-watch-icons"></a>Apple Watch-Symbole

| | | |
|-|-|-|
|iOS-App-Symbol|Wird angezeigt, auf dem iPhone und startet die übergeordnete Anwendung|![iOS-app-Symbol](icons-images/icon-ios.png)|
|Sehen Sie sich App-Symbol|Auf der Apple Watch-Startseite angezeigt wird|![WatchOS-app-Symbol](icons-images/icon-home.png)|
||Sehen Sie sich Benachrichtigungen angezeigt wird|![WatchOS-Benachrichtigungssymbol](icons-images/notification-icon.png)|
||Wird in der [iOS Apple Watch-App](~/ios/watchos/app-fundamentals/settings.md)|![iOS-Watch-App-Symbol](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Konfigurieren Ihre Lösung

Um sicherzustellen, dass Ihre iOS-app und der Watch-app mit den richtigen Namen und ein Symbol angezeigt, führen Sie diese Anweisungen für jedes Projekt aus:

### <a name="ios-app"></a>iOS-App

Finden Sie in der [Symbole für iOS-Anwendung geführt](~/ios/app-fundamentals/images-icons/app-icons.md) um sicherzustellen, dass Ihre iOS-app-Symbole ordnungsgemäß konfiguriert sind.

#### <a name="infoplist"></a>Info.plist

Die Zeichenfolge, die neben die Watch-app in der [Apple Watch-app "Einstellungen"](~/ios/watchos/app-fundamentals/settings.md) konfiguriert ist, der **iOS-app "Info.plist"**.

Überprüfen Sie, ob Ihre **"Info.plist"** verfügt über eine `CFBundleName` Schlüssel und Wert (Hinweis: Dies unterscheidet sich von der `CFBundleDisplayName`, dass beide):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch-App

Sobald Ihre [übergeordnete app](~/ios/watchos/app-fundamentals/parent-app.md) wurde ihm zugehörigen Symbole konfiguriert werden, müssen Sie einen Ressourcenkatalog für Anwendung-Symbol auf der Watch-app hinzufügen.

1. Mit der rechten Maustaste auf das Watch-App-Projekt, und wählen Sie **Datei > Hinzufügen > neue Datei… > iOS > Ressourcenkatalog** einen Asset-Katalog zum Projekt hinzufügen.

 ![](icons-images/newasset.png "Einen Asset-Katalog zum Projekt hinzufügen")

2. Doppelklicken Sie auf die **AppIcon.appiconset/Contents.json** Datei

  ![](icons-images/xcassets-iconset-sml.png "Der Inhalt AppIcon")

3. Fügen Sie alle der WatchOS-Images hinzu, wie im folgenden Screenshot gezeigt:

  [![](icons-images/appicons-sml.png "Fügen Sie alle WatchOS-Images, wie im folgenden Screenshot gezeigt hinzu.")](icons-images/appicons.png#lightbox)

  Finden Sie unter [Apple Richtlinien](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) für die erforderlichen Größen (die Dimensionen werden auch angezeigt, auf dem Bildschirm). Denken Sie daran, dass diese Symbole automatisch ausgeschnitten werden, um in einem Kreis zu rendern.

  Die Symbolliste sollte etwa wie folgt aussehen:

  ![](icons-images/xcassets-complete-sml.png "Die Symbolliste im Projektmappen-Explorer")

4. Um sicherzustellen, dass der Asset-Katalog in der app enthalten ist, fügen Sie den folgenden Schlüssel und Wert der **Watch-App "Info.plist"**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcon.appiconset</string>
```

Sie können überprüfen, ob die Symbole sind richtig konfiguriert, indem Sie überprüfen die [Apple Watch-app "Einstellungen"](~/ios/watchos/app-fundamentals/settings.md) in der iPhone-Simulator oder Generieren von einem [Benachrichtigung](~/ios/watchos/platform/notifications.md) und bestätigen das Symbol wird angezeigt, auf die Benachrichtigung der Bildschirm.

> [!NOTE]
> Symbole keine alpha-Kanal (die app wird während der App-Store-Übermittlung zurückgewiesen, falls ein Alphakanal vorhanden ist). Sie können überprüfen, ob ein Alphakanal vorhanden ist, und entfernen sie [mithilfe der Preview-app unter Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Verwandte Links

- [Apple WatchOS-Symbol und Images-Handbuch](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
