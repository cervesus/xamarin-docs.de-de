---
title: ''
description: In diesem Artikel wird erläutert, wie Sie einer ListView Interaktivität hinzufügen Xamarin.Forms , indem Sie Auswahl, Kontext Aktionen und Pull-to-refresh-Vorgänge implementieren.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5142965216b328172ae7fa04cdc0c13590f5ff38
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139885"
---
# <a name="listview-interactivity"></a>ListView-Interaktivität

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Die- Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Klasse unterstützt die Benutzerinteraktion mit den Daten, die Sie präsentiert.

## <a name="selection-and-taps"></a>Auswahl und tippen

Der [`ListView`](xref:Xamarin.Forms.ListView) Auswahlmodus wird gesteuert, indem die- [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) :

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)Gibt an, dass ein einzelnes Element ausgewählt werden kann, wobei das ausgewählte Element hervorgehoben wird. Dies ist der Standardwert.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)Gibt an, dass Elemente nicht ausgewählt werden können.

Wenn ein Benutzer auf ein Element tippt, werden zwei Ereignisse ausgelöst:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)wird ausgelöst, wenn ein neues Element ausgewählt wird.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)wird ausgelöst, wenn ein Element getippt wird.

Wenn Sie zweimal auf dasselbe Element tippen, werden zwei [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse ausgelöst, aber nur ein einzelnes [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis.

> [!NOTE]
> Die [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) -Klasse, die die Ereignis Argumente für das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) -Ereignis, die [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) -Eigenschaft und die- [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) Eigenschaft sowie eine-Eigenschaft enthält, `ItemIndex` deren Wert den Index in der [`ListView`](xref:Xamarin.Forms.ListView) des gezapften Elements darstellt. Entsprechend verfügt die [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) -Klasse, die die Ereignis Argumente für das- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis enthält, über eine [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) -Eigenschaft und eine- `SelectedItemIndex` Eigenschaft, deren Wert den Index in der `ListView` des ausgewählten Elements darstellt.

Wenn die [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaft auf festgelegt ist [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) , können Elemente in [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt werden, das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis und das-Ereignis werden ausgelöst [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) , und die- [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft wird auf den Wert des ausgewählten Elements festgelegt.

Wenn die- [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft auf festgelegt ist [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) , können Elemente in der nicht [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt werden, das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis wird nicht ausgelöst, und die- [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft bleibt erhalten `null` . Ereignisse werden jedoch weiterhin ausgelöst, [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) und das angezeigte Element wird während des Tap kurz hervorgehoben.

Wenn ein Element ausgewählt wurde und die- [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft von in geändert [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) wird [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) , wird die [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) -Eigenschaft auf festgelegt, `null` und das- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis wird mit einem- `null` Element ausgelöst.

Die folgenden Screenshots zeigen eine [`ListView`](xref:Xamarin.Forms.ListView) mit dem Standardauswahl Modus:

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>Auswahl deaktivieren

Zum Deaktivieren [`ListView`](xref:Xamarin.Forms.ListView) der Auswahl legen [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) Sie die-Eigenschaft auf fest [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) :

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>Kontextaktionen

Häufig möchten Benutzeraktionen für ein Element in einem ausführen `ListView` . Stellen Sie sich z. b. eine Liste von e-Mails in der Mail-APP vor. Unter IOS können Sie eine Nachricht löschen, um eine Nachricht zu löschen:

![](interactivity-images/context-default.png "ListView with Context Actions")

Kontext Aktionen können in c# und XAML implementiert werden. Unten finden Sie bestimmte Leitfäden für beide, aber zunächst sehen wir uns einige wichtige Implementierungsdetails für beides an.

Kontext Aktionen werden mit- `MenuItem` Elementen erstellt. Tap-Ereignisse für `MenuItems` Objekte werden von der `MenuItem` selbst, nicht von ausgelöst `ListView` . Dies unterscheidet sich von der Behandlung von TAP-Ereignissen für Zellen, bei denen das `ListView` Ereignis anstelle der Zelle auslöst. Da der `ListView` das-Ereignis aufhebt, erhält der zugehörige Ereignishandler wichtige Informationen, wie z. b. welches Element ausgewählt oder getippt wurde.

Standardmäßig kann eine `MenuItem` nicht wissen, zu welcher Zelle Sie gehört. Die `CommandParameter` -Eigenschaft ist in verfügbar `MenuItem` , um-Objekte zu speichern, z. b. das-Objekt hinter dem `MenuItem` `ViewCell` . Die `CommandParameter` -Eigenschaft kann sowohl in XAML als auch in c# festgelegt werden.

### <a name="xaml"></a>XAML

`MenuItem`-Elemente können in einer XAML-Auflistung erstellt werden. Der folgende XAML-Code veranschaulicht eine benutzerdefinierte Zelle mit zwei implementierten Kontext Aktionen:

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

Stellen Sie in der Code Behind-Datei sicher, dass die `Clicked` Methoden implementiert werden:

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
> Die `NavigationPageRenderer` für Android verfügt über eine über schreibbare `UpdateMenuItemIcon` Methode, die zum Laden von Symbolen aus einem benutzerdefinierten verwendet werden kann `Drawable` . Diese außer Kraft Setzung ermöglicht die Verwendung von SVG-Bildern als Symbole auf `MenuItem` Instanzen unter Android.

### <a name="code"></a>Code

Kontext Aktionen können in jeder `Cell` Unterklasse implementiert werden (sofern Sie nicht als Gruppen Header verwendet wird) `MenuItem` , indem Instanzen erstellt und der-Auflistung `ContextActions` für die Zelle hinzugefügt werden. Sie verfügen über die folgenden Eigenschaften, die für die Kontext Aktion konfiguriert werden können:

- **Text** &ndash; die Zeichenfolge, die im Menü Element angezeigt wird.
- **Geklickt** &ndash; das Ereignis, wenn auf das Element geklickt wird.
- **Isdestruktiv** &ndash; (optional) Wenn true, wird das Element unter IOS anders gerendert.

Einer Zelle können mehrere Kontext Aktionen hinzugefügt werden, aber nur eine muss `IsDestructive` auf festgelegt sein `true` . Der folgende Code veranschaulicht, wie Kontext Aktionen zu einem hinzugefügt werden `ViewCell` :

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

## <a name="pull-to-refresh"></a>Aktualisierung durch Ziehen

Die Benutzer werden davon ausgehen, dass durch das Abrufen einer Liste mit Daten die Liste aktualisiert wird. Das- [`ListView`](xref:Xamarin.Forms.ListView) Steuerelement unterstützt diese standardmäßig. Legen Sie zum Aktivieren der Pull-to-refresh-Funktionalität auf Folgendes fest [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) `true` :

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Der entsprechende C#-Code lautet:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Während der Aktualisierung wird ein Spinner angezeigt, der standardmäßig schwarz ist. Die spinfarbe kann jedoch unter IOS und Android geändert werden, indem die- `RefreshControlColor` Eigenschaft auf festgelegt wird [`Color`](xref:Xamarin.Forms.Color) :

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

Die folgenden Screenshots zeigen die Pull-zu-aktualisieren, nachdem der Benutzer den Pull losgelassen hat, während der Spinner angezeigt wird, während [`ListView`](xref:Xamarin.Forms.ListView) aktualisiert wird:

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`ListView`](xref:Xamarin.Forms.ListView)Löst das [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) -Ereignis aus, um die Aktualisierung zu initiieren, und die- [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) Eigenschaft wird auf festgelegt `true` . Der Code, der zum Aktualisieren des Inhalts von erforderlich ist `ListView` , sollte dann vom Ereignishandler für das- `Refreshing` Ereignis oder durch die-Methode, die von ausgeführt wird, ausgeführt werden [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) . Nachdem `ListView` aktualisiert wurde, sollte die- `IsRefreshing` Eigenschaft auf festgelegt werden `false` , oder die- [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) Methode sollte aufgerufen werden, um anzugeben, dass die Aktualisierung beendet ist.

> [!NOTE]
> Beim definieren [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) `CanExecute` von kann die-Methode des-Befehls angegeben werden, um den Befehl zu aktivieren oder zu deaktivieren.

## <a name="detect-scrolling"></a>Scrollen erkennen

[`ListView`](xref:Xamarin.Forms.ListView)definiert ein `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das folgende XAML-Beispiel zeigt `ListView` , wie ein Ereignishandler für das-Ereignis festgelegt wird `Scrolled` :

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

In diesem Codebeispiel wird der `OnListViewScrolled` Ereignishandler ausgeführt, wenn das `Scrolled` Ereignis ausgelöst wird:

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

Der `OnListViewScrolled` Ereignishandler gibt die Werte des `ScrolledEventArgs` Objekts aus, das das Ereignis begleitet.

## <a name="related-links"></a>Verwandte Links

- [ListView-Interaktivität (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
