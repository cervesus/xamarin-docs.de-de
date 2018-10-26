---
title: Bilder und Symbole in Xamarin.iOS
description: Dieser Abschnitt enthält eine Vielzahl von Artikeln, die Arbeit mit Bildern in einer Xamarin.iOS-app, wie z. B. die Verwendung als Symbole behandelt, Startbildschirme oder einschließlich werden in Steuerelemente und Symbole für benutzerdefinierte Dokumenttypen.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 09afcac8dcecd5a1d05961c2981c0940fff00a01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102359"
---
# <a name="images-and-icons-in-xamarinios"></a>Bilder und Symbole in Xamarin.iOS

_Dieser Abschnitt enthält eine Vielzahl von Artikeln, die Arbeit mit Bildern in einer Xamarin.iOS-app, wie z. B. die Verwendung als Symbole behandelt, Startbildschirme oder einschließlich werden in Steuerelemente und Symbole für benutzerdefinierte Dokumenttypen._

Es gibt mehrere Möglichkeiten, dieses Image an, die Objekte in einer iOS-app verwendet werden. Anzeige einfach ein Bild als Teil der Benutzeroberfläche einer app, wie z. B. an ein Benutzeroberflächensteuerelement Zuweisen einer `UIButton` oder `UIImageView`, für die Bereitstellung, Symbole und Startbildschirme, Xamarin.iOS erleichtert Ihnen hervorragende Bildmaterial iOS-Apps auf folgende Weise hinzufügen: 

- **Bilder mit einer Auflösung unabhängige** – verwenden Sie die integrierte Unterstützung für iOS zum Arbeiten mit Bildern in andere displayauflösung und -Typen (iPhone, iPad, usw.).
- **Asset-Katalog Bildzusammenstellungen** -Verwendung **Bildzusammenstellungen für Asset-Katalog** verwalten und gruppieren alle Version an einem bestimmten Image-Objekt, das von einer app erforderlich sind.
- **Bilder im iOS-Designer** -verwenden Sie iOS-Designer, um Bilder für Steuerelemente festzulegen.
- **Bilder in Code** – verwenden der `UIImage` Klasse, Methoden zum Laden von und Arbeiten mit Bildanlagen aus, und weisen sie Sie UI-Steuerelemente in C# Code.
- **Anwendungssymbol** -definieren Sie das app-Symbol, das von jedem iOS-app erforderlich sind. Dies ist das Symbol, das der Benutzer tippen, wird auf der Startseite für iOS zum Starten der app. Darüber hinaus wird dieses Symbol von Game Center, ggf. verwendet.
- **Spotlight-Symbol** -definieren Sie das Symbol der app-Blickpunkt. Wenn der Benutzer den Namen einer App in eine Spotlight-Suche eingibt, wird dieses Symbol angezeigt.
- **Symbol "Einstellungen"** -definieren Sie der app **Einstellungen** Symbol. Wenn der Benutzer gibt die **Einstellungen** app auf ihrem iOS-Gerät, dieses Symbol wird am Ende der Liste der Einstellungen für die app angezeigt. 
- **Startbildschirme** -Startbildschirm der app definieren. Nachdem der Benutzer auf das Symbol "app" tippt, und bevor die erste Ansicht angezeigt wird, wird ein leerer Bildschirm angezeigt. Glücklicherweise enthält die iOS-Unterstützung für ein Bild anstelle der leeren Bildschirm mit einem Storyboard anzeigen. 
- **iTunes Symbol** -Geben Sie ein iTunes-Symbol. Wenn Sie die Ad-hoc-Methode der Bereitstellung einer app (entweder für Unternehmensbenutzer oder beta-Tests auf echten Geräten) verwenden zu können, muss der Entwickler auch ein 512 x 512 und ein 1024 x 1024-Image, das zur Darstellung der app in iTunes verwendet wird.
- **Dokumentieren Sie Symbole** -verwenden Sie ein Image als Symbol für einen beliebigen spezifischen Dokumenttyp, die eine Xamarin.iOS-app unterstützt, oder erstellt.

Es gibt verschiedene Aspekte, die berücksichtigt werden sollten beim Erstellen des Image-Ressourcen für eine iOS-app als auch mehrfach, in denen diese Ressourcen verwendet wird. Jedem dieser Computer eine Auswirkung auf die nicht nur wie viele Bildanlagen benötigt werden, sondern wie diese Objekte erstellt werden. Die folgenden Themen behandelt die Typen von Images-Ressourcen, die benötigt werden, wie diese Objekte in der Anwendung-Paket enthalten sind und wie die Image-Ressourcen verwendet werden, um die erforderliche Funktionalität bereitzustellen:


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

Dieser Artikel behandelt, einschließlich ein Bildobjekt in einer Xamarin.iOS-app und dieses Bild angezeigt, entweder mithilfe von C#-Code oder durch Zuweisen eines Steuerelements in der iOS-Designer.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Anwendungssymbole](~/ios/app-fundamentals/images-icons/app-icons.md)

Dieser Artikel behandelt, einschließlich und Verwalten von ein Bildobjekt in einer Xamarin.iOS-app als App-Symbol verwendet werden soll.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Alternative App-Symbole](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple hat mehrere Erweiterungen auf iOS 10.3 hinzugefügt, mit denen eine app zum Verwalten von das entsprechende Symbols:

 - `ApplicationIconBadgeNumber` -Ruft ab oder legt den Badge, der das app-Symbol in der Springboard-Reihe.
 - `SupportsAlternateIcons` -If `true` die app verfügt über eine Alternative Gruppe von Symbolen.
 - `AlternateIconName` -Gibt den Namen des aktuell ausgewählten alternativen Symbols zurück oder `null` Wenn das primäre Symbol verwenden.
 - `SetAlternameIconName` – Verwenden Sie diese Methode, um das Symbol der app in der angegebenen alternativen Symbol wechseln.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Startbildschirme](~/ios/app-fundamentals/images-icons/launch-screens.md)

Dieser Artikel befasst sich mit der eine besondere Art von Storyboard eine universelle Startbildschirm jedes iOS-GeräteGröße und-Auflösung bereit.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Benutzerdefinierte Dokumenttypen](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Dieser Artikel behandelt, einschließlich und Verwalten von ein Bildobjekt in einer Xamarin.iOS-app als einen benutzerdefinierten Typ Dokumentsymbol verwendet werden soll.



## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefinierte Symbol und Richtlinien für die Erstellung von Images](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
