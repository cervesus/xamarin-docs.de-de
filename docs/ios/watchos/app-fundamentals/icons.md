---
title: Arbeiten mit watchos-Symbolen in xamarin
description: In diesem Dokument werden die verschiedenen Symbole beschrieben, die für eine watchos-Anwendung erforderlich sind, und es wird erläutert, wie Sie eine Lösung einrichten, um diese Symbole
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/26/2018
ms.openlocfilehash: e0bf9ec1553e6638398695157a11242b9885b168
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768099"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Arbeiten mit watchos-Symbolen in xamarin

Apple Watch Lösungen erfordern zwei Sätze von Symbolen:

- Die IOS-App-Symbole, die auf dem iPhone angezeigt werden.
- Apple Watch Symbole, die im Menü "überwachen" und in den Benachrichtigungs Bildschirmen in einem Kreis gerendert werden. Das Symbol App-APP wird auch in der [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) IOS-App angezeigt.

## <a name="apple-watch-icons"></a>Apple Watch Symbole

| | | |
|-|-|-|
|Symbol für IOS-App|Wird auf dem iPhone angezeigt und startet die übergeordnete App|![Symbol für IOS-App](icons-images/icon-ios.png)|
|Symbol "App ansehen"|Wird auf dem Apple Watch-Startbildschirm angezeigt.|![watchos-App-Symbol](icons-images/icon-home.png)|
||Wird bei Überwachungs Benachrichtigungen angezeigt.|![watchos-Benachrichtigungssymbol](icons-images/notification-icon.png)|
||Wird in der [IOS-Apple Watch-App](~/ios/watchos/app-fundamentals/settings.md) angezeigt.|![IOS Watch-App-Symbol](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Konfigurieren der Lösung

Um sicherzustellen, dass Ihre IOS-APP und die Watch-App den richtigen Namen und das richtige Symbol anzeigen, befolgen Sie die folgenden Anweisungen für jedes Projekt:

### <a name="ios-app"></a>iOS-App

Lesen Sie den [Leitfaden für IOS-Anwendungs Symbole](~/ios/app-fundamentals/images-icons/app-icons.md) , um sicherzustellen, dass die Symbole ihrer IOS-APP richtig konfiguriert sind.

#### <a name="infoplist"></a>Info.plist

Die Zeichenfolge, die neben ihrer Watch-app in der [Apple Watch Einstellungs-APP](~/ios/watchos/app-fundamentals/settings.md) angezeigt wird, wird in der Datei " **Info. plist**" der IOS-App konfiguriert.

Vergewissern Sie sich, dass die Datei " **Info. plist** " einen Schlüssel und einen `CFBundleName` Wert aufweist (Hinweis: Dies unterscheidet sich von `CFBundleDisplayName`, Sie können beides aufweisen):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch-App

Nachdem die Symbole ihrer über [geordneten App](~/ios/watchos/app-fundamentals/parent-app.md) konfiguriert wurden, müssen Sie der Watch-APP einen Anwendungssymbol-Ressourcen Katalog hinzufügen.

1. Klicken Sie mit der rechten Maustaste auf das App-Projekt überwachen, und wählen Sie **Datei > > neue Datei hinzufügen... > IOS > Asset Catalog** ein, um dem Projekt einen Ressourcen Katalog hinzuzufügen.

    ![](icons-images/newasset.png "Hinzufügen eines Ressourcen Katalogs zum Projekt")

2. Doppelklicken Sie auf die Datei " **AppIcon. appianset/Contents. JSON** ".

    ![](icons-images/xcassets-iconset-sml.png "Der AppIcon-Inhalt")

3. Fügen Sie alle watchos-Images hinzu, wie in diesem Screenshot gezeigt:

    [![](icons-images/appicons-sml.png "Fügen Sie alle watchos-Images hinzu, wie in diesem Screenshot gezeigt.")](icons-images/appicons.png#lightbox)

    Informationen zu den erforderlichen Größen (die Dimensionen werden auch auf dem Bildschirm angezeigt) finden Sie in den [Symbol Richtlinien von Apple](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) . Beachten Sie, dass diese Symbole automatisch abgeschnitten werden, um Sie in einem Kreis zu renderten.

    Die Symbolliste sollte in etwa wie folgt aussehen:

    ![](icons-images/xcassets-complete-sml.png "Die Symbolliste im Projektmappen-Explorer")

4. Um sicherzustellen, dass der Asset-Katalog in der App enthalten ist, fügen Sie der Datei " **Info. plist" der Watch-App**den folgenden Schlüssel und Wert hinzu:

    ```xml
    <key>XSAppIconAssets</key>
    <string>Images.xcassets/AppIcon.appiconset</string>
    ```

Sie können überprüfen, ob die Symbole ordnungsgemäß konfiguriert sind, indem Sie die [App Apple Watch Einstellungen](~/ios/watchos/app-fundamentals/settings.md) im iPhone-Simulator überprüfen oder eine [Benachrichtigung](~/ios/watchos/platform/notifications.md) erstellen und bestätigen, dass das Symbol auf dem Benachrichtigungs Bildschirm angezeigt wird.

> [!NOTE]
> Symbole können keinen Alphakanal aufweisen (die APP wird während der App Store-Übermittlung abgelehnt, wenn ein Alphakanal vorhanden ist). Sie können überprüfen, ob ein Alphakanal vorhanden ist, und ihn [mithilfe der Vorschau-App auf Mac OS X](~/ios/watchos/troubleshooting.md#noalpha)entfernen.

## <a name="related-links"></a>Verwandte Links

- [Das watchos-Symbol von Apple & Bild Leit Faden](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
