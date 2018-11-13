---
title: Zusammenfassung der Kapitel 24. Seitennavigation
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 24. Seitennavigation'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: d90bef4da589215247f412450905a5db541b0444
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563159"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Zusammenfassung der Kapitel 24. Seitennavigation

Viele Anwendungen bestehen aus mehreren Seiten, die für die der Benutzer navigiert. Die Anwendung immer eine *main* Seite oder *home* Seite, und von dort aus der Benutzer navigiert zu anderen Seiten, die in einem Stapel für die Navigation zurück verwaltet werden. Zusätzliche Navigationsoptionen finden Sie im [ **Kapitel 25. Seite Varianten**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Modale und nicht modale Seiten

`VisualElement` definiert eine [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Eigenschaft vom Typ [ `INavigation` ](xref:Xamarin.Forms.INavigation), darunter die folgenden beiden Methoden, um eine neue Seite zu navigieren:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Beide Methoden akzeptieren eine `Page` Instanz als ein Argument und der Rückgabewert eine `Task` Objekt. Die folgenden beiden Methoden navigieren Sie zurück zur vorherigen Seite:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Wenn die Benutzeroberfläche verfügt über ein eigenes **wieder** Schaltfläche (wie Android und Windows-Smartphones), ist es nicht erforderlich, für die Anwendung auf diese Methoden aufrufen.

Obwohl diese Methoden aus allen verfügbaren `VisualElement`, in der Regel heißen sie aus der `Navigation` Eigenschaft des aktuellen `Page` Instanz.

Anwendungen verwenden im Allgemeinen modale Seiten auf, wenn der Benutzer erforderlich ist, einige Informationen auf der Seite vor der Rückkehr zur vorherigen Seite angeben. Seiten, die nicht modale sind, werden manchmal als nicht modales bezeichnet oder *hierarchische*. "Nothing" in die Seite selbst unterscheidet es als modales oder nicht modales; Es wird stattdessen von der Methode verwendet, um ihn zu gesteuert. Um auf allen Plattformen arbeiten zu können, muss eine modale Seite eine eigene Benutzeroberfläche für Benutzer bereitstellen, für die Navigation zurück zur vorherigen Seite.

Die [ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) Beispiel können Sie den Unterschied zwischen ohne Modus und modale Seiten zu untersuchen. Jede Anwendung, Seitennavigation verwendet, muss die Startseite, übergeben die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Konstruktor in der Regel im des Programms `App` Klasse. Ein Bonus ist, dass Sie nicht mehr benötigen, legen Sie eine `Padding` auf der Seite für iOS.

Sie werden feststellen, dass für nicht modale Seiten, die Seite des [ `Title` ](xref:Xamarin.Forms.Page.Title) Eigenschaft wird angezeigt. Geben Sie ein Element der Benutzeroberfläche zurück zur vorherigen Seite navigieren, iOS, Android und die Windows-Tablet und Desktop-Plattformen. Kurs, Android und Windows Phone-Geräte müssen einen Standard **wieder** Schaltfläche, um zurückzukehren.

Für modale Seiten, die Seite `Title` nicht angezeigt wird, und kein Element der Benutzeroberfläche wird bereitgestellt, um zur vorherigen Seite zurückzukehren. Sie können zwar den Android- und Windows Phone-Standard verwenden **wieder** Schaltfläche, um zurück zur vorherigen Seite, die modale Seite auf den anderen Plattformen muss einen eigenen Mechanismus erneut bereitstellen.

### <a name="animated-page-transitions"></a>Animierte Seitenübergänge.

Alternative Versionen der verschiedenen Navigationsmethoden werden bereitgestellt, mit einer zweiten boolesches Argument, das Sie, um festlegen `true` ggf. die Seitenübergänge, um eine Animation einzuschließen:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Allerdings enthalten die standard-Seite-Navigation-Methoden die Animation in der Standardeinstellung handelt sich also nur nützlich für die Navigation zu einer bestimmten Seite beim Start (wie am Ende dieses Kapitels beschrieben) oder bei der Bereitstellung Ihres eigenen Animationstyps "entrance", (wie in beschrieben [ **Chapter22. Animation**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Visuelle und funktionale-Varianten

`NavigationPage` enthält zwei Eigenschaften, die Sie, beim Instanziieren der Klasse in festlegen können Ihrem `App` Methode:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` Außerdem umfasst vier angefügte bindbare Eigenschaften, die die jeweiligen Seite beeinflussen, auf der sie festgelegt werden:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) und [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) und [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) und [ `GetBackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) funktionieren nur unter iOS
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) und [ `GetTitleIcon` ](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) können nur iOS und Android

### <a name="exploring-the-mechanics"></a>Untersuchen der Mechanismen

Die Navigationsmethoden der Seite sind alle asynchronen und sollte verwendet werden, mit `await`. Die Vervollständigung nicht angegeben, die Seitennavigation, jedoch nur abgeschlossen wurde, die es sicher ist, überprüfen Sie die Seitennavigation Stapel ist.

Wenn eine Seite zu einer anderen navigiert, ruft die erste Seite in der Regel einen Aufruf von der [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) -Methode, und der zweiten Seite ruft einen Aufruf der [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) Methode. Wenn eine Seite in ein anderes zurückgegeben wird, ruft die erste Seite auf ähnliche Weise einen Aufruf der `OnDisappearing` -Methode, und der zweiten Seite ruft in der Regel einen Aufruf der `OnAppearing` Methode. Die Reihenfolge dieser Aufrufe (und den Abschluss der asynchronen Methoden, der die Navigation aufruft) ist plattformabhängig. Die Verwendung des Worts "Allgemein" in den zwei vorherigen Anweisungen ist aufgrund von Android modale-Seitennavigation, in denen diese-Methode wird aufgerufen, nicht.

Außerdem Aufrufe von der `OnAppearing` und `OnDisappearing` Methoden nicht unbedingt Seitennavigation.

Die `INavigation` Schnittstelle enthält zwei Auflistungseigenschaften, mit denen Sie den Navigationsstapel untersuchen können:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) Der Typ `IReadOnlyList<Page>` für das nicht modale Stapeln
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) Der Typ `IReadOnlyList<Page>` für den modalen Stapel

