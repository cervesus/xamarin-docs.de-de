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
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998177"
---
# <a name="listview-performance"></a>ListView-Leistung

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

Beim Schreiben von mobilen Anwendungen ist die Leistung wichtig. Benutzer stammen, ebenso der geglättete Bildlauf und schnelle Ladezeiten zu erwarten. Nicht den Erwartungen der Benutzer erfüllt wird, kostet Sie Bewertungen in den App Store oder im Falle einer Line-of-Business-Anwendung, die Ihr Unternehmen Zeit und Geld Kosten.

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) ist eine leistungsstarke Ansicht für die Anzeige von Daten, weist jedoch einige Einschränkungen auf. Die Bild Laufleistung kann bei der Verwendung benutzerdefinierter Zellen beeinträchtigt werden, insbesondere wenn Sie tief geschachtelte Ansichts Hierarchien enthalten oder bestimmte Layouts verwenden, die eine komplexe Messung erfordern. Es gibt Methoden, die Sie verwenden können, um Leistungseinbußen zu vermeiden.

## <a name="caching-strategy"></a>Strategie zum Zwischenspeichern

ListViews werden häufig verwendet, um wesentlich mehr Daten anzuzeigen, als auf den Bildschirm passen. Eine Musik-app kann z. b. eine Bibliothek von Liedern mit Tausenden von Einträgen enthalten. Wenn Sie ein Element für jeden Eintrag erstellen, würde dies zu einer geringen Speicherauslastung führen. Das kontinuierliche erstellen und zerstören von Zeilen würde dazu führen, dass die Anwendung ständig Objekte instanziiert und bereinigt, was ebenfalls eine schlechte Leistung mit sich bringt.

Um Arbeitsspeicher zu sparen, [`ListView`](xref:Xamarin.Forms.ListView) verfügen die nativen Entsprechungen für jede Plattform über integrierte Funktionen für die Wiederverwendung von Zeilen. Nur die Zellen, die auf dem Bildschirm sichtbar sind in den Arbeitsspeicher geladen und die **Inhalt** in vorhandenen Zellen geladen wird. Dieses Muster verhindert, dass die Anwendung tausende von Objekten instanziiert, und spart Zeit und Arbeitsspeicher.

Xamarin. Forms ermöglicht [`ListView`](xref:Xamarin.Forms.ListView) die Wiederverwendung von [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) Zellen über die-Enumeration mit den folgenden Werten:

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

Die [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) cachingstrategie gibt an, dass die [ `ListView` ](xref:Xamarin.Forms.ListView) generiert eine Zelle für jedes Element in der Liste aus, und der Standardwert ist `ListView` Verhalten. Sie sollte in den folgenden Situationen verwendet werden:

- Jede Zelle verfügt über eine große Anzahl von Bindungen (20-30 +).
- Die Zellen Vorlage ändert sich häufig.
- Das Testen zeigt, `RecycleElement` dass die zwischen Speicherungs Strategie zu einer geringeren Ausführungsgeschwindigkeit führt.

Es ist wichtig zu erkennen, die Folgen der [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) cachingstrategie, bei der Arbeit mit benutzerdefinierten Zellen. Jede Zelle Initialisierungscode ausführen für jede Zelle erstellen, müssen die möglicherweise mehrere Male pro Sekunde. In diesem Fall werden Layouttechniken, die auf einer Seite gut waren, wie z. [`StackLayout`](xref:Xamarin.Forms.StackLayout) b. die Verwendung mehrerer geclusterte Instanzen, zu Leistungs Engpässen, wenn Sie in Echtzeit eingerichtet und zerstört werden, während der Benutzer einen Bildlauf durchführt.

### <a name="recycleelement"></a>RecycleElement

Die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie gibt an, dass die [ `ListView` ](xref:Xamarin.Forms.ListView) wird versucht, die Arbeitsspeicher-Speicherbedarf und die ausführungsgeschwindigkeit zu minimieren, durch Wiederverwenden von Listenzellen. Dieser Modus bietet nicht immer eine Leistungsverbesserung, und Tests sollten durchgeführt werden, um alle Verbesserungen zu ermitteln. Dies ist jedoch die bevorzugte Wahl und sollte in den folgenden Situationen verwendet werden:

- Jede Zelle hat eine kleine bis mittlere Anzahl von Bindungen.
- In jeder Zelle [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) werden alle Zelldaten definiert.
- Jede Zelle ist größtenteils ähnlich, und die Zellen Vorlage ändert sich unverändert.

Während der Virtualisierung die Zelle wird dem Bindungskontext aktualisiert haben, und daher eine Anwendung in diesem Modus verwendet sie müssen sicherstellen, dass Kontext bindungsaktualisierungen richtig behandelt werden. Alle Daten zur Zelle müssen aus dem Bindungskontext stammen oder Konsistenz treten möglicherweise Fehler auf. Dieses Problem kann vermieden werden, indem Sie die Datenbindung zum Anzeigen von Zelldaten verwenden. Alternativ sollten Zellendaten festgelegt werden, der `OnBindingContextChanged` außer Kraft setzen, anstatt im Konstruktor der benutzerdefinierten Zelle ab, wie im folgenden Codebeispiel wird veranschaulicht:

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

