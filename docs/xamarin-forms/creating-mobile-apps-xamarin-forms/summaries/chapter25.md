---
title: Zusammenfassung der Kapitel 25. Seitenvarianten
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 25. Seitenvarianten'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: db6c329c029f52180fe508f277a1cf4834ab493a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331810"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Zusammenfassung der Kapitel 25. Seitenvarianten

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

Bisher haben Sie zwei abgeleitete Klassen gesehen `Page`: `ContentPage` und `NavigationPage`. Dieses Kapitel enthält zwei Zahlen:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) verwaltet zwei Seiten, eine Master- und einem
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) verwaltet mehrere untergeordnete Seiten zugegriffen werden, durch die Registerkarten

Diese Seite bereitstellen komplexer Navigationsoptionen als die `NavagationPage` ausführlicher [Kapitel 24. Seitennavigation](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Master- und Detailtabelle

Die [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) definiert zwei Eigenschaften vom Typ `Page`: [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) und [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail). In der Regel legen Sie jede dieser Eigenschaften zu einem `ContentPage`. Die `MasterDetailPage` zeigt an, und wechselt zwischen diese beiden Seiten.

Es gibt zwei grundlegende Verfahren, wechseln Sie zwischen diesen zwei Seiten:

- *Teilen* , in denen die Master- und Detailtabelle werden nebeneinander
- *im Popover* , in denen die Detailseite behandelt oder teilweise abdeckt den Master-Seite

Es gibt mehrere Varianten der *Popover* Ansatz (*Folie*, *überlappen*, und *Swap*), aber diese sind im Allgemeinen Plattform abhängige. Sie können festlegen, die [ `MasterDetailBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) Eigenschaft `MasterDetailPage` auf einen Member der [ `MasterBehavior` ](xref:Xamarin.Forms.MasterBehavior) Enumeration:

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

Allerdings hat diese Eigenschaft keine Auswirkungen auf Telefonen. Smartphones verfügen immer über ein Popover Verhalten. Nur für Tablets und desktop-Windows können eine Split-Verhalten verwenden.

### <a name="exploring-the-behaviors"></a>Untersuchen des Verhaltens

Die [ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) Beispiel ermöglicht Ihnen das Experimentieren mit dem Standardverhalten auf verschiedenen Geräten. Das Programm enthält zwei Separate `ContentPage` ableitungen für die Master- und Detailtabelle (mit einer `Title` -Eigenschaft auf festgelegt), und eine andere abgeleitete Klasse `MasterDetailPage` , die kombiniert werden. Die Seite "Details" steht in einem `NavigationPage` , da die UWP-Anwendung ohne diese funktionieren würde.

Die Windows 8.1 und Windows Phone 8.1-Plattformen erfordern, dass eine Bitmap festgelegt werden, um die `Icon` Eigenschaft der Masterseite.

### <a name="back-to-school"></a>Zurück auf die Schulbank

Die [ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) Beispiel verwendet einen etwas anderen Ansatz zum Erstellen des Programms zum Anzeigen von Schülern und Studenten die [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) Bibliothek.

Die `Master` und `Detail` Eigenschaften definiert sind, mit visuellen Strukturen in der [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) -Datei, die abgeleitet `MasterDetailPage`. Diese Anordnung ermöglicht datenbindungen, die zwischen den Master- und Detailtabelle Seiten festgelegt werden.

Außerdem wird der XAML-Datei der [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Eigenschaft `MasterDetailPage` zu `True`. Dies bewirkt, dass die Masterseite, die beim Start angezeigt werden; Standardmäßig wird die Seite "Details" angezeigt. Die [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) legt `IsPresented` zu `false` Wenn ein Element ausgewählt ist, aus der `ListView` auf der Masterseite. Die Seite "Details" wird dann angezeigt:

[![Dreifacher Screenshot der Schule und Detailansicht](images/ch25fg09-small.png "Detailseite aus einem MasterDetailPage")](images/ch25fg09-large.png#lightbox "Detailseite aus einem MasterDetailPage")

### <a name="your-own-user-interface"></a>Eine eigene Benutzeroberfläche

Obwohl Xamarin.Forms eine Benutzeroberfläche für das Wechseln zwischen der Master- und Detailtabelle Ansichten bietet, können Sie Ihre eigenen bereitstellen. Gehen Sie hierzu wie folgt vor:

- Legen Sie die [ `IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) Eigenschaft `false` Wischen deaktivieren
- Überschreiben der [ `ShouldShowToolbarButton` ](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) Methode, und geben `false` zum Ausblenden der Schaltflächen der Symbolleiste auf Windows 8.1 und Windows Phone 8.1.

Sie müssen eine Möglichkeit zum Wechseln zwischen der Master- und Detailtabelle-Seiten, klicken Sie dann angeben, wie z. B. von veranschaulicht die [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) Beispiel.

Die [ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) Beispiel zeigt die Verwendung von einem anderen Ansatz eine `TapGestureRecognizer` auf den Master- und Detailtabelle Seiten.

## <a name="tabbedpage"></a>"Tabbedpage"

Die [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) ist eine Sammlung von Seiten, die Sie zwischen Registerkarten wechseln können. Es leitet sich von `MultiPage<Page>` und definiert keine öffentlichen Eigenschaften oder Methoden selbst. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1), jedoch eine Eigenschaft definieren:

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) Eigenschaft des Typs `IList<T>`

