---
title: ''
description: Obwohl ListView eine leistungsstarke Ansicht für die Anzeige von Daten ist, bestehen einige Einschränkungen. In diesem Artikel wird erläutert, wie Sie mit einer Xamarin.Forms ListView in einer Anwendung eine hohe Leistung sicherstellen.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e2b8e057d9687cd0a472451fc73cc578f9358277
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139879"
---
# <a name="listview-performance"></a>ListView-Leistung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

Beim Schreiben mobiler Anwendungen ist die Leistung von Bedeutung. Benutzer erwarten einen reibungslosen Bildlauf und schnelle Ladezeiten. Wenn Sie die Erwartungen Ihrer Benutzer nicht erfüllen, können Sie Ihre Bewertungen im Anwendungs Speicherkosten, oder bei einer Branchen Anwendung Kosten und Ihre Organisation Zeit und Geld Kosten.

Die Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) ist eine leistungsstarke Ansicht für die Anzeige von Daten, aber Sie weist einige Einschränkungen auf. Die Bild Laufleistung kann bei der Verwendung benutzerdefinierter Zellen beeinträchtigt werden, insbesondere wenn Sie tief geschachtelte Ansichts Hierarchien enthalten oder bestimmte Layouts verwenden, die eine komplexe Messung erfordern. Glücklicherweise gibt es Techniken, die Sie verwenden können, um eine schlechte Leistung zu vermeiden.

## <a name="caching-strategy"></a>Strategie zum Zwischenspeichern

ListViews werden häufig verwendet, um wesentlich mehr Daten anzuzeigen, als auf den Bildschirm passen. Eine Musik-app kann z. b. eine Bibliothek von Liedern mit Tausenden von Einträgen enthalten. Wenn Sie ein Element für jeden Eintrag erstellen, würde dies zu einer geringen Speicherauslastung führen. Das kontinuierliche erstellen und zerstören von Zeilen würde dazu führen, dass die Anwendung ständig Objekte instanziiert und bereinigt, was ebenfalls eine schlechte Leistung mit sich bringt.

Um Arbeitsspeicher zu sparen, verfügen die nativen [`ListView`](xref:Xamarin.Forms.ListView) Entsprechungen für jede Plattform über integrierte Funktionen für die Wiederverwendung von Zeilen. Nur die auf dem Bildschirm sichtbaren Zellen werden in den Arbeitsspeicher geladen, und der **Inhalt** wird in vorhandene Zellen geladen. Dieses Muster verhindert, dass die Anwendung tausende von Objekten instanziiert, und spart Zeit und Arbeitsspeicher.

