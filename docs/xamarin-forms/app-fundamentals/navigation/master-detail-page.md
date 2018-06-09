---
title: Xamarin.Forms Master / Detail-Seite
description: Die Xamarin.Forms-MasterDetailPage ist eine Seite, die verwaltet zwei verwandte Seiten mit Informationen – einer Masterseite, in dem Elemente dargestellt, und eine Detailseite, in dem Informationen zu Elementen auf der Seite "master" dargestellt. In diesem Artikel erläutert, wie eine MasterDetailPage und seine Seiten mit Informationen zu navigieren.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 80d86e1aa6a00d4a55c0fdba1b858bfef7bcbc84
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241343"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms Master / Detail-Seite

_Die Xamarin.Forms-MasterDetailPage ist eine Seite, die verwaltet zwei verwandte Seiten mit Informationen – einer Masterseite, in dem Elemente dargestellt, und eine Detailseite, in dem Informationen zu Elementen auf der Seite "master" dargestellt. In diesem Artikel erläutert, wie eine MasterDetailPage und seine Seiten mit Informationen zu navigieren._

## <a name="overview"></a>Übersicht

Eine Masterseite zeigt eine Liste von Elementen, in der Regel an, wie in den folgenden Screenshots dargestellt:

[![](master-detail-page-images/masterpage-components.png "Master-Page-Komponenten")](master-detail-page-images/masterpage-components-large.png#lightbox "Master Page-Komponenten")

Der Speicherort der Liste der Elemente auf jeder Plattform identisch ist, und wählen eines der Elemente, wechseln Sie automatisch zur entsprechenden Detailseite. Darüber hinaus bietet die Masterseite auch eine Navigationsleiste, die eine Schaltfläche, die verwendet werden kann enthält, um zur Detailseite active zu navigieren:

- Bei iOS kann die Navigationsleiste am oberen Rand der Seite "vorhanden ist und verfügt über eine Schaltfläche, die zur Detailseite navigiert. Darüber hinaus kann active Detailseite navigiert werden durch ein Lesegerät Masterseite auf der linken Seite.
- Auf Android-Geräten die Navigationsleiste am oberen Rand der Seite vorhanden ist, und zeigt einen Titel, ein Symbol und eine Schaltfläche, die navigiert zur Detailseite. Das Symbol wird definiert, der `[Activity]` -Attribut, das ergänzt die `MainActivity` Klasse im Android plattformspezifische Projekt. Darüber hinaus die aktiven Detailseite kann navigiert werden, durch ein Lesegerät Masterseite auf der linken Seite, tippen Sie auf der Detailseite am rechten Rand des Bildschirms und durch Tippen auf die *wieder* am unteren Rand des Bildschirms.
- Auf die universelle Windows Plattform (UWP) die Navigationsleiste am oberen Rand der Seite "vorhanden ist und verfügt über eine Schaltfläche, die zur Detailseite navigiert.

Eine Seite zeigt Detaildaten, die das Element entspricht, der auf dem masterzielserver Seite ausgewählt, und die Hauptkomponenten der Seite "Details" in den folgenden Screenshots dargestellt werden:

![](master-detail-page-images/detailpage-components.png "Detail Page-Komponenten")

Die Seite "Details" enthält eine Navigationsleiste, deren Inhalt plattformabhängige sind:

- Bei iOS kann die Navigationsleiste am oberen Rand der Seite "vorhanden ist und zeigt einen Titel, und verfügt über eine Schaltfläche, die auf der Masterseite zurückgibt, vorausgesetzt, dass die Seiteninstanz Detail umschlossen wird die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz. Darüber hinaus kann die Gestaltungsvorlage Streifen Detailseite nach rechts um zurückgegeben werden.
- Auf Android-Geräten eine Navigationsleiste am oberen Rand der Seite vorhanden ist, und zeigt einen Titel, ein Symbol und eine Schaltfläche, die auf der Masterseite zurückgibt. Das Symbol wird definiert, der `[Activity]` -Attribut, das ergänzt die `MainActivity` Klasse im Android plattformspezifische Projekt.
- Die Navigationsleiste auf UWP am oberen Rand der Seite "vorhanden ist und einen Titel angezeigt und verfügt über eine Schaltfläche, die auf der Masterseite zurückgibt.

### <a name="navigation-behavior"></a>Navigationsverhalten

Das Verhalten der Darstellung Navigation zwischen Master- und Detailtabelle Seiten ist plattformabhängig:

- Bei iOS kann die Seite "Details" *Folien* rechts aus der linken Seite und dem linken Teil des Details als die Masterseite Folien Seite immer noch sichtbar ist.
- Auf Android-Geräten die Detail- und Master-Seiten sind *Rasterquadrat* voneinander abhängig.
- Für universelle Windows-Plattform, die Detaildaten und Master-Seiten sind *ausgetauscht*.

Ein ähnliches Verhalten wird im Querformat, beachtet werden, außer dass die Gestaltungsvorlage auf IOS- und Android eine ähnliche Breite als Masterseite im Hochformat, hat damit mehrere der Detailseite angezeigt werden.

Informationen zum Steuern der Navigationsverhalten, finden Sie unter [steuern das Verhalten Seite Details](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Erstellen eine MasterDetailPage

Ein [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) enthält [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) und [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Eigenschaften, die sowohl vom Typ sind [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), die werden verwendet, um das Abrufen und die Master- und Detailtabelle Seiten bzw. festlegen.

> [!IMPORTANT]
> Ein [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) soll eine Seite "Root", und als untergeordnete Seite in anderen Seitentypen unerwartete und inkonsistentes Verhalten führen kann. Darüber hinaus, es wird empfohlen, die die Masterseite von einer [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) muss immer ein [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) -Instanz, und die Seite "Details" nur mit aufgefüllt werden soll [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), und `ContentPage` Instanzen. Dies hilft, um eine konsistente benutzerumgebung für alle Plattformen sicherzustellen.

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) festlegt, die die [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) und [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Eigenschaften:

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

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) in c# erstellt:

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

Die [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) -Eigenschaftensatz auf eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanz. Die [ `MasterDetailPage.Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) -Eigenschaftensatz auf eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) , enthält eine `ContentPage` Instanz.

### <a name="creating-the-master-page"></a>Erstellen der Masterseite

Das folgende XAML-Code-Beispiel zeigt die Deklaration von der `MasterPage` -Objekt, das über verwiesen wird, ist die [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Eigenschaft:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView">
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

Besteht aus die Seite eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , die mit Daten in XAML aufgefüllt wird, durch Festlegen seiner [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) Eigenschaft auf ein Array von `MasterPageItem` Instanzen. Jede `MasterPageItem` definiert `Title`, `IconSource`, und `TargetType` Eigenschaften.

Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zugewiesen ist die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) -Eigenschaft, um jeden anzuzeigen `MasterPageItem`. Die `DataTemplate` enthält eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) , besteht eine [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) und ein [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/). Die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zeigt die `IconSource` Eigenschaftswert, und die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zeigt die `Title` Eigenschaftswert für die einzelnen `MasterPageItem`.

Die Seite verfügt über seine [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) und [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) festgelegten Eigenschaften. Das Symbol wird auf der Detailseite angezeigt, vorausgesetzt, dass die Detailseite eine Titelleiste hat. Dies muss auf iOS durch wrapping die Seiteninstanz Detail in aktiviert werden eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz.

> [!NOTE]
> Die [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Seite benötigen ihre [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) -Eigenschaft festgelegt werden soll, oder eine Ausnahme ausgelöst wird.

Im folgenden Codebeispiel wird gezeigt, die entsprechende Seite, die in c# erstellt wird:

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

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

Die folgenden Screenshots zeigen die Gestaltungsvorlage auf jeder Plattform:

![](master-detail-page-images/masterpage.png "Master-Seite-Beispiel")

### <a name="creating-and-displaying-the-detail-page"></a>Erstellen und Anzeigen der Seite "Details"

Die `MasterPage` Instanz enthält eine `ListView` Eigenschaft, die verfügbar macht seine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Instanz, damit die `MainPage` [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Instanz registrieren kann ein Ereignishandler zur Behandlung der [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Ereignis. Dies ermöglicht die `MainPage` -Instanz die [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Eigenschaft, um die Seite, die dem ausgewählten darstellt `ListView` Element. Im folgenden Codebeispiel wird der Ereignishandler veranschaulicht:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.ListView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.ListView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

Die `OnItemSelected` Methode führt die folgenden Aktionen aus:

- Ruft ab die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) aus der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) -Instanz und angegeben, dass es nicht `null`, legt die Detailseite auf eine neue Instanz des Typs Seite in der gespeichert`TargetType`Eigenschaft von der `MasterPageItem`. Seitentyp umschlossen ist eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz, um sicherzustellen, dass das Symbol "verwiesen wird, über die [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) Eigenschaft auf die `MasterPage` wird auf der Detailseite in iOS angezeigt.
- Das ausgewählte Element in der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) auf festgelegt ist `null` um sicherzustellen, dass keines der `ListView` Elemente werden beim nächsten ausgewählt der `MasterPage` erhält.
- Die Seite "Details" erhält der Benutzer durch Festlegen der [ `MasterDetailPage.IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) Eigenschaft `false`. Diese Eigenschaft steuert, ob die Seite Master oder Detail angezeigt werden. Es sollte festgelegt werden, um `true` zum Anzeigen der Masterseite und `false` auf der Seite "Details" angezeigt.

Die folgenden Screenshots zeigen die `ContactPage` Seite "Details", der angezeigt wird, nachdem es für die Gestaltungsvorlage ausgewählt wurde, wird:

![](master-detail-page-images/detailpage.png "Beispiel für Detail-Seite")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Steuern des Verhaltens der Detail-Seite anzeigen

Wie die [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) verwaltet die Master- und Detailtabelle Seiten richtet sich nach, ob die Anwendung, auf einem Telefon oder Tablet, die Ausrichtung des Geräts und der Wert ausgeführt wird der [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) Diese Eigenschaft. Diese Eigenschaft bestimmt, wie die Detailseite angezeigt werden sollen. Die möglichen Werte sind:

- **Standardmäßige** – werden die Seiten mit der Plattform standardmäßig angezeigt.
- **Popover** – die Seite "Details" behandelt, oder die Gestaltungsvorlage teilweise abdeckt.
- **Split** – die Masterseite wird auf der linken Seite angezeigt, und die Seite "Details" auf der rechten Seite ist.
- **SplitOnLandscape** – eine geteilten Ansicht wird verwendet, wenn das Gerät im Querformat ist.
- **SplitOnPortrait** – eine geteilten Ansicht wird verwendet, wenn das Gerät im Hochformat befindet.

Im folgende Codebeispiel für die Verwendung von XAML-veranschaulicht das Festlegen der [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) Eigenschaft auf einen [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) in c# erstellt:

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

Allerdings den Wert der [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) Eigenschaft betrifft nur Anwendungen, die auf Tablets oder auf dem Desktop ausgeführt wird. Anwendungen, die immer auf Telefonen ausgeführt haben die *Popover* Verhalten.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) , und navigieren Sie zwischen den Seiten von Informationen. Der Xamarin.Forms `MasterDetailPage` eine Seite, die verwaltet zwei Seiten verwandter Informationen – einer Masterseite, in dem Elemente dargestellt, und eine Detailseite, in dem Informationen zu Elementen auf der Seite "master" dargestellt wird.


## <a name="related-links"></a>Verwandte Links

- [Seite "-Varianten](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)
