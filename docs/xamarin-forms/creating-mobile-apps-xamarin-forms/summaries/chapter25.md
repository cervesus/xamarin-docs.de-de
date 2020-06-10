---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 25. Page varieties''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e66fb50b8d537ee0267457d5b0ab0f417813e676
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136616"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Zusammenfassung von Kapitel 25. Seitenvarianten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

Bisher haben Sie zwei Klassen kennengelernt, die von `Page` abgeleitet werden: `ContentPage` und `NavigationPage`. In diesem Kapitel werden zwei weitere vorgestellt:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) verwaltet zwei Seiten, eine Master- und eine Detailseite.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) verwaltet mehrere untergeordnete Seiten, auf die über Registerkarten zugegriffen wird.

Diese Seitentypen bieten anspruchsvollere Navigationsoptionen als die `NavagationPage`, die in [Kapitel 24, „Seitennavigation“](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md), besprochen wird.

## <a name="master-and-detail"></a>Master und Detail

Die [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) definiert zwei Eigenschaften vom Typ `Page`: [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) und [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail). Im Allgemeinen legen Sie jede dieser Eigenschaften auf eine `ContentPage` fest. Die `MasterDetailPage` zeigt diese beiden Seiten an und wechselt zwischen ihnen.

Es gibt zwei grundlegende Möglichkeiten, um zwischen diesen beiden Seiten zu wechseln:

- *Teilen*, wobei Master- und Detailseite nebeneinander angeordnet werden.
- *Popover*, wobei die Detailseite die Masterseite vollständig oder teilweise verdeckt.

Es gibt mehrere Varianten des *Popover*-Ansatzes (*gleiten*, *überlappen* und *tauschen*), aber diese sind in der Regel plattformabhängig. Sie können die [`MasterDetailBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior)-Eigenschaft von `MasterDetailPage` auf einen Member der [`MasterBehavior`](xref:Xamarin.Forms.MasterBehavior)-Enumeration festlegen:

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

Diese Eigenschaft wirkt sich jedoch nicht auf Telefone aus. Smartphones weisen immer ein Popover-Verhalten auf. Nur Tablets und Desktopfenster können geteilt werden.

### <a name="exploring-the-behaviors"></a>Untersuchen der Verhalten

Mit dem [**MasterDetailBehaviors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors)-Beispiel können Sie mit dem Standardverhalten auf verschiedenen Geräten experimentieren. Das Programm enthält zwei separate `ContentPage`-Ableitungen für die Master- und Detailseite (wobei für beide eine `Title`-Eigenschaft festgelegt ist) und eine weitere Klasse, die von `MasterDetailPage` abgeleitet wird, in der sie kombiniert werden. Die Detailseite ist in einer `NavigationPage` enthalten, da das UWP-Programm ohne dies nicht funktionieren würde.

Die Windows 8.1- und Windows Phone 8.1-Plattformen erfordern, dass eine Bitmap auf die `Icon`-Eigenschaft der Masterseite festgelegt wird.

### <a name="back-to-school"></a>Schulanfang

Das [**SchoolAndDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail)-Beispiel verfolgt einen etwas anderen Ansatz zur Erstellung des Programms, um Kursteilnehmer aus der [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)-Bibliothek anzuzeigen.

Die Eigenschaften `Master` und `Detail` sind mit visuellen Strukturen in der Datei [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) definiert, die von `MasterDetailPage` abgeleitet wird. Diese Einrichtung ermöglicht die Festlegung von Datenbindungen zwischen den Master- und Detailseiten.

Diese XAML-Datei legt außerdem die [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented)-Eigenschaft von `MasterDetailPage` auf `True` fest. Dies bewirkt, dass beim Start die Masterseite angezeigt wird. Standardmäßig wird die Detailseite angezeigt. Die [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs)-Datei legt `IsPresented` auf `false` fest, wenn ein Element aus der `ListView` auf der Masterseite ausgewählt wird. Die Detailseite wird dann angezeigt:

[![Dreifacher Screenshot von Schule und Detail](images/ch25fg09-small.png "Detailseite von einer MasterDetailPage")](images/ch25fg09-large.png#lightbox "Detailseite von einer MasterDetailPage")

### <a name="your-own-user-interface"></a>Ihre eigene Benutzeroberfläche

Obwohl Xamarin.Forms eine Benutzeroberfläche zum Wechseln zwischen Master- und Detailansicht bereitstellt, können Sie Ihre eigene bereitstellen. Gehen Sie hierzu wie folgt vor:

- Legen Sie die [`IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled)-Eigenschaft auf `false` fest, um Wischen zu deaktivieren.
- Setzen Sie die [`ShouldShowToolbarButton`](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton)-Methode außer Kraft, und geben Sie `false` zurück, um die Schaltflächen der Symbolleiste unter Windows 8.1 und Windows Phone 8.1 auszublenden.

