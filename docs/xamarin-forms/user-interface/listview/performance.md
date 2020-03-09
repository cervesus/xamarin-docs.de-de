---
title: ListView-Leistung
description: ListView eine leistungsfähige Ansicht zum Anzeigen von Daten ist, hat aber einige Einschränkungen. In diesem Artikel wird erläutert, wie hohe Leistung mit einer Xamarin.Forms-ListView, in einer Anwendung sichergestellt wird.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: ed920db129d2af203046a648c0069580f99a295e
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916930"
---
# <a name="listview-performance"></a>ListView-Leistung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

Beim Schreiben von mobilen Anwendungen ist die Leistung wichtig. Benutzer stammen, ebenso der geglättete Bildlauf und schnelle Ladezeiten zu erwarten. Nicht den Erwartungen der Benutzer erfüllt wird, kostet Sie Bewertungen in den App Store oder im Falle einer Line-of-Business-Anwendung, die Ihr Unternehmen Zeit und Geld Kosten.

Die xamarin. Forms- [`ListView`](xref:Xamarin.Forms.ListView) ist eine leistungsstarke Ansicht für die Anzeige von Daten, aber Sie weist einige Einschränkungen auf. Die Bild Laufleistung kann bei der Verwendung benutzerdefinierter Zellen beeinträchtigt werden, insbesondere wenn Sie tief geschachtelte Ansichts Hierarchien enthalten oder bestimmte Layouts verwenden, die eine komplexe Messung erfordern. Es gibt Methoden, die Sie verwenden können, um Leistungseinbußen zu vermeiden.

## <a name="caching-strategy"></a>Strategie zum Zwischenspeichern

ListViews werden häufig verwendet, um wesentlich mehr Daten anzuzeigen, als auf den Bildschirm passen. Eine Musik-app kann z. b. eine Bibliothek von Liedern mit Tausenden von Einträgen enthalten. Wenn Sie ein Element für jeden Eintrag erstellen, würde dies zu einer geringen Speicherauslastung führen. Das kontinuierliche erstellen und zerstören von Zeilen würde dazu führen, dass die Anwendung ständig Objekte instanziiert und bereinigt, was ebenfalls eine schlechte Leistung mit sich bringt.

Um Arbeitsspeicher zu sparen, verfügen die systemeigenen [`ListView`](xref:Xamarin.Forms.ListView) Entsprechungen für jede Plattform über integrierte Funktionen für die Wiederverwendung von Zeilen. Nur die auf dem Bildschirm sichtbaren Zellen werden in den Arbeitsspeicher geladen, und der **Inhalt** wird in vorhandene Zellen geladen. Dieses Muster verhindert, dass die Anwendung tausende von Objekten instanziiert, und spart Zeit und Arbeitsspeicher.

