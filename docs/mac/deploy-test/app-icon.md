---
title: Anwendungssymbol für Xamarin.Mac-Apps
description: In diesem Artikel wird beschrieben, wie Sie die für das Anwendungssymbol einer Xamarin.Mac-Anwendung erforderlichen Bilder erstellen, wie Sie diese Bilder in eine ICNS-Datei bündeln, und wie Sie das Symbol zu dem Xamarin.Mac-Projekt hinzufügen.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 685a29eea4b03361b185e25ae0e146be7b5e69b6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792033"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Anwendungssymbol für Xamarin.Mac-Apps

_In diesem Artikel wird beschrieben, wie Sie die für das Anwendungssymbol einer Xamarin.Mac-Anwendung erforderlichen Bilder erstellen, wie Sie diese Bilder in eine ICNS-Datei bündeln, und wie Sie das Symbol zu dem Xamarin.Mac-Projekt hinzufügen._


## <a name="overview"></a>Übersicht

Ein Entwickler, der mit C# und .NET in einer Xamarin.Mac-Anwendung arbeitet, hat Zugang zu denselben Bild- und Symboltools wie ein Entwickler, der in *Objective-C* und *Xcode* arbeitet.

Ein aussagekräftiges Symbol sollte den Hauptzweck der Xamarin.Mac-App darstellen und auf Funktionen hindeuten, die ein Benutzer von der Anwendung erwarten kann. In diesem Artikel wird beschrieben, wie Sie Bildanlagen erstellen, die sie für ein Anwendungssymbol benötigen, wie Sie diese Bildanlagen in eine `AppIcons.appiconset`-Datei bündeln und wie diese Datei anschließend in einer Xamarin.Mac-App verarbeitet wird.

![Der AppIcons.appiconset-Editor](app-icon-images/intro01.png "The AppIcons.appiconset editor")


## <a name="application-icon"></a>Anwendungssymbol

Ein aussagekräftiges Symbol sollte den Hauptzweck einer Xamarin.Mac-App darstellen und auf Funktionen hindeuten, die ein Benutzer von der Anwendung erwarten kann. Für jede macOS-App muss das Symbol in mehreren Größen verfügbar sein, in denen es in der Suchleiste, im Dock, im Launchpad und an anderen Stellen auf dem Computer angezeigt wird.


## <a name="designing-the-icon"></a>Entwerfen des Symbols

Apple gibt folgende Tipps zum Entwerfen von Anwendungssymbolen:

- Geben Sie dem Symbol eine realistische und einzigartige Form.
- Wenn es für die macOS-App auch eine iOS-Version gibt, sollten Sie nicht das Symbol der iOS-Anwendung wiederverwenden.
- Verwenden Sie universelle Bilder mit hohem Wiedererkennungswert.
- Gestalten Sie das Symbol möglichst einfach.
- Setzen Sie nur wenige Farben und Schattierungen ein, um mit dem Symbol die Geschichte der App zu erzählen.
- Mischen Sie den eigentlichen Text nicht mit _griechischem_ Text oder Zeilen, in denen auf Text hingewiesen wird.
- Erstellen Sie besser eine idealisierte Version des Symbolmotivs als ein Foto zu verwenden.
- Verwenden Sie keine Benutzeroberflächenelemente von macOS in den Symbolen.
- Verwenden Sie in Ihren Symbolen keine Replikate von Apple-Symbolen.

Lesen Sie die Abschnitte [App Icon Gallery (Galerie der Anwendungssymbole)](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) und [Designing App Icons (Entwerfen von Anwendungssymbolen)](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) in den [OS X Human Interface Guidelines (Eingaberichtlinien für OS X)](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) von Apple, bevor Sie ein Anwendungssymbol für eine Xamarin.Mac-App entwerfen.


## <a name="required-image-sizes-and-filenames"></a>Erforderliche Bildgrößen und Dateinamen

Wie für jede andere Bildressource, die der Entwickler in einer Xamarin.Mac-App verwendet, muss auch für das App-Symbol sowohl eine Version in Standardauflösung als auch eine in Retina-Auflösung vorhanden sein. Verwenden Sie, ebenfalls wie bei jedem anderen Bild auch, ein `@2x`-Format für die Benennung von Symboldateien:

- **Standardauflösung**  - _Bildname_**.**_Dateinamenerweiterung_ (Beispiel: **icon_512x512.png**)
- **Hohe Auflösung**  - _Bildname_**@2x.**_Dateinamenerweiterung_ (Beispiel: **icon_512x512@2x.png**)

Um beispielsweise eine 512 × 512-Version des App-Symbols zu erstellen, sollten die Dateien wie folgt benannt werden: **icon_512x512.png** und **icon_512x512@2x.png**.

Um sicherzustellen, dass das Symbol überall, wo es der Benutzer sehen kann, gut aussieht, sollten Sie Ressourcen in den folgenden Größen zur Verfügung stellen:

|Dateiname|Größe in Pixeln|
|---|---|
|icon_512x512@2x.png|1024 x 1024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 x 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16|

Weitere Informationen finden Sie unter [Provide High-Resolution Versions of All App Graphics Resources (Erstellen von Versionen in hoher Auflösung für alle Grafikressourcen einer App)](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) in der Apple-Dokumentation.


## <a name="packaging-the-icon-resources"></a>Packen der Symbolressourcen

Nachdem Sie das Symbol entworfen und mit den erforderlichen Dateigrößen und -namen gespeichert haben, können Sie die Dateien mithilfe von Visual Studio für Mac ganz einfach den Bildressourcen zur Verwendung mit Xamarin.Mac zuweisen.

Führen Sie folgende Schritte aus:

1. Öffnen Sie **Assets.xcassets** > **AppIcons.appiconset** im **Lösungspad**: 

    ![Bearbeiten von AppIcons.appiconset](app-icon-images/intro01.png "Editing the AppIcons.appiconset")
2. Klicken Sie für jede benötigte Symbolgröße auf das Symbol und wählen Sie die dazugehörige Bilddatei aus, die Sie zuvor erstellt haben: 

    [![Auswählen eines Symbolbilds](app-icon-images/intro02.png "Selecting an icon image")](app-icon-images/intro02-large.png#lightbox)
3. Speichern Sie die Änderungen.


## <a name="using-the-icon"></a>Verwenden des Symbols

Sobald die `AppIcons.appiconset`-Datei erstellt wurde, muss sie dem Xamarin.Mac-Projekt in Visual Studio für Mac zugewiesen werden.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf **Info.plist** im **Projektmappen-Explorer**, um die **Projektoptionen** zu öffnen.
2. Klicken Sie im Abschnitt **Mac OS X Application Target (Anwendungsziel für Mac OS X Application Target)** auf **App-Symbole**, um die `AppIcons.appiconset`-Datei auszuwählen: 

    [![Festlegen der Symbole](app-icon-images/icon01.png "Setting the icon set")](app-icon-images/icon01-large.png#lightbox)
3. Speichern Sie die Änderungen.

Wenn die App ausgeführt wird, wird das neue Symbol im Dock angezeigt:

![Beispiel eines App-Symbols im macOS-Dock](app-icon-images/icon04.png "An example of an app icon in the macOS dock")


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit zum Erstellen von App-Symbolen für macOS benötigten Bildern und das Komprimieren von Symbolen, einschließlich Symbolen in einem Xamarin.Mac-Projekt, detailliert beschrieben.


## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Working with Images (Arbeiten mit Bildern)](~/mac/app-fundamentals/image.md)
- [Richtlinien für die macOS-Eingabe: Symbole und Bilder](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [Informationen über hohe Auslösung für OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Symbolbuilder](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
