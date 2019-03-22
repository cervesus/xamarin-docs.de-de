---
title: Xamarin.Forms CollectionView
description: 'Die CollectionView ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.'
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
---

# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![Vorschau](~/media/shared/preview.png)

> [!IMPORTANT]
> Die `CollectionView` ist derzeit eine Vorschauversion und verfügt nicht über einen Teil der geplanten Funktionen. Darüber hinaus kann die API ändern, wie die Implementierung abgeschlossen ist.

`CollectionView` ist eine Ansicht für die Darstellung von Listen mit Daten anderes Layout-Spezifikationen verwenden. Es zielt darauf ab, die eine flexiblere, bereitstellen und leistungsfähige Alternative zu [ `ListView` ](xref:Xamarin.Forms.ListView). Während der `CollectionView` und `ListView` APIs sind ähnlich, es gibt einige wichtige Unterschiede:

- `CollectionView` verfügt über ein flexibles Layout-Modell, die Daten vertikal oder horizontal in einer Liste oder einem Raster angezeigt werden kann.
- `CollectionView` verfügt über kein Konzept für die Zellen aus. Stattdessen wird eine Datenvorlage verwendet, um die Darstellung der einzelnen Elemente der Daten in der Liste zu definieren.
- `CollectionView` automatisch nutzt die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt.
- `CollectionView` reduziert die API-Oberfläche des [ `ListView` ](xref:Xamarin.Forms.ListView). Viele Eigenschaften und Ereignisse von `ListView` sind nicht im `CollectionView`.
- `CollectionView` umfasst keine integrierte Trennzeichen.

`CollectionView` ist in der Xamarin.Forms-4.0-Vorabversionen verfügbar. Allerdings es ist derzeit experimentell und kann nur verwendet werden, durch die folgende Zeile des Codes zum Hinzufügen Ihrer `AppDelegate` Klasse unter iOS, oder um Ihre `MainActivity` Klasse, die unter Android werden vor dem Aufruf `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` ist nur unter iOS und Android verfügbar.

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[CollectionView mit Daten auffüllen.](populate-data.md)

Ein `CollectionView` mit Daten aufgefüllt wird, durch Festlegen seiner `ItemsSource` Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`. Die Darstellung der einzelnen Elemente in der Liste definiert werden, indem Sie die Einstellung der `ItemTemplate` Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="specify-collectionview-layoutlayoutmd"></a>[Geben Sie CollectionView-layout](layout.md)

Standardmäßig eine `CollectionView` werden die Elemente in einer vertikalen Liste angezeigt. Allerdings können vertikale und horizontale Listen und Raster angegeben werden.

## <a name="set-collectionview-selection-modeselectionmd"></a>[Festlegen des Auswahlmodus CollectionView](selection.md)

In der Standardeinstellung `CollectionView` Auswahl ist deaktiviert. Allerdings kann die Auswahl von einzelnen und mehreren aktiviert werden.

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[Ein EmptyView angezeigt wird, wenn Daten nicht verfügbar ist.](emptyview.md)

In `CollectionView`, eine leere Ansicht kann angegeben werden, Feedback an den Benutzer bereitstellt, wenn keine Daten für die Anzeige verfügbar sind. Die leere Sicht kann es sich um eine Zeichenfolge, eine Sicht oder mehrere Ansichten sein.

## <a name="scroll-an-item-into-viewscrollingmd"></a>[Führen Sie ein Element in der Ansicht einen Bildlauf](scrolling.md)

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, kann die Endposition des Bildlaufs gesteuert werden, damit Elemente vollständig angezeigt werden. Darüber hinaus `CollectionView` definiert zwei `ScrollTo` Methoden, die programmgesteuert Elemente in der Ansicht zu scrollen. Eine der Überladungen führt einen Bildlauf durch das Element am angegebenen Index in der Ansicht, während die andere das angegebene Element in der Ansicht einen Bildlauf durchführt.
