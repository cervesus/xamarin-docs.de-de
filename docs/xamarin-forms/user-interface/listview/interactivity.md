---
title: "ListView-Interaktivität"
description: "Hinzufügen von Interaktivität zu Ihrem ListView durch Implementieren der Auswahl, Streifen zu löschen und aktualisieren ziehen."
ms.topic: article
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 74b275b77b6a70b3d074d68b14a97c73615753d2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="listview-interactivity"></a>ListView-Interaktivität

ListView unterstützt die Interaktion mit den Daten, die er mithilfe der folgenden Methoden präsentiert:

- [**Auswahl & Taps** ](#selectiontaps) &ndash; datenabzweigungen zu Auswahl/Deselections von Elementen zu reagieren. Aktivieren Sie oder deaktivieren Sie die Auswahl von Zeilen (standardmäßig aktiviert).
- [**Kontext Aktionen** ](#Context_Actions) &ndash; machen Funktionalität pro Element, z. B. Streifen zu löschen.
- [**Zum Aktualisieren ziehen** ](#Pull_to_Refresh) &ndash; Idioms aktualisieren ziehen, die Benutzer erwarten stammen haben aus systemeigenen Erfahrungen zu implementieren.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Auswahl & Datenabzweigungen
`ListView` unterstützt die Auswahl eines Elements zu einem Zeitpunkt. Auswahl ist standardmäßig aktiviert. Wenn ein Benutzer ein Element tippt, werden zwei Ereignisse ausgelöst: `ItemTapped` und `ItemSelected`. Beachten Sie, dass dasselbe Element zweimal tippen nicht mehrere ausgelöst `ItemSelected` Ereignisse, aber immer dann ausgelöst, mehrere `ItemTapped` Ereignisse. Beachten Sie, dass `ItemSelected` wird aufgerufen, wenn ein Element aufgehoben wird.

Beachten Sie, `ItemSelected` wird aufgerufen, wenn Elemente deaktiviert sind und bei der Auswahl. Das bedeutet, müssen Sie die Prüfung auf Null `SelectedItem` in Ihrer `ItemSelected` Ereignishandler, bevor Sie ihn verwenden können:

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>Deaktivieren der Auswahl

Wenn Sie deaktivieren möchten, behandeln die `ItemSelected` Ereignis, und legen die `SelectedItem` -Eigenschaft auf null:

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

Mit der Auswahl aktiviert:

![](interactivity-images/selection-default.png "ListView mit Auswahl aktiviert")

Beachten Sie, dass auf Windows Phone, einige der Zellen, einschließlich `SwitchCell` ihre visuellen Zustands als Antwort auf die Auswahl nicht aktualisieren.

<a name="Context_Actions" />

## <a name="context-actions"></a>Kontext-Aktionen
Häufig Benutzer sollten Maßnahmen auf ein Element in einem `ListView`. Betrachten Sie beispielsweise eine Liste von e-Mail-Adressen in die e-Mail-app. Bei iOS kann können navigieren Sie zum Löschen einer Nachricht und auf Windows Phone-Long-drücken Sie eine Nachricht und löschen Sie ihn:

![](interactivity-images/context-default.png "ListView mit Kontext Aktionen")

Kontext-Aktionen können in c# und XAML implementiert werden. Im folgenden finden Sie spezifische Leitfäden für beide jedoch zunächst sehen wir einen Blick auf einige wichtige Implementierungsdetails für beide.

Kontext-Aktionen werden mit erstellt `MenuItem`s. Tap-Ereignisse für MenuItems werden durch die "MenuItem" selbst, nicht die ListView ausgelöst. Dies unterscheidet sich von wie Tap-Ereignisse für Zellen, behandelt werden, in dem ListView die Zelle, statt das Ereignis auslöst. Da ListView das Ereignis auslöst, erhält ein Ereignishandler wichtige Informationen, wie das Element ausgewählt oder abgerufen wurde.

Standardmäßig hat eine "MenuItem" keine Möglichkeit zu wissen, welche Zelle mit dem er gehört. `CommandParameter` steht für `MenuItem` zum Speichern von Objekten, z. B. das Objekt hinter die "MenuItem" ViewCell. `CommandParameter` kann in XAML und c# festgelegt werden.

### <a name="c"></a>C#  

Kontext Aktionen können implementiert werden, in einem `Cell` Unterklasse (sofern nicht wie eine Gruppenkopfzeile verwendet wird) durch das Erstellen `MenuItem`s und Hinzufügen der `ContextActions` Auflistung für die Zelle. Sie verfügen über die folgenden Eigenschaften können konfiguriert werden, für die Kontext-Aktion:

* **Text** &ndash; die Zeichenfolge, die in das Menüelement angezeigt wird.
* **Geklickt** &ndash; das Ereignis, wenn das Element geklickt wird.
* **IsDestructive** &ndash; (optional) bei "true" das Element wird gerendert unterschiedlich auf iOS.

Mehrere Kontext Aktionen können auf eine Zelle hinzugefügt werden, aber nur eine haben sollten `IsDestructive` festgelegt `true`. Das folgende Codebeispiel veranschaulicht, wie die Kontext-Aktionen hinzugefügt würde eine `ViewCell`:

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

`MenuItem`s kann auch deklarativ in einer XAML-Auflistung erstellt werden. Der XAML-Code unten zeigt eine benutzerdefinierte Zelle mit zwei Kontext Aktionen implementiert:

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

Der Code-Behind-Datei, vergewissern Sie sich die `Clicked` Methoden implementiert werden:

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

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Ziehen Sie nach der Aktualisierung
Benutzer stammen erwarten, dass eine Liste der Daten nach unten ziehen, Liste aktualisieren. `ListView` Diese Out-of-the-Box wird unterstützt. Legen Sie zum Aktivieren von Funktionen zum Aktualisieren ziehen `IsPullToRefreshEnabled` auf "true":

```csharp
listView.IsPullToRefreshEnabled = true;
```

Pull aktualisieren, als der Benutzer ist abrufen:

![](interactivity-images/refresh-start.png "ListView Pull In Bearbeitung aktualisieren")

Pull aktualisieren, als der Benutzer hat die Pull veröffentlicht. Dies ist, was dem Benutzer angezeigt, während Sie die Liste aktualisiert haben: ![ ] (interactivity-images/refresh-in-progress.png "ListView Pull auf vollständige aktualisieren")

Beachten Sie, dass zum Zeitpunkt der Xamarin.Forms 1.4.3, Pull zum Aktualisieren auf Windows Phone 8.1 nicht unterstützt wird. Auf Windows Phone 8 ist zum Aktualisieren ziehen keine systemeigene Plattform-Funktion, damit eine Implementierung von Pull zum Aktualisieren von Xamarin.Forms bereitgestellt wird. Schließlich bedenken, dass Pull-datenaktualisierung funktioniert nicht auf Windows Phone, wenn alle Elemente in der Liste (das heißt, wenn vertikaler Bildlauf nicht erforderlich ist) auf dem Bildschirm passen.

ListView macht einige Ereignisse, die Sie zum Aktualisieren ziehen Ereignisse reagieren können.

-  Die `RefreshCommand` aufgerufen wird und die `Refreshing` -Ereignis aufgerufen. `IsRefreshing` festgelegt, um `true`.
-  Sie sollten ausführen, beliebigen Code erforderlich ist, um den Inhalt der Listenansicht angezeigt, die entweder in den Befehl oder ein Ereignis zu aktualisieren.
-  Bei der Aktualisierung abgeschlossen ist, rufen Sie `EndRefresh` oder `IsRefreshing` auf `false` die Listenansicht zu informieren, die Sie fertig sind.

Die `CanExecute` -Eigenschaft wird berücksichtigt, wo Sie mit der gesteuert, ob der Befehl zum Aktualisieren ziehen aktiviert werden soll.



## <a name="related-links"></a>Verwandte Links

- [ListView-Interaktivität (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [Anmerkungen zu dieser Version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Anmerkungen zu dieser Version 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
