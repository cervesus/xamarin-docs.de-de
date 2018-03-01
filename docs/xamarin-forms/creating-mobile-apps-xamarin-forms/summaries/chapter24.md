---
title: Zusammenfassung der Kapitel 24. Seitennavigation
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b8eac45c52093dea23c08a19d219fa0bbd8d55ab
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>Zusammenfassung der Kapitel 24. Seitennavigation

Viele Anwendungen bestehen aus mehreren Seiten, zwischen, denen der Benutzer navigiert. Die Anwendung immer eine *main* Seite oder *home* Seite und von dort aus der Benutzer navigiert zu anderen Seiten, die in einem Stapel für die Navigation zurück verwaltet werden. Finden Sie zusätzliche Navigationsoptionen [ **Chapter 25. Seite Varianten**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Modale und nicht modale Seiten

`VisualElement` definiert eine [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft vom Typ [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), darunter die folgenden beiden Methoden zum Navigieren zu einer neuen Seite:

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

Beide Methoden akzeptieren ein `Page` Instanz als ein Argument und der Rückgabewert eine `Task` Objekt. Die folgenden beiden Methoden navigieren Sie zurück zur vorherigen Seite:

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

Wenn die Benutzeroberfläche einen eigenen hat **wieder** Schaltfläche (wie Android- und Windows-Smartphones ausgeführt werden), ist es nicht notwendig, für die Anwendung auf diese Methoden aufrufen.

Obwohl diese Methoden von einem beliebigen verfügbar sind `VisualElement`, sie werden in der Regel aufgerufen, aus der `Navigation` Eigenschaft des aktuellen `Page` Instanz.

Anwendungen verwenden im Allgemeinen modale Seiten, wenn der Benutzer erforderlich ist, um einige Informationen auf der Seite vor der Rückgabe zur vorherigen Seite bereitzustellen. Seiten, die nicht gebunden sind, werden manchmal als nicht modalen bezeichnet oder *hierarchische*. "Nothing" in die Seite selbst unterscheidet es als gebunden oder ungebunden; Es wird stattdessen die Methode verwendet, um ihn zu unterliegen. Um auf allen Plattformen zu arbeiten, muss eine modale Seite die eigenen Benutzeroberfläche bereitstellen, für die Navigation zurück zur vorherigen Seite.

Die [ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) Beispiel können Sie den Unterschied zwischen Seiten nicht modalen und modale untersuchen. Jede Anwendung, für die Seitennavigation muss die Startseite zum Übergeben der [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) -Konstruktor in der Regel in der Anwendung `App` Klasse. Ein Vorteil ist, dass Sie nicht mehr benötigen, Festlegen einer `Padding` auf der Seite für iOS.

Sie werden feststellen, dass für nicht modalen Seiten, die Seite des [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) Eigenschaft wird angezeigt. Geben Sie ein Element der Benutzeroberfläche wieder zur vorherigen Seite navigieren, iOS, Android und die Windows-Tablet und PC-Plattformen. Der Kurs, Android und Windows Phone-Geräte haben einen Standard **wieder** Schaltfläche, um zurückzukehren.

Für modale Seiten, die Seite `Title` nicht angezeigt wird, und kein Element der Benutzeroberfläche wird bereitgestellt, um zur vorherigen Seite zurückzukehren. Obwohl Sie mit den Android- und Windows Phone-Standard **wieder** Schaltfläche, um zur vorherigen Seite zurückzukehren modale Seite auf anderen Plattformen muss einen eigenen Mechanismus zurückdatieren angeben.

### <a name="animated-page-transitions"></a>Animierte Seitenübergänge

Alternative Versionen verschiedene Navigationsmethoden mit einer zweiten boolesches Argument, mit denen sich zur Verfügung `true` ggf. auf den Seitenübergang eine Animation einschließen:

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

Allerdings enthalten die Standardnavigation Seite Methoden die Animation wird standardmäßig, damit diese nur für die Navigation zu einer bestimmten Seite beim Start (wie gegen Ende der in diesem Kapitel beschriebenen) wertvoller sind oder beim Eingang Animation bereitstellen (wie in beschrieben [ **Chapter22. Animation**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Visuelle und funktionale Varianten

`NavigationPage` enthält zwei Eigenschaften, die Sie, beim Instanziieren der Klasse in festlegen können Ihrem `App` Methode:

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` Außerdem enthält vier angefügte bindbare Eigenschaften, die bestimmte Seite betreffen, auf der sie festgelegt wurden:

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) und [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) und [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) und [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) funktioniert nur iOS
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) und [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) auf IOS- und Android nur Geschäfts-

### <a name="exploring-the-mechanics"></a>Untersuchen der Mechanismen

Die Navigationsmethoden für die Seite sind alle asynchronen und sollte verwendet werden, mit `await`. Die Beendigung anzugeben nicht, die Seitennavigation, jedoch nur abgeschlossen ist, die Seite Navigationsstapel prüfen sicher ist.

Wenn eine Seite zu einem anderen navigiert wird, ruft die erste Seite in der Regel einen Aufruf von seiner [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) -Methode und die zweite Seite ruft einen Aufruf von seiner [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) Methode. Wenn eine Seite in einen anderen Wert zurückgibt, ruft die erste Seite auf ähnliche Weise einen Aufruf der `OnDisappearing` -Methode und die zweite Seite ruft in der Regel auf einen Aufruf von seiner `OnAppearing` Methode. Die Reihenfolge dieser Aufrufe (und den Abschluss der asynchronen Methoden, der die Navigation aufruft) ist plattformabhängig. Die Verwendung des Worts "Allgemein" in den beiden vorangehenden Anweisungen ist aufgrund von Android modale Navigationsmodell, in dem diese Methodenaufrufe treten nicht.

Darüber hinaus Aufrufe von der `OnAppearing` und `OnDisappearing` Methoden nicht unbedingt Seitennavigation.

Die `INavigation` Schnittstelle enthält zwei Sammlungseigenschaften, mit denen Navigationsstapel untersuchen können:

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) Der Typ `IReadOnlyList<Page>` für den Stapel nicht modal
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) Der Typ `IReadOnlyList<Page>` für modale Stapel

Ist es am sichersten, diese Aufruflisten von Zugriff auf die `Navigation` Eigenschaft der `NavigationPage` (sein sollten die `App` Klasse [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) Eigenschaft). Es ist nur sicher, diese Aufruflisten untersuchen, nachdem die asynchronen Seitennavigation Methoden abgeschlossen haben. Die [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) Eigenschaft von der `NavigationPage` nicht an die aktuelle Seite ist die aktuelle Seite eine modale Seite aber gibt an, stattdessen die letzte nicht modalen Seite.

Die [ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) -Beispiel können Sie die Seitennavigation und die Stapel und die zulässigen Typen von Seitennavigation untersuchen:

- Eine nicht modale Seite kann zu einem anderen nicht modale Seite oder ein modales navigieren.
- Eine modale Seite kann nur zu einer anderen modalen Seite navigieren.

### <a name="enforcing-modality"></a>Das Durchsetzen Modalität

Eine Anwendung verwendet eine modale Seite bei Bedarf einige Informationen vom Benutzer abgerufen. Der Benutzer muss unterbunden werden, zur vorherigen Seite zurückgeben, bis diese Informationen bereitgestellt wird. Bei iOS kann es ist einfach, geben Sie eine **wieder** Schaltfläche aus, und aktivieren Sie ihn mit der Option nur, wenn der Benutzer mit der Seite abgeschlossen ist. Für Android und Windows Phone-Geräte, sollte die Anwendung Überschreibung ist jedoch die [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) -Methode und der Rückgabewert `true` wenn Programm behandelt hat die **wieder** Schaltfläche selbst, wie in die [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) Beispiel.

Die [ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) Beispiel wird veranschaulicht, wie dies in einem Szenario mit MVVM funktioniert.

## <a name="navigation-variations"></a>Navigation Varianten

Wenn eine bestimmte modale Seite zu mehrmals navigiert werden kann, sollte es Informationen beibehalten, damit der Benutzer die Informationen, anstatt ihn einzugeben bearbeiten kann sich im erneut aus. Sie können dies durch Beibehalten der bestimmten Instanz der modale Seite behandeln, jedoch ein besserer Ansatz (insbesondere für iOS) ist gleichzeitig die Informationen in einem Ansichtsmodell.

### <a name="making-a-navigation-menu"></a>Erstellen eines Navigationsmenüs

Die [ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) Beispiel veranschaulicht die Verwendung einer `TableView` auf Menüelemente Liste. Jedes Element zugeordnet ist ein `Type` Objekt für eine bestimmte Seite. Wenn dieses Element ausgewählt ist, wird das Programm Seite instanziiert und darauf navigiert.

[![Dreifacher Screenshot der Katalog Ansichtstyp](images/ch24fg21-small.png "TableView Auflisten von Menüelementen")](images/ch24fg21-large.png "TableView Auflisten von Menüelementen")

Die [ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) Beispiel unterscheidet sich insofern, dass Sie im Menü Instanzen von jeder Seite statt mit Typen enthält. Dadurch werden die Informationen über jede Seite beibehalten, aber alle Seiten müssen beim Programmstart instanziiert werden.

### <a name="manipulating-the-navigation-stack"></a>Bearbeiten von Navigationsstapel

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) veranschaulicht verschiedene Funktionen definiert, indem `INavigation` , mit deren Hilfe Sie die Navigationsstapel strukturierten bearbeiten:

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) und [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) mit optionalen Animation

### <a name="dynamic-page-generation"></a>Dynamische Seite generation

Die [ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) Beispiel veranschaulicht das Erstellen eines einer Seite zur Laufzeit basierend auf Benutzereingaben.

## <a name="patterns-of-data-transfer"></a>Muster für die Datenübertragung

Häufig ist es erforderlich, um Daten zwischen Seiten & #x 2014 freizugeben. Übertragen von Daten zu einer Seite Navigation und für eine Seite, um Daten zur Seite zurückzukehren, die sie aufgerufen hat. Es gibt mehrere Verfahren dazu.

### <a name="constructor-arguments"></a>Konstruktorargumente

Beim Navigieren zu einer neuen Seite, ist es möglich, die Page-Klasse mit einem Konstruktorargument zu instanziieren, das können die Seite, um sich selbst zu initialisieren. Die [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) Beispiel veranschaulicht dies. Es ist auch möglich, für die Navigation Seite haben seine `BindingContext` festlegen, indem die Seite, zu der sie navigiert.

### <a name="properties-and-method-calls"></a>Eigenschaften und Methodenaufrufe

Die verbleibenden Daten übertragungsbeispiele untersuchen das Problem der Übergabe von Informationen zwischen Seiten, wenn eine Seite zu einer anderen Seite navigiert und wieder zurück. In diesen Diskussionen der *home* Seite navigiert zu der *Info* Seite muss zu übertragen und initialisierten Informationen zu den *Info* Seite. Die *Info* Seite zusätzliche Plattforminformationen des Benutzers und werden die Informationen zum Übertragen der *home* Seite.

Die *home* Seite kann problemlos Zugriff auf öffentliche Methoden und Eigenschaften in der *Info* Seite als Seite instanziiert. Die *Info* Seite kann auch Zugriff auf öffentliche Methoden und Eigenschaften in der *home* Seite, jedoch einen guten Zeitpunkt auswählen, für die dies als schwierig sein kann. Die [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) Beispiel ruft zu diesem seiner `OnDisappearing` außer Kraft setzen. Ein Nachteil ist, die die *Info* Seite muss wissen, welche die *home* Seite.

### <a name="messagingcenter"></a>MessagingCenter

Der Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) -Klasse bietet eine andere Möglichkeit für zwei Seiten miteinander kommunizieren. Nachrichten werden durch eine Textzeichenfolge identifiziert und können jedes Objekt begleitet werden.

Ein Programm, das zum Empfangen von Nachrichten von einem bestimmten Typ möchte müssen sie abonnieren mit [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) , und geben Sie eine Rückruffunktion. Sie können später kündigen, durch den Aufruf [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/). Die Rückruffunktion erhält jede Nachricht, die über den angegebenen Typ mit dem angegebenen Namen gesendeter gesendet der [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Methode.

Die [ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) Programm veranschaulicht, wie zum Übertragen von Daten über das messaging Center jedoch erneut dies erfordert, dass die *Info* Seite den Typ der der kennen*home* Seite.

### <a name="events"></a>Ereignisse

Das Ereignis ist ein Zeitmanagement Ansatz für eine Klasse, Informationen zu einer anderen Klasse zu senden, ohne diese Klasse Typ zu kennen. In der [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) Beispiel der *Info* Klasse definiert ein Ereignis, das es ausgelöst wird, wenn die Informationen bereit ist. Es ist jedoch keine einfache Möglichkeit für die *home* Seite, um den Ereignishandler zu trennen.

### <a name="the-app-class-intermediary"></a>Die Vermittler die App-Klasse

Die [ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) Beispiel zeigt, wie in definierte Eigenschaften den Zugriff auf die `App` Klasse sowohl die *home* Seite und die *Info*Seite. Dies ist eine gute Lösung, aber im nächsten Abschnitt beschrieben etwas besser.

### <a name="switching-to-a-viewmodel"></a>Wechsel zu einem ViewModel

Ein ViewModel für diesen Informationen können von der *home* Seite und die *Info* Seite, um die Instanz der Informationsklasse gemeinsam nutzen. Dies wird dargestellt, der [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) Beispiel.

### <a name="saving-and-restoring-page-state"></a>Speichern und Wiederherstellen des Status der Seite "

Die `App` Klasse Durchgangs- oder das ViewModel-Ansatz ist ideal, wenn die Anwendung Informationen speichern muss, wenn das Programm in den Ruhezustand wechselt während der *Info* Seite aktiv ist. Die [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) Beispiel veranschaulicht dies.

## <a name="saving-and-restoring-the-navigation-stack"></a>Speichern und Wiederherstellen von Navigationsstapel

Im allgemeinen Fall sollte einer mehrseitigen Programm, das in den Ruhezustand versetzt zur selben Seite navigieren, wenn sie wiederhergestellt wird. Dies bedeutet, dass ein solches Programm auf den Inhalt der Navigationsstapel speichern soll. In diesem Abschnitt wird die Automatisierung dieses Prozesses in einer Klasse, die für diesen Zweck entwickelten veranschaulicht. Diese Klasse ruft auch die einzelnen Seiten, um ihnen das Speichern und Wiederherstellen von ihrer Seite Status gewähren.

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Library definiert eine Schnittstelle mit dem Namen [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) , dass Klassen implementiert werden kann, um zu speichern und Wiederherstellen von Elementen in der `Properties`Wörterbuch.

Die [ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Bibliothek leitet sich von `Application`. Sie können dann ableiten, Ihre `App` -Klasse aus `MultiPageRestorableApp` und einige Wartungsaufgaben ausführen.

Die [ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) veranschaulicht die Verwendung von `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Ein Element wie eine realen-app

Die [ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) Beispiel auch binärencoder `MultiPageRestorableApp` und ermöglicht das eingeben und Bearbeiten von Notizen, die in gespeichert werden die `Properties` Wörterbuch.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 24 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Kapitel 24-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Modale Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md)
