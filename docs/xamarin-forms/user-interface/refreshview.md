---
title: Xamarin. Forms-Ansicht "erfrischend"
description: Die Aktualisierungs Ansicht von xamarin. Forms ist ein Container Steuerelement, das Pull zum Aktualisieren von Bild lauffähigen Inhalten bereitstellt.
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
ms.openlocfilehash: b53c58a5e859bf7752855c3954666a062261599d
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697740"
---
# <a name="xamarinforms-refreshview"></a>Xamarin. Forms-Ansicht "erfrischend"

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshview/)

Der `RefreshView` ist ein Container Steuerelement, das Pull zur Aktualisierungs Funktionalität für scrollbaren Inhalt bereitstellt. Daher muss das untergeordnete Element einer `RefreshView` ein scrollbares Steuerelement sein, z. b. [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`CollectionView`](xref:Xamarin.Forms.CollectionView)oder [`ListView`](xref:Xamarin.Forms.ListView).

`RefreshView` definiert die folgenden Eigenschaften:

- `Command` vom Typ `ICommand`, der beim Auslösen einer Aktualisierung ausgeführt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.
- `IsRefreshing` vom Typ `bool`, der den aktuellen Zustand der `RefreshView` angibt.
- `RefreshColor` vom Typ `Color` die Farbe des Fortschritts Kreises, der während der Aktualisierung angezeigt wird.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können und formatiert sind.

> [!NOTE]
> Auf dem universelle Windows-Plattform kann die Pull-Richtung einer `RefreshView` mit einem plattformspezifischen festgelegt werden. Weitere Informationen finden Sie unter [pullrichtung](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)für den Pullvorgang.

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

Eine `RefreshView` kann auch im Code erstellt werden:

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

In diesem Beispiel stellt der `RefreshView` Pull-to-refresh-Funktionalität für einen [`ScrollView`](xref:Xamarin.Forms.ScrollView) bereit, dessen untergeordnetes Element ein [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)ist. Der `FlexLayout` verwendet ein bindbares Layout, um seinen Inhalt durch Binden an eine Auflistung von Elementen zu generieren, und legt die Darstellung der einzelnen Elemente mit einem [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)fest. Weitere Informationen zu bindbaren Layouts finden Sie unter [bindbare Layouts in xamarin. Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md).

Der Wert der `RefreshView.IsRefreshing`-Eigenschaft gibt den aktuellen Status des `RefreshView` an. Wenn eine Aktualisierung durch den Benutzer ausgelöst wird, wird diese Eigenschaft automatisch zu `true` übergehen. Nachdem die Aktualisierung abgeschlossen ist, sollten Sie die-Eigenschaft auf `false` zurücksetzen.

Wenn der Benutzer eine Aktualisierung initiiert, wird die von der `Command`-Eigenschaft definierte `ICommand` ausgeführt, wodurch die angezeigten Elemente aktualisiert werden. Eine Aktualisierungs Visualisierung wird angezeigt, während die Aktualisierung durchgeführt wird, die aus einem animierten Fortschritts Kreis besteht:

[![Screenshot einer Aktualisierungs Ansicht für das Aktualisieren von Daten unter IOS und Android](refreshview-images/default-progress-circle.png "Aktualisierungs Ansicht zum Aktualisieren von Daten")](refreshview-images/default-progress-circle-large.png#lightbox "Aktualisierungs Ansicht zum Aktualisieren von Daten")

> [!NOTE]
> Wenn Sie die `IsRefreshing`-Eigenschaft manuell auf `true` festlegen, wird die Aktualisierungs Visualisierung auslöst, und die von der `Command`-Eigenschaft definierte `ICommand` wird ausgeführt.

## <a name="refreshview-appearance"></a>Erscheinungsbild der Bild Anzeige

Zusätzlich zu den Eigenschaften, die `RefreshView` von der [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse erbt, definiert `RefreshView` auch die `RefreshColor`-Eigenschaft. Diese Eigenschaft kann festgelegt werden, um die Farbe des Fortschritts Kreises zu definieren, der während der Aktualisierung angezeigt wird:

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

Der folgende Screenshot zeigt eine `RefreshView`, bei der die `RefreshColor`-Eigenschaft festgelegt ist:

[![Screenshot einer aktuverlaufs Ansicht mit einem aktiven Status Kreis unter IOS und Android](refreshview-images/teal-progress-circle.png "Aktuverlaufs Ansicht mit einem aktiven Status Kreis")](refreshview-images/teal-progress-circle-large.png#lightbox "Aktuverlaufs Ansicht mit einem aktiven Status Kreis")

Außerdem kann die `BackgroundColor`-Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color) festgelegt werden, der die Hintergrundfarbe des Fortschritts Kreises darstellt.

> [!NOTE]
> Unter IOS legt die `BackgroundColor`-Eigenschaft die Hintergrundfarbe des `UIView` fest, der den Status Kreis enthält.

## <a name="disable-a-refreshview"></a>Deaktivieren einer aktuaktivierungs Ansicht

Eine Anwendung kann einen Status eingeben, bei dem der Pull-Vorgang zum Aktualisieren kein gültiger Vorgang ist. In solchen Fällen kann die `RefreshView` durch Festlegen der `IsEnabled`-Eigenschaft auf `false` deaktiviert werden. Dadurch wird verhindert, dass Benutzer in der Lage sind, Pull zum Aktualisieren zu initiieren.

Wenn Sie die `Command`-Eigenschaft definieren, können Sie alternativ den `CanExecute` Delegaten der `ICommand` angeben, um den Befehl zu aktivieren oder zu deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [Aktuerfrischendes Ansicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshview/)
- [Bindbare Layouts in xamarin. Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [Aktualisierbare Pull Direction-plattformspezifisch](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
