---
title: 'IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 4ff7b88ad63870246acbf2b29d01775f6711a8ce
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031193"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.

## <a name="fixed-in-xcode-611"></a>Korrigiert in Xcode 6.1.1

Apple hat diesen `ibtool`-Fehler in Xcode 6.1.1 [behoben](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1), sodass ein Upgrade auf Xcode 6.1.1 oder höher die einfachste Möglichkeit zur Fehlerbehebung darstellt.

* * *

## <a name="description-of-the-problem"></a>Beschreibung des Problems

Der Befehl "`ibtool`" in Xcode 6,0 enthielt einen Fehler in OS X 10,10 Yosemite. Xamarin. IOS verwendet `ibtool` von Xcode, um Storyboards und `XIB` Dateien zu kompilieren.

Weitere Informationen über den Fehler im Zusammenhang mit Xcode finden Sie im folgenden Stack Overflow Beitrag: [https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Fehlermeldung

> Das Dokument "mainstoryboard. Storyboard" konnte nicht geöffnet werden. Der Vorgang konnte nicht abgeschlossen werden. (com. Apple. InterfaceBuilder-Fehler-1.)

## <a name="workarounds-for-xcode-60"></a>Problem Umgehungen (für Xcode 6,0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Option 1: Alle `UIImageView.Image` Eigenschaften im Code verwalten

Anstatt die `Image`-Eigenschaft eines `UIImageView` in der Storyboard-oder `.xib`-Datei festzulegen, können Sie die-Eigenschaft in einer der Überschreibungs Methoden zum Anzeigen des Lebenszyklus im Ansichts Controller festlegen (z. b. in `ViewDidLoad()`). Weitere Tipps zur Verwendung von `UIImage.FromBundle()` und `UIImage.FromFile()`finden Sie [unter Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) .

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Option 2: Verschieben Sie alle Bild Ressourcen in den `Resources` Ordner der obersten Ebene.

Nachdem Sie die Bilder in den `Resources` Ordner der obersten Ebene verschoben haben, müssen Sie das Storyboard und `.xib` Dateien aktualisieren, um die neuen Bild Pfade zu verwenden.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Option 3: Legen Sie die `LogicalName` für alle problematischen Bild Ressourcen fest, damit Sie auf die oberste Ebene des`.app` Pakets kopiert werden.

Angenommen, die ursprüngliche `.csproj` Datei enthält den folgenden Eintrag:

`<BundleResource Include="Resources\Images\image.png" />`

Sie können dieses Element ändern und eine `LogicalName` hinzufügen, damit das Bild stattdessen auf die oberste Ebene des `.app` Pakets kopiert wird:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

In Visual Studio für Mac die `LogicalName` auch über das Feld `Resource ID` für das Bild unter **Ansicht > Pads > Eigenschaften**festgelegt werden. (Siehe auch: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Nach dieser Änderung müssen Sie die Storyboard-und `.xib` Dateien aktualisieren, damit die neuen Bild Pfade der obersten Ebene verwendet werden. Visual Studio für Mac wird die Liste der automatischen Vervollständigungen für die `Image`-Eigenschaft im IOS-Designer automatisch aktualisieren. In Visual Studio müssen Sie den Pfad manuell bearbeiten. Der IOS-Designer zeigt dies als fehlendes Bild an, das Projekt wird jedoch ordnungsgemäß erstellt und ausgeführt.

### <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. . 
