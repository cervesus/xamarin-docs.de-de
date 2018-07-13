---
title: ListView-Leistung
description: ListView eine leistungsfähige Ansicht zum Anzeigen von Daten ist, hat aber einige Einschränkungen. In diesem Artikel wird erläutert, wie hohe Leistung mit einer Xamarin.Forms-ListView, in einer Anwendung sichergestellt wird.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: c6a6ed38ec64c681075ffa3e42f3ffaf58c576ec
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997216"
---
# <a name="listview-performance"></a>ListView-Leistung

Beim Schreiben von mobilen Anwendungen ist die Leistung wichtig. Benutzer stammen, ebenso der geglättete Bildlauf und schnelle Ladezeiten zu erwarten. Nicht den Erwartungen der Benutzer erfüllt wird, kostet Sie Bewertungen in den App Store oder im Falle einer Line-of-Business-Anwendung, die Ihr Unternehmen Zeit und Geld Kosten.

Obwohl [ `ListView` ](xref:Xamarin.Forms.ListView) ist eine leistungsfähige Ansicht zum Anzeigen von Daten, es gelten einige Einschränkungen. Die bildlaufleistung kann beeinträchtigt werden, bei Verwendung von benutzerdefinierten Zellen, insbesondere, wenn sie tief geschachtelte Hierarchien enthalten, oder verwenden bestimmte Layouts, die viele der Messung zu erfordern. Es gibt Methoden, die Sie verwenden können, um Leistungseinbußen zu vermeiden.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Cachingstrategie

ListViews werden häufig verwendet, zum Anzeigen von viel mehr Daten als auf dem Bildschirm anpassen. Denken Sie zum Beispiel eine Musik-app. Eine Bibliothek von Songs möglicherweise Tausende von Einträgen. Der einfache Ansatz, der würden eine Zeile für jede "Song" erstellt werden, müsste eine schlechte Leistung. Dieser Ansatz verschwendet wertvollen Speicherplatz und kann langsam einen Bildlauf zu "Durchforstung". Ein anderer Ansatz ist, erstellen und Zerstören von Zeilen, wie Daten in die Ansicht gescrollt werden. Dies erfordert Konstante Instanziierung und Bereinigung der Objekte anzeigen, die sehr langsam sein kann.

Zum Einsparen von Arbeitsspeicher, die Native [ `ListView` ](xref:Xamarin.Forms.ListView) Entsprechungen für jede Plattform verfügen über integrierte Wiederverwendung von Zeilen. Nur die Zellen, die auf dem Bildschirm sichtbar sind in den Arbeitsspeicher geladen und die **Inhalt** in vorhandenen Zellen geladen wird. Dies verhindert, dass die Anwendung, die Tausende von Objekten, sparen Sie Zeit und Arbeitsspeicher instanziieren müssen.

Xamarin.Forms ermöglicht [ `ListView` ](xref:Xamarin.Forms.ListView) Zelle erneut zu verwenden, über die [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) -Enumeration, die in den folgenden Werten:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Die universelle Windows-Plattform (UWP) ignoriert die [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) cachingstrategie, da es immer mithilfe der Zwischenspeicherung die um Leistung zu verbessern. Aus diesem Grund standardmäßig verhält sich wie die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie angewendet wird.

### <a name="retainelement"></a>RetainElement

Die [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) cachingstrategie gibt an, dass die [ `ListView` ](xref:Xamarin.Forms.ListView) generiert eine Zelle für jedes Element in der Liste aus, und der Standardwert ist `ListView` Verhalten. Sie sollten in der Regel in folgenden Situationen verwendet werden:

- Wenn jede Zelle wurde eine große Anzahl von Bindungen (20-30 +).
- Wenn die Zellvorlage häufig geändert wird.
- Wenn Test zeigt, dass die `RecycleElement` Strategie für die Ergebnisse in einer reduzierten ausführungsgeschwindigkeit Zwischenspeichern.

