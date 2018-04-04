---
title: Erstellen eines benutzerdefinierten Layouts
description: Xamarin.Forms definiert vier Klassen für Layout – StackLayout, AbsoluteLayout RelativeLayout und Raster, und jede ordnet die untergeordneten Elemente auf andere Weise. In einigen Fällen ist es jedoch erforderlich, zum Organisieren von Seiteninhalt mithilfe eines Layouts dargestellt, die nicht von Xamarin.Forms bereitgestellt. In diesem Artikel wird erläutert, wie eine benutzerdefiniertes Layout-Klasse schreiben und eine Ausrichtung Akzent WrapLayout-Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente, um zusätzliche Zeilen veranschaulicht.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: f0728ac110fcf86f44a5ccb5ddd80b00af1b8d62
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-custom-layout"></a>Erstellen eines benutzerdefinierten Layouts

_Xamarin.Forms definiert vier Klassen für Layout – StackLayout, AbsoluteLayout RelativeLayout und Raster, und jede ordnet die untergeordneten Elemente auf andere Weise. In einigen Fällen ist es jedoch erforderlich, zum Organisieren von Seiteninhalt mithilfe eines Layouts dargestellt, die nicht von Xamarin.Forms bereitgestellt. In diesem Artikel wird erläutert, wie eine benutzerdefiniertes Layout-Klasse schreiben und eine Ausrichtung Akzent WrapLayout-Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente, um zusätzliche Zeilen veranschaulicht._

## <a name="overview"></a>Übersicht

