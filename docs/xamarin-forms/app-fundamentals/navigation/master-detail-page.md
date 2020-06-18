---
title: Klasse „MasterDetailPage“ von Xamarin.Forms
description: Die Klasse „MasterDetailPage“ ist eine Xamarin.Forms-Seite, die zwei verwandte Seiten mit Informationen verwaltet. Eine davon ist die Masterseite, die Elemente darstellt, und die andere ist eine Detailseite, die Informationen zu Elementen auf der Masterseite darstellt. In diesem Artikel wird erläutert, wie Sie eine MasterDetailPage-Klasse verwenden und zwischen den Seiten mit Informationen navigieren.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c751a1843479f1e98739964631999dfdb0e3b634
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84569633"
---
# <a name="xamarinforms-master-detail-page"></a>Klasse „MasterDetailPage“ von Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage)

_Die Klasse „MasterDetailPage“ ist eine Xamarin.Forms-Seite, die zwei verwandte Seiten mit Informationen verwaltet. Eine davon ist die Masterseite, die Elemente darstellt, und die andere ist eine Detailseite, die Informationen zu Elementen auf der Masterseite darstellt. In diesem Artikel wird erläutert, wie Sie eine MasterDetailPage-Klasse verwenden und zwischen den Seiten mit Informationen navigieren._

## <a name="overview"></a>Übersicht

In der Regel zeigt eine Masterseite wie in den folgenden Screenshots gezeigt eine Liste von Elementen an:

