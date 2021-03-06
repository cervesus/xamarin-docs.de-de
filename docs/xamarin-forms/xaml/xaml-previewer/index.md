---
title: XAML-Vorschau fürXamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie die XAML-Vorschau verwenden, um Ihre Layouts bei der Xamarin.Forms Typisierungs Darstellung anzuzeigen. Der XAML-Previewer ist in Visual Studio 2019 und Visual Studio 2019 für Mac verfügbar.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6916d5fde688c5b1162f12db0d36bc3ca27156d8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137318"
---
# <a name="xaml-previewer-for-xamarinforms"></a>XAML-Vorschau fürXamarin.Forms

_Anzeigen der Xamarin.Forms gerenderten Layouts bei der Typdarstellung_

## <a name="overview"></a>Übersicht

Die XAML-Vorschau zeigt Ihnen, wie Ihre Xamarin.Forms XAML-Seite unter IOS und Android aussehen wird. Wenn Sie Änderungen an Ihrem XAML vornehmen, werden diese sofort neben dem Code angezeigt. Der XAML-Previewer ist in Visual Studio und Visual Studio für Mac verfügbar.

## <a name="getting-started"></a>Erste Schritte

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

Sie können den XAML-Previewer öffnen, indem Sie im Bereich geteilte Ansicht auf die Pfeile klicken. Wenn Sie das Standardverhalten der Split-Ansicht ändern möchten, verwenden Sie das Dialogfeld **Tools > Optionen > xamarin > Xamarin.Forms XAML Previewer** . In diesem Dialogfeld können Sie die Standarddokument Ansicht und die geteilte Ausrichtung auswählen.

[![Xamarin.FormsPreviewer-Optionen in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "[! Schel. No-Loc (xamarin. Forms)] Previewer-Optionen in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Wenn Sie eine XAML-Datei öffnen, wird der Editor entweder in voller Größe oder neben dem Previewer geöffnet, basierend auf den Einstellungen, die im Dialogfeld **Tools > Optionen > xamarin > Xamarin.Forms XAML Previewer** ausgewählt sind. Die Aufteilung kann jedoch für jede Datei im Editor Fenster geändert werden.

#### <a name="xaml-preview-controls"></a>XAML-Vorschau Steuerelemente

Wählen Sie aus, ob Sie Ihren Code, die XAML-Vorschau oder beides anzeigen möchten, indem Sie diese Schaltflächen im Bereich geteilte Ansicht auswählen. Die mittlere Schaltfläche vertauscht die Seite, auf der sich der Previewer und der Code befinden:

[![Xamarin.FormsPreviewer-Steuerelemente für den Wechsel zwischen Design, Quelle und geteilter Ansicht in Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "[! Schel. No-Loc (xamarin. Forms)] Previewer-Steuerelemente für den Wechsel zwischen Design, Quelle und geteilter Ansicht in Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Sie können ändern, ob der Bildschirm vertikal oder horizontal aufgeteilt wird, oder einen Bereich vollständig reduzieren:

[![Xamarin.FormsAusrichtung von Steuerelementen im Vorschaubereich in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "[! Schel. "NO-LOC (xamarin. Forms)]"-Ausrichtung Steuerelemente im Vorschaubereich in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

#### <a name="enable-or-disable-the-xaml-previewer"></a>Aktivieren oder Deaktivieren der XAML-Vorschau

Sie können den XAML-Previewer im Dialog **> Optionen > xamarin > Xamarin.Forms XAML Previewer** deaktivieren, indem Sie als Standard- **XML** -Editor als **Standard-XAML-Editor**auswählen. Dies deaktiviert auch die Dokument Gliederung, den Eigenschaften Bereich und die XAML-Toolbox. Wenn Sie den XAML-Previewer und diese Tools wieder aktivieren möchten, ändern Sie den **standardmäßigen XAML-Editor** in ** Xamarin.Forms Previewer**.

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Die Schaltfläche **Vorschau** wird im Editor angezeigt, wenn Sie eine XAML-Seite öffnen. Zeigen Sie die Vorschau an oder blenden Sie Sie aus, indem Sie die Schaltflächen " **Vorschau** " oder " **Teilen** " unten links in jedem XAML-Dokument Fenster drücken:

[![Xamarin.FormsPreviewer aktiviert mit der Schaltfläche "Vorschau" oder "teilen"](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> In älteren Versionen von Visual Studio für Mac befand sich die Schaltfläche **Vorschau** in der oberen rechten Ecke des Fensters.

#### <a name="enable-or-disable-the-xaml-previewer"></a>Aktivieren oder Deaktivieren der XAML-Vorschau

Sie können den XAML-Previewer in den **Visual Studio-> Einstellungen > Text-Editor > XAML** -Dialogfeld deaktivieren, indem Sie als Standard- **XML** -Editor als **Standard-XAML-Editor**auswählen. Dies deaktiviert auch die Dokument Gliederung, den Eigenschaften Bereich und die XAML-Toolbox. Wenn Sie den XAML-Previewer und diese Tools wieder aktivieren möchten, ändern Sie den **standardmäßigen XAML-Editor** in ** Xamarin.Forms Previewer**.

::: zone-end

## <a name="xaml-previewer-options"></a>XAML-previeweroptionen

Die Optionen am oberen Rand des Vorschau Bereichs lauten wie folgt:

* **Android** – zeigt die Android-Version des Bildschirms an
* **IOS** – zeigt die IOS-Version des Bildschirms an (*Hinweis: Wenn Sie Visual Studio unter Windows verwenden, müssen Sie mit [einem Mac gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md) sein, um diesen Modus zu verwenden*).
* **Geräte** -Dropdown Liste von Android-oder IOS-Geräten, einschließlich Auflösung und Bildschirmgröße
* Hochformat **(Symbol)** – verwendet die Hochformat Ausrichtung für die Vorschau.
* **Querformat (Symbol)** – verwendet die quer Ausrichtung für die Vorschau.

## <a name="detect-design-mode"></a>Erkennen des Entwurfs Modus

Die statische- [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) Eigenschaft gibt Aufschluss darüber, ob die Anwendung in der Previewer ausgeführt wird. Wenn Sie ihn verwenden, können Sie Code angeben, der nur ausgeführt wird, wenn die Anwendung im Previewer ausgeführt wird oder nicht ausgeführt wird:

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
* Stellen Sie sicher, dass die `App` Klasse über einen Parameter losen Konstruktor verfügt.
* Überprüfen Sie Ihre Xamarin.Forms Version-Sie muss mindestens Xamarin.Forms 3,6 sein. Sie können Xamarin.Forms über nuget auf die neueste Version aktualisieren.
* Überprüfen der JDK-Installation: Android erfordert mindestens [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Versuchen Sie, alle initialisierten Klassen im c#-Code der Seite in zu umwickeln `if (!DesignMode.IsDesignModeEnabled)` .

### <a name="custom-controls-arent-rendering"></a>Benutzerdefinierte Steuerelemente nicht Rendering

Versuchen Sie, Ihr Projekt zu entwickeln. Der Vorschau zeigt die Basisklasse des Steuer Elements an, wenn das Steuerelement nicht gerendert werden kann oder wenn der Ersteller des Steuer Elements das Rendern der Entwurfszeit deaktiviert hat. Weitere Informationen finden Sie unter [Rendering Custom Controls in the XAML Previewer](render-custom-controls.md).
