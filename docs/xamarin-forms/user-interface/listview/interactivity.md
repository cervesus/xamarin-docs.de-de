---
title: ListView-Interaktivität
description: In diesem Artikel wird erläutert, wie eine Xamarin.Forms-ListView Interaktivität hinzugefügt, durch die Implementierung von Auswahl und kontextaktionen zum Aktualisieren ziehen.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: 3949dd85492a8181ee53e23b3ba2e986e59f8f47
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121625"
---
# <a name="listview-interactivity"></a>ListView-Interaktivität

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

[`ListView`](xref:Xamarin.Forms.ListView)unterstützt die Interaktion mit den Daten, die Sie präsentiert.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Auswahl & Datenabzweigungen

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

### <a name="disabling-selection"></a>Auswahl deaktivieren

So deaktivieren Sie [ `ListView` ](xref:Xamarin.Forms.ListView) Auswahlsatz der [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>Kontextaktionen

Häufig möchten Benutzer Aktion auf ein Element in einem `ListView`. Betrachten Sie beispielsweise eine Liste von e-Mails in der Mail-app. Unter iOS, können Sie zum Löschen einer Nachricht Wischen::

![](interactivity-images/context-default.png "ListView mit Kontextaktionen")

Kontextaktionen können in C# und XAML implementiert werden. Im folgenden finden Sie spezifische Anleitungen für beide aber zuerst sehen wir sehen Sie sich einige wichtige Implementierungsdetails für beide.

Kontextaktionen werden mit erstellt `MenuItem`s. Tippen Sie auf Ereignisse für MenuItems werden durch die "MenuItem" selbst, nicht die ListView ausgelöst. Dies unterscheidet sich von wie die Tap-Ereignisse für Zellen, behandelt werden, in denen die ListView die Zelle, statt das Ereignis auslöst. Da die ListView das Ereignis ausgelöst wird, erhält ihr Ereignishandler Schlüsselinformationen wie das Element aktiviert oder angetippt wurde.

Standardmäßig kann ein MenuItem-Element nicht zu wissen, welche Zelle mit dem er angehört. `CommandParameter` finden Sie `MenuItem` zum Speichern von Objekten, z. B. das Objekt hinter die "MenuItem" ViewCell. `CommandParameter` kann in XAML und C# -Code festgelegt werden.

### <a name="c"></a>C\#

Kontextaktionen können implementiert werden, in einem `Cell` Unterklasse (sofern nicht wie eine Gruppenkopfzeile verwendet wird.) durch das Erstellen `MenuItem`s und Hinzufügen der `ContextActions` Auflistung für die Zelle. Sie verfügen über die folgenden Eigenschaften für die kontextaktion konfiguriert werden können:

- **Text** &ndash; die Zeichenfolge, die im Menüelement angezeigt wird.
- **Geklickt** &ndash; das Ereignis, wenn das Element geklickt wird.
- **IsDestructive** &ndash; (optional) Wenn "true" das Element gerendert wird anders unter iOS.

Mehrere kontextaktionen können in eine Zelle hinzugefügt werden, jedoch nur eine muss `IsDestructive` festgelegt `true`. Der folgende Code zeigt, wie kontextaktionen hinzugefügt werden würde eine `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s kann auch deklarativ in einer XAML-Auflistung erstellt werden. Die folgenden XAML veranschaulicht eine benutzerdefinierte Zelle mit zwei kontextaktionen implementiert:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
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
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> Die `NavigationPageRenderer` für Android eine überschreibbare ist `UpdateMenuItemIcon` -Methode, die verwendet werden kann, laden Sie Symbole aus einem benutzerdefinierten `Drawable`. Diese Außerkraftsetzung ist es möglich, SVG-Bildern als Symbole auf `MenuItem` -Instanzen unter Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Zum Aktualisieren ziehen

Benutzer schon lange davon ausgehen, dass in der Liste der Daten nach unten ziehen, Liste aktualisiert. [`ListView`](xref:Xamarin.Forms.ListView)unterstützt diese standardmäßig. Legen Sie zum Aktivieren der Pull-to-refresh [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) - `true`Funktionalität auf Folgendes fest:

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