### <a name="set-the-caching-strategy"></a>Festlegen der cachingstrategie

Die [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Enumerationswert angegeben ist, mit einer [ `ListView` ](xref:Xamarin.Forms.ListView) Überladung des Konstruktors, wie im folgenden Codebeispiel gezeigt:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

Legen Sie in XAML das `CachingStrategy` -Attribut fest, wie in der folgenden XAML-Datei gezeigt:

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

Das Festlegen `CachingStrategy` des-Attributs aus XAML für [`ListView`](xref:Xamarin.Forms.ListView) eine untergeordnete Klasse führt nicht zum gewünschten Verhalten, da `CachingStrategy` keine Eigenschaft `ListView`für vorhanden ist. Wenn [xamlc](~/xamarin-forms/xaml/xamlc.md) aktiviert ist, wird außerdem die folgende Fehlermeldung erzeugt: **Es wurde keine Eigenschaft, keine bindbare Eigenschaft oder ein Ereignis für ' cachingstrategy ' gefunden.**

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

## <a name="listview-performance-suggestions"></a>ListView-Leistungs Vorschläge

Es gibt viele Techniken zum Verbessern der Leistung von `ListView`. Die folgenden Vorschläge können die Leistung von ListView verbessern.

- Binden der `ItemsSource` Eigenschaft, um eine `IList<T>` -Sammlung anstelle einer `IEnumerable<T>` -Auflistung, da `IEnumerable<T>` Sammlungen nicht zufälligen Zugriff unterstützt.
- Verwenden Sie `TextCell` die integrierten Zellen (  /  `SwitchCell` z. b.) `ViewCell` anstelle von, wenn dies möglich ist.
- Verwenden Sie weniger Elemente. Verwenden Sie z. b. eine `FormattedString` einzelne Bezeichnung anstelle mehrerer Bezeichnungen.
- Ersetzen Sie die `ListView` mit einem `TableView` Anzeigen von nicht homogen sind. Daten – d. h. Daten verschiedener Typen.
- Einschränken der Verwendung der [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode. Wenn übermäßig beansprucht, wird Leistung beeinträchtigt werden.
- Unter Android, vermeiden Sie das Festlegen einer `ListView`des Trennzeichen Sichtbarkeit Zeile oder die Farbe, nachdem es instanziiert wurde, da dies zu großen Leistungseinbußen führt.
- Vermeiden von Änderungen das Zellenlayout basierend auf den [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Das Ändern des Layouts führt zu großen Messungs-und Initialisierungs Kosten.
- Vermeiden Sie tief verschachtelte Layout-Hierarchien. Verwendung `AbsoluteLayout` oder `Grid` zur Reduzierung der Schachtelungsebenen.
- Vermeiden Sie `LayoutOptions` eine bestimmte `Fill` andere`Fill` als (ist der günstigste Wert für Compute).
- Ordnen Sie keine `ListView` innerhalb einer `ScrollView` aus den folgenden Gründen:
  - Die `ListView` implementiert eigene scrollen.
  - Die `ListView` erhalten, wie sie vom übergeordneten Element verarbeitet werden, werden keine Bewegungen `ScrollView`.
  - Die `ListView` kann mit den Elementen der Liste, möglicherweise der Funktionalität bietet, die eine benutzerdefinierte Kopf- und Fußzeile, die gescrollt darstellen der `ScrollView` für verwendet wurde. Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Nehmen Sie einen benutzerdefinierten Renderer in Erwägung, wenn Sie einen bestimmten komplexen Entwurf benötigen, der in ihren Zellen dargestellt wird.

`AbsoluteLayout`bietet die Möglichkeit, Layouts ohne einen einzigen Measure-Befehl auszuführen, sodass es sehr leistungsfähig ist. Wenn `AbsoluteLayout` nicht verwendet haben, sollten Sie [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout). Wenn `RelativeLayout`, die direkte Übergabe der Einschränkungen werden wesentlich schneller als mit dem API-Ausdruck. Diese Methode ist schneller, da die Ausdrucks-API JIT verwendet, und unter IOS muss die Struktur interpretiert werden, was langsamer ist. Der Ausdruck API eignet sich für Seitenlayouts, in denen nur erforderlich, es auf das anfängliche Layout sowie die Drehung, aber im `ListView`, bei der Ausführung im Bildlauf, ständig es beeinträchtigt die Leistung.

Erstellen eines benutzerdefinierten Renderers für eine [ `ListView` ](xref:Xamarin.Forms.ListView) oder deren Zellen erreicht werden, indem die Auswirkungen der layoutberechnungen mit bildlaufleistung. Weitere Informationen finden Sie unter [Anpassen einer ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) und [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer-Ansicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Benutzerdefinierte Renderer ViewCell (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
