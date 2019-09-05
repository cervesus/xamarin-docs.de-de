---
title: 'IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 695410937e82b885e31827d02f5fefd138497523
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290274"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.

## <a name="fixed-in-xcode-611"></a>Korrigiert in Xcode 6.1.1

Apple hat diesen `ibtool`-Fehler in Xcode 6.1.1 [behoben](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1), sodass ein Upgrade auf Xcode 6.1.1 oder höher die einfachste Möglichkeit zur Fehlerbehebung darstellt.

* * *

## <a name="description-of-the-problem"></a>Beschreibung des Problems

Der `ibtool` Befehl in Xcode 6,0 enthielt einen Fehler auf OS X 10,10 Yosemite. Xamarin. IOS verwendet Xcode zum `ibtool` Kompilieren von Storyboards und `XIB` Dateien.

Weitere Informationen über den Fehler im Zusammenhang mit Xcode finden Sie im folgenden Stack Overflow Beitrag:[https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Fehlermeldung

> Das Dokument "mainstoryboard. Storyboard" konnte nicht geöffnet werden. Der Vorgang konnte nicht abgeschlossen werden. (com. Apple. InterfaceBuilder-Fehler-1.)

## <a name="workarounds-for-xcode-60"></a>Problem Umgehungen (für Xcode 6,0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Option 1: Alle `UIImageView.Image` Eigenschaften im Code verwalten

Anstatt die `Image` -Eigenschaft `UIImageView` eines in der Storyboard-oder `.xib` -Datei festzulegen, können Sie die-Eigenschaft in einer der Überschreibungs Methoden zum Anzeigen des Lebenszyklus im Ansichts Controller ( `ViewDidLoad()`z. b. in) festlegen. Weitere Tipps zur Verwendung von vs `UIImage.FromFile()`finden Sie `UIImage.FromBundle()` [unter Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) .

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Option 2: Alle Bild Ressourcen in den Ordner der obersten Ebene `Resources` verschieben

Nachdem Sie die Bilder in den Ordner der obersten `Resources` Ebene verschoben haben, müssen Sie das Storyboard und `.xib` die Dateien aktualisieren, damit die neuen Bild Pfade verwendet werden.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Option 3: Legen Sie`.app` für alle problematischen Bild Ressourcen fest, damit Sie auf die oberste Ebene des Bündels kopiert werden. `LogicalName`

Angenommen, die ursprüngliche `.csproj` Datei enthält den folgenden Eintrag:

`<BundleResource Include="Resources\Images\image.png" />`

Sie können dieses Element ändern und eine `LogicalName` hinzufügen, sodass das Bild stattdessen auf die oberste Ebene `.app` des Pakets kopiert wird:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

In Visual Studio für Mac `LogicalName` kann auch mithilfe des `Resource ID` -Felds für das Bild unter **Ansicht > Pads > Eigenschaften**festgelegt werden. (Siehe auch: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Nach dieser Änderung müssen Sie das Storyboard und `.xib` die Dateien aktualisieren, um die neuen Bild Pfade der obersten Ebene zu verwenden. Visual Studio für Mac wird die Liste der automatischen Vervollständigungen für die `Image` Eigenschaft im IOS-Designer automatisch aktualisieren. In Visual Studio müssen Sie den Pfad manuell bearbeiten. Der IOS-Designer zeigt dies als fehlendes Bild an, das Projekt wird jedoch ordnungsgemäß erstellt und ausgeführt.

### <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. . 

