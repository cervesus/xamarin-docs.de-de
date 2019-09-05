---
title: Erstellen eines benutzerdefinierten Layouts in xamarin. Forms
description: In diesem Artikel wird erläutert, wie eine benutzerdefinierte Layout-Klasse schreiben, und veranschaulicht eine Ausrichtung keine Unterscheidung nach Kanatyp WrapLayout-Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente auf zusätzliche Zeilen.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 0828d780ed075a6e3b18ba5020f5908fb8c06189
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292576"
---
# <a name="create-a-custom-layout-in-xamarinforms"></a>Erstellen eines benutzerdefinierten Layouts in xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)

_Xamarin.Forms definiert vier Klassen für Layout – StackLayout, die von "AbsoluteLayout", RelativeLayout und Raster, und jedes seiner untergeordneten Elemente auf andere Weise angeordnet. Allerdings ist es manchmal notwendig, mit einem Layout, die nicht von Xamarin.Forms bereitgestelltes Seiteninhalt zu organisieren. In diesem Artikel wird erläutert, wie eine benutzerdefinierte Layout-Klasse schreiben, und veranschaulicht eine Ausrichtung keine Unterscheidung nach Kanatyp WrapLayout-Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente auf zusätzliche Zeilen._

In Xamarin.Forms alle Layout Klassen leiten sich von der [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) -Klasse und beschränken Sie den generischen Typ um [ `View` ](xref:Xamarin.Forms.View) und seine abgeleiteten Typen. Im Gegenzug die `Layout<T>` Klasse leitet sich von der [ `Layout` ](xref:Xamarin.Forms.Layout) -Klasse, die den Mechanismus für die Positionierung und größenanpassung untergeordnete Elemente bereitstellt.

Jedes visuelle Element ist zuständig für das eigene bevorzugte Größe, die so genannte ermitteln die *angefordert* Größe. [`Page`](xref:Xamarin.Forms.Page), [ `Layout` ](xref:Xamarin.Forms.Layout), und [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) abgeleitete Typen für die Bestimmung der Position und Größe eines ihrer untergeordneten oder untergeordneten Elemente relativ zu sich selbst verantwortlich sind. Aus diesem Grund umfasst das Layout eine über-/ unterordnungsbeziehung, die, in dem das übergeordnete Element bestimmt, was die Größe der untergeordneten Elemente werden soll, und es wird versucht, um die angeforderte Größe des untergeordneten Elements zu berücksichtigen.

Umfassende Informationen zu der Xamarin.Forms-Layout und die invalidierung Zyklen ist erforderlich, um ein benutzerdefiniertes Layout zu erstellen. Diesen Zyklen werden jetzt erläutert.

## <a name="layout"></a>Layout

Layout beginnt am oberen Rand der visuellen Struktur mit einer Seite, und es durchläuft alle Verzweigungen der visuellen Struktur so, dass jedes visuelle Element auf einer Seite eingeschlossen. Elemente, die übergeordneten Elemente für andere Elemente sind für die größenanpassung und Positionierung von deren untergeordnete Elemente relativ zu sich selbst verantwortlich.

