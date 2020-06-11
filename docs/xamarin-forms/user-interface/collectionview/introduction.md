---
Title: " Xamarin.Forms CollectionView Introduction" Description: "CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen layoutspezifikationen."
ms. Prod: xamarin ms. assetid: 5c08f 687-b9e6-4ce4-8726-f 287f 6d0b6a7 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 12/11/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview-introduction"></a>Xamarin.FormsEinführung in CollectionView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen Layoutspezifikationen. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu bereitzustellen [`ListView`](xref:Xamarin.Forms.ListView) . Die folgenden Screenshots zeigen z. b. ein `CollectionView` , das ein vertikales Raster mit zwei Spalten verwendet und das mehrere Auswahlmöglichkeiten ermöglicht:

[![Screenshot eines vertikalen auflistungsansichts Layouts unter IOS und Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertikal Raster Layout mit Mehrfachauswahl")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertikal Raster Layout mit Mehrfachauswahl")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ist in Xamarin.Forms 4,3 verfügbar.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)ist unter IOS und Android verfügbar, ist aber nur [teilweise](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) auf dem universelle Windows-Plattform verfügbar.

## <a name="collectionview-and-listview-differences"></a>Unterschiede zwischen CollectionView und ListView

Obwohl die [`CollectionView`](xref:Xamarin.Forms.CollectionView) -und- [`ListView`](xref:Xamarin.Forms.ListView) APIs ähnlich sind, gibt es einige wichtige Unterschiede:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)verfügt über ein flexibles Layoutmodell, das das vertikale oder horizontale darstellen von Daten in einer Liste oder einem Raster ermöglicht.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt die einfache und mehrfache Auswahl.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwendet automatisch die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellte Virtualisierung.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)verringert die API-Oberfläche von [`ListView`](xref:Xamarin.Forms.ListView) . Viele Eigenschaften und Ereignisse von [`ListView`](xref:Xamarin.Forms.ListView) sind in nicht vorhanden `CollectionView` .
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)enthält keine integrierten Trennzeichen.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)löst eine Ausnahme aus, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) vom UI-Thread aktualisiert wird.

## <a name="move-from-listview-to-collectionview"></a>Aus ListView in CollectionView verschieben

[`ListView`](xref:Xamarin.Forms.ListView)Implementierungen in vorhandenen Xamarin.Forms Implementierungen können mithilfe der folgenden Tabelle zu-Implementierungen migriert werden [`CollectionView`](xref:Xamarin.Forms.CollectionView) :

| Konzept | ListView-API | CollectionView |
|---|---|---|
| Daten | `ItemsSource` | Eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird durch Festlegen der-Eigenschaft mit Daten aufgefüllt `ItemsSource` . Weitere Informationen finden Sie unter Auffüllen [einer CollectionView mit Daten](populate-data.md#populate-a-collectionview-with-data). |
| Element Darstellung | `ItemTemplate` | Die Darstellung der einzelnen Elemente in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann definiert werden, indem die-Eigenschaft auf festgelegt wird `ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Weitere Informationen finden Sie unter [Definieren der Element](populate-data.md#define-item-appearance)Darstellung. |
| Zellen | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)hat kein Konzept von Zellen und daher kein Konzept von Offenlegungs Indikatoren. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren. |
| Zeilen Trennzeichen | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)enthält keine integrierten Trennzeichen. Diese können, falls gewünscht, in der Element Vorlage angegeben werden. |
| Auswahl | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt die einfache und mehrfache Auswahl. Weitere Informationen finden Sie unter [ Xamarin.Forms CollectionView-Auswahl](selection.md). |
| Zeilenhöhe | `HasUnevenRows`, `RowHeight` | In einem `CollectionView` wird die Zeilenhöhe jedes Elements durch die-Eigenschaft bestimmt `ItemSizingStrategy` . Weitere Informationen finden Sie unter [Item Sizing](layout.md#item-sizing).|
| Zwischenspeicherung | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwendet automatisch die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellte Virtualisierung. |
| Kopf- und Fußzeilen | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)kann eine Kopfzeile und eine Fußzeile darstellen, die mit den Elementen in der Liste über `Header` die `Footer` Eigenschaften,, `HeaderTemplate` und Scrollen `FooterTemplate` . Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](layout.md#headers-and-footers). |
| Gruppierung | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)zeigt ordnungsgemäß gruppierte Daten durch Festlegen der- `IsGrouped` Eigenschaft auf an `true` . Gruppen Kopfzeilen und-Fußzeilen können durch Festlegen der `GroupHeaderTemplate` -Eigenschaft und der-Eigenschaft auf-Objekte angepasst werden `GroupFooterTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Weitere Informationen finden Sie unter [ Xamarin.Forms CollectionView-Gruppierung](grouping.md). |
| Aktualisierung durch Ziehen | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Die Funktion "Pull to refresh" wird unterstützt, indem ein als untergeordnetes Element eines festgelegt wird [`CollectionView`](xref:Xamarin.Forms.CollectionView) `RefreshView` . Weitere Informationen finden Sie unter [Pull to refresh](populate-data.md#pull-to-refresh). |
| Kontextmenüelemente | `ContextActions` | Kontextmenü Elemente werden unterstützt, indem ein `SwipeView` als Stamm Ansicht in der festgelegt wird, die die Darstellung [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) der einzelnen Datenelemente in der definiert [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Weitere Informationen finden Sie unter [Kontextmenüs](populate-data.md#context-menus). |
| Scrollen | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert `ScrollTo` Methoden, die Elemente in der Ansicht scrollen. Weitere Informationen finden Sie unter [scrollvorgänge](scrolling.md). |

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