Xamarin. Forms ermöglicht die Wiederverwendung von [`ListView`](xref:Xamarin.Forms.ListView) Zellen über die [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) -Enumeration, die über die folgenden Werte verfügt:

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Die universelle Windows-Plattform (UWP) ignoriert die [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Caching-Strategie, da Sie immer die Leistung verbessert, um die Leistung zu verbessern. Daher verhält es sich standardmäßig so, als ob die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Caching-Strategie angewendet wird.

### <a name="retainelement"></a>RetainElement

Die Strategie für die [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Zwischenspeicherung gibt an, dass die [`ListView`](xref:Xamarin.Forms.ListView) eine Zelle für jedes Element in der Liste generiert, und ist das Standardverhalten für `ListView`. Sie sollte in den folgenden Situationen verwendet werden:

- Jede Zelle verfügt über eine große Anzahl von Bindungen (20-30 +).
- Die Zellen Vorlage ändert sich häufig.
- Das Testen zeigt, dass die `RecycleElement` Cache Strategie zu einer geringeren Ausführungsgeschwindigkeit führt.

Beim Arbeiten mit benutzerdefinierten Zellen ist es wichtig, die Konsequenzen der Strategie für die [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Zwischenspeicherung zu erkennen. Jede Zelle Initialisierungscode ausführen für jede Zelle erstellen, müssen die möglicherweise mehrere Male pro Sekunde. In diesem Fall werden Layouttechniken, die auf einer Seite gut waren, wie die Verwendung mehrerer [`StackLayout`](xref:Xamarin.Forms.StackLayout) Instanzen, zu Leistungs Engpässen, wenn Sie in Echtzeit eingerichtet und zerstört werden, während der Benutzer einen Bildlauf durchführt.

### <a name="recycleelement"></a>RecycleElement

Die Strategie für die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Zwischenspeicherung gibt an, dass der [`ListView`](xref:Xamarin.Forms.ListView) versucht, den Speicherbedarf und die Ausführungsgeschwindigkeit durch wieder verwenden von Listen Zellen zu minimieren. Dieser Modus bietet nicht immer eine Leistungsverbesserung, und Tests sollten durchgeführt werden, um alle Verbesserungen zu ermitteln. Dies ist jedoch die bevorzugte Wahl und sollte in den folgenden Situationen verwendet werden:

- Jede Zelle hat eine kleine bis mittlere Anzahl von Bindungen.
- In den [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) einer Zelle werden alle Zelldaten definiert.
- Jede Zelle ist größtenteils ähnlich, und die Zellen Vorlage ändert sich unverändert.

Während der Virtualisierung die Zelle wird dem Bindungskontext aktualisiert haben, und daher eine Anwendung in diesem Modus verwendet sie müssen sicherstellen, dass Kontext bindungsaktualisierungen richtig behandelt werden. Alle Daten zur Zelle müssen aus dem Bindungskontext stammen oder Konsistenz treten möglicherweise Fehler auf. Dieses Problem kann vermieden werden, indem Sie die Datenbindung zum Anzeigen von Zelldaten verwenden. Alternativ sollten Zellen Daten im `OnBindingContextChanged` Überschreibungs-und nicht im Konstruktor der benutzerdefinierten Zelle festgelegt werden, wie im folgenden Codebeispiel gezeigt:

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

Weitere Informationen finden Sie unter [Bindungs Kontext Änderungen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Unter iOS und Android müssen Zellen benutzerdefinierte Renderer verwenden, sie sicherstellen, dass eigenschaftenänderungen ordnungsgemäß implementiert wird. Wenn Zellen wieder verwendet werden, ändern sich Ihre Eigenschaftswerte, wenn der Bindungs Kontext auf den einer verfügbaren Zelle aktualisiert wird, wobei `PropertyChanged` Ereignisse ausgelöst werden. Weitere Informationen finden Sie unter [Anpassen einer viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement mit einem DataTemplateSelector

Wenn eine [`ListView`](xref:Xamarin.Forms.ListView) einen [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) verwendet, um eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)auszuwählen, werden `DataTemplate`s von der [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Caching-Strategie nicht zwischengespeichert. Stattdessen wird für jedes Datenelement in der Liste ein `DataTemplate` ausgewählt.

> [!NOTE]
> Die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Cache Strategie verfügt über eine Voraussetzung, die in xamarin. Forms 2,4 eingeführt wurde. Dies ist der Fall, wenn ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) aufgefordert wird, eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) auszuwählen, die jedes `DataTemplate` denselben [`ViewCell`](xref:Xamarin.Forms.ViewCell) Typ zurückgeben muss. Wenn z. b. eine [`ListView`](xref:Xamarin.Forms.ListView) mit einer `DataTemplateSelector` zurückgeben kann, die entweder `MyDataTemplateA` zurückgeben kann (wobei `MyDataTemplateA` eine `ViewCell` des Typs `MyViewCellA`zurückgibt) oder `MyDataTemplateB` (wobei `MyDataTemplateB` einen `ViewCell` vom Typ `MyViewCellB`zurückgibt), muss die Rückgabe `MyDataTemplateA` zurückgegeben werden, oder es wird eine Ausnahme ausgelöst.`MyViewCellA`

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

Die [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) zwischen Speicherungs Strategie basiert auf der [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Caching-Strategie, indem sichergestellt wird, dass, wenn eine [`ListView`](xref:Xamarin.Forms.ListView) einen [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)verwendet, `DataTemplate`s vom Typ des Elements in der Liste zwischengespeichert werden. Aus diesem Grund werden `DataTemplate`s einmal pro Elementtyp und nicht einmal pro Element Instanz ausgewählt.

> [!NOTE]
> Die [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Cache Strategie hat eine Voraussetzung dafür, dass die vom [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zurückgegebenen `DataTemplate`s den [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) -Konstruktor verwenden müssen, der einen `Type`annimmt.

### <a name="set-the-caching-strategy"></a>Festlegen der cachingstrategie

Der [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Enumerationswert wird mit einer [`ListView`](xref:Xamarin.Forms.ListView) -Konstruktorüberladung angegeben, wie im folgenden Codebeispiel gezeigt:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

Legen Sie in XAML das `CachingStrategy`-Attribut fest, wie in der folgenden XAML-Datei gezeigt:

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

Diese Methode hat dieselbe Auswirkung wie das Festlegen des cachingstrategiearguments im Konstruktor C#in.

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>Festlegen der zwischen Speicherungs Strategie in einer untergeordneten "ListView"

Das Festlegen des `CachingStrategy` Attributs von XAML auf eine untergeordnete [`ListView`](xref:Xamarin.Forms.ListView) erzeugt nicht das gewünschte Verhalten, da es in `ListView`keine `CachingStrategy` Eigenschaft gibt. Wenn [xamlc](~/xamarin-forms/xaml/xamlc.md) aktiviert ist, wird außerdem die folgende Fehlermeldung erzeugt: **keine Eigenschaft, bindbare Eigenschaft oder Ereignis für "cachingstrategy" gefunden** .

Die Lösung für dieses Problem besteht darin, einen Konstruktor für die untergeordnete [`ListView`](xref:Xamarin.Forms.ListView) anzugeben, die einen [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Parameter annimmt und an die Basisklasse übergibt:

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

Anschließend kann der [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Enumerationswert mithilfe der `x:Arguments` Syntax aus XAML angegeben werden:

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>ListView-Leistungs Vorschläge

Es gibt viele Techniken zum Verbessern der Leistung einer `ListView`. Die folgenden Vorschläge können die Leistung von ListView verbessern.

- Binden Sie die `ItemsSource`-Eigenschaft an eine `IList<T>` Auflistung anstatt an eine `IEnumerable<T>` Collection, da `IEnumerable<T>` Auflistungen keinen zufälligen Zugriff unterstützen.
- Verwenden Sie die integrierten Zellen (z. b. `TextCell` / `SwitchCell`) anstelle von `ViewCell`, wann immer dies möglich ist.
- Verwenden Sie weniger Elemente. Verwenden Sie z. b. eine einzelne `FormattedString` Bezeichnung anstelle mehrerer Bezeichnungen.
- Ersetzen Sie die `ListView` durch eine `TableView`, wenn nicht homogene Daten angezeigt werden, d. –. Daten von unterschiedlichen Typen.
- Beschränken Sie die Verwendung der [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) -Methode. Wenn übermäßig beansprucht, wird Leistung beeinträchtigt werden.
- Vermeiden Sie unter Android das Festlegen der Sichtbarkeit oder Farbe eines `ListView`Zeilen Trennzeichens, nachdem Sie instanziiert wurde, da dies zu einer hohen Leistungs Einbuße führt.
- Vermeiden Sie das Ändern des Zellen Layouts basierend auf dem [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). Das Ändern des Layouts führt zu großen Messungs-und Initialisierungs Kosten.
- Vermeiden Sie tief verschachtelte Layout-Hierarchien. Verwenden Sie `AbsoluteLayout` oder `Grid`, um die Schachtelung zu verringern.
- Vermeiden Sie eine bestimmte `LayoutOptions` außer `Fill` (`Fill` ist der günstigste Wert für Compute).
- Vermeiden Sie aus folgenden Gründen das Platzieren einer `ListView` in einer `ScrollView`:
  - Der `ListView` implementiert seinen eigenen Bildlauf.
  - Der `ListView` empfängt keine Gesten, da diese von der übergeordneten `ScrollView`behandelt werden.
  - Der `ListView` kann eine angepasste Kopf-und Fußzeile darstellen, die mit den Elementen der Liste scrollt und möglicherweise die Funktionalität bietet, für die der `ScrollView` verwendet wurde. Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Nehmen Sie einen benutzerdefinierten Renderer in Erwägung, wenn Sie einen bestimmten komplexen Entwurf benötigen, der in ihren Zellen dargestellt wird.

`AbsoluteLayout` hat die Möglichkeit, Layouts ohne einen einzigen Measure-Befehl auszuführen, sodass es sehr leistungsfähig ist. Wenn `AbsoluteLayout` nicht verwendet werden kann, sollten Sie [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)in Erwägung gezogen. Wenn Sie `RelativeLayout`verwenden, sind die Übergabe von Einschränkungen erheblich schneller als die Verwendung der Expression-API. Diese Methode ist schneller, da die Ausdrucks-API JIT verwendet, und unter IOS muss die Struktur interpretiert werden, was langsamer ist. Die Expression-API eignet sich für Seitenlayouts, bei denen Sie nur beim anfänglichen Layout und der Drehung erforderlich ist, aber in `ListView`, wo Sie während des Bildlaufs ständig ausgeführt wird, beeinträchtigt Sie die Leistung.

Das Erstellen eines benutzerdefinierten Renderers für eine [`ListView`](xref:Xamarin.Forms.ListView) oder seine Zellen ist ein Ansatz, um die Auswirkungen der Layoutberechnungen auf die scrollleistung zu reduzieren. Weitere Informationen finden Sie unter [Anpassen einer ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) und [Anpassen einer viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte rendereransicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Benutzerdefinierter Renderer viewcell (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [Listviewcachingstrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