Xamarin.Formsermöglicht die [`ListView`](xref:Xamarin.Forms.ListView) Wiederverwendung von Zellen über die- [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Enumeration mit den folgenden Werten:

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Die universelle Windows-Plattform (UWP) ignoriert die zwischen Speicherungs [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Strategie, weil Sie immer die Zwischenspeicherung verwendet, um die Leistung zu verbessern. Daher verhält es sich standardmäßig so, als ob die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie angewendet wird.

### <a name="retainelement"></a>Retainelement

Die Cache [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Strategie gibt an, dass die [`ListView`](xref:Xamarin.Forms.ListView) eine Zelle für jedes Element in der Liste generiert und das Standard `ListView` Verhalten ist. Sie sollte in den folgenden Situationen verwendet werden:

- Jede Zelle verfügt über eine große Anzahl von Bindungen (20-30 +).
- Die Zellen Vorlage ändert sich häufig.
- Das Testen zeigt, dass die zwischen Speicherungs `RecycleElement` Strategie zu einer geringeren Ausführungsgeschwindigkeit führt.

Beim Arbeiten mit benutzerdefinierten Zellen ist es wichtig, die Konsequenzen der [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) cachingstrategie zu erkennen. Jeder Zellen Initialisierungs Code muss für jede Zell Erstellung ausgeführt werden, die mehrmals pro Sekunde ausgeführt werden kann. In diesem Fall werden Layouttechniken, die auf einer Seite gut waren, wie z. b. die Verwendung mehrerer geclusterte [`StackLayout`](xref:Xamarin.Forms.StackLayout) Instanzen, zu Leistungs Engpässen, wenn Sie in Echtzeit eingerichtet und zerstört werden, während der Benutzer einen Bildlauf durchführt.

### <a name="recycleelement"></a>Recycleelement

Die Cache [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Strategie gibt an, dass versucht, den [`ListView`](xref:Xamarin.Forms.ListView) Speicherbedarf und die Ausführungsgeschwindigkeit durch die Wiederverwendung von Listen Zellen zu minimieren. Dieser Modus bietet nicht immer eine Leistungsverbesserung, und Tests sollten durchgeführt werden, um alle Verbesserungen zu ermitteln. Dies ist jedoch die bevorzugte Wahl und sollte in den folgenden Situationen verwendet werden:

- Jede Zelle hat eine kleine bis mittlere Anzahl von Bindungen.
- In jeder Zelle werden [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) alle Zelldaten definiert.
- Jede Zelle ist größtenteils ähnlich, und die Zellen Vorlage ändert sich unverändert.

Während der Virtualisierung wird der Bindungs Kontext der Zelle aktualisiert. Wenn eine Anwendung diesen Modus verwendet, muss daher sichergestellt werden, dass Bindungs Kontext Aktualisierungen entsprechend behandelt werden. Alle Daten über die Zelle müssen aus dem Bindungs Kontext stammen, oder es können Konsistenz Fehler auftreten. Dieses Problem kann vermieden werden, indem Sie die Datenbindung zum Anzeigen von Zelldaten verwenden. Alternativ sollten Zellen Daten in der `OnBindingContextChanged` Überschreibung statt im Konstruktor der benutzerdefinierten Zelle festgelegt werden, wie im folgenden Codebeispiel gezeigt:

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

Wenn Zellen unter IOS und Android benutzerdefinierte Renderer verwenden, müssen Sie sicherstellen, dass die Benachrichtigung über die Eigenschafts Änderung ordnungsgemäß implementiert wird. Wenn Zellen wieder verwendet werden, ändern sich die Eigenschaftswerte, wenn der Bindungs Kontext auf den einer verfügbaren Zelle aktualisiert wird, wobei `PropertyChanged` Ereignisse ausgelöst werden. Weitere Informationen finden Sie unter [Anpassen einer viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>Recycleelement mit DataTemplateSelector

Wenn eine [`ListView`](xref:Xamarin.Forms.ListView) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zum Auswählen eines verwendet [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , werden von der [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie keine `DataTemplate` s zwischengespeichert. Stattdessen `DataTemplate` wird für jedes Datenelement in der Liste eine ausgewählt.

> [!NOTE]
> Die zwischen Speicherungs [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Strategie verfügt über eine Voraussetzung, die in 2,4 eingeführt wurde Xamarin.Forms , wenn eine [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zur Auswahl einer aufgefordert wird, die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) jeweils `DataTemplate` denselben Typ zurückgeben muss [`ViewCell`](xref:Xamarin.Forms.ViewCell) . Wenn z. b. ein mit einem-Wert zurückgegeben wird, der [`ListView`](xref:Xamarin.Forms.ListView) `DataTemplateSelector` entweder zurück `MyDataTemplateA` gibt (wobei `MyDataTemplateA` einen `ViewCell` vom Typ zurückgibt `MyViewCellA` ), oder `MyDataTemplateB` (wobei `MyDataTemplateB` eine `ViewCell` vom Typ zurückgibt `MyViewCellB` ), muss zurückgegeben werden, `MyDataTemplateA` `MyViewCellA` oder es wird eine Ausnahme ausgelöst.

### <a name="recycleelementanddatatemplate"></a>Recycleelementanddatatemplate

Die [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) zwischen Speicherungs Strategie basiert auf der [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie, indem sichergestellt wird, dass, wenn eine [`ListView`](xref:Xamarin.Forms.ListView) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zum Auswählen eines verwendet [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die `DataTemplate` e durch den Typ des Elements in der Liste zwischengespeichert werden. Daher `DataTemplate` werden s einmal pro Elementtyp und nicht einmal pro Element Instanz ausgewählt.

> [!NOTE]
> Die zwischen Speicherungs [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Strategie hat eine Voraussetzung, dass die `DataTemplate` von zurückgegebenen s [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) den- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) Konstruktor verwenden müssen, der einen annimmt `Type` .

### <a name="set-the-caching-strategy"></a>Festlegen der cachingstrategie

Der- [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Enumerationswert wird mit einer [`ListView`](xref:Xamarin.Forms.ListView) Konstruktorüberladung angegeben, wie im folgenden Codebeispiel gezeigt:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

Legen Sie in XAML das-Attribut fest, `CachingStrategy` wie in der folgenden XAML-Datei gezeigt:

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

Diese Methode hat dieselbe Auswirkung wie das Festlegen des cachingstrategie-Arguments im Konstruktor in c#.

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>Festlegen der zwischen Speicherungs Strategie in einer untergeordneten "ListView"

Das Festlegen des- `CachingStrategy` Attributs aus XAML für eine untergeordnete Klasse [`ListView`](xref:Xamarin.Forms.ListView) führt nicht zum gewünschten Verhalten, da keine `CachingStrategy` Eigenschaft für vorhanden ist `ListView` . Wenn [xamlc](~/xamarin-forms/xaml/xamlc.md) aktiviert ist, wird außerdem die folgende Fehlermeldung erzeugt: **keine Eigenschaft, bindbare Eigenschaft oder Ereignis für "cachingstrategy" gefunden** .

Die Lösung für dieses Problem besteht in der Angabe eines Konstruktors für die Unterklasse [`ListView`](xref:Xamarin.Forms.ListView) , die einen [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Parameter akzeptiert und an die Basisklasse übergibt:

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

Anschließend [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) kann der Enumerationswert mithilfe der Syntax aus XAML angegeben werden `x:Arguments` :

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>ListView-Leistungs Vorschläge

Es gibt viele Techniken zum Verbessern der Leistung von `ListView` . Die folgenden Vorschläge können die Leistung von ListView verbessern.

- Binden Sie die- `ItemsSource` Eigenschaft an eine-Auflistung `IList<T>` anstatt an eine-Auflistung, da Auflistungen `IEnumerable<T>` `IEnumerable<T>` keinen zufälligen Zugriff unterstützen.
- Verwenden Sie die integrierten Zellen (z `TextCell`  /  `SwitchCell` . b.) anstelle von, `ViewCell` Wenn dies möglich ist.
- Verwenden Sie weniger Elemente. Verwenden Sie z. b. eine einzelne `FormattedString` Bezeichnung anstelle mehrerer Bezeichnungen.
- Ersetzen Sie `ListView` durch eine `TableView` , wenn nicht homogene Daten angezeigt werden, d. –. Daten von unterschiedlichen Typen.
- Beschränken Sie die Verwendung der- [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode. Wenn diese über verwendet wird, wird die Leistung beeinträchtigt.
- Vermeiden Sie unter Android das Festlegen `ListView` der Sichtbarkeit oder Farbe von Zeilen Trennzeichen, nachdem Sie instanziiert wurde. Dies führt zu einer hohen Leistungs Einbuße.
- Vermeiden Sie das Ändern des Zellen Layouts auf der Grundlage der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) . Das Ändern des Layouts führt zu großen Messungs-und Initialisierungs Kosten.
- Vermeiden Sie tief geschachtelte layouthierarchien. Verwenden `AbsoluteLayout` `Grid` Sie oder, um die Schachtelung zu verringern.
- Vermeiden Sie eine bestimmte `LayoutOptions` andere als `Fill` ( `Fill` ist der günstigste Wert für Compute).
- Vermeiden Sie `ListView` `ScrollView` aus den folgenden Gründen das Platzieren eines in einem:
  - Der `ListView` implementiert seinen eigenen Bildlauf.
  - Der `ListView` empfängt keine Gesten, da diese vom übergeordneten Element behandelt werden `ScrollView` .
  - Der `ListView` kann eine angepasste Kopf-und Fußzeile darstellen, die mit den Elementen der Liste rollt und potenziell die Funktionalität bereitstellt, für die der `ScrollView` verwendet wurde. Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Nehmen Sie einen benutzerdefinierten Renderer in Erwägung, wenn Sie einen bestimmten komplexen Entwurf benötigen, der in ihren Zellen dargestellt wird.

`AbsoluteLayout`bietet die Möglichkeit, Layouts ohne einen einzigen Measure-Befehl auszuführen, sodass es sehr leistungsfähig ist. Wenn `AbsoluteLayout` nicht verwendet werden kann, sollten Sie in Erwägung gezogen [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . Wenn verwendet wird `RelativeLayout` , sind die Übergabe von Einschränkungen erheblich schneller als die Verwendung der Expression-API. Diese Methode ist schneller, da die Ausdrucks-API JIT verwendet, und unter IOS muss die Struktur interpretiert werden, was langsamer ist. Die Expression-API eignet sich für Seitenlayouts, bei denen Sie nur beim anfänglichen Layout und der Drehung erforderlich ist, aber in `ListView` , wo Sie während des Bildlaufs ständig ausgeführt wird, beeinträchtigt Sie die Leistung.

Das Erstellen eines benutzerdefinierten Renderers für eine [`ListView`](xref:Xamarin.Forms.ListView) oder seine Zellen ist ein Ansatz, mit dem die Auswirkungen von Layoutberechnungen auf die scrollleistung reduziert werden. Weitere Informationen finden Sie unter [Anpassen einer ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) und [Anpassen einer viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte rendereransicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Benutzerdefinierter Renderer viewcell (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [Listviewcachingstrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
