---
title: Benutzerdefinierte Dokument Symbole in xamarin. IOS
description: In diesem Artikel wird das einschließen und Verwalten eines Image Assets in einer xamarin. IOS-App behandelt, die als benutzerdefiniertes Dokumenttyp Symbol verwendet werden soll.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/23/2017
ms.openlocfilehash: 09fc582182729d3d8e17b85ac0a3ecc4bdcfce7e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939814"
---
# <a name="custom-document-icons-in-xamarinios"></a>Benutzerdefinierte Dokument Symbole in xamarin. IOS

_In diesem Artikel wird das einschließen und Verwalten eines Image Assets in einer xamarin. IOS-App behandelt, die als benutzerdefiniertes Dokumenttyp Symbol verwendet werden soll._

Wenn eine xamarin. IOS-APP das Laden eines bestimmten Dokument Typs unterstützt, kann der Entwickler beim Auffinden dieses Dokument Typs Symbole bereitstellen, die vom System verwendet werden, z. b. Wenn ein Benutzer eine Anlage in der e- *Mail-Anwendung* ablegt, wie hier gezeigt:

 [![Ein Beispiel für Dokumenttyp Symbole](custom-document-types-images/17.png)](custom-document-types-images/17.png#lightbox)

Der Entwickler kann Dokumenttyp Informationen für ein Dateiformat hinzufügen, das die APP öffnen kann, indem Sie Wörterbucheinträge für die `CFBundleTypeName` Zeichenfolge und das `LSItemContentTypes` Array in der der APP einschließt `Info.plist` . Die Symbole für den Dokumenttyp werden im `CFBundleTypeIconFiles` Array angezeigt. Wenn ein Dokument Symbol nicht bereitgestellt wird, leitet IOS eines von dem App-Symbol ab.
Symbole können für verschiedene Größen bereitgestellt werden, die für die verschiedenen Geräte Auflösungen optimiert sind. 

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Um diese Werte in Visual Studio für Mac zuzuweisen, verwenden Sie den Abschnitt **Dokumenttypen** auf der Registerkarte **erweitert** im `Info.plist` Editor, um den Dokumenttyp hinzuzufügen und Bildsymbole zuzuweisen. Hier sehen Sie beispielsweise einen Screenshot, der die Registrierung für die PDF-Unterstützung anzeigt:

 [![Der Abschnitt "Dokumenttypen" auf der Registerkarte "Erweitert" im Editor "Info. plist"](custom-document-types-images/18.png)](custom-document-types-images/18.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Um diese Werte in Visual Studio zuzuweisen, verwenden Sie den Abschnitt **Dokumenttypen** auf der Registerkarte Erweitert auf der Registerkarte **erweitert** `Info.plist` :

 ![Dokumenttypen Abschnitt auf der Registerkarte "Erweitert" öffnen](custom-document-types-images/doc01w.png)

Klicken Sie auf die Schaltfläche **Dokumenttyp hinzufügen** , und füllen Sie die erforderlichen Felder aus:

![Das Formular "Dokumenttyp hinzufügen"](custom-document-types-images/doc02w.png)

-----

Weitere Informationen zu Dokumenttypen finden Sie unter Referenz zu den [einheitlichen Typbezeichner](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) von Apple und in den [Programmierthemen zur Dokument Interaktion für IOS](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Richtlinien für benutzerdefiniertes Symbol und Bild Erstellung](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
