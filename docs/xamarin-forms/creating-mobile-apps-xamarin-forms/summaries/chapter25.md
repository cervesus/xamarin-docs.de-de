---
title: Zusammenfassung der Kapitel 25. Seite "-Varianten
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 30642709519fc809d30da9a437728112f56a64d6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>Zusammenfassung der Kapitel 25. Seite "-Varianten

Bisher haben Sie gesehen, zwei abgeleitete Klassen `Page`: `ContentPage` und `NavigationPage`. Dieses Kapitel bietet zwei Zahlen:

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) verwaltet zwei Seiten, die einem Masterauftrag für den und ein detail
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) verwaltet von mehreren untergeordneten Seiten erfolgt über Registerkarten

Geben Sie diese Seitentypen komplexere Navigationsoptionen als die `NavagationPage` erläutert [Kapitel 24. Die Seitennavigation](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Master- und Detailtabelle

Die [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) definiert zwei Eigenschaften des Typs `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) und [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/). In der Regel legen Sie jede dieser Eigenschaften zu einem `ContentPage`. Die `MasterDetailPage` zeigt und wechselt zwischen diesen zwei Seiten.

Es gibt zwei grundlegende Methoden zum Wechseln zwischen diesen zwei Seiten:

- *Teilen* , in dem die Master- und Detailtabelle werden nebeneinander
- *Popover* Seite an, auf der Seite "Details" behandelt oder teilweise abdeckt Master

Es gibt mehrere Varianten von der *Popover* Ansatz (*Folie*, *überlappen*, und *Swap*), doch diese sind im Allgemeinen Plattform abhängige. Sie können festlegen, die [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) Eigenschaft `MasterDetailPage` an ein Mitglied der [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) Enumeration:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

Diese Eigenschaft hat jedoch keine Auswirkungen auf Telefonen. Smartphones weisen immer eine Popover Verhalten auf. Nur Tablet-PCs und Windows desktop können ein Split-Verhalten aufweisen.

### <a name="exploring-the-behaviors"></a>Untersuchen die Verhalten

Die [ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) Beispiel können Sie das Standardverhalten für verschiedene Geräte experimentieren. Das Programm enthält zwei Separate `ContentPage` ableitungen für die Master- und Detailtabelle (mit einer `Title` -Eigenschaft auf festgelegt), und eine andere Klasse, abgeleitet wird `MasterDetailPage` , die kombiniert. Die Seite "Details" steht in einem `NavigationPage` , da die uwp-Anwendung ohne diese funktionieren würde.

Die Windows 8.1 und Windows Phone 8.1-Plattformen erforderlich ist, dass eine Bitmap festgelegt werden, um die `Icon` Eigenschaft der Masterseite.

### <a name="back-to-school"></a>Zurück zu "School"

Die [ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) Beispiel nimmt einen etwas anderen Ansatz für das Erstellen des Programms anzuzeigenden Studenten aus der [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) Bibliothek.

Die `Master` und `Detail` Eigenschaften werden definiert, mit visueller Strukturen in den [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) -Datei, die abgeleitet `MasterDetailPage`. Diese Anordnung ermöglicht datenbindungen zwischen der Master- und Detailtabelle Seiten festgelegt werden.

XAML-Datei fest, die auch die [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) Eigenschaft `MasterDetailPage` auf `True`. Dies bewirkt, dass die Gestaltungsvorlage, die beim Start angezeigt werden; Standardmäßig wird die Detailseite angezeigt. Die [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) Dateisätzen `IsPresented` auf `false` Wenn ein Element ausgewählt ist, aus der `ListView` in die Masterseite. Die Seite "Details" wird angezeigt:

[![Dreifacher Screenshot School und Detailebene](images/ch25fg09-small.png "Detailseite aus einem MasterDetailPage")](images/ch25fg09-large.png#lightbox "Detailseite aus einem MasterDetailPage")

### <a name="your-own-user-interface"></a>Eine eigene Benutzeroberfläche

Obwohl Xamarin.Forms eine Benutzeroberfläche für den Wechsel zwischen den Master- und Detailtabelle Ansichten enthält, können Sie eigene angeben. Dazu:

- Legen Sie die [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) Eigenschaft `false` Streifen deaktivieren
- Überschreiben Sie die [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) -Methode und der Rückgabewert `false` auf Windows 8.1 und Windows Phone 8.1 die Schaltflächen der Symbolleiste ausblenden.

Sie müssen eine Möglichkeit zum Wechseln zwischen den Seiten Master- und Detailtabelle dann bereitstellen, z. B. zeigt die [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) Beispiel.

Die [ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) Beispiel wird veranschaulicht, einen anderen Ansatz mit einem `TapGestureRecognizer` auf den Seiten Master- und Detailtabelle.

## <a name="tabbedpage"></a>TabbedPage

Die [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) ist eine Sammlung von Seiten, die Sie zwischen Registerkarten wechseln können. Er leitet sich von `MultiPage<Page>` und keine öffentlichen Eigenschaften oder Methoden selbst definiert. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), allerdings eine Eigenschaft zu definieren:

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) Eigenschaft vom Typ `IList<T>`

Sie füllen Sie das `Children` Auflistung mit Page-Objekte.

Ein anderer Ansatz können Sie definieren die `TabbedPage` Kinder ähnlich wie eine `ListView` mithilfe dieser zwei Eigenschaften, die die Seiten im Registerformat automatisch generiert:

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) vom Typ `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) vom Typ `DataTemplate`