Sie füllen Sie das `Children` Sammlung mit Page-Objekte.

Ein anderer Ansatz können Sie definieren die `TabbedPage` untergeordneten Elemente ähnlich wie eine `ListView` verwenden diese beiden Eigenschaften, die die Seiten im Registerformat automatisch zu generieren:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) Der Typ `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Der Typ `DataTemplate`

Dieser Ansatz funktioniert nicht gut unter iOS jedoch, wenn die Auflistung mehr als ein paar Elemente enthält.

`MultiPage<T>` definiert zwei weitere Eigenschaften, mit denen Sie überwachen, welche Seite derzeit angezeigt wird:

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) Der Typ `T`, bezieht sich auf der Seite "
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) Der Typ `Object`, bezieht sich auf das Objekt in der `ItemsSource` Auflistung

`MultiPage<T>` definiert außerdem zwei Ereignisse:

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) Wenn die `ItemsSource` Auflistung geändert wird
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) Wenn die angezeigte Seite geändert wird

### <a name="discrete-tab-pages"></a>Diskrete Registerkarten

Die [ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) Beispiel besteht aus drei Seiten im Registerformat, die Farben in drei verschiedene Arten anzeigen. Jede Registerkarte dient eine `ContentPage` Ableitung, und klicken Sie dann die `TabbedPage` Ableitung [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) kombiniert die drei Seiten.

Für jede Seite wird, in angezeigt eine `TabbedPage`, wird die `Title` Eigenschaft ist erforderlich, um den Text auf der Registerkarte angeben und den Apple Store erfordert, dass ein Symbol ebenfalls verwendet werden also die `Icon` -Eigenschaftensatz für iOS:

[![Dreifacher Screenshot, der diskrete im Registerkartenformat Farben](images/ch25fg13-small.png "\"tabbedpage\"")](images/ch25fg13-large.png#lightbox "\"tabbedpage\"")

Die [ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) Beispiel verfügt über eine Startseite, die alle Studenten auflistet. Wenn ein Schüler/Student getippt wird wird, hierdurch navigieren Sie zu einer `TabbedPage` Ableitung [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), enthält drei `ContentPage` Objekte in der visuellen Struktur, von denen kann einige Hinweise für diese "Student" eingeben.

### <a name="using-an-itemtemplate"></a>Verwenden eine Elementvorlage

Die [ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) -Beispiel verwendet die [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Die [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) legt die `DataTemplate` Eigenschaft `TabbedPage` in eine visuelle Struktur, beginnend mit `ContentPage` , enthält die Bindung an Eigenschaften des `NamedColor` (einschließlich einer Bindung an die `Title` Eigenschaft).

Dies ist jedoch problematisch für iOS. Nur einige der Elemente angezeigt werden können, und es ist keine gute Möglichkeit, diese Symbole geben.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 25 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Kapitel 25-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Master / Detail-Seite](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Seite im Registerformat](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