Ist es am sichersten, diese Aufruflisten von Zugriff auf die `Navigation` Eigenschaft der `NavigationPage` (sein sollten die `App` -Klasse [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) Eigenschaft). Es ist nur sicher, diese Aufruflisten untersuchen, nachdem die asynchrone Seite-Navigation-Methoden abgeschlossen haben. Die [ `CurrentPage` ](xref:Xamarin.Forms.NavigationPage.CurrentPage) Eigenschaft der `NavigationPage` gibt nicht an die aktuelle Seite ist die aktuelle Seite eine modale Seite, jedoch gibt an, stattdessen die letzte nicht modale Seite.

Die [ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) Beispiel können Sie die Seitennavigation und die Stapel und die zulässigen Typen von Seitennavigation erkunden:

- Eine modale Seite kann zu einer anderen nicht modale Seite oder eine modale Seite navigieren.
- Eine modale Seite kann nur zu einer anderen modale Seite navigieren.

### <a name="enforcing-modality"></a>Erzwingen der Modalität

Eine Anwendung verwendet eine modale Seite aus, wenn einige Informationen aus der Benutzer abgerufen werden muss. Der Benutzer muss unterbunden werden, zur vorherigen Seite zurückgegeben werden, bis die Informationen bereitgestellt wird. Unter iOS, es ist einfach, geben Sie einen **wieder** Schaltfläche, und aktivieren Sie sie nur, wenn der Benutzer mit der Seite abgeschlossen hat. Aber für Android und Windows Phone-Geräte, die Anwendung außer Kraft setzen sollten die [ `OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) Methode, und geben `true` wenn Programm behandelt hat die **wieder** Schaltfläche selbst, wie in die [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) Beispiel.

Die [ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) Beispiel wird veranschaulicht, wie dies in einem Szenario mit MVVM funktioniert.

## <a name="navigation-variations"></a>Navigation-Varianten

Wenn eine bestimmte modale Seite mehrere Male navigiert werden kann, sollte es Informationen beibehalten, so, dass der Benutzer eingeben, anstatt die Informationen bearbeiten kann sich im erneut aus. Sie können dies durch Beibehalten der bestimmten Instanz einer der modale Seite behandeln, aber ein besserer Ansatz (insbesondere für iOS) wird gleichzeitig die Informationen in einem View Model.

### <a name="making-a-navigation-menu"></a>Machen ein Navigationsmenü

Die [ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) Beispiel zeigt die Verwendung einer `TableView` beim Auflisten von Elementen im Menü. Jedes Element zugeordnet ist eine `Type` -Objekt für eine bestimmte Seite. Wenn dieses Element ausgewählt ist, wird die Anwendung instanziiert die Seite und navigiert zu ihr.

[![Dreifacher Screenshot der Ansicht Sammlungstyp](images/ch24fg21-small.png "TableView auflisten Menüelemente")](images/ch24fg21-large.png#lightbox "TableView Auflisten von Menüelementen")

Die [ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) Beispiel ist ein wenig anders, klicken Sie im Menü auf Instanzen von jeder Seite verwenden, statt die Typen enthält. Dadurch können die Informationen aus den einzelnen Seiten beibehalten, aber alle Seiten müssen beim Programmstart instanziiert werden.

### <a name="manipulating-the-navigation-stack"></a>Bearbeiten von dem Navigationsstapel

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) veranschaulicht mehrere Funktionen, die durch definierten `INavigation` , mit denen Sie den Navigationsstapel auf strukturierte Weise ändern:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) und [ `PopToRootAsync` ](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) mit optionalen Animation

### <a name="dynamic-page-generation"></a>Dynamische Seite generation

Die [ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) Beispiel veranschaulicht das Erstellen eines von einer Seite zur Laufzeit auf Grundlage der Benutzereingabe.

## <a name="patterns-of-data-transfer"></a>Muster der zu übertragenden Daten

Es ist häufig erforderlich, um Daten zwischen Seiten freizugeben &mdash; zum Übertragen von Daten zu einer Seite navigiert, und klicken Sie für eine Seite, um Daten zur Seite zurückzukehren, die sie aufgerufen hat. Es gibt verschiedene Techniken, um dieses Ziel erreichen.

### <a name="constructor-arguments"></a>Konstruktorargumente

Wenn Sie zu einer neuen Seite zu navigieren, ist es möglich, die Page-Klasse mit einem Konstruktorargument zu instanziieren, die auf der Seite, um sich selbst zu initialisieren können. Die [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) veranschaulicht dies. Es ist auch möglich, für die Seite navigiert, damit die `BindingContext` festlegen, indem die Seite, zu der sie navigiert.

### <a name="properties-and-method-calls"></a>Eigenschaften und Methodenaufrufe

Die übrigen Daten übertragungsbeispiele untersuchen das Problem für die Übergabe von Informationen zwischen Seiten, wenn eine Seite zu einer anderen Seite navigiert und wieder zurück. In diesen Diskussionen die *home* Seite navigiert zu der *Informationen* Seite und müssen die initialisierten Informationen zum Übertragen der *Informationen* Seite. Die *Informationen* Seite Ruft zusätzliche Informationen vom Benutzer ab und überträgt die Informationen zu den *home* Seite.

Die *home* Seite kann ganz einfach Zugriff auf öffentliche Methoden und Eigenschaften in der *Informationen* Seite, sobald es auf dieser Seite instanziiert. Die *Informationen* Seite kann auch Zugriff auf öffentliche Methoden und Eigenschaften in der *home* Seite, jedoch einen guten Zeitpunkt auswählen, für die dies schwierig sein kann. Die [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) Beispiel hierfür die `OnDisappearing` außer Kraft setzen. Ein Nachteil besteht darin, die die *Informationen* Seite muss wissen, welche die *home* Seite.

### <a name="messagingcenter"></a>MessagingCenter

Die Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse bietet eine weitere Möglichkeit, zwei Seiten miteinander kommunizieren. Nachrichten werden durch eine Zeichenfolge identifiziert und können von jedem Objekt begleitet werden.

Ein Programm, das Empfangen von Nachrichten von einem bestimmten Typ möchte muss abonniert werden mithilfe von [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) , und geben Sie eine Callback-Funktion. Sie können später kündigen, indem Aufrufen [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*). Die Callback-Funktion empfängt alle Nachrichten aus dem angegebenen Typ mit dem angegebenen Namen, die durch gesendete versendet die [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Methode.

Die [ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) Programm veranschaulicht, wie zum Übertragen von Daten, die über das messaging Center, aber es dies erfordert, dass die *Informationen* Seite bekannt sein, die die *home* Seite.

### <a name="events"></a>Ereignisse

Das Ereignis ist ein seit langem bewährten Ansatz für eine Klasse, Informationen zu einer anderen Klasse zu senden, ohne den Klasse Typ zu kennen. In der [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) Beispiel der *Informationen* -Klasse definiert ein Ereignis, das es ausgelöst wird, wenn die Informationen bereit ist. Allerdings besteht keine praktische Möglichkeit für die *home* Seite, um den Ereignishandler zu trennen.

### <a name="the-app-class-intermediary"></a>Der Vermittler der App-Klasse

Die [ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) Beispiel zeigt, wie in definierte Eigenschaften den Zugriff auf die `App` Klasse sowohl die *home* Seite und die *Informationen*Seite. Dies ist eine gute Lösung, aber im nächste Abschnitt wird beschrieben, etwas besser.

### <a name="switching-to-a-viewmodel"></a>Um ein "ViewModel" wechseln

Mithilfe ein "ViewModels" aus, für die Informationen kann die *home* Seite und die *Informationen* Seite, um die Instanz der Informationsklasse gemeinsam nutzen. Dies wird veranschaulicht, der [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) Beispiel.

### <a name="saving-and-restoring-page-state"></a>Speichern und Wiederherstellen des seitenspezifischen Status

Die `App` Klasse Vermittler oder der ViewModel-Ansatz ist ideal, wenn die Anwendung Informationen speichern muss, wenn die Anwendung wechselt in den Ruhezustand während der *Informationen* Seite aktiv ist. Die [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) veranschaulicht dies.

## <a name="saving-and-restoring-the-navigation-stack"></a>Speichern und Wiederherstellen von dem Navigationsstapel

Im Allgemeinen sollte ein mehrseitigen Programm, das in den Ruhezustand versetzt zur selben Seite navigieren, wenn sie wiederhergestellt wird. Dies bedeutet, dass ein solches Programm auf den Inhalt im Navigationsstapel speichern soll. In diesem Abschnitt wird die Automatisierung dieses Prozesses in einer Klasse, die für diesen Zweck entwickelten veranschaulicht. Diese Klasse ruft auch die einzelnen Seiten, um ihnen das Speichern und Wiederherstellen ihrer Seitenzustand gewähren.

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Library definiert eine Schnittstelle, die mit dem Namen [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) , dass Klassen implementiert werden können, um zu speichern und Wiederherstellen von Elementen in der `Properties`Wörterbuch.

Die [ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Bibliothek leitet sich von `Application`. Sie können dann ableiten, Ihre `App` -Klasse aus `MultiPageRestorableApp` und eine Überprüfung ausführen.

Die [ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) veranschaulicht die Verwendung von `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Etwa eine realen app

Die [ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) verwendet auch Beispiel `MultiPageRestorableApp` und für die Eingabe und Bearbeitung von Anmerkungen zu dieser Version, die in gespeichert werden können die `Properties` Wörterbuch.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 24 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Kapitel 24-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Modale Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md)
