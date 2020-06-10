---
Title: "Erstellen eines benutzerdefinierten Layouts in Xamarin.Forms " Description: "in diesem Artikel wird erläutert, wie eine benutzerdefinierte Layoutklasse geschrieben wird. Außerdem wird eine Orientierungs abhängige wraplayout-Klasse veranschaulicht, die die untergeordneten Elemente horizontal auf der Seite anordnet und dann die Anzeige der nachfolgenden untergeordneten Elemente in zusätzliche Zeilen umschließt.
ms. Prod: xamarin ms. assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 03/29/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="create-a-custom-layout-in-xamarinforms"></a>Erstellen eines benutzerdefinierten Layouts inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)

_Xamarin. Forms definiert fünf layoutklassen – Stacklayout, AbsoluteLayout, relativelayout, Grid und flexlayout, und jede ordnet die untergeordneten Elemente auf andere Weise an. Manchmal ist es jedoch notwendig, den Seiten Inhalt mithilfe eines Layouts zu organisieren, das nicht von bereitgestellt wird Xamarin.Forms . In diesem Artikel wird erläutert, wie eine benutzerdefinierte Layoutklasse geschrieben wird, und es wird eine Orientierung-sensible wraplayout-Klasse veranschaulicht, die die untergeordneten Elemente horizontal auf der Seite anordnet und dann die Anzeige der nachfolgenden untergeordneten Elemente in zusätzliche Zeilen_

In Xamarin.Forms werden alle layoutklassen von der- [`Layout<T>`](xref:Xamarin.Forms.Layout`1) Klasse abgeleitet und beschränken den generischen Typ auf [`View`](xref:Xamarin.Forms.View) und die abgeleiteten Typen. Die `Layout<T>` -Klasse wird wiederum von der- [`Layout`](xref:Xamarin.Forms.Layout) Klasse abgeleitet, die den Mechanismus zur Positionierung und Größenanpassung von untergeordneten Elementen bereitstellt.

Jedes visuelle Element ist für die Bestimmung seiner eigenen bevorzugten Größe zuständig, die als die *angeforderte* Größe bezeichnet wird. [`Page`](xref:Xamarin.Forms.Page), [`Layout`](xref:Xamarin.Forms.Layout) und [`Layout<View>`](xref:Xamarin.Forms.Layout`1) abgeleitete Typen sind dafür verantwortlich, die Position und Größe ihrer untergeordneten Elemente relativ zu sich selbst zu bestimmen. Das Layout umfasst daher eine Beziehung zwischen über-und untergeordneten Elementen, bei der das übergeordnete Element bestimmt, welche Größe der untergeordneten Elemente sein sollte, aber versucht, die angeforderte Größe des untergeordneten Elements zu berücksichtigen.

Xamarin.FormsZum Erstellen eines benutzerdefinierten Layouts ist ein umfassendes Verständnis der Layout-und invalidierungszyklen erforderlich. Diese Zyklen werden jetzt erläutert.

## <a name="layout"></a>Layout

Layout beginnt am oberen Rand der visuellen Struktur mit einer Seite und durchläuft alle Verzweigungen der visuellen Struktur, um alle visuellen Elemente auf einer Seite zu umfassen. Elemente, die übergeordnete Elemente anderer Elemente sind, sind für die Größenanpassung und Positionierung ihrer untergeordneten Elemente in Bezug auf sich verantwortlich.

Die- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse definiert eine [ `Measure` ] (Xref:) Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . Measures)) Methode, mit der ein Element für Layoutvorgänge und ein [ `Layout` ] (Xref:) gemessen wird Xamarin.Forms . Visualelement. Layout ( Xamarin.Forms . Rechteck))-Methode, die den rechteckigen Bereich angibt, in dem das Element gerendert wird. Wenn eine Anwendung gestartet wird und die erste Seite angezeigt wird, wird ein *layoutcycle* , der zuerst aus `Measure` -Aufrufen besteht und dann `Layout` aufruft, auf dem- [`Page`](xref:Xamarin.Forms.Page) Objekt gestartet:

