---
title: XAML-Vorschau für Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie mit, dass die XAML-Vorschau finden Sie unter Ihrer Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe. Die XAML-Vorschau steht in Visual Studio 2017 und Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: d29186681f79a2f7591978411b5d15fd9f90f807
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563471"
---
# <a name="xaml-previewer-for-xamarinforms"></a>XAML-Vorschau für Xamarin.Forms

_Finden Sie unter Ihrer Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe._

## <a name="requirements"></a>Anforderungen

Projekte erfordern das neueste Xamarin.Forms-NuGet-Paket für die XAML-Vorschau funktioniert. Vorschau von Android-apps erfordert [JDK 1.8 X64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Weitere Informationen in den [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Erste Schritte

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die XAML-Vorschau ist standardmäßig aktiviert und kann gesteuert werden, aus der **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. In diesem Dialogfeld können Sie die Standardansicht für das Dokument und die Ausrichtung der geteilten auswählen.

[![ListView-Steuerelement-Vorschau in Visual Studio](xaml-previewer-images/xamlp-options-vs.png "Forms-Vorschau-Optionen in Visual Studio")](xaml-previewer-images/xamlp-options-vs.png#lightbox "Forms-Vorschau-Optionen in Visual Studio")

Wenn eine XAML-Seite, die der Editor teilt öffnen auf Grundlage der Einstellungen ausgewählt werden, der **Tools > Optionen > Xamarin > Forms-Vorschau** Dialogfeld. Allerdings können diese Einstellungen im Editor-Fenster geändert werden.

## <a name="xaml-preview-controls"></a>XAML-Vorschau-Steuerelemente

Oben im Editor-Fenster verfügt über die Schaltflächen auswählen, welcher Bereich verwendet werden, die oberste Schaltfläche mit dem Wechsel in das Entwurfsfenster, und die Schaltfläche unten, wechseln in den Quellcodebereich ist. Die mittlere Schaltfläche vertauscht die Reihenfolge im Bereich.

[![ListView-Steuerelement-Vorschau in Visual Studio](xaml-previewer-images/xamlp-controls-vs.png "Forms-Vorschau-Bereich-Steuerelemente in Visual Studio")](xaml-previewer-images/xamlp-controls-vs.png#lightbox "Forms-Vorschau-Bereich-Steuerelemente in Visual Studio")

Unten im Editor-Fenster verfügt über die Schaltflächen, die Bereiche, horizontal und vertikal zu teilen und zu erweitern oder reduzieren die aktuellen untergeordneten Bereich.

[![ListView-Steuerelement-Vorschau in Visual Studio](xaml-previewer-images/xamlp-controls2-vs.png "Forms-Vorschau-Bereich-Steuerelemente in Visual Studio")](xaml-previewer-images/xamlp-controls2-vs.png#lightbox "Forms-Vorschau-Bereich-Steuerelemente in Visual Studio")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die **Vorschau** Schaltfläche wird im Editor angezeigt, wenn Sie eine XAML-Seite zu öffnen. Im Vorschaufenster kann ein- oder ausgeblendet wird, drücken Sie die **Vorschau** Schaltfläche in der oberen rechten Ecke von einem beliebigen XAML-Dokument-Fenster:

[![ListView-Steuerelement-Vorschau in Visual Studio für Mac](xaml-previewer-images/xamlp-list-sml.png "Forms-Vorschau in Visual Studio für Mac")](xaml-previewer-images/xamlp-list.png#lightbox "Forms-Vorschau in Visual Studio für Mac")

-----

## <a name="xaml-preview-options"></a>Optionen für die Vorschauversion von XAML

Die Optionen am oberen Rand des Vorschaufensters sind:

* **Phone** : Rendern in einem Bildschirm Phone-Größe
* **Tablet** – in einem Tablet-Size-Bildschirm rendern (Beachten Sie Zoom-Steuerelemente zu unten rechts im Bereich befinden.)
* **Android** – zeigen Sie die Android-Version des Bildschirms
* **iOS** – zeigen Sie die iOS-Version des Bildschirms
* Portait (Symbol) – verwendet Hochformat, für die Vorschau
* Querformat (Symbol) – verwendet Querformat für die Vorschau

## <a name="adding-design-time-data"></a>Hinzufügen von Entwurfszeit-Daten

Einige Layouts möglicherweise schwer vorstellbar ohne Daten an Steuerelemente der Benutzeroberfläche gebunden. Um die Vorschau nützlicher zu machen, weisen Sie einige statischen Daten zu den Steuerelementen durch hartcodierung einen Bindungskontext (entweder im Code-Behind oder mithilfe von XAML).

Finden Sie in der montemagnos [Blogbeitrag zum Hinzufügen von Entwurfszeitdaten](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) zu erfahren, wie Sie an einem statischen ViewModel in XAML zu binden.

## <a name="detecting-design-mode"></a>Erkennen von Entwurfsmodus

Die statische [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) Eigenschaft untersucht werden kann, um zu bestimmen, ob die Anwendung, in der Vorschau ausgeführt wird. Dadurch können Sie Code an, die nur ausgeführt, wenn die Anwendung, in der Vorschau ausgeführt wird:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie die folgenden Probleme und die [Xamarin-Foren](https://forums.xamarin.com/categories/xamarin-forms), wenn Probleme auftreten.

### <a name="xaml-preview-isnt-showing"></a>XAML-Vorschau nicht angezeigt.

Überprüfen Sie folgende Punkte:

* Projekt soll (kompilierte) erstellt werden, bevor Sie versuchen, die XAML-Dateien (Vorschau).
* Der Designer-Agent muss Setup beim ersten Verwenden Sie eine XAML-Datei in der Vorschau anzeigen: eine Statusanzeige erscheint in der Vorschau, zusammen mit der statusmeldungen, bis dieser bereit ist.
* Versuchen Sie es zu schließen und die XAML-Datei erneut zu öffnen.
* Sicherstellen, dass Ihre `App` -Klasse verfügt über einen parameterlosen Konstruktor.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>Ungültige XAML: Android-Projekt muss erstellt werden, damit die Vorschau erstellt werden können

Die XAML-Vorschau erfordert, dass das Projekt vor dem Rendern einer Seite erstellt werden.
Wenn die folgende Fehlermeldung am oberen Rand im Vorschaufenster angezeigt wird, erstellen Sie die Anwendung neu, und versuchen Sie es erneut.

![Fehlermeldung: Projekt muss zuerst erstellt werden](xaml-previewer-images/error-not-built-sml.png "Fehlermeldung angezeigt: das Projekt neu erstellen")
