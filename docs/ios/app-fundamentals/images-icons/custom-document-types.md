---
title: Benutzerdefinierte Dokument Symbole in xamarin. IOS
description: In diesem Artikel wird das einschließen und Verwalten eines Image Assets in einer xamarin. IOS-App behandelt, die als benutzerdefiniertes Dokumenttyp Symbol verwendet werden soll.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/23/2017
ms.openlocfilehash: 25b4e5a564c8dabf4cb44881c25e0a10ade47350
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767743"
---
# <a name="custom-document-icons-in-xamarinios"></a>Benutzerdefinierte Dokument Symbole in xamarin. IOS

_In diesem Artikel wird das einschließen und Verwalten eines Image Assets in einer xamarin. IOS-App behandelt, die als benutzerdefiniertes Dokumenttyp Symbol verwendet werden soll._

Wenn eine xamarin. IOS-APP das Laden eines bestimmten Dokument Typs unterstützt, kann der Entwickler beim Auffinden dieses Dokument Typs Symbole bereitstellen, die vom System verwendet werden, z. b. Wenn ein Benutzer eine Anlage in der e- *Mail-Anwendung* ablegt, wie hier gezeigt:

 [![](custom-document-types-images/17.png "Ein Beispiel für Dokumenttyp Symbole")](custom-document-types-images/17.png#lightbox)

Der Entwickler kann Dokumenttyp Informationen für ein Dateiformat hinzufügen, das die APP öffnen kann, indem Sie Wörterbuch `CFBundleTypeName` Einträge für `LSItemContentTypes` die Zeichenfolge und das `Info.plist`Array in der der APP einschließt. Die Symbole für den Dokumenttyp werden im `CFBundleTypeIconFiles` Array angezeigt. Wenn ein Dokument Symbol nicht bereitgestellt wird, leitet IOS eines von dem App-Symbol ab.
Symbole können für verschiedene Größen bereitgestellt werden, die für die verschiedenen Geräte Auflösungen optimiert sind. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um diese Werte in Visual Studio für Mac zuzuweisen, verwenden Sie den Abschnitt **Dokumenttypen** auf der `Info.plist` Registerkarte Erweitert im Editor, um den Dokumenttyp hinzuzufügen und Bildsymbole zuzuweisen. Hier sehen Sie beispielsweise einen Screenshot, der die Registrierung für die PDF-Unterstützung anzeigt:

 [![](custom-document-types-images/18.png "Der Abschnitt \"Dokumenttypen\" auf der Registerkarte \"Erweitert\" im Editor \"Info. plist\"")](custom-document-types-images/18.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um diese Werte in Visual Studio zuzuweisen, verwenden Sie den Abschnitt **Dokumenttypen** auf der Registerkarte Erweitert `Info.plist`auf der Registerkarte Erweitert:

 ![](custom-document-types-images/doc01w.png "Dokumenttypen Abschnitt auf der Registerkarte \"Erweitert\" öffnen")

Klicken Sie auf die Schaltfläche **Dokumenttyp hinzufügen** , und füllen Sie die erforderlichen Felder aus:

![](custom-document-types-images/doc02w.png "Das Formular \"Dokumenttyp hinzufügen\"")

-----

Weitere Informationen zu Dokumenttypen finden Sie unter Referenz zu den [einheitlichen Typbezeichner](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) von Apple und in den [Programmierthemen zur Dokument Interaktion für IOS](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Richtlinien für benutzerdefiniertes Symbol und Bild Erstellung](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
