---
title: XAML-Vorschau für Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie mit, dass die XAML-Vorschau finden Sie unter Ihrer Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe. Die XAML-Vorschau steht in Visual Studio 2017, Visual Studio für Mac und Visual Studio 2019 (Vorschau) zur Verfügung.
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: 7834b87687db8fd1a3d51a40a4657b24e46bac17
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670401"
---
# <a name="xaml-previewer-for-xamarinforms"></a>XAML-Vorschau für Xamarin.Forms

_Finden Sie unter Ihrer Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe._

## <a name="requirements"></a>Anforderungen

Projekte erfordern das neueste Xamarin.Forms-NuGet-Paket für die XAML-Vorschau funktioniert. Vorschau von Android-apps erfordert [JDK 1.8 X64](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Weitere Informationen in den [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Erste Schritte

Die Xamarin.Forms-XAML-Vorschau zeigt eine plattformspezifische Vorschau Ihrer XAML-Datei in Echtzeit, während Sie sie bearbeiten.

::: zone pivot="windows"

### <a name="visual-studio"></a>Visual Studio

Durch Erweitern der geteilten Ansicht Bereichs ist die XAML-Vorschau verfügbar. Standardparameter zum Teilen von View-Verhalten kann gesteuert werden, aus der **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. In diesem Dialogfeld können Sie die Standardansicht für das Dokument und die Ausrichtung der geteilten auswählen.

[![Xamarin.Forms-Vorschau "Optionen" in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "Xamarin.Forms-Vorschau \"Optionen\" in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Wenn Sie eine XAML-Seite zu öffnen, wird der Editor basierend auf den Einstellungen, die im ausgewählten Teilen der **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. Die Aufteilung kann jedoch für jede Datei im Editor-Fenster geändert werden.

#### <a name="xaml-preview-controls"></a>XAML-Vorschau-Steuerelemente

Oben im Editor-Fenster verfügt über die Schaltflächen auswählen, welcher Bereich verwendet werden, die oberste Schaltfläche mit dem Wechsel in das Entwurfsfenster, und die Schaltfläche unten, wechseln in den Quellcodebereich ist. Die mittlere Schaltfläche vertauscht die Reihenfolge im Bereich.

[![Xamarin.Forms-Vorschau zum Wechseln zwischen den Entwurf, Quelle und geteilten Ansicht im Visual Studio steuert](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms-Vorschau-Steuerelemente zum Wechseln zwischen den Entwurf, Quelle und geteilten Ansicht im Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Unten im Editor-Fenster verfügt über die Schaltflächen, die Bereiche, horizontal und vertikal zu teilen und zu erweitern oder reduzieren die aktuellen untergeordneten Bereich.

[![Xamarin.Forms-Vorschau Bereich Ausrichtung Steuerelemente in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Xamarin.Forms-Vorschau Bereich Ausrichtung Steuerelemente in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Die **Vorschau** Schaltfläche wird im Editor angezeigt, wenn Sie eine XAML-Seite zu öffnen. Im Vorschaufenster kann ein- oder ausgeblendet wird, drücken Sie die **Vorschau** Schaltfläche in der oberen rechten Ecke von einem beliebigen XAML-Dokument-Fenster:

[![Xamarin.Forms-Vorschau in Visual Studio für Mac](xaml-previewer-images/xamlp-list-sml.png "Xamarin.Forms-Vorschau in Visual Studio für Mac")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end
::: zone pivot="win-dev16"

### <a name="visual-studio-2019-preview"></a>Visual Studio 2019 (Vorschau)

Durch Erweitern der geteilten Ansicht Bereichs ist die XAML-Vorschau verfügbar. Standardparameter zum Teilen von View-Verhalten kann gesteuert werden, aus der **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. In diesem Dialogfeld können Sie die Standardansicht für das Dokument und die Ausrichtung der geteilten auswählen.

[![Xamarin.Forms-Vorschau "Optionen" in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "Xamarin.Forms-Vorschau \"Optionen\" in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Wenn Sie eine XAML-Seite zu öffnen, wird der Editor basierend auf den Einstellungen, die im ausgewählten Teilen der **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. Die Aufteilung kann jedoch für jede Datei im Editor-Fenster geändert werden.

#### <a name="xaml-preview-controls"></a>XAML-Vorschau-Steuerelemente

Oben im Editor-Fenster verfügt über die Schaltflächen auswählen, welcher Bereich verwendet werden, die oberste Schaltfläche mit dem Wechsel in das Entwurfsfenster, und die Schaltfläche unten, wechseln in den Quellcodebereich ist. Die mittlere Schaltfläche vertauscht die Reihenfolge im Bereich.

[![Xamarin.Forms-Vorschau zum Wechseln zwischen den Entwurf, Quelle und geteilten Ansicht im Visual Studio steuert](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms-Vorschau-Steuerelemente zum Wechseln zwischen den Entwurf, Quelle und geteilten Ansicht im Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Unten im Editor-Fenster verfügt über die Schaltflächen, die Bereiche, horizontal und vertikal zu teilen und zu erweitern oder reduzieren die aktuellen untergeordneten Bereich.

[![Xamarin.Forms-Vorschau Bereich Ausrichtung Steuerelemente in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Xamarin.Forms-Vorschau Bereich Ausrichtung Steuerelemente in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>XAML-Vorschau-Optionen

Die Optionen am oberen Rand des Vorschaufensters sind:

* **Phone** – Preview auf einem Telefon Größe-Bildschirm
* **Tablet** -Vorschau auf einem Größe Tablettbildschirm
* **Android** – zeigen Sie die Android-Version des Bildschirms
* **iOS** – zeigen Sie die iOS-Version des Bildschirms (*beachten: Wenn Sie Visual Studio auf Windows nutzen, muss Sie [mit einem Mac gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md) mit diesem Modus*)
* **Hochformat (Symbol)** – Hochformat verwendet, für die Vorschau
* **Querformat (Symbol)** – verwendet Querformat für die Vorschau

::: zone pivot="win-dev16"

## <a name="device-drop-down-menu"></a>Geräte-Dropdownmenü

In Visual Studio-2019 (Vorschau) können Sie ein Gerät aus einer Dropdown-Liste von Android oder iOS-Geräten für Vorschau auswählen. Dropdown-Liste ersetzt die Optionen "Telefon" und "Tablet". Bei iOS der Vorschau, wird der Dropdown-iPhone und iPad-Geräte angezeigt. Wenn Sie Android anzeigen zu können, wird der Dropdown-verschiedene Android-Telefone und Tablets angezeigt. Beide Listen enthalten die Bildschirmgrößen und bildschirmauflösungen.

::: zone-end

## <a name="detect-design-mode"></a>Ermitteln Sie im Entwurfsmodus

Die statische [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) Eigenschaft untersucht werden kann, um zu bestimmen, ob die Anwendung, in der Vorschau ausgeführt wird. Dadurch können Sie Code an, die nur ausgeführt, wenn die Anwendung, in der Vorschau ausgeführt wird:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

::: zone pivot="win-dev16"

## <a name="enable-design-time-rendering-for-custom-controls"></a>Aktivieren der Design-Time Rendering für benutzerdefinierte Steuerelemente

Benutzerdefinierte Steuerelemente müssen opt-in zum Entwerfen von Zeit zu rendern, angezeigt, die in der Vorschau, unabhängig davon, ob das Steuerelement befindet sich in Ihrem Projekt oder aus einer Bibliothek importiert werden. Dies kann erreicht werden, durch das Hinzufügen der [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute) auf der Klasse des Steuerelements:

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

Ein Beispiel hierfür finden Sie in [montemagnoss ImageCirclePlugins Basisklasse](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs).

::: zone-end

## <a name="add-design-time-data"></a>Hinzufügen von Entwurfszeit-Daten

Einige Layouts möglicherweise schwer vorstellbar ohne Daten an Steuerelemente der Benutzeroberfläche gebunden. Um die Vorschau nützlicher zu machen, weisen Sie einige statischen Daten zu den Steuerelementen durch hartcodierung einen Bindungskontext (entweder im Code-Behind oder mithilfe von XAML).

Finden Sie in der montemagnos [Blogbeitrag zum Hinzufügen von Entwurfszeitdaten](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) zu erfahren, wie Sie an einem statischen ViewModel in XAML zu binden.

## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie die folgenden Probleme und die [Xamarin-Foren](https://forums.xamarin.com/categories/xamarin-forms), wenn Probleme auftreten.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML-Vorschau nicht angezeigt oder wird ein Fehler angezeigt

Überprüfen Sie folgende Punkte:

* Der Designer-Agent muss Setup beim ersten Verwenden Sie eine XAML-Datei in der Vorschau anzeigen: eine Statusanzeige erscheint in der Vorschau, zusammen mit der statusmeldungen, bis dieser bereit ist.
* Versuchen Sie es zu schließen und die XAML-Datei erneut zu öffnen.
* Sicherstellen, dass Ihre `App` -Klasse verfügt über einen parameterlosen Konstruktor.

::: zone pivot="win-dev16"

### <a name="custom-controls-arent-rendering"></a>Benutzerdefinierte Steuerelemente werden nicht gerendert.

Stellen Sie sicher, dass das Projekt erstellt wird. Wenn der Previewer nicht das Steuerelement ohne Fehler gerendert oder des Steuerelements Ersteller nicht teilnehmen zum Rendern von Zeit zu entwerfen, zeigt die Vorschau des Steuerelements-Basisklasse.

::: zone-end
