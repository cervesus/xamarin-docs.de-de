---
title: ListView-Interaktivität
description: In diesem Artikel wird erläutert, wie Sie einer xamarin. Forms-ListView Interaktivität hinzufügen, indem Sie Auswahl, Kontext Aktionen und Pull-to-refresh-Vorgänge implementieren.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: aa717792bdaefe24d957c9781934933b67aaf92b
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696875"
---
# <a name="listview-interactivity"></a>ListView-Interaktivität

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Die xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) -Klasse unterstützt die Benutzerinteraktion mit den Daten, die Sie präsentiert.

## <a name="selection-and-taps"></a>Auswahl und tippen

Der [`ListView`](xref:Xamarin.Forms.ListView) Auswahlmodus wird gesteuert, indem die [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaft auf einen Wert der [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) -Enumeration festgelegt wird:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) gibt an, dass ein einzelnes Element ausgewählt werden kann, wobei das ausgewählte Element hervorgehoben wird. Dies ist der Standardwert.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) gibt an, dass Elemente nicht ausgewählt werden können.

Wenn ein Benutzer auf ein Element tippt, werden zwei Ereignisse ausgelöst:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) ausgelöst, wenn ein neues Element ausgewählt wird.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) ausgelöst, wenn ein Element getippt wird.

Wenn Sie zweimal auf dasselbe Element tippen, werden zwei [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse ausgelöst, es wird jedoch nur ein einzelnes [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis ausgelöst.

> [!NOTE]
> Die [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) -Klasse, die die Ereignis Argumente für das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) -Ereignis enthält, verfügt über [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) und [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) Eigenschaften sowie über eine `ItemIndex`-Eigenschaft, deren Wert den Index im [0](xref:Xamarin.Forms.ListView) des gezapften Elements darstellt. Ebenso hat die [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) -Klasse, die die Ereignis Argumente für das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis enthält, eine [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) -Eigenschaft und eine `SelectedItemIndex`-Eigenschaft, deren Wert den Index im `ListView` des ausgewählten Elements darstellt.

Wenn die [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaft auf [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)festgelegt ist, können Elemente im [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt werden, die [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -und [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) -Ereignisse werden ausgelöst, und die [1](xref:Xamarin.Forms.ListView.SelectedItem) -Eigenschaft wird auf den Wert des ausgewählten Position.

Wenn die [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaft auf [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)festgelegt ist, können Elemente im [`ListView`](xref:Xamarin.Forms.ListView) nicht ausgewählt werden, das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis wird nicht ausgelöst, und die [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) -Eigenschaft bleibt 0. [@No__t_1](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse werden jedoch weiterhin ausgelöst, und das angezeigte Element wird während des Tap kurz hervorgehoben.

Wenn ein Element ausgewählt wurde und die [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaft von [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) in [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)geändert wird, wird die Eigenschaft [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) auf `null` festgelegt, und das [0](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis wird mit einem 1 Element ausgelöst.

Die folgenden Screenshots zeigen eine [`ListView`](xref:Xamarin.Forms.ListView) mit dem Standardauswahl Modus:

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>Auswahl deaktivieren

Legen Sie die Eigenschaft [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) auf [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)fest, um [`ListView`](xref:Xamarin.Forms.ListView) Auswahl zu deaktivieren:

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>Kontextaktionen

Häufig möchten Benutzeraktionen für ein Element in einer `ListView` durchführen. Stellen Sie sich z. b. eine Liste von e-Mails in der Mail-APP vor. Unter IOS können Sie eine Nachricht löschen, um eine Nachricht zu löschen:

![](interactivity-images/context-default.png "ListView with Context Actions")

Kontext Aktionen können in C# und XAML implementiert werden. Unten finden Sie bestimmte Leitfäden für beide, aber zunächst sehen wir uns einige wichtige Implementierungsdetails für beides an.

Kontext Aktionen werden mithilfe von `MenuItem` Elementen erstellt. Tap-Ereignisse für `MenuItems` Objekte werden von der `MenuItem` selbst, nicht vom `ListView` ausgelöst. Dies unterscheidet sich von der Behandlung von TAP-Ereignissen für Zellen, bei denen der `ListView` das Ereignis anstelle der Zelle auslöst. Da der `ListView` das-Ereignis aufhebt, erhält der zugehörige Ereignishandler wichtige Informationen, wie z. b. welches Element ausgewählt oder abgetippt wurde.

Standardmäßig kann eine `MenuItem` nicht wissen, zu welcher Zelle Sie gehört. Die `CommandParameter`-Eigenschaft ist auf `MenuItem` verfügbar, um Objekte zu speichern, z. b. das-Objekt hinter dem `ViewCell` des `MenuItem`. Die `CommandParameter`-Eigenschaft kann sowohl in XAML als auch C#in festgelegt werden.

### <a name="xaml"></a>XAML

`MenuItem` Elemente können in einer XAML-Auflistung erstellt werden. Der folgende XAML-Code veranschaulicht eine benutzerdefinierte Zelle mit zwei implementierten Kontext Aktionen:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore"
                      CommandParameter="{Binding .}"
                      Text="More" />
            <MenuItem Clicked="OnDelete"
                      CommandParameter="{Binding .}"
                      Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

Stellen Sie in der Code Behind-Datei sicher, dass die `Clicked`-Methoden implementiert werden:

```csharp
public void OnMore (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> Der `NavigationPageRenderer` für Android verfügt über eine über schreibbare `UpdateMenuItemIcon` Methode, die zum Laden von Symbolen aus einem benutzerdefinierten `Drawable` verwendet werden kann. Diese außer Kraft Setzung ermöglicht es, SVG-Images als Symbole auf `MenuItem`-Instanzen unter Android zu verwenden.

### <a name="code"></a>Code

Kontext Aktionen können in jeder `Cell` Unterklasse implementiert werden (sofern Sie nicht als Gruppen Header verwendet wird), indem `MenuItem` Instanzen erstellt und der `ContextActions`-Auflistung für die Zelle hinzugefügt werden. Sie verfügen über die folgenden Eigenschaften, die für die Kontext Aktion konfiguriert werden können:

- **Text** &ndash; die Zeichenfolge, die im Menü Element angezeigt wird.
- **Klicken** Sie auf &ndash; das Ereignis, wenn auf das Element geklickt wird.
- **Isdestruktiv** &ndash; (optional) Wenn true, wird das Element unter IOS anders gerendert.

Einer Zelle können mehrere Kontext Aktionen hinzugefügt werden, es muss jedoch nur eine `IsDestructive` auf `true` festgelegt sein. Der folgende Code veranschaulicht, wie einem `ViewCell` Kontext Aktionen hinzugefügt werden:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

## <a name="pull-to-refresh"></a>Pull zum Aktualisieren

Die Benutzer werden davon ausgehen, dass durch das Abrufen einer Liste mit Daten die Liste aktualisiert wird. Das [`ListView`](xref:Xamarin.Forms.ListView) -Steuerelement unterstützt diese standardmäßig. Legen Sie zum Aktivieren der Pull-to-refresh-Funktionalität [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) auf `true` fest:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Der entsprechende C#-Code lautet:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Während der Aktualisierung wird ein Spinner angezeigt, der standardmäßig schwarz ist. Die spinfarbe kann jedoch unter IOS und Android geändert werden, indem die `RefreshControlColor`-Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color)festgelegt wird:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

Der entsprechende C#-Code lautet:

```csharp
listView.RefreshControlColor = Color.Red;
```

Die folgenden Screenshots zeigen Pull-to-refresh-Vorgänge, während der Benutzer den Abruf durchläuft:

![](interactivity-images/refresh-start.png "ListView Pull to Refresh In-Progress")

Die folgenden Screenshots zeigen Pull-to-refresh, nachdem der Benutzer den Pull-Vorgang freigegeben hat, während der Spinner angezeigt wird, während die [`ListView`](xref:Xamarin.Forms.ListView) aktualisiert wird:

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`ListView`](xref:Xamarin.Forms.ListView) löst das [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) Ereignis aus, um die Aktualisierung zu initiieren, und die [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) -Eigenschaft wird auf `true` festgelegt. Der Code, der erforderlich ist, um den Inhalt des `ListView` zu aktualisieren, sollte dann vom Ereignishandler für das `Refreshing` Ereignis oder von der Methode ausgeführt werden, die vom [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)ausgeführt wird. Nachdem der `ListView` aktualisiert wurde, sollte die Eigenschaft `IsRefreshing` auf `false` festgelegt werden, oder die [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) -Methode sollte aufgerufen werden, um anzugeben, dass die Aktualisierung beendet ist.

> [!NOTE]
> Wenn Sie einen [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)definieren, kann die `CanExecute`-Methode des Befehls angegeben werden, um den Befehl zu aktivieren oder zu deaktivieren.

## <a name="detect-scrolling"></a>Scrollen erkennen

[`ListView`](xref:Xamarin.Forms.ListView) definiert ein `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das folgende XAML-Beispiel zeigt eine `ListView`, die einen Ereignishandler für das `Scrolled`-Ereignis festlegt:

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

Der entsprechende C#-Code lautet:

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

In diesem Codebeispiel wird der `OnListViewScrolled` Ereignishandler ausgeführt, wenn das `Scrolled`-Ereignis ausgelöst wird:

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

Der `OnListViewScrolled`-Ereignishandler gibt die Werte des `ScrolledEventArgs` Objekts aus, das das Ereignis begleitet.

## <a name="related-links"></a>Verwandte Links

- [ListView-Interaktivität (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
