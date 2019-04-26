---
title: App-Store-Symbole in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Asset-Katalogs verwenden, um ein App-Store-Symbol für eine Xamarin.iOS-Anwendung zu verwalten. App-Store-Symbole wurden zuvor mit iTunes Connect verwaltet.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/26/2017
ms.openlocfilehash: 53e25ae9f4650254f2aaaa03dc8727fae674c9f0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61258090"
---
# <a name="app-store-icons-in-xamarinios"></a>App-Store-Symbole in Xamarin.iOS

Vor dem Xcode 9 wurden die Symbole für alle App-Store über iTunes Connect hinzugefügt. Dies ist jedoch nicht mehr der Fall. App-Store-Symbole müssen als Teil Ihrer Projekt-Paket enthalten und werden in ein Asset-Katalog hinzugefügt. Apps, die kein App-Store-Symbol enthalten werden von Apple abgelehnt.

Das App-Store-Symbol ist das Gesicht Ihrer Anwendung für Benutzer, damit sie merken werden muss und auch bei geringer Größe. Einprägsame Symbole sind übersichtlich, einfach und sofort wiedererkennbar.

Apple empfiehlt Folgendes für Anwendungssymbole:

- Entwerfen Sie ein für Ihre Anwendung geeignetes Symbol.
- Erstellen Sie ein einfaches Symbol, das dem Design der Anwendung entspricht.
- Verwenden Sie keine Wörter.
- Denken Sie global: Ein einzelnes App-Symbol wird im App Store für alle Regionen verwendet.

Sie benötigen dafür ein Bild mit 1024 x 1024 Pixel.  Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

Weitere Informationen finden Sie auf der Apple [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Hinzufügen eines App-Store-Symbols

App Store-Symbole sollten nun über einen Ressourcenkatalog bereitgestellt werden. 

Zum Hinzufügen ein App-Store-Symbols folgendermaßen vor:

1. Suchen Sie die **AppIcon** -Image festzulegen, der **Assets.xcassets** -Datei Ihres Projekts. 
    - Alle neuen Projekte sollten verfügen über eine ein **Assets.xcassets** Datei, die eine Gruppe von Bildern AppIcon enthält.
    - Um einen neuen Ressourcenkatalog hinzufügen, mit der Maustaste, auf Ihr Projekt, und wählen Sie **hinzufügen > neue Datei > Ressourcenkatalog**.
    - Um ein neues Image einen app-Symbolsatz hinzuzufügen, mit der rechten Maustaste den Symbolbereich-Gruppe, und wählen **-App-Symbole und Startbilder > neue App-Symbol**:
    
    ![Neue Option zur Gruppe hinzufügen](app-store-icon-images/image1.png)

2. Scrollen Sie zu der **App Store** Symbol in der Liste:

    ![App Store-Symbol](app-store-icon-images/image2.png)

3. Klicken Sie auf das Symbol, und suchen Sie nach Ihrem 1024 x 1024 Pixel großes Bild. Speichern Sie den Asset-Katalog.




## <a name="related-links"></a>Verwandte Links

- [Verwalten von Symbolen mit Ressourcenkataloge](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
