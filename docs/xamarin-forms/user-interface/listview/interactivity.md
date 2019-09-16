---
title: ListView-Interaktivität
description: In diesem Artikel wird erläutert, wie eine Xamarin.Forms-ListView Interaktivität hinzugefügt, durch die Implementierung von Auswahl und kontextaktionen zum Aktualisieren ziehen.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: e2d51f42339b1ff2a99f2a00bb5a9e662fb01d87
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998079"
---
# <a name="listview-interactivity"></a>ListView-Interaktivität

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Die xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) -Klasse unterstützt die Benutzerinteraktion mit den Daten, die Sie präsentiert.

## <a name="selection-and-taps"></a>Auswahl und tippen

Die [ `ListView` ](xref:Xamarin.Forms.ListView) Auswahlmodus wird gesteuert, indem die [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft mit einem Wert, der die [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) Enumeration:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Gibt an, dass ein einzelnes Element ausgewählt werden kann, mit dem ausgewählten Element hervorgehoben wird. Dies ist der Standardwert.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Gibt an, dass Elemente nicht ausgewählt werden können.

Wenn ein Benutzer ein Element tippt, werden zwei Ereignisse ausgelöst:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) wird ausgelöst, wenn ein neues Element ausgewählt ist.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) wird ausgelöst, wenn ein Element getippt wird.