Es ist wichtig zu erkennen, die Folgen der [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) cachingstrategie, bei der Arbeit mit benutzerdefinierten Zellen. Jede Zelle Initialisierungscode ausführen für jede Zelle erstellen, müssen die möglicherweise mehrere Male pro Sekunde. In diesem Fall Layout-Techniken, die auf einer Seite in Ordnung, indem z.B. mehrere geschachtelte [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Instanzen von Leistungsengpässen werden, wenn sie Setup sind und in Echtzeit zerstört wird, als der Benutzer einen Bildlauf.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

Die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie gibt an, dass die [ `ListView` ](xref:Xamarin.Forms.ListView) wird versucht, die Arbeitsspeicher-Speicherbedarf und die ausführungsgeschwindigkeit zu minimieren, durch Wiederverwenden von Listenzellen. Dieser Modus ist nicht immer eine Leistungssteigerung bieten und Tests sollten ausgeführt werden, um Verbesserungen zu bestimmen. Allerdings ist in der Regel die bevorzugte Option und sollte in den folgenden Situationen verwendet werden:

- Wenn jeder Zelle eine kleine bis mittlere Anzahl der Bindungen hat.
- Wenn jede Zelle [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) die Zellendaten definiert.
- Wenn jede Zelle größtenteils ähnlich, mit der Zellvorlage unveränderlich ist.

Während der Virtualisierung die Zelle wird dem Bindungskontext aktualisiert haben, und daher eine Anwendung in diesem Modus verwendet sie müssen sicherstellen, dass Kontext bindungsaktualisierungen richtig behandelt werden. Alle Daten zur Zelle müssen aus dem Bindungskontext stammen oder Konsistenz treten möglicherweise Fehler auf. Dies kann erfolgen, mithilfe der Datenbindung auf Zellendaten anzeigen. Alternativ sollten Zellendaten festgelegt werden, der `OnBindingContextChanged` außer Kraft setzen, anstatt im Konstruktor der benutzerdefinierten Zelle ab, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

Weitere Informationen finden Sie unter [Kontextänderungen Bindung](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Unter iOS und Android müssen Zellen benutzerdefinierte Renderer verwenden, sie sicherstellen, dass eigenschaftenänderungen ordnungsgemäß implementiert wird. Wenn Zellen wiederverwendet werden ihre Eigenschaftswerte ändert, wenn der Bindungskontext mit einer Zelle für zur Verfügung, mit aktualisiert wird `PropertyChanged` Ereignisse, die ausgelöst wird. Weitere Informationen finden Sie unter [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement mit einem DataTemplateSelector

Wenn eine [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet eine [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) auswählen eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Zwischenspeichern Strategie zwischenspeichert keine `DataTemplate`s. Stattdessen eine `DataTemplate` für jedes Datenelement in der Liste ausgewählt ist.

> [!NOTE]
> Die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie ist eine erforderliche Komponente, in Abschnitt 2.4 Xamarin.Forms eingeführt, die bei einer [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) wird gebeten, wählen Sie eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), von denen jedes `DataTemplate` muss die gleiche zurückgeben [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) Typ. Angenommen, ein [ `ListView` ](xref:Xamarin.Forms.ListView) mit einer `DataTemplateSelector` können zurückgeben `MyDataTemplateA` (, in denen `MyDataTemplateA` gibt eine `ViewCell` vom Typ `MyViewCellA`), oder `MyDataTemplateB` (, in denen `MyDataTemplateB`gibt eine `ViewCell` vom Typ `MyViewCellB`), wenn `MyDataTemplateA` wird zurückgegeben, sie muss zurückgeben `MyViewCellA` oder eine Ausnahme ausgelöst.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

Die [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) cachingstrategie basiert auf der [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie Leuchten Sie außerdem, dass bei einem [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet eine [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) Auswählen einer [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), `DataTemplate`s werden zwischengespeichert, durch den Typ des Elements in der Liste. Aus diesem Grund `DataTemplate`s werden einmal pro handelt, nicht einmal bei jeder Instanz ausgewählt.

> [!NOTE]
> Der [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) cachingstrategie ist eine erforderliche Komponente, die die `DataTemplate`s zurückgegeben, die von der [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) verwenden, müssen die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) Konstruktor, eine `Type`.

### <a name="setting-the-caching-strategy"></a>Festlegen der Caching-Strategie

Die [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Enumerationswert angegeben ist, mit einer [ `ListView` ](xref:Xamarin.Forms.ListView) Überladung des Konstruktors, wie im folgenden Codebeispiel gezeigt:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

In XAML, Festlegen der `CachingStrategy` -Attribut wie im folgenden Code gezeigt:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Dies hat dieselbe Wirkung wie das Festlegen des Arguments Strategie für die Zwischenspeicherung im Konstruktor in c#; Beachten Sie, dass es keine `CachingStrategy` Eigenschaft `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Festlegen der Strategie für die Zwischenspeicherung in ein untergeordnetes ListView

Festlegen der `CachingStrategy` Attribut in XAML für ein untergeordnetes [ `ListView` ](xref:Xamarin.Forms.ListView) erzeugt nicht das gewünschte Verhalten, da gibt es keine `CachingStrategy` Eigenschaft `ListView`. Darüber hinaus, wenn [XAMLC](~/xamarin-forms/xaml/xamlc.md) ist aktiviert, wird die folgende Fehlermeldung erzeugt: **keine Eigenschaft, bindbare Eigenschaft oder das Ereignis für 'CachingStrategy' gefunden.**

Die Lösung für dieses Problem besteht darin, einen Konstruktor zu spezifizieren, auf den Unterklassen [ `ListView` ](xref:Xamarin.Forms.ListView) , akzeptiert eine [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Parameter und übergibt ihn an die Basisklasse der Klasse:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Die [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Enumerationswert kann in XAML angegeben werden, mithilfe der `x:Arguments` Syntax:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>Verbessern der ListView-Leistung

Es gibt viele Techniken zum Verbessern der Leistung einer `ListView`:

-  Binden der `ItemsSource` Eigenschaft, um eine `IList<T>` -Sammlung anstelle einer `IEnumerable<T>` -Auflistung, da `IEnumerable<T>` Sammlungen nicht zufälligen Zugriff unterstützt.
-  Verwenden Sie die integrierten Zellen (z. B. `TextCell`  /  `SwitchCell` ) anstelle von `ViewCell` können Sie jedes Mal, wenn.
-  Verwenden Sie weniger Elemente. Betrachten Sie beispielsweise mit einem einzigen `FormattedString` Bezeichnung anstelle von mehreren Bezeichnungen.
-  Ersetzen Sie die `ListView` mit einem `TableView` Anzeigen von nicht homogen sind. Daten – d. h. Daten verschiedener Typen.
-  Einschränken der Verwendung der [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode. Wenn übermäßig beansprucht, wird Leistung beeinträchtigt werden.
-  Unter Android, vermeiden Sie das Festlegen einer `ListView`des Trennzeichen Sichtbarkeit Zeile oder die Farbe, nachdem es instanziiert wurde, da dies zu großen Leistungseinbußen führt.
-  Vermeiden von Änderungen das Zellenlayout basierend auf den [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Dies führt zu große Layout und die Initialisierung Kosten.
-  Vermeiden Sie tief verschachtelte Layout-Hierarchien. Verwendung `AbsoluteLayout` oder `Grid` zur Reduzierung der Schachtelungsebenen.
-  Vermeiden Sie bestimmte `LayoutOptions` außer `Fill` (Füllung ist kostspielige berechnet).
-  Ordnen Sie keine `ListView` innerhalb einer `ScrollView` aus den folgenden Gründen:
    - Die `ListView` implementiert eigene scrollen.
    - Die `ListView` erhalten, wie sie vom übergeordneten Element verarbeitet werden, werden keine Bewegungen `ScrollView`.
    - Die `ListView` kann mit den Elementen der Liste, möglicherweise der Funktionalität bietet, die eine benutzerdefinierte Kopf- und Fußzeile, die gescrollt darstellen der `ScrollView` für verwendet wurde. Weitere Informationen finden Sie unter [Kopf- und Fußzeilen](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Erwägen Sie einen benutzerdefinierten Renderer, bei Bedarf einen sehr spezifischen, komplexen Entwurf, die in die Zellen angezeigt.

`AbsoluteLayout` hat das Potenzial, Layouts, ohne ein einzelnes Measure-Aufruf durchführen. Dies macht es für die Leistung sehr leistungsstark. Wenn `AbsoluteLayout` nicht verwendet haben, sollten Sie [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout). Wenn `RelativeLayout`, die direkte Übergabe der Einschränkungen werden wesentlich schneller als mit dem API-Ausdruck. Der Grund ist der API-Ausdruck verwendet die JIT-Kompilierung, und unter iOS die Struktur besitzt interpretiert werden soll, das langsamer ist. Der Ausdruck API eignet sich für Seitenlayouts, in denen nur erforderlich, es auf das anfängliche Layout sowie die Drehung, aber im `ListView`, bei der Ausführung im Bildlauf, ständig es beeinträchtigt die Leistung.

Erstellen eines benutzerdefinierten Renderers für eine [ `ListView` ](xref:Xamarin.Forms.ListView) oder deren Zellen erreicht werden, indem die Auswirkungen der layoutberechnungen mit bildlaufleistung. Weitere Informationen finden Sie unter [Anpassen einer ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) und [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer-Ansicht (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Benutzerdefinierte Renderer ViewCell (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
