---
title: Benutzerdefinierte Dokumenteigenschaften-Symbole in Xamarin.iOS
description: Dieser Artikel behandelt, einschließlich und Verwalten von ein Bildobjekt in einer Xamarin.iOS-app als einen benutzerdefinierten Typ Dokumentsymbol verwendet werden soll.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/23/2017
ms.openlocfilehash: 51abd00f9a21b702811bb3897f273deff54f7d01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110484"
---
# <a name="custom-document-icons-in-xamarinios"></a>Benutzerdefinierte Dokumenteigenschaften-Symbole in Xamarin.iOS

_Dieser Artikel behandelt, einschließlich und Verwalten von ein Bildobjekt in einer Xamarin.iOS-app als einen benutzerdefinierten Typ Dokumentsymbol verwendet werden soll._

Wenn eine Xamarin.iOS-app unterstützt, ein bestimmtes Dokument zu laden, kann der Entwickler Symbole, die vom System verwendet wird, wenn es auf diesen Dokumenttyp, z. B. wenn ein Benutzer unten ein Anhang in hält trifft Bereitstellen der *Mail-Anwendung* als hier gezeigt:

 [![](custom-document-types-images/17.png "Ein Beispiel für Dokumentsymbole")](custom-document-types-images/17.png#lightbox)

Entwickler kann die Dokumenttypinformationen hinzufügen, für die app eine Datei im Format Öffnen durch Einschließen der Wörterbucheinträge für kann die `CFBundleTypeName` Zeichenfolge und `LSItemContentTypes` Arrays in der app `Info.plist`. Die Symbole für den Dokumenttyp wechseln Sie der `CFBundleTypeIconFiles` Array. Wenn ein Dokumentsymbol nicht angegeben wird, wird iOS aus dem app-Symbol abgeleitet werden.
Symbole können für verschiedene Größen, optimiert für die verschiedenen Lösungen für Geräte bereitgestellt werden. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

So weisen Sie diese Werte in Visual Studio für Mac verwenden die **Dokumenttypen** unter im Abschnitt der **erweitert** Registerkarte die `Info.plist` -Editor, um den Dokumenttyp hinzufügen und Bildsymbole zuzuweisen. Hier ist z. B. einen Screenshot der Registrierung für PDF-Unterstützung:

 [![](custom-document-types-images/18.png "Abschnitt \"Dokumenttypen\" unter der Registerkarte \"Erweitert\" auf \"Info.plist\"-editor")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um diese Werte in Visual Studio zuzuweisen, verwenden die **Dokumenttypen** unter im Abschnitt der **erweitert** Registerkarte die `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Öffnen Sie den Abschnitt \"Dokumente\" unter der Registerkarte \"Erweitert\"")

Klicken Sie auf die **Dokumenttyp hinzufügen** Schaltfläche, und füllen Sie die erforderlichen Felder:

![](custom-document-types-images/doc02w.png "Das Formular Dokumenttyp hinzufügen")

-----


Weitere Informationen zu Typen von Dokumenten, finden Sie in der Apple [Uniform Typverweis Bezeichner](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) und [Interaktion Programmierung Dokumentthemen für iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefinierte Symbol und Richtlinien für die Erstellung von Images](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
