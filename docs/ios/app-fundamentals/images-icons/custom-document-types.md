---
title: Benutzerdefinierte Dokumenteigenschaften Symbole
description: Dieser Artikel behandelt die einschließlich und verwalten ein Standardimage-Medienobjekt in einem Xamarin.iOS-app als einen benutzerdefinierten Typ Dokumentsymbol verwendet werden soll.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: b369667bd728f7c8b6e8bcfed9cf5bca2916bf69
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="custom-document-icons"></a>Benutzerdefinierte Dokumenteigenschaften Symbole

_Dieser Artikel behandelt die einschließlich und verwalten ein Standardimage-Medienobjekt in einem Xamarin.iOS-app als einen benutzerdefinierten Typ Dokumentsymbol verwendet werden soll._

Wenn eine app Xamarin.iOS einen bestimmten Dokumenttyp zu laden unterstützt, kann der Entwickler Symbole, die vom System verwendet wird, stößt Dokument Typs, z. B. wenn ein Benutzer nach unten eine Anlage enthält Bereitstellen der *Mail-Anwendung* als hier gezeigt:

 [![](custom-document-types-images/17.png "Ein Beispiel der Dokument-Typ-Symbole")](custom-document-types-images/17.png#lightbox)

Entwickler kann die Dokumenttypinformationen hinzufügen, für die app eine Datei im Format Öffnen mit Wörterbucheinträge für kann die `CFBundleTypeName` Zeichenfolge und `LSItemContentTypes` Array in der app `Info.plist`. Rufen Sie die Symbole für den Dokumenttyp in der `CFBundleTypeIconFiles` Array. Wenn ein Symbol "Dokument" nicht bereitgestellt wird, werden iOS aus dem Symbol "app" abgeleitet.
Symbole können für verschiedene Größen, optimiert für die verschiedenen Lösungen für Geräte bereitgestellt werden. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Verwenden Sie diese Werte in Visual Studio für Mac zuzuweisen der **Dokumenttypen** Handlerbereich unter dem der **erweitert** Registerkarte die `Info.plist` -Editor, um den Dokumenttyp hinzufügen und Bildsymbole zuweisen. Hier ist z. B. einen Screenshot der Registrierung für PDF-Unterstützung:

 [![](custom-document-types-images/18.png "Abschnitt Dokumenttypen unter der Registerkarte "Erweitert" im Editor "Info.plist"")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Verwenden, um diese Werte in Visual Studio zum Zuweisen der **Dokumenttypen** Handlerbereich unter dem der **erweitert** Registerkarte die `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Öffnen Sie den Abschnitt Dokumenttypen unter der Registerkarte "Erweitert"")

Klicken Sie auf die **Dokumenttyp hinzufügen** Schaltfläche aus, und geben Sie die erforderlichen Felder:

![](custom-document-types-images/doc02w.png "Das Formular Dokumenttyp hinzufügen")

-----


Weitere Informationen zu Standarddokumenttypen finden Sie in der Apple [Uniform Typverweis Bezeichner](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) und [Dokument Interaktion Programmierung Themen für iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefiniertes Symbol und Richtlinien für die Erstellung von Images](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