[![](master-detail-page-images/masterpage-components.png "Master Page Components")](master-detail-page-images/masterpage-components-large.png#lightbox "Master Page Components")

Der Speicherort für die Liste der Elemente ist auf jeder Plattform identisch. Wenn Sie eines der Elemente auswählen, wird zur entsprechenden Detailseite navigiert. Darüber hinaus verfügt die Masterseite auch über eine Navigationsleiste, die eine Schaltfläche enthält, mit der Sie zur aktiven Detailseite navigieren können:

- Unter iOS finden Sie die Navigationsleiste am oberen Rand der Seite, und sie verfügt über eine Schaltfläche, mit der zur Detailseite navigiert werden kann. Sie können außerdem zur aktiven Detailseite navigieren, indem Sie auf der Masterseite nach links wischen.
- Unter Android finden Sie die Navigationsleiste ebenfalls am oberen Rand der Seite, und sie zeigt einen Titel, ein Symbol und eine Schaltfläche an, mit der zur Detailseite navigiert werden kann. Das Symbol wird im `[Activity]`-Attribut definiert, das die `MainActivity`-Klasse im plattformspezifischen Android-Projekt ergänzt. Darüber hinaus können Sie zur aktiven Detailseite navigieren, indem Sie auf der Masterseite nach links wischen, auf der rechten Seite des Bildschirms auf die Detailseite tippen oder am unteren Rand des Bildschirms auf *Zurück* tippen.
- Bei der UWP (Universelle Windows-Plattform) finden Sie die Navigationsleiste am oberen Rand der Seite, und sie verfügt über eine Schaltfläche, mit der zur Detailseite navigiert werden kann.

Auf einer Detailseite werden Daten angezeigt, die dem auf der Masterseite ausgewählten Element entsprechen. In den folgenden Screenshots sehen Sie die Hauptkomponenten der Detailseite:

![](master-detail-page-images/detailpage-components.png "Detail Page Components")

Die Detailseite enthält eine Navigationsleiste, deren Inhalt von der jeweiligen Plattform abhängen:

- Unter iOS finden Sie die Navigationsleiste am oberen Rand der Seite. Dort werden ein Titel und eine Schaltfläche angezeigt, mit der zur Masterseite zurück navigiert werden kann, vorausgesetzt, die Detailseiteninstanz ist in der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz umschlossen. Sie können außerdem zur Masterseite zurückkehren, indem Sie auf der Detailseite nach rechts wischen.
- Unter Android finden Sie die Navigationsleiste ebenfalls am oberen Rand der Seite, und sie zeigt einen Titel, ein Symbol und eine Schaltfläche an, mit der zurück zur Masterseite navigiert werden kann. Das Symbol wird im `[Activity]`-Attribut definiert, das die `MainActivity`-Klasse im plattformspezifischen Android-Projekt ergänzt.
- Bei der UWP finden Sie die Navigationsleiste ebenfalls am oberen Rand der Seite, und sie zeigt einen Titel, ein Symbol und eine Schaltfläche an, mit der zurück zur Masterseite navigiert werden kann.

### <a name="navigation-behavior"></a>Navigationsverhalten

Das Verhalten der Navigationsfunktion zwischen Master- und Detailseiten ist plattformabhängig:

- Unter iOS können Sie die Detailseite nach rechts und die Masterseite nach links *schieben*, und der linke Teil der Detailseite bleibt weiterhin sichtbar.
- Unter Android werden die Detail- und Masterseiten miteinander *überlagert*.
- Auf der Universellen Windows-Plattform werden Masterseiten von der linken Seite aus eingeblendet und bedecken die Detailseite teilweise, sofern die Eigenschaft [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) auf `Popover` festgelegt ist. Weitere Informationen finden Sie unter [Steuern des Anzeigeverhaltens der Detailseite](#controlling-the-detail-page-display-behavior).

Im Querformat wird ein ähnliches Verhalten angewandt, mit der Ausnahme, dass die Masterseite unter iOS und Android eine ähnliche Breite wie die Masterseite im Hochformat aufweist, weshalb mehr von der Detailseite angezeigt wird.

Informationen zum Steuern des Navigationsverhaltens finden Sie unter [Steuern des Anzeigeverhaltens der Detailseite](#controlling-the-detail-page-display-behavior).

## <a name="creating-a-masterdetailpage"></a>Erstellen einer MasterDetailPage-Klasse

Eine [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse enthält die Eigenschaften [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) und [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) vom Typ [`Page`](xref:Xamarin.Forms.Page), die beide jeweils dazu verwendet werden, die Master- und Detailseiten abzurufen und festzulegen.

> [!IMPORTANT]
> Eine [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse fungiert als Stammseite. Wenn Sie sie für andere Seitentypen als untergeordnete Seite verwenden, kann dies zu unerwartetem und inkonsistentem Verhalten führen. Außerdem wird empfohlen, dass die Masterseite einer [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse immer eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanz ist und die Detailseite nur mit Instanzen von [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) und `ContentPage` aufgefüllt wird. Dadurch wird eine konsistente Benutzerumgebung auf allen Plattformen gewährleistet.

Im folgenden XAML-Codebeispiel wird eine [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse veranschaulicht, die die Eigenschaften [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) und [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) festlegt:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Im folgenden Codebeispiel wird die entsprechende [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse in C# dargestellt:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

Die [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master)-Eigenschaft wird auf eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanz festgelegt. Die [`MasterDetailPage.Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)-Eigenschaft wird auf eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse festgelegt, die eine `ContentPage`-Instanz enthält.

### <a name="creating-the-master-page"></a>Erstellen der Masterseite

Im folgenden XAML-Codebeispiel wird die Deklaration des `MasterPage`-Objekts gezeigt, auf das von der [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master)-Eigenschaft verwiesen wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             IconImageSource="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Die Seite besteht aus einer [`ListView`](xref:Xamarin.Forms.ListView)-Klasse, die in XAML mit Daten gefüllt wird, indem ihre [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)-Eigenschaft auf ein Array von `MasterPageItem`-Instanzen festgelegt wird. Jede `MasterPageItem`-Instanz definiert die Eigenschaften `Title`, `IconSource` und `TargetType`.

Der Eigenschaft [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) wird eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zugewiesen, um alle `MasterPageItem`-Instanzen anzuzeigen. Die `DataTemplate`-Klasse enthält eine [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Klasse, die aus den Klassen [`Image`](xref:Xamarin.Forms.Image) und [`Label`](xref:Xamarin.Forms.Label) besteht. Für jede `MasterPageItem`-Instanz zeigt die [`Image`](xref:Xamarin.Forms.Image)-Klasse den Eigenschaftswert `IconSource` und die [`Label`](xref:Xamarin.Forms.Label)-Klasse den Eigenschaftswert `Title` an.

Die [`Title`](xref:Xamarin.Forms.Page.Title)- und [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource)-Eigenschaften der Seite sind festgelegt. Das Symbol wird auf der Detailseite angezeigt, wenn sie über eine Titelleiste verfügt. Unter iOS muss dies aktiviert werden, indem Sie die Detailseiteninstanz mit einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz umschließen.

> [!NOTE]
> Die [`Title`](xref:Xamarin.Forms.Page.Title)-Eigenschaft der [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master)-Seite muss festgelegt sein, da sonst eine Ausnahme ausgelöst wird.

Im folgenden Codebeispiel wird die Erstellung der entsprechenden Seite in C# dargestellt:

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    IconImageSource = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

In den folgenden Screenshots wird die Masterseite auf den jeweiligen Plattformen veranschaulicht:

![](master-detail-page-images/masterpage.png "Master Page Example")

### <a name="creating-and-displaying-the-detail-page"></a>Erstellen und Anzeigen der Detailseite

Die `MasterPage`-Instanz enthält eine `ListView`-Eigenschaft, die die [`ListView`](xref:Xamarin.Forms.ListView)-Instanz zur Verfügung stellt, damit die [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Instanz von `MainPage` einen Ereignishandler zum Verarbeiten des [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignisses registrieren kann. Dies ermöglicht der `MainPage`-Instanz, die [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)-Eigenschaft auf die Seite festzulegen, die das ausgewählte `ListView`-Element darstellt. Im folgenden Codebeispiel wird der Ereignishandler gezeigt:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

Die `OnItemSelected`-Methode führt die folgenden Aktionen aus:

- Sofern sie nicht den Wert `null` aufweist, ruft sie das [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)-Element aus der [`ListView`](xref:Xamarin.Forms.ListView)-Instanz ab und legt die Detailseite auf eine neue Instanz des Seitentyps fest, der in der `TargetType`-Eigenschaft der `MasterPageItem`-Klasse enthalten ist. Der Seitentyp wird in einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz umschlossen, um sicherzustellen, dass das Symbol, auf das mit der [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource)-Eigenschaft von `MasterPage` verwiesen wird, unter iOS auf der Detailseite angezeigt wird.
- Für das in der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse ausgewählte Element wird `null` festgelegt, um sicherzustellen, dass keines der `ListView`-Elemente beim nächsten Mal ausgewählt wird, wenn `MasterPage` angezeigt wird.
- Die Detailseite wird dem Benutzer angezeigt, indem die [`MasterDetailPage.IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented)-Eigenschaft auf `false` festgelegt wird. Diese Eigenschaft steuert, ob die Master- oder die Detailseite angezeigt wird. Sie sollte auf `true` festgelegt werden, um die Masterseite anzuzeigen, oder auf `false`, um die Detailseite anzuzeigen.

In den folgenden Screenshots wird die Detailseite `ContactPage` gezeigt, die angezeigt wird, wenn sie auf der Masterseite ausgewählt wurde:

![](master-detail-page-images/detailpage.png "Detail Page Example")

### <a name="controlling-the-detail-page-display-behavior"></a>Steuern des Anzeigeverhaltens der Detailseite

Wie die [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse die Master- und Detailseiten verwaltet hängt davon ab, ob die Anwendung auf einem Smartphone oder Tablet ausgeführt wird, wie das Gerät ausgerichtet ist und welchen Wert die [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)-Eigenschaft aufweist. Diese Eigenschaft bestimmt, wie die Detailseite angezeigt wird. Folgende Werte sind möglich:

- **Default:** Die Seiten werden gemäß des plattformspezifischen Standards angezeigt.
- **Popover:** Die Detailseite verdeckt die Masterseite vollständig oder teilweise.
- **Split:** Die Masterseite wird auf der linken Seite angezeigt und die Detailseite auf der rechten.
- **SplitOnLandscape:** Wenn das Gerät sich im Querformat befindet, wird der Bildschirm mittels Bildschirmaufteilung aufgeteilt.
- **SplitOnPortrait:** Wenn das Gerät sich im Hochformat befindet, wird der Bildschirm mittels Bildschirmaufteilung aufgeteilt.

Im folgenden XAML-Codebeispiel wird gezeigt, wie die [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)-Eigenschaft einer [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse festgelegt wird:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Im folgenden Codebeispiel wird die entsprechende [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse in C# dargestellt:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

Allerdings wirkt sich der Wert der [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)-Eigenschaft nur auf Anwendungen aus, die auf einem Tablet oder Desktop ausgeführt werden. Anwendungen, die auf Smartphones ausgeführt werden, weisen immer das Verhalten *Popover* auf.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie eine [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse verwenden und zwischen den Seiten mit Informationen navigieren. `MasterDetailPage` ist eine Xamarin.Forms-Seite, die zwei verwandte Seiten mit Informationen verwaltet. Eine davon ist die Masterseite, die Elemente darstellt, und die andere ist eine Detailseite, die Informationen zu Elementen auf der Masterseite darstellt.

## <a name="related-links"></a>Verwandte Links

- [Page Varieties (Seitenvarianten)](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (Beispiel für die MasterDetailPage-Klasse)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
