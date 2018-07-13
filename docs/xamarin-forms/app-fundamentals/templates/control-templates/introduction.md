---
title: Einführung in Xamarin.Forms-Steuerelementvorlagen
description: Vorlagen für Xamarin.Forms-Steuerelemente bieten die Möglichkeit, einfache Weise Designs und überarbeitet Anwendungsseiten zur Laufzeit. Dieser Artikel enthält eine Einführung in Steuerelementvorlagen.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994424"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Einführung in Xamarin.Forms-Steuerelementvorlagen

_Vorlagen für Xamarin.Forms-Steuerelemente bieten die Möglichkeit, einfache Weise Designs und überarbeitet Anwendungsseiten zur Laufzeit. Dieser Artikel enthält eine Einführung in Steuerelementvorlagen._

Steuerelemente verfügen über unterschiedliche Eigenschaften, z. B. `BackgroundColor` und `TextColor`, die Aspekte der Darstellung des Steuerelements definieren können. Diese Eigenschaften können festgelegt werden, mithilfe von [Stile](~/xamarin-forms/user-interface/styles/index.md), dem kann geändert werden, zur Laufzeit, um das grundlegende Design zu implementieren. Allerdings Stile keine saubere Trennung zwischen der Darstellung einer Seite und den Inhalt, und die Änderungen, die vorgenommen werden können, indem Sie diese Eigenschaften sind beschränkt.

Mit Steuerelementvorlagen eine saubere Trennung zwischen der Darstellung einer Seite und den Inhalt, und ermöglichen somit die Erstellung von Seiten, die problemlos mit Design versehen werden kann. Beispielsweise kann eine Anwendung auf Anwendungsebene Steuerelementvorlagen enthalten, die ein Design "dunkel" und ein helles Design bereitstellen. Jede [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) in der Anwendung kann sein Design durch Anwenden eines die Vorlagen ohne Änderungen des Inhalts von jeder Seite angezeigt wird. Darüber hinaus sind die Designs von Steuerelementvorlagen bereitgestellten nicht darauf beschränkt, zum Ändern der Eigenschaften von Steuerelementen. Sie können auch die Steuerelemente, die zum Implementieren des Designs ändern.

## <a name="creating-a-controltemplate"></a>Erstellen eines ControlTemplate-Objekts

Ein [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) gibt die Darstellung einer Seite oder Ansicht, und enthält ein Root-Layout, und klicken Sie in das Layout an, die Steuerelemente, die die Vorlage zu implementieren. In der Regel eine `ControlTemplate` werden verwendet, die eine [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) markieren, wo der Inhalt von der Seite oder Ansicht angezeigt werden soll angezeigt wird. Der Seite oder Ansicht, die verarbeitet die `ControlTemplate` wird von anzuzeigenden Inhalt definiert die `ContentPresenter`. Das folgende Diagramm veranschaulicht eine `ControlTemplate` für eine Seite, eine Reihe von Steuerelementen enthält, einschließlich, einer `ContentPresenter` durch ein blaues Rechteck markiert:

![](introduction-images/control-template.png "Die Steuerelementvorlage für eine Seite")

Ein [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) kann auf die folgenden Typen angewendet werden, durch Festlegen ihrer `ControlTemplate` Eigenschaften:

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

Wenn eine [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) erstellt und zugewiesen auf diese Typen vorhandenen Darstellung durch die Darstellung im definierten ersetzt die `ControlTemplate`. Darüber hinaus sowie das Festlegen von Darstellung mithilfe der `ControlTemplate` Eigenschaft Steuerelement Vorlagen auch angewendet werden, können über Stile zur weiteren Erweitern Design Möglichkeit.

> [!NOTE]
>  *Was sind die `TemplatedPage` und `TemplatedView` Typen?* `TemplatedPage` ist die Basisklasse für `ContentPage`, und ist der einfachste Typ von Xamarin.Forms bereitgestelltes. Im Gegensatz zu `ContentPage`, `TemplatedPage` verfügt nicht über eine `Content` Eigenschaft. Aus diesem Grund Inhalt kann nicht direkt hinzugefügt werden eine `TemplatedPage` Instanz. Stattdessen Inhalt hinzugefügt wird, indem Sie die Steuerelementvorlage für den `TemplatedPage` Instanz. Auf ähnliche Weise `TemplatedView` ist die Basisklasse für `ContentView`. Im Gegensatz zu `ContentView`, `TemplatedView` verfügt nicht über eine `Content` Eigenschaft. Aus diesem Grund Inhalt kann nicht direkt hinzugefügt werden eine `TemplatedView` Instanz. Stattdessen Inhalt hinzugefügt wird, indem Sie die Steuerelementvorlage für den `TemplatedView` Instanz.

Steuerelementvorlagen können in XAML und C# -Code erstellt werden:

- In XAML erstellte Steuerelementvorlagen werden in definiert eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) , zugewiesen ist die [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung einer Seite oder in der Regel um die [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Auflistung von der Anwendung.
- In c# erstellte Steuerelementvorlagen werden in der Regel definiert, in der Klasse von der Seite oder in eine Klasse, die Global zugegriffen werden kann.

Auswählen des Installationsorts für das Definieren einer [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Instanz wirkt sich auf, wo sie verwendet werden kann:

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) Instanzen, die auf der Ebene der Seite definiert, können nur auf der Seite angewendet werden.
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) auf der Ebene der Anwendung definierte Instanzen können auf Seiten in der gesamten Anwendung angewendet werden.

Steuerelementvorlagen, die weiter unten in der Hierarchie von Inhaltsansichten haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) mit dem Namen `DarkTheme` , definiert ist, auf der Seitenebene Vorrang vor einer gleichnamigen Vorlage, die auf der Ebene der Anwendung definiert. Aus diesem Grund sollten eine Steuerelementvorlage, die ein Design auf jeder Seite in einer Anwendung angewendet werden definiert, auf der Ebene der Anwendung definiert werden.


## <a name="related-links"></a>Verwandte Links

- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