Anschließend müssen Sie eine Methode bereitstellen, um zwischen den Master- und Detailseiten zu wechseln, wie im Beispiel [**ColorsDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) veranschaulicht.

Im [**MasterDetailTaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps)-Beispiel wird ein anderer Ansatz veranschaulicht, der einen `TapGestureRecognizer` auf den Master- und Detailseiten verwendet.

## <a name="tabbedpage"></a>TabbedPage

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ist eine Sammlung von Seiten, zwischen denen Sie mithilfe von Registerkarten wechseln können. Sie wird von `MultiPage<Page>` abgeleitet und definiert keine eigenen öffentlichen Eigenschaften oder Methoden. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1) definiert jedoch eine Eigenschaft:

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children)-Eigenschaft vom Typ `IList<T>`.

Diese `Children`-Sammlung füllen Sie mit Seitenobjekten.

Ein anderer Ansatz erlaubt es Ihnen, die untergeordneten Elemente von `TabbedPage` ganz ähnlich wie eine `ListView` zu definieren, indem Sie diese beiden Eigenschaften verwenden, die die Registerkartenseiten automatisch erstellen:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vom Typ `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) vom Typ `DataTemplate`

Dieser Ansatz funktioniert jedoch nicht gut unter iOS, wenn die Sammlung mehr als ein paar Elemente enthält.

`MultiPage<T>` definiert zwei weitere Eigenschaften, mit denen Sie nachverfolgen können, welche Seite zurzeit angezeigt wird:

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) vom Typ `T`, die auf die Seite verweist.
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) vom Typ `Object`, das auf das Objekt in der `ItemsSource`-Sammlung verweist.

`MultiPage<T>` definiert auch zwei Ereignisse:

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged), wenn sich die `ItemsSource`-Sammlung ändert.
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged), wenn sich die angezeigte Seite ändert.

### <a name="discrete-tab-pages"></a>Diskrete Registerkartenseiten

Das [**DiscreteTabbedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors)-Beispiel besteht aus drei Registerkartenseiten, auf denen Farben auf drei unterschiedliche Weisen angezeigt werden. Jede Registerkarte ist eine `ContentPage`-Ableitung, und die `TabbedPage`-Ableitung [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) kombiniert dann die drei Seiten.

Für jede Seite, die auf einer `TabbedPage` angezeigt wird, ist die `Title`-Eigenschaft erforderlich, um den Text auf der Registerkarte anzugeben, und der Apple Store erfordert, dass außerdem ein Symbol verwendet wird, sodass die `Icon`-Eigenschaft für iOS festgelegt ist:

[![Dreifacher Screenshot von diskreten Registerkarten mit Farben](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

Das [**StudentNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes)-Beispiel besitzt eine Startseite, auf der alle Kursteilnehmer aufgeführt werden. Wenn Sie auf einen Kursteilnehmer tippen, erfolgt die Navigation zu einer `TabbedPage`-Ableitung, [`StudentNotesDataPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), die drei `ContentPage`-Objekte in ihrer visuellen Struktur enthält, von denen eins die Eingabe von Notizen zu diesem Schüler/Studenten ermöglicht.

### <a name="using-an-itemtemplate"></a>Verwenden einer „ItemTemplate“

Das [**MultiTabbedColor**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors)-Beispiel verwendet die [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs)-Klasse aus der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek. Die Datei [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) legt die `DataTemplate`-Eigenschaft der `TabbedPage` auf eine visuelle Struktur fest, beginnend mit `ContentPage`, die Bindungen zu Eigenschaften von `NamedColor` enthält (einschließlich einer Bindung an die `Title`-Eigenschaft).

Dies ist unter iOS jedoch problematisch. Nur ein paar dieser Elemente können angezeigt werden, und es gibt keine gute Möglichkeit, um ihnen Symbole zuzuordnen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 25 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Kapitel 25 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Master-Detail-Seite](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Registerkartenseite](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