1. Während des layoutcycle ist jedes übergeordnete Element für das Aufrufen der- `Measure` Methode auf seinen untergeordneten Elementen verantwortlich.
1. Nachdem die untergeordneten Elemente gemessen wurden, ist jedes übergeordnete Element für das Aufrufen der-Methode für seine untergeordneten Elemente verantwortlich `Layout` .

Dadurch wird sichergestellt, dass alle visuellen Elemente auf der Seite Aufrufe an die `Measure` -Methode und die- `Layout` Methode empfangen. Der Prozess wird in der folgenden Abbildung dargestellt:

![](custom-images/layout-cycle.png "Xamarin.Forms Layout Cycle")

> [!NOTE]
> Beachten Sie, dass layoutzyklen auch für eine Teilmenge der visuellen Struktur auftreten können, wenn sich etwas ändert, das sich auf das Layout auswirkt. Dies schließt Elemente ein, die einer Auflistung hinzugefügt oder daraus entfernt werden, z. b. in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout) , eine Änderung der- [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) Eigenschaft eines-Elements oder eine Änderung in der Größe eines Elements.

Jede Xamarin.Forms Klasse, die eine- `Content` Eigenschaft oder eine-Eigenschaft aufweist, `Children` verfügt über eine über schreibbare [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Methode. Benutzerdefinierte layoutklassen, die von abgeleitet [`Layout<View>`](xref:Xamarin.Forms.Layout`1) werden, müssen diese Methode überschreiben und sicherstellen, dass [ `Measure` ] (Xref: Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . -Mess Flags)) und [ `Layout` ] (Xref: Xamarin.Forms . Visualelement. Layout ( Xamarin.Forms . Rechteck)) Methoden werden für alle untergeordneten Elemente aufgerufen, um das gewünschte benutzerdefinierte Layout bereitzustellen.

Außerdem muss jede Klasse, die von oder abgeleitet wird [`Layout`](xref:Xamarin.Forms.Layout) [`Layout<View>`](xref:Xamarin.Forms.Layout`1) , die- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methode überschreiben, wobei eine Layoutklasse die Größe bestimmt, die Sie benötigen, indem Sie Aufrufe an [ `Measure` ] (Xref:) sendet Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . Accessreflags)) Methoden der untergeordneten Elemente.

> [!NOTE]
> -Elemente bestimmen ihre Größe auf der Grundlage von *Einschränkungen*, die angeben, wie viel Speicherplatz für ein Element innerhalb des übergeordneten Elements des Elements verfügbar ist. Einschränkungen, die an [ `Measure` ] (Xref:) übermittelt werden Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . "Messreflags") und- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methoden können zwischen 0 und liegen `Double.PositiveInfinity` . Ein Element ist *eingeschränkt*oder *vollständig eingeschränkt*, wenn es einen aufzurufenden [ `Measure` ] (Xref:) empfängt Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . Messreflags)) Methode mit nicht unendlichen Argumenten: das Element ist auf eine bestimmte Größe beschränkt. Ein Element ist *nicht eingeschränkt*oder *teilweise eingeschränkt*, wenn es einen aufzurufenden Befehl `Measure` mit mindestens einem Argument erhält, das gleich ist `Double.PositiveInfinity` – die unendliche Einschränkung kann sich als Angabe der autoSizing vorstellen.

## <a name="invalidation"></a>Aufheben einer Validierung

Die Invalidierung ist der Prozess, durch den eine Änderung in einem Element auf einer Seite einen neuen layoutcycle auslöst. Elemente werden als ungültig betrachtet, wenn Sie nicht mehr über die richtige Größe oder Position verfügen. Wenn beispielsweise die- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) Eigenschaft einer [`Button`](xref:Xamarin.Forms.Button) geändert wird, wird die als `Button` ungültig bezeichnet, da Sie nicht mehr die richtige Größe hat. Wenn Sie die Größe des ändern, `Button` wirkt sich dies möglicherweise auf die Änderungen im Layout über den Rest einer Seite aus.