Dieser Ansatz funktioniert nicht gut auf iOS jedoch, wenn die Auflistung mehrere Elemente enthält.

`MultiPage<T>` definiert zwei weitere Eigenschaften, mit denen Sie überwachen, welche Seite aktuell angezeigt wird:

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) Der Typ `T`, das auf der Seite "
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) Der Typ `Object`, das auf das Objekt in der `ItemsSource` Auflistung

`MultiPage<T>` definiert außerdem zwei Ereignisse:

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) Wenn die `ItemsSource` sammlungsänderungen
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) Wenn die angezeigte Seite ändert

### <a name="discrete-tab-pages"></a>Diskrete Registerkarten

Die [ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) Beispiel besteht aus drei Seiten im Registerformat, in denen auf drei verschiedene Arten Farben angezeigt. Jede Registerkarte ist eine `ContentPage` Ableitung, und klicken Sie dann die `TabbedPage` Ableitung [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) kombiniert drei Seiten.

Für jede Seite, die in eine `TabbedPage`, die `Title` Eigenschaft ist erforderlich, um den Text auf der Registerkarte angeben, und dem Apple Store erfordert, dass ein Symbol ebenfalls verwendet werden daher die `Icon` Eigenschaftensatz für iOS:

[![Dreifacher Screenshot der diskreten im Registerkartenformat Farben](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

Die [ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) Beispiel verfügt über eine Startseite, die alle Studenten auflistet. Wenn eine Student abgerufen wird, navigiert dies zu einer `TabbedPage` Ableitung [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), die umfasst drei `ContentPage` in seiner visuellen Struktur zu Objekten, von denen einige Hinweise für diese Student eingeben kann.

### <a name="using-an-itemtemplate"></a>Verwenden eine Elementvorlage

Die [ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) -Beispiel verwendet die [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Die [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) legt die `DataTemplate` Eigenschaft `TabbedPage` zu einer visuellen Struktur mit `ContentPage` , Bindungen, um Eigenschaften des enthält `NamedColor` (z. B. eine Bindung an die `Title` Eigenschaft).

Dies ist jedoch auf iOS problematisch. Nur wenige Elemente angezeigt werden können, und es ist keine gute Möglichkeit, diese Symbole geben.



## <a name="related-links"></a>Verwandte Links

- [Chapter 25 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Chapter 25-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Master / Detail-Seite](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Seite im Registerformat](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
