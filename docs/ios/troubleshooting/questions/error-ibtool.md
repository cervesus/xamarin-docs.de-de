---
title: 'IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4647227ad208bfa968f8282a966220a09ab7f4a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.

## <a name="fixed-in-xcode-611"></a>In Xcode 6.1.1 behoben wurden

Apple [fester](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) dies `ibtool` Bug in Xcode 6.1.1, also Upgrade to Xcode 6.1.1 oder höher ist die einfachste Lösung.

* * *

## <a name="description-of-the-problem"></a>Beschreibung des Problems

Die `ibtool` Befehl in Xcode 6.0 ist einen Fehler auf OS X 10.10 Yosemite hatte. Xamarin.iOS verwendet die Xcode `ibtool` Storyboards zu kompilieren und `XIB` Dateien.

Weitere Informationen über den Fehler in Bezug auf Xcode befinden sich auf den folgenden Post Stack Overflow: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Fehlermeldung

> Das Dokument "MainStoryboard.storyboard" konnte nicht geöffnet werden. Der Vorgang konnte nicht abgeschlossen werden. (com.apple.InterfaceBuilder Fehler – 1).

## <a name="workarounds-for-xcode-60"></a>Problemumgehungen (für Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Option 1: Verwalten Sie alle `UIImageView.Image` Eigenschaften im Code

Statt die `Image` Eigenschaft eine `UIImageView` in das Storyboard oder `.xib` Datei einstellbaren die Eigenschaft in einem der Sicht Lebenszyklus Überschreibungsmethoden im Controller anzeigen (z. B. in `ViewDidLoad()`). Siehe auch [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Tipps zur Verwendung von `UIImage.FromBundle()` im Vergleich zu `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Option 2: Verschieben aller Bildressourcen auf der obersten Ebene `Resources` Ordner

Verschieben Sie die Bilder auf der obersten Ebene `Resources` Ordner, müssen Sie das Storyboard aktualisieren und `.xib` Dateien auf den neuen Image verwenden.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Option 3: Festlegen der `LogicalName` für alle problematisch Bildanlagen, damit sie in der obersten Ebene der kopiert werden die`.app` Paket

Angenommen, Ihre ursprüngliche `.csproj` -Datei enthält den folgenden Eintrag:

`<BundleResource Include="Resources\Images\image.png" />`

Sie können dieses Element zu ändern und hinzufügen eine `LogicalName` , damit das Bild auf der obersten Ebene der stattdessen kopiert werden die `.app `Bundle:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

In Visual Studio für Mac der `LogicalName` kann auch mit eingerichtet werden die `Resource ID` Feld für das Bild unter **Ansicht > füllt > Eigenschaften**. (Siehe auch: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Nach dieser Änderung müssen Sie das Storyboard aktualisieren und `.xib` Dateien an den neuen übergeordneten Image verwenden. Visual Studio für Mac aktualisiert automatisch die Liste der Autocompletions für die `Image` Eigenschaft in der iOS-Designer. In Visual Studio müssen Sie den Pfad von hand bearbeiten. Die iOS-Designer zeigt diese dann als ein fehlendes Bild, aber das Projekt erstellt und ordnungsgemäß ausgeführt.

### <a name="next-steps"></a>Nächste Schritte

Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf. 