Die [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) -Klasse definiert eine [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Methode, die ein Element für Layoutvorgänge, misst und [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) -Methode, Der rechteckige Bereich, die, dem in das Element gerendert wird. Beim Start einer Anwendung und die erste Seite angezeigt wird, eine *Layoutzyklus* besteht der erste von `Measure` aufrufen, und klicken Sie dann `Layout` aufruft, beginnt auf der [ `Page` ](xref:Xamarin.Forms.Page) Objekt:

1. Während des Layoutzyklus jedes übergeordnete Element ist verantwortlich für das Aufrufen der `Measure` Methode für die untergeordneten Elemente.
1. Jedes übergeordnete Element ist verantwortlich für das aufrufen, nachdem die untergeordneten Elemente gemessen wurden, die `Layout` Methode für die untergeordneten Elemente.

Dieser Zyklus wird sichergestellt, dass jedes visuelle Element auf der Seite aufrufen empfängt die `Measure` und `Layout` Methoden. Der Prozess wird im folgenden Diagramm dargestellt:

![](custom-images/layout-cycle.png "Xamarin.Forms-Layoutzyklus")

> [!NOTE]
> Beachten Sie, dass die Layout-Zyklen können auch für eine Teilmenge der visuellen Struktur auftreten, wenn etwas ändert, um das Layout beeinflussen. Dazu gehören Elemente, die hinzugefügt oder entfernt aus einer Auflistung, z. B. in einem [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), eine Änderung in der [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) Eigenschaft eines Elements oder einer Änderung der Größe eines Elements.

Alle Xamarin.Forms-Klasse, die eine `Content` oder `Children` Eigenschaft verfügt über eine überschreibbare [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Methode. Benutzerdefiniertes Layout von abgeleiteten Klassen [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) müssen diese Methode überschreiben, und stellen sicher, dass die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) und [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methoden sind wird aufgerufen, für alle seiner untergeordneten Elemente, um die gewünschte benutzerdefinierte Anordnung bereitzustellen.

Darüber hinaus jede Klasse, die abgeleitet [ `Layout` ](xref:Xamarin.Forms.Layout) oder [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) überschreiben, muss die [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) -Methode, die ist eine Layoutklasse Bestimmt die Größe, die benötigt werden, indem Sie Aufrufe an die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Methoden der untergeordneten Elemente.

> [!NOTE]
> Elemente zu ermitteln, deren Größe basierend auf *Einschränkungen*, die angeben, wie viel Speicherplatz für ein Element innerhalb des übergeordneten Elements verfügbar ist. Einschränkungen, die an die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) und [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methoden reichen von 0 bis `Double.PositiveInfinity`. Ein Element ist *eingeschränkt*, oder *vollständig eingeschränkt*, wenn sie einen Aufruf empfängt die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Methode endliche Argumente - Elements ist eingeschränkt Um eine bestimmte Größe. Ein Element ist *uneingeschränkte*, oder *teilweise eingeschränkt*, wenn sie einen Aufruf empfängt die `Measure` -Methode mit mindestens einem Argument gleich `Double.PositiveInfinity` – die unbegrenzte Einschränkung möglich als betrachtet werden, der angibt, automatisches Anpassen der Größe.

## <a name="invalidation"></a>Aufheben einer Validierung

Aufhebung der Gültigkeit wird mit dem eine Änderung in einem Element auf einer Seite ein neues Layoutzyklus auslöst. Elemente werden als ungültig betrachtet, wenn sie nicht mehr die richtige Größe oder Position verfügen. Z. B. wenn die [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize) Eigenschaft eine [ `Button` ](xref:Xamarin.Forms.Button) Änderungen, die `Button` gilt als ungültig, da es nicht mehr über die richtige Größe hat. Ändern der Größe der `Button` möglicherweise einen Welleneffekt Änderungen im Layout der restlichen Seite.

Elemente für ungültig erklären sich durch den Aufruf der [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) -Methode, wenn in der Regel eine Eigenschaft des Elements ändert, die möglicherweise einer neuen Größe des Elements. Diese Methode löst die [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) -Ereignis, das des übergeordneten Elements behandelt wird, um eine neue Layoutzyklus auszulösen.

Die [ `Layout` ](xref:Xamarin.Forms.Layout) -Klasse legt einen Handler für die [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) Ereignis für jede untergeordnete hinzugefügt seine `Content` Eigenschaft oder `Children` -Auflistung, und trennt den Ereignishandler bei der untergeordnetes Element wird entfernt. Aus diesem Grund wird jedes Element in der visuellen Struktur, die über untergeordnete Elemente verfügt gewarnt, wenn eines der untergeordneten Größe ändert. Das folgende Diagramm veranschaulicht, wie eine Änderung der Größe eines Elements in der visuellen Struktur Änderungen verursachen kann, die der Struktur aufwärts ripple:

![](custom-images/invalidation.png "Invalidierung in der visuellen Struktur")

Allerdings die `Layout` Klasse versucht wird, um die Auswirkungen einer Änderung in ein untergeordnetes Element des-Größe, auf das Layout einer Seite zu beschränken. Wenn das Layout eingeschränkt ist, klicken Sie dann wirkt eine Änderung der Dateigröße untergeordneten etwas höher als das übergeordnete Layout in der visuellen Struktur sich nicht. Allerdings wirkt sich auf in der Regel eine Änderung der Größe eines Layouts wie das Layout seiner untergeordneten Elemente anordnet. Aus diesem Grund jede Änderung in einem Layout der Größe startet einen Layoutzyklus für das Layout, und das Layout erhalten Aufrufe an die [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) und [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Methoden.

Die [ `Layout` ](xref:Xamarin.Forms.Layout) -Klasse definiert außerdem eine [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Methode mit einem ähnlichen Zweck der [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) Methode. Die `InvalidateLayout` Methode aufgerufen werden soll, wenn eine Änderung, die betroffen sind vorgenommen wird, wie das Layout passt Position und Größe von untergeordneten Elemente. Z. B. die `Layout` Klasse ruft die `InvalidateLayout` -Methode auf, wenn ein untergeordnetes Element hinzugefügt oder aus einem Layout entfernt.

Die [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) überschrieben werden, um einen Zwischenspeicher, um wiederholte Aufrufe von minimieren implementieren die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Methoden, der das Layout der untergeordneten Elemente. Überschreiben der `InvalidateLayout` Methode erhalten eine Benachrichtigung, wenn untergeordnete Elemente hinzugefügt oder aus dem Layout entfernt werden. Auf ähnliche Weise die [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) Methode kann überschrieben werden, um eine Benachrichtigung bereitzustellen, wenn das Layout der untergeordneten Elemente eines Größe ändert. Für beide methodenüberschreibungen sollten ein benutzerdefiniertes Layout reagieren, indem Sie zum Löschen des Zwischenspeichers. Weitere Informationen finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

## <a name="create-a-custom-layout"></a>Erstellen eines benutzerdefinierten Layouts

Der Prozess zum Erstellen eines benutzerdefinierten Layouts lautet wie folgt aus:

1. Erstellen Sie eine von der `Layout<View>`-Klasse abgeleitete Klasse. Weitere Informationen finden Sie unter [erstellen eine WrapLayout](#creating).
1. [*optional*] Hinzufügen von Eigenschaften, gesichert durch bindbare Eigenschaften für alle Parameter, die auf der Layoutklasse festgelegt werden soll. Weitere Informationen finden Sie unter [Hinzufügen von Eigenschaften, die durch die bindbare Eigenschaften unterstützt](#adding_properties).
1. Überschreiben der [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) aufzurufende Methode der [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Methode auf alle des Layouts der untergeordneten Elemente und Rückgabe einer angeforderten Größe für das Layout. Weitere Informationen finden Sie unter [Überschreiben der Methode OnMeasure](#onmeasure).
1. Überschreiben der [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) aufzurufende Methode der [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methode für alle das Layout der untergeordneten Elemente. Fehler beim Aufrufen der [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methode für jedes untergeordnete Element in einem Layout führt dazu, in der untergeordneten nie empfangen eine richtige Größe oder Position, und daher das untergeordnete Element wird nicht auf der Seite sichtbar. Weitere Informationen finden Sie unter [Überschreiben der Methode LayoutChildren](#layoutchildren).

    > [!NOTE]
    > Beim Auflisten der untergeordneten Elemente in der [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) und [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Außerkraftsetzungen, überspringen Sie alle untergeordneten, deren [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) -Eigenschaftensatz auf `false`. Dadurch wird sichergestellt, dass das benutzerdefinierte Layout für nicht sichtbare untergeordnete Elemente Platz wird nicht.

1. [*optional*] außer Kraft setzen der [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Methode, um benachrichtigt werden, wenn ein untergeordnetes Element hinzugefügt oder aus dem Layout entfernt werden. Weitere Informationen finden Sie unter [Überschreiben der Methode InvalidateLayout](#invalidatelayout).
1. [*optional*] außer Kraft setzen der [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) -Methode benachrichtigt werden, wenn das Layout der untergeordneten Elemente eines Größe ändert. Weitere Informationen finden Sie unter [Überschreiben der Methode OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Beachten Sie, dass die [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Außerkraftsetzung wird nicht aufgerufen werden, wenn die Größe des Layouts mit seinem übergeordneten Element, statt ihren untergeordneten Elementen gesteuert wird. Allerdings die Außerkraftsetzung wird werden aufgerufen, wenn eine oder beide der Einschränkungen unendlich sind oder wenn die Layoutklasse nicht standardmäßige hat [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) oder [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaftswerte. Aus diesem Grund die [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) außer Kraft setzen kann nicht auf untergeordnete Größen erhalten während der basieren die [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methodenaufruf. Stattdessen `LayoutChildren` müssen aufrufen, die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Methode für das Layout der untergeordneten Elemente vor dem Aufrufen der [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methode. Sie können auch die Größe der untergeordneten Elemente abgerufen, in der `OnMeasure` außer Kraft setzen kann zwischengespeichert werden, um später zu vermeiden `Measure` Aufrufe in die `LayoutChildren` außer Kraft setzen, aber die Layoutklasse müssen bekannt sein, wenn die Größen erneut abgerufen werden müssen. Weitere Informationen finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

Die Layoutklasse kann dann verwendet werden, fügen es zu einem [ `Page` ](xref:Xamarin.Forms.Page), und durch das Hinzufügen von untergeordneten Elemente des Layouts. Weitere Informationen finden Sie unter [nutzen die WrapLayout](#consuming).

<a name="creating" />

### <a name="create-a-wraplayout"></a>Erstellen eines wraplayout

Die beispielanwendung zeigt eine Ausrichtung keine Unterscheidung nach Kanatyp `WrapLayout` -Klasse, ordnet die untergeordneten Elemente horizontal über die Seite und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente auf zusätzliche Zeilen.

Die `WrapLayout` Klasse weist die gleiche Menge an Speicherplatz für jedes untergeordnete Element, bekannt als die *Zelle Größe*basierend auf die maximale Größe der untergeordneten Elemente. Untergeordnete Elemente, die kleiner ist als die Zellengröße in der Zelle positioniert werden kann, basierend auf ihren [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaftswerte.

Die `WrapLayout` Klassendefinition wird im folgenden Codebeispiel gezeigt:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculate-and-cache-layout-data"></a>Berechnen und Zwischenspeichern von Layoutdaten

Die `LayoutData` -Struktur speichert die Daten über eine Auflistung von untergeordneten Elementen in eine Reihe von Eigenschaften:

- `VisibleChildCount` – die Anzahl der Kinder, die im Layout angezeigt werden.
- `CellSize` – die maximale Größe aller untergeordneten Elemente, auf die Größe des Layouts angepasst.
- `Rows` – die Anzahl der Zeilen.
- `Columns` – die Anzahl der Spalten.

Die `layoutDataCache` Feld dient zum Speichern von mehreren `LayoutData` Werte. Beim Starten der Anwendung, zwei `LayoutData` Objekte zwischengespeichert werden soll, in der `layoutDataCache` Wörterbuch für die aktuelle Ausrichtung – eine für die Einschränkung Argumente für die `OnMeasure` außer Kraft setzen und eine für die `width` und `height` Argumente um die `LayoutChildren` außer Kraft setzen. Beim Drehen des Geräts in Querformat, die `OnMeasure` außer Kraft setzen und die `LayoutChildren` Außerkraftsetzung wird erneut aufgerufen, die führt zu einem anderen zwei `LayoutData` Objekte, die in das Wörterbuch zwischengespeichert wird. Allerdings bei der Rückgabe des Geräts auf Hochformat keine weitere Berechnungen sind erforderlich, da die `layoutDataCache` hat bereits die erforderlichen Daten.

Das folgende Codebeispiel zeigt die `GetLayoutData` -Methode, die die Eigenschaften des berechnet die `LayoutData` strukturierte basierend auf einer bestimmten Größe:

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

Die `GetLayoutData` Methode führt die folgenden Vorgänge:

- Es bestimmt, ob eine berechnete `LayoutData` Wert befindet sich bereits im Cache und wird zurückgegeben, sofern diese verfügbar ist.
- Andernfalls, listet es über alle untergeordneten Elemente, Aufrufen der `Measure` Methode für jedes untergeordnete Element mit einem unendliche Breite und Höhe und bestimmt die maximale untergeordneten Größe.
- Vorausgesetzt, dass mindestens eine der sichtbaren untergeordneten Elements wird es berechnet die Anzahl der Zeilen und Spalten, die erforderlich sind und berechnet dann die Zellengröße einer für die untergeordneten Elemente basierend auf die Abmessungen des der `WrapLayout`. Beachten Sie, dass die Zelle in der Regel etwas breiter als die maximale untergeordneten Größe beträgt, aber es könnte auch kleinere sein wenn die `WrapLayout` ist nicht breit genug ist, für die breitesten untergeordneten oder hoch genug für die höchsten untergeordneten.
- Sie speichert das neue `LayoutData` Wert im Cache.

<a name="adding_properties" />

#### <a name="add-properties-backed-by-bindable-properties"></a>Hinzufügen von Eigenschaften, die von bindbaren Eigenschaften gestützt werden

Die `WrapLayout` -Klasse definiert `ColumnSpacing` und `RowSpacing` Eigenschaften, deren Werte werden verwendet, um die Zeilen und Spalten im Layout zu trennen, und die werden durch bindbare Eigenschaften unterstützt. Die bindbare Eigenschaften werden im folgenden Codebeispiel dargestellt:

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

Ruft der Handler durch geänderte Eigenschaften ausgelöste jeder bindbare Eigenschaft der `InvalidateLayout` Methode außer Kraft setzen, um ein neues Layout auszulösen übergeben wird, auf die `WrapLayout`. Weitere Informationen finden Sie unter [Überschreiben der Methode InvalidateLayout](#invalidatelayout) und [Überschreiben der Methode OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="override-the-onmeasure-method"></a>Überschreiben der onmeasure-Methode

Die `OnMeasure` außer Kraft setzen, wird im folgenden Codebeispiel wird dargestellt:

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

Ruft die Überschreibung der `GetLayoutData` -Methode und Konstrukte ein `SizeRequest` Objekt aus den zurückgegebenen Daten, während ebenso berücksichtigt die `RowSpacing` und `ColumnSpacing` Eigenschaftswerte. Weitere Informationen zu den `GetLayoutData` -Methode finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

> [!IMPORTANT]
> Die [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) und [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Methoden sollten nie eine unendliche Dimension anfordern, indem die Rückgabe einer [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest) Wert einer Eigenschaft auf festgelegt `Double.PositiveInfinity`. Allerdings mindestens eine Einschränkung Argumente `OnMeasure` kann `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="override-the-layoutchildren-method"></a>Überschreiben der layoutchildren-Methode

Die `LayoutChildren` außer Kraft setzen, wird im folgenden Codebeispiel wird dargestellt:

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

Die Außerkraftsetzung beginnt mit einem Aufruf der `GetLayoutData` -Methode, und listet dann alle untergeordneten Elemente, die Größe, und positionieren Sie sie in jedes untergeordnete Element der Zelle. Dies erfolgt durch Aufrufen der [ `LayoutChildIntoBoundingRegion` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) -Methode, die verwendet wird, positionieren Sie ein untergeordnetes Element innerhalb eines Rechtecks, das basierend auf dessen [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)Eigenschaftswerte. Dies entspricht dem Aufruf des untergeordnetes Standortes [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methode.

> [!NOTE]
> Beachten Sie, die das Rechteck an der `LayoutChildIntoBoundingRegion` Methode enthält den gesamten Bereich, in dem sich das untergeordnete Element befinden kann.

Weitere Informationen zu den `GetLayoutData` -Methode finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

<a name="invalidatelayout" />

#### <a name="overridethe-invalidatelayout-method"></a>Overridethe invalidatelayout-Methode

Die [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Außerkraftsetzung wird immer dann aufgerufen, wenn untergeordnete Elemente hinzugefügt oder entfernt aus dem Layout oder wenn eine von der `WrapLayout` eigenschaftenänderungen-Wert, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Die Außerkraftsetzung erklärt das Layout und verwirft alle zwischengespeicherten Layoutinformationen.

> [!NOTE]
> Zum Beenden der [ `Layout` ](xref:Xamarin.Forms.Layout) Klasse Aufrufen der [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) -Methode auf, wenn ein untergeordnetes Element hinzugefügt oder aus einem Layout entfernt, außer Kraft setzen der [ `ShouldInvalidateOnChildAdded` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded(Xamarin.Forms.View)) und [ `ShouldInvalidateOnChildRemoved` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved(Xamarin.Forms.View)) Methoden und Rückgabe `false`. Die Layoutklasse kann dann einen benutzerdefinierten Prozess implementieren, wenn untergeordnete Elemente hinzugefügt oder entfernt werden.

<a name="onchildmeasureinvalidated" />

#### <a name="override-the-onchildmeasureinvalidated-method"></a>Überschreiben der onchildmessreinvaliveralteten-Methode

Die [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) außer Kraft setzen wird immer dann aufgerufen, wenn eine der das Layout der untergeordneten Elemente die Größe ändert und wird im folgenden Codebeispiel wird dargestellt:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Die Außerkraftsetzung führt das Layout der untergeordneten, und verwirft alle zwischengespeicherten Layoutinformationen.

<a name="consuming" />

### <a name="consume-the-wraplayout"></a>Verwenden des wraplayout

Die `WrapLayout` Klasse kann genutzt werden, indem Sie diesen auf eine [ `Page` ](xref:Xamarin.Forms.Page) abgeleiteten Typ, aus, wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Der entsprechende C#-Code wird unten gezeigt:

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

Untergeordnete Elemente können anschließend hinzugefügt werden, um die `WrapLayout` nach Bedarf. Das folgende Codebeispiel zeigt [ `Image` ](xref:Xamarin.Forms.Image) hinzugefügte Elemente der `WrapLayout`:

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

Wenn die Seite mit den `WrapLayout` angezeigt wird, die beispielanwendung asynchron greift auf eine entfernte JSON-Datei mit einer Liste von Fotos, erstellt eine [ `Image` ](xref:Xamarin.Forms.Image) -Element für jedes Foto, und fügt es der hinzu`WrapLayout`. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

![](custom-images/portait-screenshots.png "Beispiel-Anwendung im Hochformat Screenshots")

Die folgenden Screenshots zeigen die `WrapLayout` nachdem es Querformat gedreht wird:

![](custom-images/landscape-ios.png "Beispiel für iOS-Anwendung im Querformat Screenshot")
![](custom-images/landscape-android.png "Android-Anwendung im Querformat Beispielscreenshot")
![](custom-images/landscape-uwp.png " UWP-Anwendung im Querformat-Beispielscreenshot:")

Die Anzahl der Spalten in jeder Zeile hängt davon ab, die Größe des Fotos, die Bildschirmbreite und die Anzahl der Pixel pro geräteunabhängige Einheit. Die [ `Image` ](xref:Xamarin.Forms.Image) Elemente asynchron geladen, die Fotos, und daher die `WrapLayout` Klasse erhalten häufig Aufrufe an die [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) -Methode, wie jede `Image` Element erhält eine neue Größe basierend auf dem Foto geladen.

## <a name="related-links"></a>Verwandte Links

- [WrapLayout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)
- [Benutzerdefinierte Layouts](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Erstellen von benutzerdefinierten Layouts in Xamarin.Forms (video)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Layout\<T >](xref:Xamarin.Forms.Layout`1)
- [Layout](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
