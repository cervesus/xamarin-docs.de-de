---
title: App-Store-Symbole in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Asset-Kataloge zu verwenden, um ein App-Store-Symbol für eine Anwendung Xamarin.iOS zu verwalten. Zuvor wurden App Store Symbole mit iTunes Connect verwaltet.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 749dbf01af382a54fe24652706f6a605ac7b20b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783609"
---
# <a name="app-store-icons-in-xamarinios"></a>App-Store-Symbole in Xamarin.iOS

Vor dem Xcode 9 wurden alle Symbole der App Store über iTunes Connect hinzugefügt. Dies ist jedoch nicht mehr der Fall. App Store Symbole müssen jetzt werden als Bestandteil Ihres Projekts-Pakets und innerhalb einer Asset-Katalog hinzugefügt. Apps, die kein Symbol für die App Store enthalten, werden vom Apple zurückgewiesen.

Das Symbol "App-Store" ist dem Zifferblatt Ihrer Anwendung für Benutzer, daher muss einprägsamen sein und auch an einer kleinen Größe anzuzeigen. Einprägsame Symbole sind übersichtlich, einfach und sofort wiedererkennbar.

Apple empfiehlt Folgendes für Anwendungssymbole:

- Entwerfen Sie ein für Ihre Anwendung geeignetes Symbol.
- Erstellen Sie ein einfaches Symbol, das dem Design der Anwendung entspricht.
- Verwenden Sie keine Wörter.
- Denken Sie global: Ein einzelnes App-Symbol wird im App Store für alle Regionen verwendet.

Sie benötigen dafür ein Bild mit 1024 x 1024 Pixel.  Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

Weitere Informationen finden Sie auf der Apple [iOS-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Hinzufügen von einem App Store-Symbol

App Store-Symbole sollten nun über einen Ressourcenkatalog bereitgestellt werden. 

So fügen Sie ein Symbol App Store, führen Sie die folgenden hinzu:

1. Suchen Sie die **AppIcon** Bild festgelegt wird, der **Assets.xcassets** Datei des Projekts. 
    - Alle neuen Projekte Funktionsumfang sollte eine ein **Assets.xcassets** Datei, die einen Satz von AppIcon enthält.
    - Um einen neuen Asset-Katalog hinzuzufügen, mit der Maustaste, auf Ihr Projekt, und wählen **hinzufügen > neuen Datei > Asset-Katalog**.
    - Um ein neues einen app-Symbolsatz Bild hinzuzufügen, mit der rechten Maustaste die Symbolbereich-Satz, und wählen **App Symbole & starten Bilder > Symbol "neue App"**:
    
    ![Hinzufügen von neuen Image Set-option](app-store-icon-images/image1.png)

2. Führen Sie einen Bildlauf zu der **App Store** Symbol in der Liste:

    ![Symbol "App-Store"](app-store-icon-images/image2.png)

3. Klicken Sie auf das Symbol ", und suchen Sie nach Ihrem 1024 x 1024 Pixel großes Bild. Speichern Sie das Asset-Katalog.




## <a name="related-links"></a>Verwandte Links

- [Verwalten von Symbolen mit Asset-Kataloge](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
