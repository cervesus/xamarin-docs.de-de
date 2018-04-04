---
title: Bildern und Symbolen
description: Dieser Abschnitt enthält eine Vielzahl von Artikeln, die Arbeiten mit Bildern in einem Xamarin.iOS-app, wie beispielsweise verwenden sie als Symbole abdecken, starten Sie Bildschirme oder einschließlich sie Steuerelemente und Bereitstellen von Symbolen für benutzerdefinierte Dokumenttypen.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd191c898d5bb015d2d394d42db1049bb0128fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="images-and-icons"></a>Bildern und Symbolen

_Dieser Abschnitt enthält eine Vielzahl von Artikeln, die Arbeiten mit Bildern in einem Xamarin.iOS-app, wie beispielsweise verwenden sie als Symbole abdecken, starten Sie Bildschirme oder einschließlich sie Steuerelemente und Bereitstellen von Symbolen für benutzerdefinierte Dokumenttypen._

Es gibt mehrere Möglichkeiten, Bild, das Ressourcen innerhalb einer iOS-app verwendet werden. Aus einfach Anzeigen eines Bilds als Teil einer app-Benutzeroberfläche, z. B. an ein Benutzeroberflächensteuerelement Zuweisen einer `UIButton` oder `UIImageView`, versichern Sie Symbole und Startbildschirme, Xamarin.iOS erleichtert hervorragende Bildmaterial einer iOS-app auf folgende Weise hinzufügen: 

- **Unabhängige Bilder Auflösung** – integrierte des iOS-Unterstützung zum Arbeiten mit Bildern in anderen Gerät Auflösungen und Typen (iPhone, iPad, usw.) verwenden.
- **Asset-Katalog-Image-Sätze** -Verwendung **Asset-Katalog-Image-Sätze** verwalten und gruppieren alle Version von einem bestimmten imagemedienobjekt von einer app erforderlich.
- **Bilder in der iOS-Designer** -verwenden Sie die iOS-Designer Bilder für Steuerelemente festlegen.
- **Bilder in Code** – verwenden der `UIImage` Klassenmethoden laden und Arbeiten mit Bildanlagen und weisen sie Sie UI-Steuerelemente in C#-Code.
- **Symbol "Anwendung"** -definieren Sie das app-Symbol, das alle iOS-app benötigt. Dies ist das Symbol, das der Benutzer tippen sollen auf der iOS-Startseite auf die app zu starten. Darüber hinaus wird dieses Symbol von Game Center "," ggf. verwendet.
- **Symbol "Spotlight** -definieren Sie die app Spotlight-Symbol. Wenn der Benutzer den Namen einer App in einem Spotlight-Suche eingibt, wird dieses Symbol angezeigt.
- **Symbol "Einstellungen"** -definieren Sie der app **Einstellungen** Symbol. Wenn der Benutzer gibt die **Einstellungen** app auf seinem iOS-Gerät, dieses Symbol wird am Ende der Liste der Einstellungen für die app angezeigt werden. 
- **Starten Sie Bildschirme** -Startbildschirm der app zu definieren. Nach dem Tippen auf des Symbol "app", und bevor die erste Ansicht angezeigt wird, wird ein leerer Bildschirm angezeigt werden. Glücklicherweise umfasst iOS-Unterstützung für ein Bild anstelle der leeren Bildschirm mithilfe eines Drehbuchs anzeigen. 
- **iTunes Symbol** -Geben Sie ein iTune-Symbol. Wenn Sie die Ad-hoc-Methode der Lieferung einer app (entweder für Benutzer im Unternehmen oder Betatests auf echten Geräten) zu verwenden, muss der Entwickler auch umfassen eine 512 x 512 und ein 1024 x 1024-Image, das verwendet wird, um die app im iTunes darzustellen.
- **Dokumentieren Sie Symbole** -verwenden Sie ein Image als Symbol für jeden bestimmten Dokumenttyp, die eine Xamarin.iOS-app unterstützt, oder erstellt.

Es gibt verschiedene Aspekte, die berücksichtigt werden sollten beim Erstellen von Bildanlagen für ein iOS-app als auch an mehreren Stellen, in denen diese Ressourcen verwendet wird. Jede dieser haben eine Auswirkung auf die nicht nur wie viele Bildanlagen benötigt werden, sondern wie diese Objekte erstellt werden. Die folgenden Themen behandeln die Typen von Bildern-Ressourcen, die benötigt werden, wie diese Objekte in der Anwendung-Paket enthalten sind und wie die Bildanlagen genutzt werden, um die erforderliche Funktionalität bereitzustellen:


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

Dieser Artikel umfasst z. B. ein Standardimage-Medienobjekt in einem Xamarin.iOS-app und sich das Bild anzeigen, mithilfe von C#-Code oder indem Sie ein Steuerelement in der iOS-Designer zuweisen.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Anwendungssymbole](~/ios/app-fundamentals/images-icons/app-icons.md)

Dieser Artikel behandelt die einschließlich und verwalten ein Standardimage-Medienobjekt in einem Xamarin.iOS-app als ein Symbol "App" verwendet werden soll.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Alternative App-Symbole](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple hat mehrere Erweiterungen für iOS-10.3 hinzugefügt, mit denen eine app, deren Symbol verwalten können:

 - `ApplicationIconBadgeNumber` -Ruft ab oder legt das Signal für das Symbol "app" in der Springboard fest.
 - `SupportsAlternateIcons` -If `true` die app hat eine Alternative Gruppe von Symbolen.
 - `AlternateIconName` -Gibt den Namen des aktuell ausgewählten alternativen Symbols oder `null` Wenn das Symbol "primary" verwenden.
 - `SetAlternameIconName` -Verwenden Sie diese Methode, um das Symbol für die app des angegebenen alternativen Symbols zu wechseln.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Startbildschirme](~/ios/app-fundamentals/images-icons/launch-screens.md)

Dieser Artikel umfasst, verwenden eine besondere Art von Storyboard alle iOS-GeräteGröße und-Auflösung einen universellen starten Bildschirm bereit.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Benutzerdefinierte Dokumenttypen](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Dieser Artikel behandelt die einschließlich und verwalten ein Standardimage-Medienobjekt in einem Xamarin.iOS-app als einen benutzerdefinierten Typ Dokumentsymbol verwendet werden soll.



## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefiniertes Symbol und Richtlinien für die Erstellung von Images](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
