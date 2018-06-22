---
title: ListView-Leistung
description: Obwohl ListView eine leistungsstarke Ansicht zum Anzeigen von Daten ist, gibt es einige Einschränkungen. In diesem Artikel erläutert die gute Leistung mit Xamarin.Forms ListView in einer Anwendung zu gewährleisten.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: f1707a6b2a1dc03ae1346520bf29ff83f0fe74fb
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309810"
---
# <a name="listview-performance"></a>ListView-Leistung

Beim Schreiben von mobilen Anwendungen, wichtig ist die Leistung. Benutzer haben Sie smooth Durchführen eines Bildlaufs und schnellen Ladezeiten erwartet stammen. Wegen eines Fehlers beim Ihrer Benutzer Erwartungen werden Kosten Sie Bewertungen in den App Store, oder sich für eine Line-of-Business-Anwendung Kosten Ihrem Unternehmen Zeit und Geld.

Obwohl [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ist eine leistungsstarke Ansicht zum Anzeigen von Daten aus, es gelten einige Einschränkungen. Durchführen eines Bildlaufs Leistung kann beeinträchtigt werden, wenn benutzerdefinierte Zellen zu verwenden, insbesondere, wenn sie in der Schachtelungshierarchie Ansicht Hierarchien enthalten oder verwenden bestimmte Layouts, die viele der Messung erfordern. Glücklicherweise sind Techniken, die Sie verwenden können, um Leistungseinbußen zu vermeiden.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Zwischenspeicherstrategie

Listenansichten werden häufig verwendet, um erheblich mehr Daten als Anzeige anpassen, die auf dem Bildschirm. Betrachten Sie eine Musik-app, beispielsweise ein. Eine Bibliothek von Songs ermöglicht möglicherweise Tausende von Einträgen. Die einfache Ansatz, nämlich die zum Erstellen einer Zeile für jede Musiktitel wäre, würde eine schlechte Leistung haben. Dieser Ansatz wertvollen Speicherplatz verschwendet und verlangsamt Bildlauf zu "Durchforstung". Ein anderer Ansatz besteht darin erstellen und Zerstören von Zeilen, wie Daten einen Bildlauf angezeigt werden. Dies erfordert Konstante Instanziierung und Bereinigung der Objekte anzeigen, die sehr langsam sein kann.

Freigeben von Arbeitsspeicher, der systemeigenen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Entsprechungen für jede Plattform haben integrierte Funktionen für die erneute Verwendung von Zeilen. Nur die Zellen, die auf dem Bildschirm angezeigt werden in den Arbeitsspeicher geladen und die **Inhalt** in vorhandenen Zellen geladen wird. Dies verhindert, dass die Anwendung nicht zu Tausende von Objekten, Zeit gespart und der Arbeitsspeicher zu instanziieren.

Xamarin.Forms ermöglicht [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Zelle erneut verwenden, über die [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) -Enumeration, die den folgenden Werten:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Die universelle Windows-Plattform (UWP) ignoriert die [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) cachingstrategie, da sie verwendet stets die Zwischenspeicherung zur Verbesserung der Leistung. Aus diesem Grund standardmäßig verhält sich wie die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie angewendet wird.

### <a name="retainelement"></a>RetainElement

Die [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) cachingstrategie gibt an, dass die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) generiert eine Zelle für jedes Element in der Liste aus, und ist die Standardeinstellung `ListView` Verhalten. Sie sollte in der Regel in folgenden Situationen verwendet werden:

- Wenn jede Zelle verfügt über eine große Anzahl von Bindungen (20-30 +).
- Wenn der Zellvorlage häufig geändert wird.
- Wenn Tests zeigt, dass die `RecycleElement` Strategie für die Ergebnisse in einer reduzierten ausführungsgeschwindigkeit zwischenzuspeichern.

Es ist wichtig, die Folgen der erkennt die [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) cachingstrategie beim Arbeiten mit benutzerdefinierten Zellen. Eine beliebige Zelle Initialisierungscode muss für jede Zelle erstellen, ausführen, die möglicherweise mehrere Male pro Sekunde. Unter diesen Umständen Layout-Techniken, die auf einer Seite, was waren gerne mit mehrere geschachtelte [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) -Instanzen, Leistungsengpässe werden, wenn sie Setup sind und in Echtzeit zerstört wurden, als der Benutzer einen Bildlauf.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

Die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie gibt an, dass die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird versucht, die Geschwindigkeit des Arbeitsspeichers Platzbedarf und Ausführung zu minimieren, indem Sie die Listenzellen wiederverwenden. Dieser Modus bietet nicht immer eine Leistungssteigerung erzielt, und Tests sollten ausgeführt werden, um zu bestimmen, welche Verbesserungen. Allerdings ist in der Regel die bevorzugte Option und sollte in folgenden Situationen verwendet werden:

- Wenn jede Zelle ein kleines moderate Anzahl von Bindungen aufweist.
- Wenn jede Zelle [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) aller Zellendaten definiert.
- Wenn jede Zelle mit unveränderlichen Zellvorlage weitgehend ähnlich ist.

Während der Virtualisierung die Zelle wird dem Bindungskontext aktualisiert haben, und daher eine Anwendung in diesem Modus wird verwendet, es muss sicherstellen, dass Bindung Kontext Updates entsprechend behandelt werden. Alle Daten über die Zelle müssen aus dem Bindungskontext stammen oder Konsistenzfehler auftreten. Dies kann durch Verwenden der Datenbindung anzuzeigenden von Zellendaten erfolgen. Alternativ sollten Zellendaten festgelegt werden, der `OnBindingContextChanged` zu überschreiben, anstatt im Konstruktor der benutzerdefinierten Zelle, wie im folgenden Codebeispiel wird veranschaulicht:

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

Weitere Informationen finden Sie unter [Kontextänderungen binden](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Auf IOS- und Android müssen Zellen benutzerdefinierte Renderern verwendet werden sie sicherstellen, dass änderungsbenachrichtigung für die Eigenschaft ordnungsgemäß implementiert wird. Wenn Zellen wiederverwendet werden wird ihre Eigenschaftswerte ändern, wenn der Bindungskontext mit einer verfügbaren Zelle aktualisiert wird, mit `PropertyChanged` Ereignisse, die ausgelöst wird. Weitere Informationen finden Sie unter [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>Mit einem DataTemplateSelector RecycleElement

Wenn eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet eine [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) Auswählen einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Zwischenspeichern Strategie ist nicht im cache `DataTemplate`s. Stattdessen eine `DataTemplate` für jedes Datenelement in der Liste ausgewählt ist.

> [!NOTE]
> Die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie ist eine Voraussetzung in Xamarin.Forms-2.4 eingeführt, die bei einer [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) wird gebeten, wählen Sie eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), da jedes `DataTemplate` muss die gleiche zurückgeben [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) Typ. Angenommen, ein [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) mit einer `DataTemplateSelector` , können entweder die zurückgeben `MyDataTemplateA` (, in dem `MyDataTemplateA` gibt eine `ViewCell` vom Typ `MyViewCellA`), oder `MyDataTemplateB` (, in denen `MyDataTemplateB`gibt eine `ViewCell` des Typs `MyViewCellB`), wenn `MyDataTemplateA` wird zurückgegeben, sie muss zurückgeben `MyViewCellA` oder eine Ausnahme ausgelöst.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

Die [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) cachingstrategie basiert auf der [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie Leuchten Sie außerdem, dass bei einer [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet eine [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) Auswählen einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`s werden zwischengespeichert, nach dem Typ des Elements in der Liste. Aus diesem Grund `DataTemplate`s pro Elementtyp nicht einmal pro Instanz einmal ausgewählt werden.

> [!NOTE]
> Die [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) cachingstrategie hat eine erforderliche Komponente, die die `DataTemplate`s zurückgegebenes der [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) verwenden, müssen die [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) Konstruktor, akzeptiert eine `Type`.

### <a name="setting-the-caching-strategy"></a>Die Zwischenspeicherstrategie festlegen

Die [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) Enumerationswert ist angegeben, mit einer [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Konstruktorüberladung, wie im folgenden Codebeispiel gezeigt:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

Legen Sie in XAML die `CachingStrategy` -Attribut wie im folgenden Code gezeigt:

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

Dies hat dieselbe Wirkung wie das Zwischenspeichern Strategie Argument im Konstruktor in c# festlegen; Beachten Sie, dass es keine `CachingStrategy` Eigenschaft `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Festlegen der Zwischenspeicherstrategie in ein untergeordnetes ListView

Festlegen der `CachingStrategy` Attribut in XAML für ein untergeordnetes [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird nicht das gewünschte Verhalten erzeugen, da es ist keine `CachingStrategy` Eigenschaft `ListView`. Darüber hinaus, wenn [XAMLC](~/xamarin-forms/xaml/xamlc.md) ist aktiviert, wird die folgende Fehlermeldung erzeugt: **keine Eigenschaft, bindbare Eigenschaft oder das Ereignis für "CachingStrategy" gefunden.**

Die Lösung für dieses Problem besteht darin, geben Sie einen Konstruktor für den Unterklassen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , akzeptiert eine [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) Parameter und übergibt sie in der Basisklasse:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Die [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) Enumerationswert aus XAML angegeben werden kann, mithilfe der `x:Arguments` Syntax:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>Verbessern der Leistung von ListView

Es gibt viele Techniken zum Verbessern der Leistung einer `ListView`:

-  Binden der `ItemsSource` Eigenschaft, um eine `IList<T>` Auflistung anstelle von einer `IEnumerable<T>` Sammlung, da `IEnumerable<T>` Auflistungen unterstützen keine random-Access.
-  Verwenden Sie die integrierte Zellen (z. B. `TextCell`  /  `SwitchCell` ) anstelle von `ViewCell` können Sie bei jedem.
-  Verwenden Sie weniger Elemente. Betrachten Sie beispielsweise mit einer einzigen `FormattedString` Bezeichnung anstelle mehrerer Bezeichnungen.
-  Ersetzen Sie die `ListView` mit einer `TableView` nicht homogene Daten – d. h. Daten verschiedener Typen angezeigt.
-  Einschränken der Verwendung der [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) Methode. Wenn übermäßig beansprucht, wird er die Leistung beeinträchtigen.
-  Vermeiden Sie auf Android-Geräten die Einstellung einer `ListView`des Trennzeichen Sichtbarkeit Zeile oder die Farbe, nachdem es instanziiert wurde, da dies zu einer großen zu Leistungseinbußen führt.
-  Vermeiden Sie ändern die Zelle Layouts auf der Grundlage der [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Dies verursacht Kosten für große Layout und die Initialisierung.
-  Vermeiden Sie in der Schachtelungshierarchie Layout Hierarchien. Verwendung `AbsoluteLayout` oder `Grid` zur Reduzierung der Schachtelungsebene.
-  Vermeiden Sie bestimmte `LayoutOptions` außer `Fill` (Füllung ist kostspielige berechnet).
-  Ordnen Sie eine `ListView` innerhalb einer `ScrollView` folgende Gründe vorliegen:
    - Die `ListView` implementiert einen eigenen Durchführen eines Bildlaufs.
    - Die `ListView` empfangen als sie vom übergeordneten Element behandelt werden, werden keine Gesten `ScrollView`.
    - Die `ListView` kann mit den Elementen der Liste, möglicherweise der Funktionalität angeboten, die eine benutzerdefinierte Kopf- und Fußzeile, die einen Bildlauf durchführt stellen die `ScrollView` für verwendet wurde. Weitere Informationen finden Sie unter [Kopf- und Fußzeilen](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Erwägen Sie einen benutzerdefinierten Renderer, ggf. eine sehr spezifisch, komplexe entwerfen, die in den Zellen angezeigt.

`AbsoluteLayout` wurde das Potenzial Layouts ohne ein einzelnes Measure-Aufruf ausführen. Dies macht es für die Leistung sehr leistungsstark. Wenn `AbsoluteLayout` nicht verwendet, sollten Sie [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Wenn `RelativeLayout`, direkte Übergabe der Einschränkungen werden deutlich schneller als mit dem Ausdruck-API. Das ist daran, dass der Ausdruck API JIT, und für iOS die Struktur verfügt interpretiert werden soll, die langsamer. Der Ausdruck API eignet sich für Seitenlayouts, in denen nur erforderlich, es anfängliche Layout und Drehung, aber in `ListView`, bei der Ausführung ständig beim Durchführen eines Bildlaufs, beeinträchtigt die Leistung.

Erstellen eines benutzerdefinierten Renderers für eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) oder seine Zellen wird ein Ansatz zum Durchführen eines Bildlaufs Leistung Layout Berechnungen zu reduzieren. Weitere Informationen finden Sie unter [Anpassen einer ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) und [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Ansicht der Renderer (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Benutzerdefinierte Renderer ViewCell (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