In Xamarin.Forms mit, allen Layout-Klassen abgeleitet werden, aus der [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Klasse und einschränken den generischen Typ, [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) und die abgeleiteten Typen. Wiederum die `Layout<T>` Klasse leitet sich von der [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Klasse, die den Mechanismus zum Positionieren und Sizing untergeordnete Elemente bereitstellt.

Jedes visuelle Element ist zuständig für das Ermitteln seiner eigenen geeignete Größe, die so genannte der *angefordert* Größe. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), und [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) abgeleitete Typen sind zuständig für das die Position und Größe seiner untergeordneten oder untergeordnete Elemente, die relativ zum selbst ermitteln. Aus diesem Grund umfasst das Layout eine über-/ unterordnungsbeziehung, die, in dem das übergeordnete Element bestimmt, wie die Größe der untergeordneten Elemente sein soll, jedoch wird versucht, die angeforderte Größe des untergeordneten Elements.

Eine gründliche Kenntnisse Xamarin.Forms Layout und Ungültigkeit Zyklen ist erforderlich, um ein benutzerdefiniertes Layout zu erstellen. Dieser Zyklen werden jetzt erläutert werden.

## <a name="layout"></a>Layout

Layout beginnt am oberen Rand der visuellen Struktur mit einer Seite, und es durchläuft alle Zweige der visuellen Struktur jedes visuelle Element auf einer Seite einnimmt. Elemente, die übergeordneten Objekte auf andere Elemente sind sind verantwortlich für die größenanpassung und Positionierung von ihren untergeordneten Elementen relativ zum selbst.

Die [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Klasse definiert ein [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) -Methode, die ein Element für Layoutvorgänge, measures und ein [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methode, die angibt Der rechteckige Bereich, dem in das Element gerendert wird. Beim Starten einer Anwendung und die erste Seite angezeigt wird, eine *Layoutzyklus* besteht der erste von `Measure` aufruft, und klicken Sie dann `Layout` aufgerufen wird, beginnt auf der [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Objekt:

1. Während der Layoutzyklus jedes übergeordnete Element ist verantwortlich für das Aufrufen der `Measure` -Methode für seine untergeordneten Elemente.
1. Jede übergeordnete Element ist verantwortlich für das aufrufen, nachdem die untergeordneten Elemente gemessen worden sein, die `Layout` Methode auf seine untergeordneten Elemente.

Dieser Zyklus wird sichergestellt, dass alle visuellen Element auf der Seite Aufrufe empfängt die `Measure` und `Layout` Methoden. Der Prozess wird im folgenden Diagramm dargestellt:

![](custom-images/layout-cycle.png "Xamarin.Forms Layoutzyklus")

> [!NOTE]
> Beachten Sie, dass Zyklen Layout auch auf eine Teilmenge der visuellen Struktur auftreten können, wenn etwas geändert, um das Layout zu beeinflussen. Dies schließt Elemente hinzugefügt oder entfernt aus einer Auflistung, z. B. in einem [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), eine Änderung in der [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Eigenschaft eines Elements oder einer Änderung der Größe eines Elements.

Jede Xamarin.Forms-Klasse, verfügt ein `Content` oder ein `Children` Eigenschaft verfügt über eine überschreibbare [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) Methode. Abgeleitete Klassen benutzerdefiniertes Layout [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) müssen diese Methode überschreiben und sicherstellen, dass die [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) und [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methoden sind wird aufgerufen, auf alle seiner untergeordneten Elemente, um die gewünschte benutzerdefinierte Anordnung bereitzustellen.

Darüber hinaus jede Klasse, die abgeleitet [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) oder [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) überschreiben, müssen die [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) -Methode, die ist eine Layoutklasse Bestimmt die Größe, die benötigt werden durch Aufrufe an die [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) Methoden seiner untergeordneten Elemente.

> [!NOTE]
> Bestimmen die Elemente, deren Größe auf Grundlage *Einschränkungen*, die angeben, wie viel Speicherplatz für ein Element innerhalb des übergeordneten Elements verfügbar ist. Einschränkungen zu übergeben, um die [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) und [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) Methoden reichen von 0 bis `Double.PositiveInfinity`. Ein Element ist *eingeschränkte*, oder *vollständig eingeschränkt*, beim Empfang von eines Aufrufs von seiner [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) Methode durch nicht unendlichen Argumente - das Element beschränkt ist auf eine bestimmte Größe. Ein Element ist *uneingeschränkte*, oder *teilweise eingeschränkte*, beim Empfang von eines Aufrufs von seiner `Measure` Methode mit mindestens einem Argument ungleich `Double.PositiveInfinity` – unendliche Einschränkung kann als, der angibt, automatisches Anpassen der Größe betrachtet.

## <a name="invalidation"></a>Aufheben einer Validierung

Invalidierung wird mit dem eine Änderung in einem Element auf einer Seite eine neue Layoutzyklus auslöst. Elemente werden als ungültig betrachtet, wenn sie nicht mehr die richtige Größe oder Position aufweisen. Z. B. wenn die [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) Eigenschaft eine [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Änderungen, die `Button` gilt als ungültig, da es die richtige Größe nicht mehr verfügbar. Ändern der Größe der `Button` möglicherweise eine sich allmählich ausbreitenden Wirkung der Änderungen im Layout, über den Rest der Seite.

Elemente für ungültig zu erklären sich durch den Aufruf der [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) -Methode, wenn in der Regel eine Eigenschaft des Elements ändert, die möglicherweise einer neuen Größe des Elements. Diese Methode löst die [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) -Ereignis, das des übergeordneten Elements behandelt wird, um eine neue Layoutzyklus auszulösen.

Die [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) -Klasse legt einen Handler für die [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) Ereignis für jedes untergeordnete Element hinzugefügt seine `Content` Eigenschaft oder `Children` -Auflistung, und trennt den Ereignishandler bei der untergeordnete Element wird entfernt. Daher wird jedes Element in der visuellen Struktur, die über untergeordnete Elemente verfügt gewarnt, bei jeder Änderung eines seiner untergeordneten Elemente Größe. Das folgende Diagramm veranschaulicht, wie eine Änderung der Größe eines Elements in der visuellen Struktur Änderungen verursacht werden können, die die Struktur ripple:

![](custom-images/invalidation.png "In der visuellen Struktur invalidierung")

Allerdings die `Layout` Klasse versucht wird, um die Auswirkungen einer Änderung in einer untergeordneten Größe auf das Layout einer Seite zu beschränken. Wenn das Layout Größe beschränkt wird, klicken Sie dann wirkt eine Änderung der Textgröße untergeordneten etwas höher als das übergeordnete Layout in der visuellen Struktur sich nicht. Wirkt sich jedoch normalerweise eine Änderung in der Größe eines Layouts wie das Layout seiner untergeordneten Elemente anordnet. Aus diesem Grund Änderung der Größe eines Layouts startet einen Layoutzyklus für das Layout, und das Layout erhalten Aufrufe an die [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) und [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) Methoden.

Die [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) -Klasse definiert außerdem eine [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Methode, die einen ähnlichen Zweck, hat die [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) Methode. Die `InvalidateLayout` Methode sollte aufgerufen werden, wenn eine Änderung, die beeinflussen vorgenommen wird, wie das Layout positioniert und passt seine untergeordneten Elemente. Z. B. die `Layout` Klasse ruft der `InvalidateLayout` -Methode auf, wenn ein untergeordnetes Element hinzugefügt oder aus einem Layout entfernt.

Die [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) überschrieben werden können, um einen Cache, um wiederholte Aufrufe von minimiert implementieren die [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) Methoden das Layout der untergeordneten Elemente. Überschreiben der `InvalidateLayout` Methode bietet eine Benachrichtigung, wenn untergeordnete Elemente hinzugefügt oder aus dem Layout entfernt. Auf ähnliche Weise die [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Methode kann überschrieben werden, um eine Benachrichtigung zu bieten, wenn eines der Layout untergeordneten Elemente Größe ändert. Für beide methodenüberschreibungen soll ein benutzerdefiniertes Layout reagieren, indem Sie den Zwischenspeicher zu löschen. Weitere Informationen finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

## <a name="creating-a-custom-layout"></a>Erstellen eines benutzerdefinierten Layouts

Der Prozess zum Erstellen eines benutzerdefinierten Layouts lautet wie folgt:

1. Erstellen Sie eine von der `Layout<View>`-Klasse abgeleitete Klasse. Weitere Informationen finden Sie unter [erstellen eine WrapLayout](#creating).
1. [*optional*] Hinzufügen von Eigenschaften, die gesichert durch bindbare Eigenschaften für alle Parameter, die auf der Layoutklasse festgelegt werden soll. Weitere Informationen finden Sie unter [Hinzufügen von Eigenschaften, die durch bindbare Eigenschaften gesichert](#adding_properties).
1. Überschreiben der [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) aufzurufende Methode der [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) Methode auf alle Layout untergeordnete Elemente, und der Rückgabewert eine angeforderte Größe für das Layout. Weitere Informationen finden Sie unter [Überschreiben der Methode OnMeasure](#onmeasure).
1. Überschreiben der [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) aufzurufende Methode der [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methode auf alle Layout untergeordneten Standorten. Fehler beim Aufrufen der [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) -Methode für jedes untergeordnete Element in einem Layout führt dazu, das untergeordnete Element nie empfangen eine richtige Größe oder Position, und daher das untergeordnete Element wird auf der Seite sichtbar. Weitere Informationen finden Sie unter [Überschreiben der Methode LayoutChildren](#layoutchildren).

  > [!NOTE]
>  Beim Auflisten der untergeordneten Elemente in der [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) und [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) überschreibt, überspringen Sie allen untergeordneten Elementen, deren [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Eigenschaftensatz zu `false`. Dadurch wird sichergestellt, dass benutzerdefinierte Layouts für unsichtbar Kinder Platz wird nicht.

1. [*optional*] überschreiben die [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Methode benachrichtigt werden, wenn untergeordnete Elemente hinzugefügt oder aus dem Layout entfernt. Weitere Informationen finden Sie unter [Überschreiben der Methode InvalidateLayout](#invalidatelayout).
1. [*optional*] überschreiben die [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Methode benachrichtigt werden, wenn eine der das Layout der untergeordneten Elemente Größe ändert. Weitere Informationen finden Sie unter [Überschreiben der Methode OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Beachten Sie, dass die [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) Außerkraftsetzung wird nicht aufgerufen werden, wenn die Größe des Layouts von seinem übergeordneten Element, anstatt seine untergeordneten Elemente unterliegt. Jedoch die Außerkraftsetzung wird werden aufgerufen, wenn eine oder beide der Einschränkungen unendlich sind oder die Layoutklasse verfügt nicht standardmäßiger [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) oder [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaftswerte. Aus diesem Grund die [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) Außerkraftsetzung nicht verlässlich untergeordneten Größen abgerufen werden, während die [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) -Methodenaufruf. Stattdessen `LayoutChildren` aufrufen muss die [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) Methode auf das Layout untergeordneten Standorten, vor dem Aufrufen der [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methode. Alternativ können Sie die Größe der untergeordneten Elemente abgerufen, der `OnMeasure` überschreiben kann zwischengespeichert werden, um zu einem späteren Zeitpunkt vermeiden `Measure` Aufrufe in die `LayoutChildren` überschreiben, aber die Layoutklasse müssen wissen, wann die Größen müssen erneut abgerufen werden soll. Weitere Informationen finden Sie unter [berechnen und Zwischenspeichern von Daten Layout](#caching).

Die Layoutklasse kann dann genutzt werden, durch das Hinzufügen zu einer [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), und durch Hinzufügen von untergeordneten Elemente des Layouts. Weitere Informationen finden Sie unter [nutzen die WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Erstellen eine WrapLayout

Die beispielanwendung für veranschaulicht eine Ausrichtung Akzent `WrapLayout` -Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente, um zusätzliche Zeilen.

Die `WrapLayout` Klasse weist die gleiche Menge an Speicherplatz für jedes untergeordnete Element, bekannt als die *Größe der Zelle*, basierend auf der maximalen Größe der untergeordneten Elemente. Untergeordnete Elemente, die kleiner ist als die Größe der Zelle in der Zelle positioniert werden kann, basierend auf deren [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaftswerte.

Die `WrapLayout` Klassendefinition wird im folgenden Codebeispiel gezeigt:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Berechnen und das Zwischenspeichern Layoutdaten

Die `LayoutData` -Struktur speichert Daten zu einer Auflistung von untergeordneten Elementen in einer Reihe von Eigenschaften:

- `VisibleChildCount` – die Anzahl der Kinder, die im Layout sichtbar sind.
- `CellSize` – die maximale Größe aller untergeordneten Elemente, auf die Größe des Layouts angepasst.
- `Rows` – die Anzahl der Zeilen.
- `Columns` – die Anzahl der Spalten.

Die `layoutDataCache` Feld dient zum Speichern von mehreren `LayoutData` Werte. Beim Starten der Anwendung, zwei `LayoutData` Objekte zwischengespeichert werden soll, in der `layoutDataCache` Wörterbuch für die aktuelle Ausrichtung – eine für die Einschränkung Argumente für die `OnMeasure` überschreiben und eine für die `width` und `height` Argumente um die `LayoutChildren` außer Kraft setzen. Beim Drehen des Geräts in Querformat der `OnMeasure` außer Kraft setzen und die `LayoutChildren` Außerkraftsetzung wird erneut aufgerufen werden, was zu einem anderen zwei führt `LayoutData` Objekte, die in das Wörterbuch zwischengespeichert. Allerdings beim Zurückgeben des Geräts an Hochformat keine weiteren Berechnungen erforderlich sind, da die `layoutDataCache` hat bereits die erforderlichen Daten.

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

- Sie bestimmt, ob eine berechnete `LayoutData` Wert befindet sich bereits im Cache, und gibt ihn zurück, sofern dieser verfügbar ist.
- Andernfalls listet es über alle untergeordneten Elemente, Aufrufen der `Measure` -Methode für jedes untergeordnete Element mit einem eine unendliche Breite und Höhe und bestimmt die maximale untergeordneten Größe.
- Vorausgesetzt, es mindestens ein sichtbaren untergeordneten ist, er berechnet die Anzahl der Zeilen und Spalten, die erforderlich sind und berechnet dann die Größe einer Zelle für die untergeordneten Elemente basierend auf die Dimensionen der der `WrapLayout`. Beachten Sie, dass die Größe der Zelle in der Regel etwas breiter als die maximale untergeordneten Größe ist jedoch, dass sie auch kleinere sein kann, wenn die `WrapLayout` ist nicht breit genug für die breitesten untergeordneten oder hoch genug für die höchsten untergeordneten.
- Es speichert das neue `LayoutData` Wert im Cache.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Hinzufügen von Eigenschaften, die durch bindbare Eigenschaften gesichert

Die `WrapLayout` Klasse definiert `ColumnSpacing` und `RowSpacing` Eigenschaften, deren Werte werden verwendet, um die Zeilen und Spalten im Layout zu trennen und denen von bindbare Eigenschaften gesichert werden. Die bindungsfähigen Eigenschaften werden im folgenden Codebeispiel gezeigt:

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

Ruft der Handler durch geänderte Eigenschaften ausgelöste jeder bindbare Eigenschaft der `InvalidateLayout` Methode überschreiben, um ein neues Layout auszulösen übergeben wird, auf die `WrapLayout`. Weitere Informationen finden Sie unter [Überschreiben der Methode InvalidateLayout](#invalidatelayout) und [Überschreiben der Methode OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>Überschreiben der OnMeasure-Methode

Die `OnMeasure` Außerkraftsetzung wird im folgenden Codebeispiel gezeigt:

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

Ruft die Überschreibung der `GetLayoutData` -Methode und Konstrukte ein `SizeRequest` Objekt in den Rückgabedaten auch Berücksichtigung der `RowSpacing` und `ColumnSpacing` Eigenschaftswerte. Weitere Informationen zu den `GetLayoutData` -Methode finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

> [!IMPORTANT]
> Die [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) und [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) Methoden sollte niemals eine unendliche Dimension anfordern, indem die Rückgabe einer [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) Wert einer Eigenschaft auf festgelegt `Double.PositiveInfinity`. Jedoch mindestens eine Einschränkung Argumente `OnMeasure` kann `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>Überschreiben der LayoutChildren-Methode

Die `LayoutChildren` Außerkraftsetzung wird im folgenden Codebeispiel gezeigt:

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

Die Außerkraftsetzung beginnt mit einem Aufruf von der `GetLayoutData` -Methode, und listet dann alle untergeordneten Elemente für die Größe, und positionieren Sie sie in jedes untergeordneten Zelle. Dies erfolgt durch Aufrufen der [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) -Methode, die verwendet wird, positionieren Sie ein untergeordnetes Element innerhalb eines Rechtecks, das basierend auf seiner [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)Eigenschaftswerte. Dies ist gleichbedeutend mit einem Aufruf an der untergeordnetes [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methode.

> [!NOTE]
> Beachten Sie, die das Rechteck an die `LayoutChildIntoBoundingRegion` -Methode enthält den gesamten Bereich, in denen sich das untergeordnete Element befinden kann.

Weitere Informationen zu den `GetLayoutData` -Methode finden Sie unter [berechnen und Zwischenspeichern von Daten](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Überschreiben der InvalidateLayout-Methode

Die [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Außerkraftsetzung wird aufgerufen, wenn untergeordnete Elemente werden hinzugefügt oder entfernt werden, aus dem Layout, oder wenn bei mindestens einer von der `WrapLayout` Eigenschaften der Wert ändert, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Die Außerkraftsetzung wird das Layout ungültig und verwirft alle zwischengespeicherten Layoutinformationen.

> [!NOTE]
> So beenden Sie die [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Klasse Aufrufen der [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) -Methode auf, wenn ein untergeordnetes Element hinzugefügt oder daraus ein Layout zu überschreiben die [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) und [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) Methoden und der Rückgabewert `false`. Die Layoutklasse können Sie einen benutzerdefinierten Prozess implementieren, wenn untergeordnete Elemente hinzugefügt oder entfernt werden.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Überschreiben der OnChildMeasureInvalidated-Methode

Die [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Außerkraftsetzung wird aufgerufen, wenn eines das Layout der untergeordneten Elemente die Größe ändert und wird im folgenden Codebeispiel gezeigt:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Die Außerkraftsetzung erklärt das Layout der untergeordneten und verwirft alle zwischengespeicherten Layoutinformationen.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Nutzen die WrapLayout

Die `WrapLayout` Klasse genutzt werden kann, indem er auf Platzieren einer [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) abgeleiteten Typ, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Die entsprechende C#-Code wird unten gezeigt:

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

Untergeordnete Elemente können anschließend hinzugefügt werden, um die `WrapLayout` nach Bedarf. Das folgende Codebeispiel zeigt [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) hinzugefügte Elemente der `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

Wenn die Seite mit den `WrapLayout` angezeigt wird, die beispielanwendung greift auf eine Remotedatei von JSON mit einer Liste von Fotos asynchron ist, erstellt ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element für jedes Foto, und die hinzugefügt`WrapLayout`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

![](custom-images/portait-screenshots.png "Beispiel-Anwendung Hochformat Screenshots")

Die folgenden Screenshots zeigen die `WrapLayout` nachdem es Querformat gedreht wird:

![](custom-images/landscape-ios.png "Beispiel für iOS-Anwendung im Querformat Screenshot")
![](custom-images/landscape-android.png "Beispiel Android-Anwendung im Querformat Screenshot") 
 ![ ] (custom-images/landscape-uwp.png " Bildschirmabbildung von Beispiel uwp-Anwendung im Querformat")

Die Anzahl der Spalten in jeder Zeile hängt davon ab, die Größe des Fotos, die Bildschirmbreite und die Anzahl der Pixel pro geräteunabhängige Einheit. Die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Elemente asynchron geladen, die Fotos, und daher die `WrapLayout` Klasse erhalten häufig Aufrufe an die [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) Methode wie jede `Image` Element erhält eine neue Größe basierend auf dem geladenen Foto.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie eine benutzerdefiniertes Layout-Klasse zu schreiben, und demonstriert eine Ausrichtung Akzent `WrapLayout` -Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente, um zusätzliche Zeilen.


## <a name="related-links"></a>Verwandte Links

- [WrapLayout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Benutzerdefinierte Layouts](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Erstellen von benutzerdefinierten Layouts in Xamarin.Forms (video)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Layout<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [Layout](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
