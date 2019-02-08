---
title: ListView-Interaktivität
description: In diesem Artikel wird erläutert, wie eine Xamarin.Forms-ListView Interaktivität hinzugefügt, durch die Implementierung von Auswahl und kontextaktionen zum Aktualisieren ziehen.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: 939df6cfd17de82e28958363cfa51cd199f928cb
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831690"
---
# <a name="listview-interactivity"></a>ListView-Interaktivität

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)

[`ListView`](xref:Xamarin.Forms.ListView) unterstützt die Interaktion mit den Daten, die es bereitstellt.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Auswahl & Datenabzweigungen

Die [ `ListView` ](xref:Xamarin.Forms.ListView) Auswahlmodus wird gesteuert, indem die [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) Eigenschaft mit einem Wert, der die [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) Enumeration:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Gibt an, dass ein einzelnes Element ausgewählt werden kann, mit dem ausgewählten Element hervorgehoben wird. Dies ist der Standardwert.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Gibt an, dass Elemente nicht ausgewählt werden können.

Wenn ein Benutzer ein Element tippt, werden zwei Ereignisse ausgelöst:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) wird ausgelöst, wenn ein neues Element ausgewählt ist.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) wird ausgelöst, wenn ein Element getippt wird.

> [!NOTE]
> Tippen auf das gleiche Element zweimal ausgelöst wird, zwei [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignisse, sondern werden nur Auslösen eines einzelnen [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis.

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

Kontextaktionen können in c# und XAML implementiert werden. Im folgenden finden Sie spezifische Anleitungen für beide aber zuerst sehen wir sehen Sie sich einige wichtige Implementierungsdetails für beide.

Kontextaktionen werden mit erstellt `MenuItem`s. Tippen Sie auf Ereignisse für MenuItems werden durch die "MenuItem" selbst, nicht die ListView ausgelöst. Dies unterscheidet sich von wie die Tap-Ereignisse für Zellen, behandelt werden, in denen die ListView die Zelle, statt das Ereignis auslöst. Da die ListView das Ereignis ausgelöst wird, erhält ihr Ereignishandler Schlüsselinformationen wie das Element aktiviert oder angetippt wurde.

Standardmäßig kann ein MenuItem-Element nicht zu wissen, welche Zelle mit dem er angehört. `CommandParameter` finden Sie `MenuItem` zum Speichern von Objekten, z. B. das Objekt hinter die "MenuItem" ViewCell. `CommandParameter` kann in XAML und C# -Code festgelegt werden.

### <a name="c"></a>C#  

Kontextaktionen können implementiert werden, in einem `Cell` Unterklasse (sofern nicht wie eine Gruppenkopfzeile verwendet wird.) durch das Erstellen `MenuItem`s und Hinzufügen der `ContextActions` Auflistung für die Zelle. Sie verfügen über die folgenden Eigenschaften für die kontextaktion konfiguriert werden können:

* **Text** &ndash; die Zeichenfolge, die im Menüelement angezeigt wird.
* **Geklickt** &ndash; das Ereignis, wenn das Element geklickt wird.
* **IsDestructive** &ndash; (optional) Wenn "true" das Element gerendert wird anders unter iOS.

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

Benutzer schon lange davon ausgehen, dass in der Liste der Daten nach unten ziehen, Liste aktualisiert. [`ListView`](xref:Xamarin.Forms.ListView) Diese Out-of-the-Box wird unterstützt. Um Pull zum Aktualisieren von Funktionalität zu aktivieren, legen [ `IsPullToRefreshEnabled` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) zu `true`:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Der entsprechende C#-Code ist:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Bei der Aktualisierung, die standardmäßig Schwarz ist, wird ein Drehfeld angezeigt. Allerdings kann die Farbe des Spinner unter iOS und Android geändert werden, durch Festlegen der `RefreshControlColor` Eigenschaft, um eine [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

Der entsprechende C#-Code ist:

```csharp
listView.RefreshControlColor = Color.Red;
```

Die folgenden Screenshots zeigen zum Aktualisieren ziehen, wie der Benutzer abruft:

![](interactivity-images/refresh-start.png "ListView Pull In Ausführung befindlichen aktualisieren")

Die folgenden Screenshots zeigen Pull zum Aktualisieren, nachdem der Benutzer das Drehfeld angezeigt wird, Pull freigegeben wurde während der [ `ListView` ](xref:Xamarin.Forms.ListView) wird aktualisiert:

![](interactivity-images/refresh-in-progress.png "ListView-Pull auf die Aktualisierung abgeschlossen")

[`ListView`](xref:Xamarin.Forms.ListView) löst die [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) Ereignis, um die Aktualisierung zu initiieren und die [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) Eigenschaft auf festgelegt `true`. Jeden beliebigen Code zum Aktualisieren des Inhalts erforderlich ist der `ListView` dann vom Ereignishandler für ausgeführt werden soll die `Refreshing` Ereignis oder die Methode ausgeführt, indem die [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand). Nach der `ListView` wird aktualisiert, die `IsRefreshing` Eigenschaft sollte festgelegt werden, um `false`, oder die [ `EndRefresh` ](xref:Xamarin.Forms.ListView.EndRefresh) -Methode aufgerufen werden soll, um anzugeben, dass die Aktualisierung abgeschlossen ist.

> [!NOTE]
> Beim Definieren einer [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand), `CanExecute` Methode des Befehls zum Aktivieren oder deaktivieren den Befehl angegeben werden kann.

## <a name="related-links"></a>Verwandte Links

- [ListView-Interaktivität (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
