---
title: Einführung in Xamarin.Forms Steuerelementvorlagen
description: Xamarin.Forms-Steuerelementvorlagen bieten die Möglichkeit, leicht Design und Re-Design-Anwendungsseiten zur Laufzeit. Dieser Artikel bietet eine Einführung in Steuerelementvorlagen.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a8e5c84bfa2525a28e9af5343be0ee156564bdd6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242517"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Einführung in Xamarin.Forms Steuerelementvorlagen

_Xamarin.Forms-Steuerelementvorlagen bieten die Möglichkeit, leicht Design und Re-Design-Anwendungsseiten zur Laufzeit. Dieser Artikel bietet eine Einführung in Steuerelementvorlagen._

Steuerelemente verfügen über unterschiedliche Eigenschaften, z. B. `BackgroundColor` und `TextColor`, können die Aspekte der Darstellung des Steuerelements definiert. Diese Eigenschaften können festgelegt werden, mithilfe von [Stile](~/xamarin-forms/user-interface/styles/index.md), die kann zur Laufzeit implementiert grundlegende Designs geändert werden. Allerdings Stile keine saubere Trennung zwischen der Darstellung einer Seite und dessen Inhalt, und die Änderungen, die vorgenommen werden können, indem Sie solche Eigenschaften beschränkt sind.

Steuerelementvorlagen bieten eine saubere Trennung zwischen der Darstellung einer Seite und dessen Inhalt, und ermöglichen somit die Erstellung der Seiten, die leicht Designs werden kann. Eine Anwendung kann z. B. auf Anwendungsebene Steuerelementvorlagen enthalten, die ein Design "dunkel" und einem hellen Design bereitstellen. Jede [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) in der Anwendung möglich Designs durch Anwenden eines der Steuerelementvorlagen ohne Ändern des Inhalts von jeder Seite angezeigt wird. Darüber hinaus sind die Designs Steuerelementvorlagen gebotenen nicht zum Ändern der Eigenschaften von Steuerelementen beschränkt. Sie können auch die Steuerelemente verwendet, um das Design implementieren ändern.

## <a name="creating-a-controltemplate"></a>Erstellen eines ControlTemplate-Objekts

Ein [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) gibt die Darstellung einer Seite oder eine Sicht und ein Stammlayout enthält und in das Layout der Steuerelemente, die die Vorlage zu implementieren. In der Regel eine `ControlTemplate` nutzt eine [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) zu markieren, wo der Inhalt, der von der Seite oder der Sicht angezeigt werden angezeigt wird. Die Seite oder eine Sicht, die verarbeitet die `ControlTemplate` wird dann von anzuzeigenden Inhalt definieren den `ContentPresenter`. Das folgende Diagramm veranschaulicht eine `ControlTemplate` für eine Seite, die eine Reihe von Kontrollmechanismen, einschließlich enthält eine `ContentPresenter` durch ein blaues Rechteck markiert:

![](introduction-images/control-template.png "Für eine Seite Steuerelementvorlage")

Ein [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) kann auf die folgenden Typen angewendet werden, durch Festlegen von deren `ControlTemplate` Eigenschaften:

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

Wenn eine [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) erstellt und zugewiesen auf diese Typen vorhandenen Darstellung wird durch die Darstellung im definierten ersetzt die `ControlTemplate`. Darüber hinaus sowie das Festlegen der Darstellung mit der `ControlTemplate` Eigenschaft Steuerelement Vorlagen können auch mithilfe von Formaten zur weiteren angewendet werden Design Möglichkeit zu erweitern.

> [!NOTE]
>  *Was sind die `TemplatedPage` und `TemplatedView` Typen?* `TemplatedPage` ist die Basisklasse für `ContentPage`, und die meisten grundlegenden Seitentyp von Xamarin.Forms bereitgestellt wird. Im Gegensatz zu `ContentPage`, `TemplatedPage` verfügt nicht über eine `Content` Eigenschaft. Aus diesem Grund Inhalt kann nicht direkt hinzugefügt werden eine `TemplatedPage` Instanz. Stattdessen wird Inhalt hinzugefügt, indem Sie die Steuerelementvorlage für die `TemplatedPage` Instanz. Auf ähnliche Weise `TemplatedView` ist die Basisklasse für `ContentView`. Im Gegensatz zu `ContentView`, `TemplatedView` verfügt nicht über eine `Content` Eigenschaft. Aus diesem Grund Inhalt kann nicht direkt hinzugefügt werden eine `TemplatedView` Instanz. Stattdessen wird Inhalt hinzugefügt, indem Sie die Steuerelementvorlage für die `TemplatedView` Instanz.

Vorlagen können in XAML und c# erstellt werden:

- Steuerelementvorlagen in XAML erstellt werden definiert, einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zugewiesen ist, die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Sammlung einer Seite oder in der Regel die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) Auflistung der Anwendung.
- Steuerelementvorlagen in c# erstellt werden in der Regel definiert, in der Page-Klasse oder in einer Klasse, die Global zugegriffen werden kann.

Auswählen des Installationsorts für das Definieren einer [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Instanz wirkt sich darauf aus, wo sie verwendet werden kann:

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Instanzen, die auf der Seite "-Ebene definiert können nur auf der Seite angewendet werden.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) auf der Anwendungsebene definierte Instanzen können auf Seiten in der gesamten Anwendung angewendet werden.

Weiter unten in der Hierarchie anzeigen Steuerelementvorlagen haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) mit dem Namen `DarkTheme` , definiert ist, auf der Seitenebene Vorrang vor einer gleichnamigen Vorlage auf der Anwendungsebene definiert. Aus diesem Grund sollten eine Steuerelementvorlage, die definiert, ein Design auf jeder Seite in einer Anwendung angewendet werden soll, auf der Anwendungsebene definiert werden.


## <a name="related-links"></a>Verwandte Links

- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