Elemente werden durch Aufrufen der- [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) Methode ungültig, wenn sich eine Eigenschaft des-Elements ändert, die möglicherweise zu einer neuen Größe des-Elements führt. Diese Methode löst das- [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) Ereignis aus, über das die übergeordneten Handles des Elements einen neuen layoutcycle auslösen.

Die [`Layout`](xref:Xamarin.Forms.Layout) -Klasse legt einen Handler für das-Ereignis für jedes untergeordnete Element fest, das der [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) Eigenschaft oder Auflistung hinzugefügt `Content` `Children` wurde, und trennt den Handler, wenn das untergeordnete Element entfernt wird. Daher wird jedes Element in der visuellen Struktur, das über untergeordnete Elemente verfügt, benachrichtigt, wenn eines der untergeordneten Elemente die Größe ändert. Im folgenden Diagramm wird veranschaulicht, wie eine Änderung in der Größe eines Elements in der visuellen Strukturänderungen verursachen kann, die die Struktur aufschlagen:

![](custom-images/invalidation.png "Invalidation in the Visual Tree")

Allerdings `Layout` versucht die-Klasse, die Auswirkungen einer Änderung in der Größe eines Kindes auf das Layout einer Seite einzuschränken. Wenn das Layout der Größe eingeschränkt ist, wirkt sich eine Änderung der untergeordneten Größe nicht auf eine höhere Größe als das übergeordnete Layout in der visuellen Struktur aus. In der Regel wirkt sich eine Änderung der Größe eines Layouts jedoch darauf aus, wie das Layout seine untergeordneten Elemente anordnet. Daher wird durch jede Änderung in der Größe eines Layouts ein layoutcycle für das Layout gestartet, und das Layout empfängt Aufrufe der [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) -und- [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Methoden.

Die- [`Layout`](xref:Xamarin.Forms.Layout) Klasse definiert auch eine- [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) Methode, die einen ähnlichen Zweck hat wie die- [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) Methode. Die- `InvalidateLayout` Methode sollte immer dann aufgerufen werden, wenn eine Änderung vorgenommen wird, die sich darauf auswirkt, wie das Layout seine untergeordneten Elemente positioniert. Beispielsweise ruft die- `Layout` Klasse die- `InvalidateLayout` Methode auf, wenn ein untergeordnetes Element einem Layout hinzugefügt oder daraus entfernt wird.

[`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)Kann überschrieben werden, um einen Cache zu implementieren, um wiederkehrende Aufrufe von [ `Measure` ] (Xref:) zu minimieren Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . Accessreflags)) Methoden der untergeordneten Elemente des Layouts. Durch das Überschreiben der- `InvalidateLayout` Methode wird eine Benachrichtigung darüber bereitgestellt, wann dem Layout untergeordnete Elemente hinzugefügt oder daraus entfernt werden. Auf ähnliche Weise [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) kann die-Methode überschrieben werden, um eine Benachrichtigung bereitzustellen, wenn sich eines der untergeordneten Layoutelemente ändert. Bei beiden Methoden Überschreibungen sollte ein benutzerdefiniertes Layout reagieren, indem der Cache gelöscht wird. Weitere Informationen finden Sie unter [berechnen und Zwischenspeichern von Layoutdaten](#calculate-and-cache-layout-data).

## <a name="create-a-custom-layout"></a>Erstellen eines benutzerdefinierten Layouts

Der Vorgang zum Erstellen eines benutzerdefinierten Layouts lautet wie folgt:

1. Erstellen Sie eine von der `Layout<View>`-Klasse abgeleitete Klasse. Weitere Informationen finden Sie unter [Erstellen eines wraplayout](#create-a-wraplayout).
1. [*optional*] Fügen Sie für alle Parameter, die für die Layoutklasse festgelegt werden sollen, Eigenschaften hinzu, die von bindbaren Eigenschaften unterstützt werden. Weitere Informationen finden Sie unter [Hinzufügen von Eigenschaften, die von bindbaren Eigenschaften unterstützt](#add-properties-backed-by-bindable-properties)werden.
1. Überschreiben [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Sie die-Methode, um [ `Measure` ] (Xref:) aufzurufen Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . "Messreflags")-Methode für alle untergeordneten Layoutelemente und gibt eine angeforderte Größe für das Layout zurück. Weitere Informationen finden Sie unter über [Schreiben der onmeasure-Methode](#override-the-onmeasure-method).
1. Überschreiben [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Sie die-Methode, um [ `Layout` ] (Xref:) aufzurufen Xamarin.Forms . Visualelement. Layout ( Xamarin.Forms . Rechteck))-Methode für alle untergeordneten Elemente des Layouts. Fehler beim Aufrufen von [ `Layout` ] (Xref: Xamarin.Forms . Visualelement. Layout ( Xamarin.Forms . Rechteck)) die Methode für jedes untergeordnete Element in einem Layout führt dazu, dass das untergeordnete Element niemals eine korrekte Größe oder Position erhält, sodass das untergeordnete Element auf der Seite nicht sichtbar wird. Weitere Informationen finden Sie unter über [Schreiben der layoutchildren-Methode](#override-the-layoutchildren-method).

    > [!NOTE]
    > Wenn Sie untergeordnete Elemente in der [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) -und der- [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Überschreibung aufzählen, überspringen Sie alle untergeordneten Elemente, deren- [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) Eigenschaft auf `false` Dadurch wird sichergestellt, dass das benutzerdefinierte Layout keinen Platz für unsichtbare untergeordnete Elemente hinterlässt.

1. [*optional*] Überschreiben [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) Sie die Methode, die benachrichtigt werden soll, wenn dem Layout untergeordnete Elemente hinzugefügt oder daraus entfernt werden. Weitere Informationen finden Sie unter [außer Kraft setzen der invalidatelayout-Methode](#override-the-invalidatelayout-method).
1. [*optional*] Überschreiben [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) Sie die Methode, die benachrichtigt werden soll, wenn eine der untergeordneten Layoutelemente die Größe ändert. Weitere Informationen finden Sie unter über [Schreiben der onchildmessreinvaliveralteten-Methode](#override-the-onchildmeasureinvalidated-method).

> [!NOTE]
> Beachten Sie, dass die [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) außer Kraft Setzung nicht aufgerufen wird, wenn die Größe des Layouts durch das übergeordnete Element und nicht durch seine untergeordneten Elemente geregelt wird. Die außer Kraft setzung wird jedoch aufgerufen, wenn eine oder beide der Einschränkungen unendlich sind oder wenn die Layoutklasse nicht standardmäßige- [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) oder- [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaftswerte aufweist. Aus diesem Grund kann die [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Überschreibung nicht auf untergeordnete Größen beruhen, die während des [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methoden Aufrufes abgerufen wurden. Stattdessen `LayoutChildren` muss das [ `Measure` ] (Xref:) aufrufen Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . Accessreflags))-Methode für die untergeordneten Elemente des Layouts vor dem Aufrufen von [ `Layout` ] (Xref: Xamarin.Forms . Visualelement. Layout ( Xamarin.Forms . Rechteck))-Methode. Alternativ kann die Größe der untergeordneten Elemente, die in der Überschreibung abgerufen `OnMeasure` werden, zwischengespeichert werden, um spätere Aufrufe in der Überschreibung zu vermeiden `Measure` `LayoutChildren` . die Layoutklasse muss jedoch wissen, wann die Größen wieder abgerufen werden müssen. Weitere Informationen finden Sie unter [berechnen und Zwischenspeichern von Layoutdaten](#calculate-and-cache-layout-data).

Die Layoutklasse kann dann verwendet werden, indem Sie zu einem hinzugefügt wird [`Page`](xref:Xamarin.Forms.Page) , und indem dem Layout untergeordnete Elemente hinzugefügt werden. Weitere Informationen finden Sie unter Verwenden von [wraplayout](#consume-the-wraplayout).

### <a name="create-a-wraplayout"></a>Erstellen eines wraplayout

Die Beispielanwendung veranschaulicht eine Orientierungs abhängige Klasse, die die untergeordneten Elemente `WrapLayout` horizontal auf der Seite anordnet und dann die Anzeige der nachfolgenden untergeordneten Elemente in zusätzliche Zeilen umschließt.

Die- `WrapLayout` Klasse ordnet die gleiche Menge an Speicherplatz für jedes untergeordnete Element, das als *Zellen Größe*bezeichnet wird, basierend auf der maximalen Größe der untergeordneten Elemente zu. Untergeordnete Elemente, die kleiner als die Zellen Größe sind, können auf der Grundlage ihrer [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaftswerte und innerhalb der Zelle positioniert werden [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) .

Die `WrapLayout` Klassendefinition wird im folgenden Codebeispiel gezeigt:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

#### <a name="calculate-and-cache-layout-data"></a>Berechnen und Zwischenspeichern von Layoutdaten

Die- `LayoutData` Struktur speichert Daten zu einer Auflistung von untergeordneten Elementen in einer Reihe von Eigenschaften:

- `VisibleChildCount`– die Anzahl der untergeordneten Elemente, die im Layout sichtbar sind.
- `CellSize`– die maximale Größe aller untergeordneten Elemente entsprechend der Größe des Layouts.
- `Rows`– die Anzahl der Zeilen.
- `Columns`– die Anzahl der Spalten.

Das- `layoutDataCache` Feld wird verwendet, um mehrere Werte zu speichern `LayoutData` . Wenn die Anwendung gestartet wird, werden zwei `LayoutData` Objekte im `layoutDataCache` Wörterbuch für die aktuelle Ausrichtung zwischengespeichert – eine für die Einschränkungs Argumente der Überschreibung `OnMeasure` und eine für das `width` -Argument und das- `height` Argument für die `LayoutChildren` außer Kraft Setzung. Wenn Sie das Gerät in Querformat drehen, `OnMeasure` werden die außer Kraft Setzung und die `LayoutChildren` außer Kraft Setzung erneut aufgerufen, was dazu führt, dass weitere zwei `LayoutData` Objekte im Wörterbuch zwischengespeichert werden. Bei der Rückgabe des Geräts an die Hochformat Ausrichtung sind jedoch keine weiteren Berechnungen erforderlich, da `layoutDataCache` bereits über die erforderlichen Daten verfügt.

Im folgenden Codebeispiel wird die- `GetLayoutData` Methode veranschaulicht, die die Eigenschaften der `LayoutData` strukturierten auf der Grundlage einer bestimmten Größe berechnet:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

Die- `GetLayoutData` Methode führt die folgenden Vorgänge aus:

- Es bestimmt, ob ein berechneter `LayoutData` Wert bereits im Cache vorhanden ist, und gibt ihn zurück, wenn er verfügbar ist.
- Andernfalls werden alle untergeordneten Elemente aufgelistet, `Measure` wobei die-Methode für jedes untergeordnete Element mit einer unendlichen Breite und Höhe aufgerufen und die maximale untergeordnete Größe bestimmt wird.
- Wenn mindestens ein untergeordnetes Element vorhanden ist, wird die Anzahl der erforderlichen Zeilen und Spalten berechnet und dann basierend auf den Dimensionen von eine Zellgröße für die untergeordneten Elemente berechnet `WrapLayout` . Beachten Sie, dass die Zellen Größe in der Regel etwas breiter ist als die maximale untergeordnete Größe, aber Sie kann auch kleiner sein, wenn der für das größte untergeordnete Element `WrapLayout` nicht breit genug ist oder für das höchste untergeordnete Element hoch genug ist.
- Der neue Wert wird `LayoutData` im Cache gespeichert.

#### <a name="add-properties-backed-by-bindable-properties"></a>Hinzufügen von Eigenschaften, die von bindbaren Eigenschaften gestützt werden

Die `WrapLayout` -Klasse definiert die `ColumnSpacing` -Eigenschaft und die-Eigenschaft `RowSpacing` , deren Werte zum Trennen der Zeilen und Spalten im Layout verwendet werden und die durch bindbare Eigenschaften gestützt werden. Die bindbaren Eigenschaften werden im folgenden Codebeispiel gezeigt:

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Der von der Eigenschaft geänderte Handler jeder bindbaren Eigenschaft ruft die `InvalidateLayout` Methoden Überschreibung auf, um eine neue layoutdurchführung auf dem zu initiieren `WrapLayout` . Weitere Informationen finden Sie unter [außer Kraft setzen der invalidatelayout-Methode](#override-the-invalidatelayout-method) und über [Schreiben der onchildmessreinvaliveralteten-Methode](#override-the-onchildmeasureinvalidated-method).

#### <a name="override-the-onmeasure-method"></a>Überschreiben der onmeasure-Methode

Die `OnMeasure` außer Kraft setzung wird im folgenden Codebeispiel gezeigt:

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

Die-Überschreibung ruft die- `GetLayoutData` Methode auf und erstellt ein- `SizeRequest` Objekt aus den zurückgegebenen Daten, während die `RowSpacing` -und- `ColumnSpacing` Eigenschaftswerte berücksichtigt werden. Weitere Informationen zur `GetLayoutData` -Methode finden Sie unter [berechnen und Zwischenspeichern von Layoutdaten](#calculate-and-cache-layout-data).

> [!IMPORTANT]
> [ `Measure` ] (Xref: Xamarin.Forms . Visualelement. Measure (System. Double, System. Double, Xamarin.Forms . "Messreflags") und [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methoden sollten nie eine unendliche Dimension anfordern, indem [`SizeRequest`](xref:Xamarin.Forms.SizeRequest) Sie einen Wert zurückgeben, dessen-Eigenschaft auf festgelegt ist `Double.PositiveInfinity` . Allerdings kann mindestens eines der Einschränkungs Argumente `OnMeasure` sein `Double.PositiveInfinity` .

#### <a name="override-the-layoutchildren-method"></a>Überschreiben der layoutchildren-Methode

Die `LayoutChildren` außer Kraft setzung wird im folgenden Codebeispiel gezeigt:

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Die außer Kraft Setzung beginnt mit einem aufzurufenden- `GetLayoutData` Methode und listet dann alle untergeordneten Elemente auf, um die Größe und Position innerhalb der Zelle der einzelnen untergeordneten Elemente zu platzieren. Dies wird durch Aufrufen von [ `LayoutChildIntoBoundingRegion` ] (Xref:) erreicht Xamarin.Forms . Layout. layoutchildinteboundingregion ( Xamarin.Forms . Visualelement, Xamarin.Forms . Rechteck))-Methode, die verwendet wird, um ein untergeordnetes Element innerhalb eines Rechtecks auf der Grundlage seiner [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaftswerte zu positionieren. Dies entspricht dem Abrufen des untergeordneten Elements [ `Layout` ] (Xref: Xamarin.Forms . Visualelement. Layout ( Xamarin.Forms . Rechteck))-Methode.

> [!NOTE]
> Beachten Sie, dass das an die-Methode über gegebene Rechteck `LayoutChildIntoBoundingRegion` den gesamten Bereich enthält, in dem sich das untergeordnete Element befinden kann.

Weitere Informationen zur `GetLayoutData` -Methode finden Sie unter [berechnen und Zwischenspeichern von Layoutdaten](#calculate-and-cache-layout-data).

#### <a name="override-the-invalidatelayout-method"></a>Überschreiben der invalidatelayout-Methode

Die [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) außer Kraft setzung wird aufgerufen, wenn untergeordnete Elemente dem Layout hinzugefügt oder daraus entfernt werden oder wenn eine der `WrapLayout` Eigenschaften den Wert ändert, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Die außer Kraft Setzung macht das Layout ungültig und verwirft alle zwischengespeicherten Layoutinformationen.

> [!NOTE]
> Um zu verhindern, dass die-Klasse die- [`Layout`](xref:Xamarin.Forms.Layout) [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) Methode aufruft, wenn ein untergeordnetes Element hinzugefügt oder aus einem Layout entfernt wird, überschreiben Sie [ `ShouldInvalidateOnChildAdded` ] (Xref: Xamarin.Forms . Layout. schulendvalidateonchildadded ( Xamarin.Forms . View)) und [ `ShouldInvalidateOnChildRemoved` ] (Xref: Xamarin.Forms . Layout. Schulter-validateonchildreverschohe ( Xamarin.Forms . View))-Methoden und geben zurück `false` . Die Layoutklasse kann dann einen benutzerdefinierten Prozess implementieren, wenn untergeordnete Elemente hinzugefügt oder entfernt werden.

#### <a name="override-the-onchildmeasureinvalidated-method"></a>Überschreiben der onchildmessreinvaliveralteten-Methode

Die [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) außer Kraft setzung wird aufgerufen, wenn eine der untergeordneten Layoutelemente die Größe ändert, und wird im folgenden Codebeispiel gezeigt:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Die Überschreibung macht das untergeordnete Layout ungültig und verwirft alle zwischengespeicherten Layoutinformationen.

### <a name="consume-the-wraplayout"></a>Verwenden des wraplayout

Die `WrapLayout` Klasse kann verwendet werden, indem Sie Sie [`Page`](xref:Xamarin.Forms.Page) in einem abgeleiteten Typ platzieren, wie im folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Der entsprechende c#-Code wird unten dargestellt:

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Untergeordnete Elemente können der nach Bedarf hinzugefügt werden `WrapLayout` . Das folgende Codebeispiel zeigt [`Image`](xref:Xamarin.Forms.Image) Elemente, die dem hinzugefügt werden `WrapLayout` :

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    var images = await GetImageListAsync();
    if (images != null)
    {
        foreach (var photo in images.Photos)
        {
            var image = new Image
            {
                Source = ImageSource.FromUri(new Uri(photo))
            };
            wrapLayout.Children.Add(image);
        }
    }
}

async Task<ImageList> GetImageListAsync()
{
    try
    {
        string requestUri = "https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json";
        string result = await _client.GetStringAsync(requestUri);
        return JsonConvert.DeserializeObject<ImageList>(result);
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"\tERROR: {ex.Message}");
    }

    return null;
}
```

Wenn die Seite mit dem `WrapLayout` angezeigt wird, greift die Beispielanwendung asynchron auf eine Remote-JSON-Datei mit einer Liste von Fotos zu, erstellt ein [`Image`](xref:Xamarin.Forms.Image) -Element für jedes Foto und fügt Sie hinzu `WrapLayout` . Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

![](custom-images/portait-screenshots.png "Sample Application Portrait Screenshots")

Die folgenden Screenshots zeigen die `WrapLayout` , nachdem Sie in Querformat gedreht wurde:

![](custom-images/landscape-ios.png "Sample iOS Application Landscape Screenshot")
![](custom-images/landscape-android.png "Sample Android Application Landscape Screenshot")
![](custom-images/landscape-uwp.png "Sample UWP Application Landscape Screenshot")

Die Anzahl der Spalten in den einzelnen Zeilen hängt von der Fotogröße, der Bildschirmbreite und der Anzahl der Pixel pro geräteunabhängigen Einheit ab. Die [`Image`](xref:Xamarin.Forms.Image) Elemente werden von den-Elementen asynchron geladen, und daher empfängt die- `WrapLayout` Klasse Häufige Aufrufe an die- [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Methode, da jedes `Image` Element auf der Grundlage des geladenen Fotos eine neue Größe erhält.

## <a name="related-links"></a>Verwandte Links

- [Wraplayout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)
- [Benutzerdefinierte Layouts](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Erstellen von benutzerdefinierten Layouts in Xamarin.Forms (Video)](https://www.youtube.com/watch?v=sxjOqNZFhKU)
- [Layout\<T>](xref:Xamarin.Forms.Layout`1)
- [Layout](xref:Xamarin.Forms.Layout)
- [Visualelement](xref:Xamarin.Forms.VisualElement)
