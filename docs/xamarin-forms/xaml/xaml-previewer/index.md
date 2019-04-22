---
title: XAML-Vorschau für Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie mit, dass die XAML-Vorschau finden Sie unter Ihrer Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe. Die XAML-Vorschau steht in Visual Studio-2019 und Visual Studio-2019 für Mac.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: db243a9c8dcb25f51bc7926a7aa239531e9c24f6
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58859010"
---
# <a name="xaml-previewer-for-xamarinforms"></a>XAML-Vorschau für Xamarin.Forms

_Finden Sie unter Ihrer Xamarin.Forms-Layouts, das gerendert wird, während der Eingabe_

## <a name="overview"></a>Übersicht

Die XAML-Vorschau erfahren Sie, wie Ihre Xamarin.Forms-XAML-Seite unter iOS und Android aussehen wird. Wenn Sie Änderungen an Ihrer XAML vornehmen, sehen Sie diese Vorschau sofort zusammen mit Ihrem Code. Die XAML-Vorschau steht in Visual Studio und Visual Studio für Mac.

## <a name="getting-started"></a>Erste Schritte

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

Sie können die XAML-Vorschau öffnen, indem Sie auf die Pfeile auf den geteilten Ansichtsbereich. Wenn Sie ändern möchten, teilen der Standardwert Ansicht Verhalten, verwenden Sie die **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. In diesem Dialogfeld können Sie die Standardansicht für das Dokument und die Ausrichtung der geteilten auswählen.

[![Xamarin.Forms-Vorschau "Optionen" in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "Xamarin.Forms-Vorschau \"Optionen\" in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Wenn Sie eine XAML-Datei öffnen, wird der Editor öffnen, vergrößern oder Weiter, um die Vorschau, basierend auf den Einstellungen, die im ausgewählten der **Tools > Optionen > Xamarin > Forms Previewer** Dialogfeld. Die Aufteilung kann jedoch für jede Datei im Editor-Fenster geändert werden.

#### <a name="xaml-preview-controls"></a>XAML-Vorschau-Steuerelemente

Wählen Sie, ob Sie Ihren Code, die XAML-Vorschau anzeigen möchten, oder beides durch Auswahl dieser Schaltflächen für die Teilung Bereich anzuzeigen. Die mittlere Schaltfläche vertauscht, welche Seite der Previewer und Ihren Code auf befinden:

[![Xamarin.Forms-Vorschau zum Wechseln zwischen den Entwurf, Quelle und geteilten Ansicht im Visual Studio steuert](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms-Vorschau-Steuerelemente zum Wechseln zwischen den Entwurf, Quelle und geteilten Ansicht im Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Sie können ändern, ob der Bildschirm vertikal oder horizontal geteilt wird oder einen Bereich vollständig reduzieren:

[![Xamarin.Forms-Vorschau Bereich Ausrichtung Steuerelemente in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Xamarin.Forms-Vorschau Bereich Ausrichtung Steuerelemente in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Die **Vorschau** Schaltfläche wird im Editor angezeigt, wenn Sie eine XAML-Seite zu öffnen. Anzeigen oder ausblenden die Vorschau durch Drücken der **Vorschau** Schaltfläche in der oberen rechten Ecke von einem beliebigen XAML-Dokument-Fenster:

[![Xamarin.Forms-Vorschau in Visual Studio für Mac](xaml-previewer-images/xamlp-list-sml.png "Xamarin.Forms-Vorschau in Visual Studio für Mac")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>XAML-Vorschau-Optionen

Die Optionen am oberen Rand des Vorschaufensters sind:

* **Android** – zeigen Sie die Android-Version des Bildschirms
* **iOS** – zeigen Sie die iOS-Version des Bildschirms (*beachten: Wenn Sie Visual Studio auf Windows nutzen, muss Sie [mit einem Mac gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md) mit diesem Modus*)
* **Gerät** -Dropdown-Liste von Android oder iOS-Geräten einschließlich Auflösung und der Bildschirmgröße
* **Hochformat (Symbol)** – Hochformat verwendet, für die Vorschau
* **Querformat (Symbol)** – verwendet Querformat für die Vorschau

## <a name="detect-design-mode"></a>Ermitteln Sie im Entwurfsmodus

Die statische [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) Eigenschaft darüber informiert, ob die Anwendung, in der Vorschau ausgeführt wird. Damit können Sie Code angeben, die nur ausgeführt, wenn die Anwendung ist oder nicht ausgeführt, in der Vorschau wird:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}

if (!DesignMode.IsDesignModeEnabled)
{
  // Don't run in the Previewer  
}
```

Diese Eigenschaft ist nützlich, wenn Sie eine Bibliothek in Ihre Seitenkonstruktor initialisieren, die nicht zur Entwurfszeit ausgeführt werden.

## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie die folgenden Probleme und die [Xamarin-Foren](https://forums.xamarin.com/categories/xamarin-forms), wenn der Previewer nicht funktioniert.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML-Vorschau nicht angezeigt oder wird ein Fehler angezeigt

* Es dauert einige Zeit für den Previewer starten – Sie werden angezeigt "Gerendert wird initialisiert", bis sie bereit ist.
* Versuchen Sie es schließen und erneuten Öffnen der XAML-Datei.
* Sicherstellen, dass Ihre `App` -Klasse verfügt über einen parameterlosen Konstruktor.
* Überprüfen Sie Ihre Xamarin.Forms-Version – es muss mindestens sein Xamarin.Forms 3.6. Sie können auf die neueste Version von Xamarin.Forms über NuGet aktualisieren.
* Überprüfen Sie Ihre JDK-Installation – Vorschau Android mindestens erfordert [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Versuchen Sie es, einer wrapping initialisiert die Klassen in der Seite C# CodeBehind-Dateien `if (!DesignMode.IsDesignModeEnabled)`.

### <a name="custom-controls-arent-rendering"></a>Benutzerdefinierte Steuerelemente werden nicht gerendert.

Versuchen Sie, Ihr Projekt zu erstellen. Die Vorschau zeigt die Basisklasse des Steuerelements, wenn zum Rendern des Steuerelements ein Fehler auftritt oder des Steuerelements Ersteller entschieden Design-Time Rendering nicht. Weitere Informationen finden Sie unter [Rendern benutzerdefinierte Steuerelemente in der XAML-Vorschau](render-custom-controls.md).
