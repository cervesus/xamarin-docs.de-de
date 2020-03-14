---
title: XAML-Vorschau für xamarin. Forms
description: In diesem Artikel wird erläutert, wie Sie mit dem XAML-Previewer ihre xamarin. Forms-Layouts anzeigen, die bei der Typisierungs Darstellung gerendert Der XAML-Previewer ist in Visual Studio 2019 und Visual Studio 2019 für Mac verfügbar.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: b287d523101bb8ca7faca8ea95ee898ccf9c0bb1
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306422"
---
# <a name="xaml-previewer-for-xamarinforms"></a>XAML-Vorschau für xamarin. Forms

_Sehen Sie, wie die xamarin. Forms-Layouts gerendert werden._

## <a name="overview"></a>Übersicht

Der XAML-Previewer zeigt Ihnen, wie Ihre xamarin. Forms-XAML-Seite unter IOS und Android aussehen wird. Wenn Sie Änderungen an Ihrem XAML vornehmen, werden diese sofort neben dem Code angezeigt. Der XAML-Previewer ist in Visual Studio und Visual Studio für Mac verfügbar.

## <a name="getting-started"></a>Erste Schritte

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

Sie können den XAML-Previewer öffnen, indem Sie im Bereich geteilte Ansicht auf die Pfeile klicken. Wenn Sie das Standardverhalten der Split-Ansicht ändern möchten, verwenden Sie das Dialogfeld **Tools > Optionen > xamarin > Forms Previewer** . In diesem Dialogfeld können Sie die Standarddokument Ansicht und die geteilte Ausrichtung auswählen.

[![Xamarin. Forms-Previewer-Optionen in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "Xamarin. Forms-Previewer-Optionen in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Wenn Sie eine XAML-Datei öffnen, wird der Editor entweder in voller Größe oder neben dem Previewer geöffnet, basierend auf den Einstellungen, die im Dialogfeld **Tools > Optionen > xamarin > Forms Previewer** ausgewählt sind. Die Aufteilung kann jedoch für jede Datei im Editor Fenster geändert werden.

#### <a name="xaml-preview-controls"></a>XAML-Vorschau Steuerelemente

Wählen Sie aus, ob Sie Ihren Code, die XAML-Vorschau oder beides anzeigen möchten, indem Sie diese Schaltflächen im Bereich geteilte Ansicht auswählen. Die mittlere Schaltfläche vertauscht die Seite, auf der sich der Previewer und der Code befinden:

[![Xamarin. Forms-Previewer-Steuerelemente zum Wechseln zwischen Design, Quelle und geteilter Ansicht in Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin. Forms-Previewer-Steuerelemente zum Wechseln zwischen Design, Quelle und geteilter Ansicht in Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Sie können ändern, ob der Bildschirm vertikal oder horizontal aufgeteilt wird, oder einen Bereich vollständig reduzieren:

[![Ausrichtung von Steuerelementen im xamarin. Forms-Vorschaubereich in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Ausrichtung von Steuerelementen im xamarin. Forms-Vorschaubereich in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Die Schaltfläche **Vorschau** wird im Editor angezeigt, wenn Sie eine XAML-Seite öffnen. Zeigen Sie die Vorschau an oder blenden Sie Sie aus, indem Sie die Schaltflächen " **Vorschau** " oder " **Teilen** " unten links in jedem XAML-Dokument Fenster drücken:

[![xamarin. Forms-Previewer aktiviert mit der Schaltfläche "Vorschau" oder "teilen"](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> In älteren Versionen von Visual Studio für Mac befand sich die Schaltfläche **Vorschau** in der oberen rechten Ecke des Fensters.

::: zone-end

## <a name="xaml-previewer-options"></a>XAML-previeweroptionen

Die Optionen am oberen Rand des Vorschau Bereichs lauten wie folgt:

* **Android** – zeigt die Android-Version des Bildschirms an
* **IOS** – zeigt die IOS-Version des Bildschirms an (*Hinweis: Wenn Sie Visual Studio unter Windows verwenden, müssen Sie mit [einem Mac gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md) sein, um diesen Modus zu verwenden*).
* **Geräte** -Dropdown Liste von Android-oder IOS-Geräten, einschließlich Auflösung und Bildschirmgröße
* Hochformat **(Symbol)** – verwendet die Hochformat Ausrichtung für die Vorschau.
* **Querformat (Symbol)** – verwendet die quer Ausrichtung für die Vorschau.

## <a name="detect-design-mode"></a>Erkennen des Entwurfs Modus

Die statische [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) -Eigenschaft gibt Aufschluss darüber, ob die Anwendung in der Previewer ausgeführt wird. Wenn Sie ihn verwenden, können Sie Code angeben, der nur ausgeführt wird, wenn die Anwendung im Previewer ausgeführt wird oder nicht ausgeführt wird:

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

Diese Eigenschaft ist nützlich, wenn Sie eine Bibliothek in Ihrem Seitenkonstruktor initialisieren, die zur Entwurfszeit nicht ausgeführt werden kann.

## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie die folgenden Probleme und die [xamarin-Foren](https://forums.xamarin.com/categories/xamarin-forms), wenn der Previewer nicht funktioniert.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>Der XAML-Previewer wird nicht angezeigt oder zeigt einen Fehler an.

* Es kann einige Zeit dauern, bis der Previewer startet. Sie sehen, dass "Rendering initialisiert" ist, bis er bereit ist.
* Schließen Sie die XAML-Datei, und öffnen Sie Sie erneut.
* Stellen Sie sicher, dass die `App`-Klasse über einen Parameter losen Konstruktor verfügt.
* Überprüfen Sie die xamarin. Forms-Version. Sie muss mindestens xamarin. Forms 3,6 sein. Sie können ein Update auf die neueste Version von xamarin. Forms über nuget ausführen.
* Überprüfen der JDK-Installation: Android erfordert mindestens [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Versuchen Sie, alle initialisierten Klassen im C# Code Behind der Seite in `if (!DesignMode.IsDesignModeEnabled)`zu umwickeln.

### <a name="custom-controls-arent-rendering"></a>Benutzerdefinierte Steuerelemente nicht Rendering

Versuchen Sie, Ihr Projekt zu entwickeln. Der Vorschau zeigt die Basisklasse des Steuer Elements an, wenn das Steuerelement nicht gerendert werden kann oder wenn der Ersteller des Steuer Elements das Rendern der Entwurfszeit deaktiviert hat. Weitere Informationen finden Sie unter [Rendering Custom Controls in the XAML Previewer](render-custom-controls.md).
