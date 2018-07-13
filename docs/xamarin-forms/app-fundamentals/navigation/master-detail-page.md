---
title: Xamarin.Forms-Master-Detail-Seite
description: Die Xamarin.Forms-MasterDetailPage ist eine Seite, die verwaltet zwei verwandte Seiten mit Informationen – eine Masterseite, die Elemente darstellt, und eine Detailseite, die Details zu Elementen auf der Masterseite präsentiert. In diesem Artikel wird erläutert, wie eine MasterDetailPage und Navigieren zwischen den Seiten mit Informationen.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: a3d0edbd933339ee8b8a0a277a4f2493cc8dc70e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997464"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms-Master-Detail-Seite

_Die Xamarin.Forms-MasterDetailPage ist eine Seite, die verwaltet zwei verwandte Seiten mit Informationen – eine Masterseite, die Elemente darstellt, und eine Detailseite, die Details zu Elementen auf der Masterseite präsentiert. In diesem Artikel wird erläutert, wie eine MasterDetailPage und Navigieren zwischen den Seiten mit Informationen._

## <a name="overview"></a>Übersicht

Eine Masterseite zeigt eine Liste von Elementen, in der Regel an, wie in den folgenden Screenshots gezeigt:

[![](master-detail-page-images/masterpage-components.png "Komponenten der Master")](master-detail-page-images/masterpage-components-large.png#lightbox "Masterkomponenten-Seite")

Der Speicherort der die Liste der Elemente auf jeder Plattform identisch ist, und wählen eines der Elemente mit der entsprechenden Detailseite navigiert. Darüber hinaus bietet die Masterseite außerdem eine Navigationsleiste, die eine Schaltfläche, die verwendet werden können enthält, um zur Seite aktiv – Details zu navigieren:

- Unter iOS die Navigationsleiste am oberen Rand der Seite vorhanden ist und verfügt über eine Schaltfläche, die auf der Seite "Details" navigiert. Darüber hinaus kann die Seite aktiv – Details zu der navigiert durch Wischen von der Masterseite auf der linken Seite.
- Unter Android wird die Navigationsleiste am oberen Rand der Seite vorhanden ist und einen Titel, ein Symbol und eine Schaltfläche, die navigiert zur Detailseite angezeigt. Das Symbol wird definiert, der `[Activity]` -Attribut, das ergänzt die `MainActivity` Klasse im plattformspezifischen Projekt Android. Darüber hinaus die aktive Seite kann navigiert werden, durch Wischen von der Masterseite auf der linken Seite, indem Sie auf der Detailseite am rechten Rand des Bildschirms und durch Tippen auf die *wieder* am unteren Rand des Bildschirms.
- Auf der universellen Windows-Plattform (UWP), die Navigationsleiste am oberen Rand der Seite vorhanden ist und verfügt über eine Schaltfläche, die auf der Seite "Details" navigiert.

Eine Seite zeigt Detaildaten, die das dem Element entspricht, der auf dem Masterknoten Seite ausgewählt, und die wichtigsten Komponenten von der Seite "Details" werden in den folgenden Screenshots dargestellt:

![](master-detail-page-images/detailpage-components.png "Detail-Seite-Komponenten")

Die Seite "Details" enthält eine Navigationsleiste, deren Inhalt plattformabhängig sind:

- Unter iOS, die Navigationsleiste am oberen Rand der Seite vorhanden ist und zeigt einen Titel, und verfügt über eine Schaltfläche, die auf die Masterseite zurückgibt, vorausgesetzt, dass die Seiteninstanz Detail umschlossen wird die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz. Darüber hinaus kann die Masterseite durch Wischen von der Detailseite auf der rechten Seite, zurückgegeben werden.
- Unter Android eine Navigationsleiste am oberen Rand der Seite vorhanden ist, und zeigt einen Titel, ein Symbol und eine Schaltfläche, die auf die Masterseite zurückgibt. Das Symbol wird definiert, der `[Activity]` -Attribut, das ergänzt die `MainActivity` Klasse im plattformspezifischen Projekt Android.
- Die Navigationsleiste auf UWP am oberen Rand der Seite vorhanden ist und zeigt einen Titel und eine Schaltfläche, die auf die Masterseite zurückgibt.

### <a name="navigation-behavior"></a>Navigationsverhalten

Das Verhalten die Navigation zwischen Master- und Detailtabelle Seiten ist plattformabhängig:

- Unter iOS, die Seite "Details" *Folien* auf der rechten Seite als die Folien Masterseite von der linken Seite und dem linken Teil von Details Seite immer noch sichtbar ist.
- Unter Android werden die Details und Master-Seiten sind *überlagert* voneinander.
- Auf UWP die Detaildaten und Master-Seiten sind *ausgetauscht*.

Ein ähnliches Verhalten wird im Querformat beobachtet werden, außer dass die Masterseite unter iOS und Android eine ähnliche Breite als Masterseite im Hochformat, hat damit mehr von der Seite "Details" angezeigt werden.

Informationen zum Steuern des Verhaltens, finden Sie unter [steuern das Anzeigeverhalten des Detail-Seite](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Erstellen eine MasterDetailPage

Ein [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) enthält [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) und [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Eigenschaften, die beide vom Typ [ `Page` ](xref:Xamarin.Forms.Page), die werden verwendet, um das Abrufen und die Master- und Detailtabelle Seiten bzw. festlegen.

> [!IMPORTANT]
> Ein [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) fungiert als eine Seite "Root", und verwenden Sie, wie eine untergeordnete Seite in anderen Projekttypen, Seite unerwarteten und inkonsistenten Verhalten führen kann. Darüber hinaus, es wird empfohlen, die die Masterseite einer [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) muss immer ein [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Instanz und die Seite nur mit aufgefüllt werden soll [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), und `ContentPage` Instanzen. Dadurch wird um eine konsistente benutzererfahrung auf allen Plattformen zu gewährleisten.

Das folgende XAML-Code-Beispiel zeigt eine [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) festlegt, die die [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) und [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Eigenschaften:

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

Das folgende Codebeispiel zeigt die entsprechende [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) in c# erstellt wurde:

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

Die [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) -Eigenschaftensatz auf eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Instanz. Die [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) -Eigenschaftensatz auf eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) mit einem `ContentPage` Instanz.

### <a name="creating-the-master-page"></a>Erstellen der Masterseite

Im folgende XAML-Codebeispiel zeigt die Deklaration der `MasterPage` -Objekt, das verwiesen wird die [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Eigenschaft:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
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

Die Seite besteht aus einer [ `ListView` ](xref:Xamarin.Forms.ListView) , wird in XAML mit Daten aufgefüllt, durch Festlegen der [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) Eigenschaft in ein Array von `MasterPageItem` Instanzen. Jede `MasterPageItem` definiert `Title`, `IconSource`, und `TargetType` Eigenschaften.

Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zugewiesen wird die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) Eigenschaft verwenden, um jedes `MasterPageItem`. Die `DataTemplate` enthält eine [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) besteht aus einem [ `Image` ](xref:Xamarin.Forms.Image) und ein [ `Label` ](xref:Xamarin.Forms.Label). Die [ `Image` ](xref:Xamarin.Forms.Image) zeigt die `IconSource` Eigenschaftswert, und die [ `Label` ](xref:Xamarin.Forms.Label) zeigt die `Title` Eigenschaftswert für die einzelnen `MasterPageItem`.

Die Seite verfügt über seine [ `Title` ](xref:Xamarin.Forms.Page.Title) und [ `Icon` ](xref:Xamarin.Forms.Page.Icon) festgelegten Eigenschaften. Das Symbol wird auf der Detailseite angezeigt, vorausgesetzt, dass die Detailseite eine Titelleiste verfügt. Dies muss in iOS durch Umschließen der Detail-Seite-Instanz in aktiviert werden eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz.

> [!NOTE]
> Die [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Seite müssen die [ `Title` ](xref:Xamarin.Forms.Page.Title) -Eigenschaft festgelegt werden soll, oder eine Ausnahme ausgelöst wird.

Das folgende Codebeispiel zeigt die entsprechende Seite, die in c# erstellt wurde:

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

Die folgenden Screenshots zeigen die Masterseite auf jeder Plattform:

![](master-detail-page-images/masterpage.png "Master-Seite-Beispiel")

### <a name="creating-and-displaying-the-detail-page"></a>Erstellen und Anzeigen von der Seite "Details"

Die `MasterPage` Instanz enthält eine `ListView` -Eigenschaft, die verfügbar macht seine [ `ListView` ](xref:Xamarin.Forms.ListView) Instanz, damit die `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Instanz registrieren kann ein Ereignishandler zur Behandlung der [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis. Dies ermöglicht die `MainPage` -Instanz die [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Eigenschaft, um die Seite, die dem ausgewählten dargestellt `ListView` Element. Das folgende Codebeispiel zeigt den Ereignishandler an:

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

Die `OnItemSelected` Methode führt die folgenden Aktionen:

- Ruft ab der [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) aus der [ `ListView` ](xref:Xamarin.Forms.ListView) Instanz, und angegeben, dass es nicht `null`, wird von der Detailseite auf eine neue Instanz der den Seitentyp, der in der gespeichert`TargetType`Eigenschaft der `MasterPageItem`. Seitentyp umschlossen ist eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz, um sicherzustellen, dass das Symbol verwiesen wird, über die [ `Icon` ](xref:Xamarin.Forms.Page.Icon) Eigenschaft der `MasterPage` wird angezeigt, auf der Detailseite "in iOS.
- Das ausgewählte Element in der [ `ListView` ](xref:Xamarin.Forms.ListView) nastaven NA hodnotu `null` um sicherzustellen, dass keines der `ListView` Elemente werden beim nächsten ausgewählt der `MasterPage` wird angezeigt.
- Die Seite "Details" wird für den Benutzer angezeigt, durch Festlegen der [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Eigenschaft `false`. Diese Eigenschaft steuert, ob die Master- oder Detail-Seite angezeigt wird. Festgelegt werden sollte, um `true` um die Masterseite anzuzeigen und zu `false` auf die Detailseite angezeigt.

Die folgenden Screenshots zeigen die `ContactPage` Detailseite, die angezeigt wird, nachdem sie auf der Masterseite ausgewählt wurde, ist:

![](master-detail-page-images/detailpage.png "Beispiel für Detail-Seite")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Steuern des Verhaltens der Detail-Seite anzeigen

Wie die [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) verwaltet die Master- und Detailtabelle Seiten gibt an, ob die Anwendung auf einem Smartphone oder Tablet, die Ausrichtung des Geräts und der Wert, der ausgeführt wird, hängt die [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) Diese Eigenschaft. Diese Eigenschaft bestimmt, wie die Seite "Details" angezeigt wird. Die möglichen Werte sind:

- **Standardmäßige** – werden die Seiten angezeigt, unter Verwendung der Plattform.
- **Im Popover** : die Seite mit Details behandelt, oder die Masterseite teilweise abdeckt.
- **Split** – die Masterseite wird auf der linken Seite angezeigt, und die Seite "Details" auf der rechten Seite ist.
- **SplitOnLandscape** – Monitornutzung mittels Split Screen wird verwendet, wenn das Gerät im Querformat verwendet wird.
- **SplitOnPortrait** – Monitornutzung mittels Split Screen wird verwendet, wenn das Gerät im Hochformat befindet.

Im folgende XAML-Codebeispiel veranschaulicht das Festlegen der [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) Eigenschaft für eine [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Das folgende Codebeispiel zeigt die entsprechende [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) in c# erstellt wurde:

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

Allerdings den Wert des der [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) Eigenschaft wirkt sich nur auf Anwendungen, die auf Tablets oder auf dem Desktop ausgeführt wird. Anwendungen, die immer auf Smartphones ausgeführt haben die *Popover* Verhalten.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie Sie mit einem [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) , und navigieren Sie zwischen den Seiten mit Informationen. Die Xamarin.Forms `MasterDetailPage` ist eine Seite, die zwei Seiten verwandter Informationen – eine Masterseite, die Elemente darstellt, und eine Detailseite, die Details zu Elementen auf der Masterseite präsentiert verwaltet.


## <a name="related-links"></a>Verwandte Links

- [Seitenvarianten](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
