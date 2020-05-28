---
title: Xamarin.FormsAktuerfrischendes Ansicht
description: Die Xamarin.Forms Aktualisierungs Ansicht ist ein Container Steuerelement, das Pull zur Aktualisierungs Funktionalität für scrollbaren Inhalt bereitstellt.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d84e6bb6ed41f2fbc213cd15051d071521f588cd
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127594"
---
# <a name="xamarinforms-refreshview"></a>Xamarin.FormsAktuerfrischendes Ansicht

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

Der `RefreshView` ist ein Container Steuerelement, das Pull zur Aktualisierungs Funktionalität für scrollbaren Inhalt bereitstellt. Daher muss das untergeordnete Element von `RefreshView` ein scrollbares Steuerelement sein, z [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) . b., oder [`ListView`](xref:Xamarin.Forms.ListView) .

`RefreshView` definiert die folgenden Eigenschaften:

- `Command`vom Typ `ICommand` , der ausgeführt wird, wenn eine Aktualisierung ausgelöst wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.
- `IsRefreshing`vom Typ `bool` , der den aktuellen Zustand des angibt `RefreshView` .
- `RefreshColor`, vom Typ `Color` , die Farbe des Fortschritts Kreises, der während der Aktualisierung angezeigt wird.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Auf dem universelle Windows-Plattform kann die Pull-Richtung eines `RefreshView` mit einem plattformspezifischen festgelegt werden. Weitere Informationen finden Sie unter [pullrichtung](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)für den Pullvorgang.

## <a name="create-a-refreshview"></a>Erstellen einer aktuerfrischenden Ansicht

Im folgenden Beispiel wird gezeigt, wie ein `RefreshView` in XAML instanziiert wird:

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <ScrollView>
        <FlexLayout Direction="Row"
                    Wrap="Wrap"
                    AlignItems="Center"
                    AlignContent="Center"
                    BindableLayout.ItemsSource="{Binding Items}"
                    BindableLayout.ItemTemplate="{StaticResource ColorItemTemplate}" />
    </ScrollView>
</RefreshView>
```

Ein `RefreshView` kann auch im Code erstellt werden:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

ScrollView scrollView = new ScrollView();
FlexLayout flexLayout = new FlexLayout { ... };
scrollView.Content = flexLayout;
refreshView.Content = scrollView;
```

In diesem Beispiel stellt der `RefreshView` Pull-Vorgang für die Aktualisierungs Funktionalität in bereit, dessen untergeordnetes Element [`ScrollView`](xref:Xamarin.Forms.ScrollView) ein ist [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) . `FlexLayout`Verwendet ein bindbares Layout, um seinen Inhalt durch Binden an eine Auflistung von Elementen zu generieren, und legt die Darstellung der einzelnen Elemente mit einem fest [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Weitere Informationen zu bindbaren Layouts finden Sie unter [bindbare Layouts in Xamarin.Forms ](~/xamarin-forms/user-interface/layouts/bindable-layouts.md).

Der Wert der- `RefreshView.IsRefreshing` Eigenschaft gibt den aktuellen Zustand des an `RefreshView` . Wenn eine Aktualisierung durch den Benutzer ausgelöst wird, wird diese Eigenschaft automatisch zu migrischt `true` . Nachdem die Aktualisierung abgeschlossen ist, sollten Sie die-Eigenschaft auf Zurücksetzen `false` .

Wenn der Benutzer eine Aktualisierung initiiert, `ICommand` wird der durch die- `Command` Eigenschaft definierte ausgeführt, wodurch die angezeigten Elemente aktualisiert werden. Eine Aktualisierungs Visualisierung wird angezeigt, während die Aktualisierung durchgeführt wird, die aus einem animierten Fortschritts Kreis besteht:

[![Screenshot einer Aktualisierungs Ansicht für das Aktualisieren von Daten unter IOS und Android](refreshview-images/default-progress-circle.png "Aktualisierungs Ansicht zum Aktualisieren von Daten")](refreshview-images/default-progress-circle-large.png#lightbox "Aktualisierungs Ansicht zum Aktualisieren von Daten")

> [!NOTE]
> `IsRefreshing`Wenn Sie die-Eigenschaft manuell auf festlegen `true` , wird die Aktualisierungs Visualisierung auslöst, und die `ICommand` von der-Eigenschaft definierte wird ausgeführt `Command` .

## <a name="refreshview-appearance"></a>Erscheinungsbild der Bild Anzeige

Zusätzlich zu den Eigenschaften, die `RefreshView` von der- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse erben, `RefreshView` definiert auch die- `RefreshColor` Eigenschaft. Diese Eigenschaft kann festgelegt werden, um die Farbe des Fortschritts Kreises zu definieren, der während der Aktualisierung angezeigt wird:

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

Der folgende Screenshot zeigt ein- `RefreshView` Objekt mit der- `RefreshColor` Eigenschaft:

[![Screenshot einer aktuverlaufs Ansicht mit einem aktiven Status Kreis unter IOS und Android](refreshview-images/teal-progress-circle.png "Aktuverlaufs Ansicht mit einem aktiven Status Kreis")](refreshview-images/teal-progress-circle-large.png#lightbox "Aktuverlaufs Ansicht mit einem aktiven Status Kreis")

Außerdem kann die- `BackgroundColor` Eigenschaft auf einen festgelegt werden [`Color`](xref:Xamarin.Forms.Color) , der die Hintergrundfarbe des Fortschritts Kreises darstellt.

> [!NOTE]
> Unter IOS legt die- `BackgroundColor` Eigenschaft die Hintergrundfarbe des fest `UIView` , der den Status Kreis enthält.

## <a name="disable-a-refreshview"></a>Deaktivieren einer aktuaktivierungs Ansicht

Eine Anwendung kann einen Status eingeben, bei dem der Pull-Vorgang zum Aktualisieren kein gültiger Vorgang ist. In solchen Fällen kann der deaktiviert werden, indem die zugehörige- `RefreshView` Eigenschaft auf festgelegt wird `IsEnabled` `false` . Dadurch wird verhindert, dass Benutzer in der Lage sind, Pull zum Aktualisieren zu initiieren.

Wenn Sie die-Eigenschaft definieren, kann der-Delegat auch `Command` `CanExecute` `ICommand` zum Aktivieren oder Deaktivieren des Befehls angegeben werden.

## <a name="related-links"></a>Verwandte Links

- [Aktuerfrischendes Ansicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [Bindbare Layouts inXamarin.Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [Aktualisierbare Pull Direction-plattformspezifisch](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
