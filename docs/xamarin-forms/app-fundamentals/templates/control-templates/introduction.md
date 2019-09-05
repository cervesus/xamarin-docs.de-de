---
title: Einführung in Xamarin.Forms-Steuerelementvorlagen
description: Mit Xamarin.Forms-Steuerelementvorlagen können Sie unkompliziert zur Laufzeit Designs für Anwendungsseiten entwerfen und überarbeiten. In diesem Artikel werden Steuerelementvorlagen grundlegend vorgestellt.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 676523e461737d7820278ca8c319794d3347088d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289790"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Einführung in Xamarin.Forms-Steuerelementvorlagen

_Mit Xamarin.Forms-Steuerelementvorlagen können Sie unkompliziert zur Laufzeit Designs für Anwendungsseiten entwerfen und überarbeiten. In diesem Artikel werden Steuerelementvorlagen grundlegend vorgestellt._

Steuerelemente haben unterschiedliche Eigenschaften, z. B. `BackgroundColor` und `TextColor`, mit denen Sie Aspekte ihrer Darstellung definieren können. Diese Eigenschaften können Sie mithilfe von [Formatvorlagen](~/xamarin-forms/user-interface/styles/index.md) festlegen, die zur Laufzeit geändert werden können, um ein grundlegendes Design zu implementieren. Bei Formatvorlagen besteht jedoch keine klare Trennung zwischen dem Erscheinungsbild einer Seite und ihrem Inhalt. Zusätzlich sind die Änderungen, die durch Festlegen dieser Eigenschaften möglich sind, eingeschränkt.

Steuerelementvorlagen trennen das Aussehen und den Inhalt einer Seite klar voneinander. So erhalten Sie Seiten, die sich leicht designen lassen. Beispielsweise kann eine Anwendung auf Anwendungsebene Steuerelementvorlagen enthalten, die ein helles und ein dunkles Design bereitstellen. Sie können das Design jeder [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse in der Anwendung ändern, indem Sie eine der Steuerelementvorlagen anwenden. Der Inhalt der einzelnen Seiten verändert sich dabei nicht. Darüber hinaus gelten bei Designs. die von Steuerelementvorlagen bereitgestellt werden, keine Einschränkungen beim Ändern der Eigenschaften der Steuerelemente. Auch die Steuerelemente, mit denen das Design implementiert wird, können geändert werden.

## <a name="creating-a-controltemplate"></a>Erstellen eines ControlTemplate-Objekts

Eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse legt die Darstellung einer Seite oder Ansicht fest. Sie enthält ein Stammlayout und darin die Steuerelemente, die die Vorlage implementieren. In der Regel wird für eine `ControlTemplate`-Klasse eine [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)-Klasse verwendet, um die Position des auf der Seite oder Ansicht anzuzeigenden Inhalts anzugeben. Die Seite oder Ansicht, für die die `ControlTemplate`-Klasse gedacht ist, gibt dann den Inhalt vor, der von der `ContentPresenter`-Klasse angezeigt werden soll. Das folgende Diagramm veranschaulicht eine `ControlTemplate`-Klasse für eine Seite mit einer Reihe von Steuerelementen. Zusätzlich abgebildet ist eine `ContentPresenter`-Klasse, hier dargestellt durch ein blaues Rechteck:

![](introduction-images/control-template.png "Steuerelementvorlage für eine Seite")

Sie können die [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse auf die folgenden Typen anwenden, indem Sie die Eigenschaft `ControlTemplate` festlegen:

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

Wenn Sie eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse erstellt und diesen Typen zugewiesen haben, wird die Darstellung entsprechend der `ControlTemplate`-Klasse angepasst. Zusätzlich zur Festlegung der Darstellung mithilfe der Eigenschaft `ControlTemplate` können Sie auch über Formatvorlagen Steuerelementvorlagen anwenden. So haben Sie noch mehr Designmöglichkeiten.

> [!NOTE]
> *Was sind `TemplatedPage` und `TemplatedView` für Typen?* `TemplatedPage` ist die Basisklasse für `ContentPage` und der einfachste Seitentyp in Xamarin.Forms. Im Gegensatz zu `ContentPage` hat `TemplatedPage` nicht die Eigenschaft `Content`. Deshalb können Sie einer `TemplatedPage`-Instanz nicht direkt Inhalt hinzufügen. Stattdessen wird der Inhalt durch Festlegen der Steuerelementvorlage für die `TemplatedPage`-Instanz hinzugefügt. Bei `TemplatedView` ist es ähnlich. Dieser Typ ist die Basisklasse für `ContentView`. Im Gegensatz zu `ContentView` hat `TemplatedView` nicht die Eigenschaft `Content`. Deshalb können Sie einer `TemplatedView`-Instanz nicht direkt Inhalt hinzufügen. Stattdessen wird der Inhalt durch Festlegen der Steuerelementvorlage für die `TemplatedView`-Instanz hinzugefügt.

Steuerelementvorlagen können Sie in XAML und C# erstellen:

- In XAML erstellte Steuerelementvorlagen werden in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) erstellt, das der Sammlung [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) einer Seite oder der Sammlung [`Resources`](xref:Xamarin.Forms.Application.Resources) der Anwendung zugewiesen ist. Letztere Variante wird häufiger verwendet.
- In C# erstellte Steuerelementvorlagen werden üblicherweise in der Klasse der Seite oder in einer global zugänglichen Klasse definiert.

Die Entscheidung, wo Sie eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Instanz definieren, hat Einfluss darauf, wo Sie sie verwenden können:

- Eine auf Seitenebene definierte [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Instanz können Sie nur auf die bestimmte Seite anwenden.
- Eine auf Anwendungsebene definierte [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Instanz können Sie für alle Seiten der Anwendung nutzen.

Steuerelementvorlagen, die weiter unten in der Hierarchie der Ansichten angeordnet sind, haben Vorrang vor denjenigen, die sich weiter oben befinden. So hat z. B. eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse namens `DarkTheme`, die auf Seitenebene definiert ist, Vorrang gegenüber einer identisch benannten Vorlage auf Anwendungsebene. Deshalb sollte eine Steuerelementvorlage, mit der ein Design auf jede Seite einer Anwendung angewendet werden soll, auf Anwendungsebene definiert sein.


## <a name="related-links"></a>Verwandte Links

- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
