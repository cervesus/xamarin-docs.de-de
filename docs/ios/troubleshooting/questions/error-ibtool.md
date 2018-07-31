---
title: 'IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 593706de23d370d6bff03c97bb50e064d77a52ef
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350731"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.

## <a name="fixed-in-xcode-611"></a>In Xcode 6.1.1 behoben

Apple [festen](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) dies `ibtool` Fehler in Xcode 6.1.1, also mit Xcode 6.1.1 aktualisieren oder höher ist die einfachste Lösung.

* * *

## <a name="description-of-the-problem"></a>Beschreibung des Problems

Die `ibtool` Befehl in Xcode 6.0 wiesen einen Fehler unter OS X 10.10 Yosemite. Xamarin.iOS verwendet von Xcode `ibtool` um Storyboards zu kompilieren und `XIB` Dateien.

Weitere Informationen zu diesem Fehler in Bezug auf Xcode finden Sie auf die folgenden Stack Overflow-Beitrag: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Fehlermeldung

> Das Dokument "MainStoryboard.storyboard" konnte nicht geöffnet werden. Der Vorgang konnte nicht abgeschlossen werden. (com.apple.InterfaceBuilder Fehler – 1).

## <a name="workarounds-for-xcode-60"></a>Problemumgehungen (für Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Option 1: Verwalten Sie alle `UIImageView.Image` Eigenschaften im Code

Statt die `Image` Eigenschaft eine `UIImageView` im Storyboard oder `.xib` -Datei, Sie können die Eigenschaft festlegen in der Ansicht einen Lebenszyklus überschreiben Sie Methoden in der ansichtscontroller (z. B. in `ViewDidLoad()`). Siehe auch [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) finden Sie Tipps zur Verwendung von `UIImage.FromBundle()` im Vergleich zu `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Option 2: Verschieben Sie alle die Image-Ressourcen der obersten Ebene `Resources` Ordner

Nach dem Verschieben die Bilder auf der obersten Ebene `Resources` Ordner, Sie müssen das Storyboard zu aktualisieren und `.xib` Dateien zur Verwendung der neuen Pfade zu Bildern.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Option 3: Festlegen der `LogicalName` für alle problematisch Bildanlagen, damit sie in der obersten Ebene der kopiert werden die`.app` Bundle

Angenommen, Ihre ursprüngliche `.csproj` -Datei enthält den folgenden Eintrag:

`<BundleResource Include="Resources\Images\image.png" />`

Sie können dieses Element zu ändern und Hinzufügen einer `LogicalName` , damit das Image stattdessen auf der obersten Ebene der kopiert werden sollen die `.app `Bundle:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

In Visual Studio für Mac die `LogicalName` kann auch mithilfe von festgelegt werden die `Resource ID` Feld für das Image unter **Ansicht > Pads > Eigenschaften**. (Siehe auch: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Nach dieser Änderung müssen Sie das Storyboard zu aktualisieren und `.xib` Dateien zur Verwendung der neuen Pfade der obersten Ebene Image. Visual Studio für Mac aktualisiert automatisch die Liste der Autocompletions für die `Image` -Eigenschaft in der iOS-Designer. In Visual Studio müssen Sie den Pfad manuell zu bearbeiten. Der iOS-Designer zeigt diese dann als ein Bild fehlt, aber das Projekt erstellt und ordnungsgemäß ausgeführt werden.

### <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf. 