Tippen auf das gleiche Element zweimal ausgelöst wird, zwei [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse, sondern werden nur Auslösen eines einzelnen [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis.

> [!NOTE]
> Die [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) -Klasse, die die Ereignis Argumente für das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) -Ereignis, die [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) -Eigenschaft und die `ItemIndex` -Eigenschaft sowie eine-Eigenschaft enthält, [`ListView`](xref:Xamarin.Forms.ListView) deren Wert den Index in der des gezapften Elements darstellt. Entsprechend verfügt die [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) -Klasse, die die Ereignis Argumente für das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis enthält, [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) über eine-Eigenschaft `SelectedItemIndex` und eine-Eigenschaft, deren Wert den `ListView` Index in der des ausgewählten Elements darstellt.

Wenn die [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaftensatz auf [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), Elemente in der [ `ListView` ](xref:Xamarin.Forms.ListView) ausgewählt werden kann, die [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) und [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse ausgelöst, und die [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft wird auf den Wert des ausgewählten Elements festgelegt werden.

Wenn die [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) -Eigenschaftensatz auf [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), Elemente in der [ `ListView` ](xref:Xamarin.Forms.ListView) kann nicht ausgewählt werden, die [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis wird nicht ausgelöst, und die [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft bleibt `null`. Allerdings [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse weiterhin ausgelöst, und das angetippte Element wird bei der das Tap kurz hervorgehoben werden.

Wenn ein Element ausgewählt wurde und die [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft geändert wird [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) zu [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft auf festgelegt `null` und [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis wird ausgelöst, mit einem `null` Element.

Die folgenden Screenshots zeigen eine [ `ListView` ](xref:Xamarin.Forms.ListView) mit dem Standard-Auswahl-Modus:

![](interactivity-images/selection-default.png "ListView mit Auswahl aktiviert")

### <a name="disable-selection"></a>Auswahl deaktivieren

So deaktivieren Sie [ `ListView` ](xref:Xamarin.Forms.ListView) Auswahlsatz der [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>Kontextaktionen

Häufig möchten Benutzer Aktion auf ein Element in einem `ListView`. Betrachten Sie beispielsweise eine Liste von e-Mails in der Mail-app. Unter iOS, können Sie zum Löschen einer Nachricht Wischen::

![](interactivity-images/context-default.png "ListView mit Kontextaktionen")

Kontextaktionen können in C# und XAML implementiert werden. Im folgenden finden Sie spezifische Anleitungen für beide aber zuerst sehen wir sehen Sie sich einige wichtige Implementierungsdetails für beide.

Kontext Aktionen werden mit `MenuItem` -Elementen erstellt. Tap-Ereignisse `MenuItems` für Objekte werden von der `MenuItem` selbst, nicht `ListView`von ausgelöst. Dies unterscheidet sich von der Behandlung von TAP-Ereignissen für Zellen, `ListView` bei denen das Ereignis anstelle der Zelle auslöst. Da der `ListView` das-Ereignis aufhebt, erhält der zugehörige Ereignishandler wichtige Informationen, wie z. b. welches Element ausgewählt oder getippt wurde.

Standardmäßig kann eine `MenuItem` nicht wissen, zu welcher Zelle Sie gehört. Die `CommandParameter` -Eigenschaft ist in `MenuItem` verfügbar, um-Objekte zu speichern, z. `MenuItem`b `ViewCell`. das-Objekt hinter dem. Die `CommandParameter` -Eigenschaft kann sowohl in XAML als auch C#in festgelegt werden.

### <a name="xaml"></a>XAML

`MenuItem`-Elemente können in einer XAML-Auflistung erstellt werden. Die folgenden XAML veranschaulicht eine benutzerdefinierte Zelle mit zwei kontextaktionen implementiert:

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

Vergewissern Sie sich in der CodeBehind-Datei, die `Clicked` Methoden implementiert werden:

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
> Die `NavigationPageRenderer` für Android eine überschreibbare ist `UpdateMenuItemIcon` -Methode, die verwendet werden kann, laden Sie Symbole aus einem benutzerdefinierten `Drawable`. Diese Außerkraftsetzung ist es möglich, SVG-Bildern als Symbole auf `MenuItem` -Instanzen unter Android.

### <a name="code"></a>Code

Kontext Aktionen können in jeder `Cell` Unterklasse implementiert werden (sofern Sie nicht als Gruppen Header verwendet wird), indem Instanzen erstellt `MenuItem` und der `ContextActions` -Auflistung für die Zelle hinzugefügt werden. Sie verfügen über die folgenden Eigenschaften für die kontextaktion konfiguriert werden können:

- **Text** &ndash; die Zeichenfolge, die im Menüelement angezeigt wird.
- **Geklickt** &ndash; das Ereignis, wenn das Element geklickt wird.
- **Isdestruktiv** &ndash; (optional) Wenn true, wird das Element unter IOS anders gerendert.

Mehrere kontextaktionen können in eine Zelle hinzugefügt werden, jedoch nur eine muss `IsDestructive` festgelegt `true`. Der folgende Code zeigt, wie kontextaktionen hinzugefügt werden würde eine `ViewCell`:

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

Benutzer schon lange davon ausgehen, dass in der Liste der Daten nach unten ziehen, Liste aktualisiert. Das [`ListView`](xref:Xamarin.Forms.ListView) -Steuerelement unterstützt diese standardmäßig. Legen Sie zum Aktivieren der Pull-to-refresh [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) - `true`Funktionalität auf Folgendes fest:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Der entsprechende C#-Code ist:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Während der Aktualisierung wird ein Spinner angezeigt, der standardmäßig schwarz ist. Die spinfarbe kann jedoch unter IOS und Android geändert werden, indem die `RefreshControlColor` -Eigenschaft [`Color`](xref:Xamarin.Forms.Color)auf festgelegt wird:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

Der entsprechende C#-Code ist:

```csharp
listView.RefreshControlColor = Color.Red;
```

Die folgenden Screenshots zeigen Pull-to-refresh-Vorgänge, während der Benutzer den Abruf durchläuft:

![](interactivity-images/refresh-start.png "ListView Pull In Ausführung befindlichen aktualisieren")

Die folgenden Screenshots zeigen die Pull-zu-aktualisieren, nachdem der Benutzer den Pull losgelassen hat, während der Spinner angezeigt wird [`ListView`](xref:Xamarin.Forms.ListView) , während aktualisiert wird:

![](interactivity-images/refresh-in-progress.png "ListView-Pull zum Aktualisieren wird beendet.")

[`ListView`](xref:Xamarin.Forms.ListView)Löst das [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) -Ereignis aus, um die Aktualisierung zu [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) initiieren, und die- `true`Eigenschaft wird auf festgelegt. Der Code, der zum Aktualisieren des Inhalts von `ListView` erforderlich ist, sollte dann vom Ereignishandler für das `Refreshing` -Ereignis oder durch [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)die-Methode, die von ausgeführt wird, ausgeführt werden. Nachdem aktualisiert wurde, sollte die `IsRefreshing` -Eigenschaft auf `false`festgelegt werden, oder [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) die-Methode sollte aufgerufen werden, um anzugeben, dass die Aktualisierung beendet ist. `ListView`

> [!NOTE]
> Beim definieren [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)von kann die `CanExecute` -Methode des-Befehls angegeben werden, um den Befehl zu aktivieren oder zu deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [ListView-Interaktivität (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
