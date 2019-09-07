---
title: Bilder und Symbole in xamarin. IOS
description: Dieser Abschnitt enthält eine Vielzahl von Artikeln, in denen die Arbeit mit Bildern in einer xamarin. IOS-App behandelt wird, z. b. die Verwendung als Symbole, Startbildschirme oder deren Einbeziehung in Steuerelemente und das Bereitstellen von Symbolen für benutzerdefinierte Dokumenttypen.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 6940e07c51dbc19615454e0c51188152db22c63f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767211"
---
# <a name="images-and-icons-in-xamarinios"></a>Bilder und Symbole in xamarin. IOS

_Dieser Abschnitt enthält eine Vielzahl von Artikeln, in denen die Arbeit mit Bildern in einer xamarin. IOS-App behandelt wird, z. b. die Verwendung als Symbole, Startbildschirme oder deren Einbeziehung in Steuerelemente und das Bereitstellen von Symbolen für benutzerdefinierte Dokumenttypen._

Es gibt mehrere Möglichkeiten, wie Image-Assets in einer IOS-App verwendet werden. Wenn Sie ein Bild einfach als Teil der Benutzeroberfläche einer App anzeigen und es einem UI-Steuerelement wie z `UIButton` . b. oder `UIImageView`zuweisen, können Sie mit xamarin. IOS auf folgende Weise eine hervor artige Grafik zu einer IOS-app hinzufügen: 

- Auflösen **unabhängiger Images** – verwenden Sie die integrierte Unterstützung von IOS zum Arbeiten mit Images über verschiedene Lösungen und Typen von Geräten hinweg (iPhone, iPad usw.).
- **Asset Catalog Image Sets** : Verwenden Sie Ressourcen **Katalog-Image Gruppen** , um alle Versionen eines bestimmten Abbild Medien Objekts zu verwalten und zu gruppieren, das von einer APP benötigt wird.
- **Bilder im IOS-Designer** : Verwenden Sie den IOS-Designer, um Bilder für Steuerelemente festzulegen.
- **Bilder im Code** – verwenden Sie `UIImage` die-Methoden der-Klasse, um bildassets zu laden und mit diesen zu arbeiten C# , und weisen Sie Sie Benutzeroberflächen-Steuerelementen in
- **Anwendungssymbol** : definieren Sie das App-Symbol, das für jede IOS-App erforderlich ist. Dies ist das Symbol, auf das der Benutzer vom IOS-Startbildschirm tippt, um die APP zu starten. Außerdem wird dieses Symbol ggf. von Game Center verwendet.
- **Spotlight-Symbol** : Hiermit wird das Spotlight-Symbol der APP definiert. Jedes Mal, wenn der Benutzer den Namen einer APP in eine Spotlight-Suche eingibt, wird dieses Symbol angezeigt.
- **Symbol "Einstellungen** ": definieren Sie das Symbol " **Einstellungen** " der app. Wenn der Benutzer die app " **Einstellungen** " auf seinem IOS-Gerät eingibt, wird dieses Symbol am Ende der Einstellungs Liste für die App angezeigt. 
- **Startbildschirme** : Hiermit wird der Startbildschirm der APP definiert. Nachdem der Benutzer auf das App-Symbol tippt und bevor die erste Ansicht angezeigt wird, wird ein leerer Bildschirm angezeigt. Glücklicherweise bietet IOS Unterstützung für die Anzeige eines Bilds anstelle des leeren Bildschirms mithilfe eines Storyboards. 
- **iTunes-Symbol** : Geben Sie ein iTune-Symbol an. Bei Verwendung der Ad-hoc-Methode der Bereitstellung einer APP (entweder für Unternehmensbenutzer oder für Beta-Tests auf echten Geräten) muss der Entwickler auch ein 512 x512-und 1024 x1024-Bild einschließen, das zur Darstellung der app in iTunes verwendet wird.
- **Dokument Symbole** : Verwenden Sie ein Bild als Symbol für jeden bestimmten Dokumenttyp, der von einer xamarin. IOS-App unterstützt oder erstellt wird.

Beim Erstellen von Image Assets für eine IOS-APP und an mehreren Stellen, an denen diese Assets verwendet werden, müssen mehrere Aspekte berücksichtigt werden. Jede dieser Elemente wirkt sich nicht nur darauf aus, wie viele Bild Ressourcen erforderlich sind, sondern wie diese Assets erstellt werden. In den folgenden Themen werden die Typen von Images behandelt, die erforderlich sind, wie diese Assets im Paket der Anwendung enthalten sind und wie die Abbild Ressourcen genutzt werden, um die erforderliche Funktionalität bereitzustellen:

## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

In diesem Artikel wird beschrieben, wie Sie ein Image-Asset in eine xamarin. IOS-App einschließen C# und dieses Bild entweder mithilfe von Code oder durch Zuweisen zu einem Steuerelement im IOS-Designer anzeigen.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Anwendungssymbole](~/ios/app-fundamentals/images-icons/app-icons.md)

In diesem Artikel wird das einschließen und Verwalten eines Image Assets in einer xamarin. IOS-App behandelt, die als App-Symbol verwendet werden soll.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Alternative App-Symbole](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple hat eine Reihe von Erweiterungen zu IOS 10,3 hinzugefügt, die es einer APP ermöglichen, Ihr Symbol zu verwalten:

- `ApplicationIconBadgeNumber`: Ruft das Badge des App-Symbols im Springboard ab oder legt es fest.
- `SupportsAlternateIcons`: Wenn `true` die APP über eine alternative Gruppe von Symbolen verfügt.
- `AlternateIconName`-Gibt den Namen des alternativen Symbols zurück, das derzeit `null` ausgewählt ist, oder, wenn das primäre Symbol verwendet wird.
- `SetAlternameIconName`-Verwenden Sie diese Methode, um das Symbol der APP auf das angegebene Alternative Symbol zu wechseln.

## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Startbildschirme](~/ios/app-fundamentals/images-icons/launch-screens.md)

In diesem Artikel wird die Verwendung eines besonderen Storyboard-Typs zum Bereitstellen eines universellen Startbildschirms für jede IOS-Gerätegröße und-Auflösung behandelt.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Benutzerdefinierte Dokumenttypen](~/ios/app-fundamentals/images-icons/custom-document-types.md)

In diesem Artikel wird das einschließen und Verwalten eines Image Assets in einer xamarin. IOS-App behandelt, die als benutzerdefiniertes Dokumenttyp Symbol verwendet werden soll.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Richtlinien für benutzerdefiniertes Symbol und Bild Erstellung](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
