---
title: App Store-Symbole in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie mit Asset-Katalogen ein App Store-Symbol für eine xamarin. IOS-Anwendung verwaltet wird. Zuvor wurden App Store-Symbole mit iTunes Connect verwaltet.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/26/2017
ms.openlocfilehash: 984ef59a4571379f7b2969ed4f15674f8819e4e4
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620339"
---
# <a name="app-store-icons-in-xamarinios"></a>App Store-Symbole in xamarin. IOS

Vor Xcode 9 wurden alle App Store-Symbole über iTunes Connect hinzugefügt. Dies ist jedoch nicht mehr der Fall. App Store-Symbole müssen nun als Teil Ihres Projekt Pakets eingeschlossen und innerhalb eines Ressourcen Katalogs hinzugefügt werden. Apps, die kein App Store-Symbol enthalten, werden von Apple abgelehnt.

Das App Store-Symbol ist das Gesicht ihrer Anwendung für die Benutzer, daher muss es sich als nicht korrekt einblenden und gut angezeigt werden. Einprägsame Symbole sind übersichtlich, einfach und sofort wiedererkennbar.

Apple empfiehlt Folgendes für Anwendungssymbole:

- Entwerfen Sie ein für Ihre Anwendung geeignetes Symbol.
- Erstellen Sie ein einfaches Symbol, das dem Design der Anwendung entspricht.
- Verwenden Sie keine Wörter.
- Denken Sie global: Ein einzelnes App-Symbol wird im App Store für alle Regionen verwendet.

Sie benötigen dafür ein Bild mit 1024 x 1024 Pixel.  Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

Weitere Informationen finden Sie unter IOS- [Richtlinien für die Benutzeroberfläche](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/)von Apple.

## <a name="adding-an-app-store-icon"></a>Hinzufügen eines App Store-Symbols

App Store-Symbole sollten nun über einen Ressourcenkatalog bereitgestellt werden. 

Gehen Sie folgendermaßen vor, um ein App Store-Symbol hinzuzufügen:

1. Suchen Sie das **AppIcon** -Bild, das in der Datei **Assets. xcassets** Ihres Projekts festgelegt ist. 
    - Alle neuen Projekte sollten eine **Assets. xcassets** -Datei enthalten, die ein AppIcon-Bild enthält.
    - Um einen neuen Ressourcen Katalog hinzuzufügen, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **> neue Datei > Asset-Katalog hinzufügen**aus.
    - Zum Hinzufügen eines neuen App-Symbol Bilds klicken Sie mit der rechten Maustaste in den Bereich Symbolsatz, und wählen Sie **App-Symbole & Start Bilder > Symbol "neue APP**" aus:

    ![Neue Abbild Satz Option hinzufügen](app-store-icon-images/image1.png)

2. Scrollen Sie zum **App Store** -Symbol in der Liste:

    ![App Store-Symbol](app-store-icon-images/image2.png)

3. Klicken Sie auf das Symbol, und suchen Sie nach Ihrem 1024 x 1024 Pixel Bild. Speichern Sie den Asset-Katalog.




## <a name="related-links"></a>Verwandte Links

- [Verwalten von Symbolen mit Asset-Katalogen](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
