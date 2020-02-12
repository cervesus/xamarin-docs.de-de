---
title: Zusammenfassung der Kapitel 24. Seitennavigation
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 24. Seitennavigation'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130837"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Zusammenfassung der Kapitel 24. Seitennavigation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

Viele Anwendungen bestehen aus mehreren Seiten, die für die der Benutzer navigiert. Die Anwendung verfügt immer über eine *Haupt* Seite oder *Start* Seite, und von dort aus navigiert der Benutzer zu anderen Seiten, die in einem Stapel für die Navigation aufbewahrt werden. Weitere Navigationsoptionen werden in [**Kapitel 25 behandelt. Seiten Varianten**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Modale und nicht modale Seiten

`VisualElement` definiert eine [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) Eigenschaft des Typs [`INavigation`](xref:Xamarin.Forms.INavigation), die die folgenden beiden Methoden zum Navigieren zu einer neuen Seite enthält:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Beide Methoden akzeptieren eine `Page` Instanz als Argument und geben ein `Task`-Objekt zurück. Die folgenden beiden Methoden navigieren Sie zurück zur vorherigen Seite:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Wenn die Benutzeroberfläche über eine eigene Schaltfläche **zurück** verfügt (wie Android-und Windows-Telefone), ist es für die Anwendung nicht erforderlich, diese Methoden aufzurufen.

Obwohl diese Methoden von allen `VisualElement`verfügbar sind, werden Sie in der Regel von der `Navigation`-Eigenschaft der aktuellen `Page` Instanz aufgerufen.

Anwendungen verwenden im Allgemeinen modale Seiten auf, wenn der Benutzer erforderlich ist, einige Informationen auf der Seite vor der Rückkehr zur vorherigen Seite angeben. Nicht modale Seiten werden manchmal als "nicht modales" oder " *hierarchisch*" bezeichnet. "Nothing" in die Seite selbst unterscheidet es als modales oder nicht modales; Es wird stattdessen von der Methode verwendet, um ihn zu gesteuert. Um auf allen Plattformen arbeiten zu können, muss eine modale Seite eine eigene Benutzeroberfläche für Benutzer bereitstellen, für die Navigation zurück zur vorherigen Seite.

Mit dem [**modelessandmodal**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) -Beispiel können Sie den Unterschied zwischen nicht modalen und modalen Seiten untersuchen. Jede Anwendung, die die Seitennavigation verwendet, muss die zugehörige Startseite an den [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) -Konstruktor übergeben, in der Regel in der `App` Klasse des Programms. Ein Vorteil besteht darin, dass Sie auf der Seite für IOS keinen `Padding` mehr festlegen müssen.

Sie werden feststellen, dass die [`Title`](xref:Xamarin.Forms.Page.Title) -Eigenschaft der Seite für nicht modlose Seiten angezeigt wird. Geben Sie ein Element der Benutzeroberfläche zurück zur vorherigen Seite navigieren, iOS, Android und die Windows-Tablet und Desktop-Plattformen. Natürlich verfügen Android-und Windows Phone-Geräte über eine Standard- **zurück** -Schaltfläche, um zurückzukehren.

Bei modalen Seiten wird die Seite `Title` nicht angezeigt, und es wird kein Benutzeroberflächen Element bereitgestellt, um zur vorherigen Seite zurückzukehren. Obwohl Sie die Schaltfläche " **zurück** " für Android und Windows Phone Standard verwenden können, um zur vorherigen Seite zurückzukehren, muss die modale Seite auf den anderen Plattformen einen eigenen Mechanismus bereitstellen, um zurückzukehren.

### <a name="animated-page-transitions"></a>Animierte Seitenübergänge.

Alternative Versionen der verschiedenen Navigationsmethoden werden mit einem zweiten booleschen Argument bereitgestellt, das Sie auf `true` festlegen, wenn der Seiten Übergang eine Animation enthalten soll:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [Pushmodalasync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [Popasync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [Popmodalasync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Die standardmäßigen Seiten Navigationsmethoden enthalten jedoch standardmäßig die Animation, sodass diese nur für das Navigieren zu einer bestimmten Seite beim Start (wie im Anschluss an das Ende dieses Kapitels erläutert) oder für die Bereitstellung Ihrer eigenen Eingangs Animation (wie unter Chapter22 erläutert) nützlich sind [ **. Animation**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Visuelle und funktionale-Varianten

`NavigationPage` enthält zwei Eigenschaften, die Sie festlegen können, wenn Sie die-Klasse in ihrer `App`-Methode instanziieren:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` umfasst auch vier angefügte bindbare Eigenschaften, die sich auf die jeweilige Seite auswirken, auf der Sie festgelegt sind:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) und [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) und [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) und [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) arbeiten nur unter IOS
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) und [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) arbeiten nur unter IOS und Android

### <a name="exploring-the-mechanics"></a>Untersuchen der Mechanismen

Die Seiten Navigationsmethoden sind asynchron und sollten mit `await`verwendet werden. Die Vervollständigung nicht angegeben, die Seitennavigation, jedoch nur abgeschlossen wurde, die es sicher ist, überprüfen Sie die Seitennavigation Stapel ist.

Wenn eine Seite zu einer anderen navigiert, ruft die erste Seite in der Regel einen aufzurufenden [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) -Methode auf, und die zweite Seite erhält einen aufzurufenden [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) Methode. Wenn eine Seite an eine andere Seite zurückgegeben wird, ruft die erste Seite einen aufzurufenden `OnDisappearing`-Methode ab, und die zweite Seite ruft in der Regel einen aufzurufenden `OnAppearing` Methode auf. Die Reihenfolge dieser Aufrufe (und den Abschluss der asynchronen Methoden, der die Navigation aufruft) ist plattformabhängig. Die Verwendung des Worts "Allgemein" in den zwei vorherigen Anweisungen ist aufgrund von Android modale-Seitennavigation, in denen diese-Methode wird aufgerufen, nicht.

Außerdem geben Aufrufe der `OnAppearing`-und `OnDisappearing`-Methode nicht notwendigerweise die Seitennavigation an.

Die `INavigation`-Schnittstelle enthält zwei Sammlungs Eigenschaften, mit denen Sie den Navigations Stapel untersuchen können:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) vom Typ `IReadOnlyList<Page>` für den nicht modalem Stapel
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) vom Typ `IReadOnlyList<Page>` für den modalen Stapel

Es ist am sichersten, dass Sie über die `Navigation`-Eigenschaft des `NavigationPage` auf diese Stapel zugreifen (bei der es sich um die [`MainPage`](xref:Xamarin.Forms.Application.MainPage) -Eigenschaft der `App` Klasse handeln sollte). Es ist nur sicher, diese Aufruflisten untersuchen, nachdem die asynchrone Seite-Navigation-Methoden abgeschlossen haben. Die [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) -Eigenschaft des `NavigationPage` gibt die aktuelle Seite nicht an, wenn die aktuelle Seite eine modale Seite ist, sondern stattdessen die letzte nicht modale Seite angibt.

Mit dem Beispiel " [**singlepagenavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) " können Sie die Seitennavigation und die Stapel sowie die rechtlichen Typen der Seitennavigation untersuchen:

- Eine modale Seite kann zu einer anderen nicht modale Seite oder eine modale Seite navigieren.
- Eine modale Seite kann nur zu einer anderen modale Seite navigieren.

### <a name="enforcing-modality"></a>Erzwingen der Modalität

Eine Anwendung verwendet eine modale Seite aus, wenn einige Informationen aus der Benutzer abgerufen werden muss. Der Benutzer muss unterbunden werden, zur vorherigen Seite zurückgegeben werden, bis die Informationen bereitgestellt wird. Unter IOS ist es einfach, eine Schaltfläche " **zurück** " bereitzustellen und Sie nur dann zu aktivieren, wenn der Benutzer die Seite beendet hat. Bei Android-und Windows Phone-Geräten sollte die Anwendung jedoch die [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) -Methode überschreiben und `true` zurückgeben, wenn das Programm die Schaltfläche " **zurück** " selbst verarbeitet hat, wie im [**modalenforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) -Beispiel gezeigt.

Das [**mvvmenforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) -Beispiel veranschaulicht, wie dies in einem MVVM-Szenario funktioniert.

## <a name="navigation-variations"></a>Navigation-Varianten

Wenn eine bestimmte modale Seite mehrere Male navigiert werden kann, sollte es Informationen beibehalten, so, dass der Benutzer eingeben, anstatt die Informationen bearbeiten kann sich im erneut aus. Sie können dies durch Beibehalten der bestimmten Instanz einer der modale Seite behandeln, aber ein besserer Ansatz (insbesondere für iOS) wird gleichzeitig die Informationen in einem View Model.

### <a name="making-a-navigation-menu"></a>Machen ein Navigationsmenü

Das [**viewgallerytype**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) -Beispiel veranschaulicht die Verwendung einer `TableView` zum Auflisten von Menü Elementen. Jedes Element ist einem `Type`-Objekt für eine bestimmte Seite zugeordnet. Wenn dieses Element ausgewählt ist, wird die Anwendung instanziiert die Seite und navigiert zu ihr.

[![Dreifacher Screenshot des Ansichts Galerie Typs](images/ch24fg21-small.png "Menü Elemente in TableView auflisten")](images/ch24fg21-large.png#lightbox "Menü Elemente in TableView auflisten")

Das [**viewgalleryinst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) -Beispiel ist ein wenig anders, da das Menü anstelle von Typen Instanzen der einzelnen Seiten enthält. Dadurch können die Informationen aus den einzelnen Seiten beibehalten, aber alle Seiten müssen beim Programmstart instanziiert werden.

### <a name="manipulating-the-navigation-stack"></a>Bearbeiten von dem Navigationsstapel

[**Stackmanipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) veranschaulicht verschiedene Funktionen, die von `INavigation` definiert werden, mit denen Sie den Navigations Stapel auf strukturierte Weise bearbeiten können:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) und [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) mit optionaler Animation

### <a name="dynamic-page-generation"></a>Dynamische Seite generation

Das [**buildapage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) -Beispiel veranschaulicht das Erstellen einer Seite zur Laufzeit basierend auf Benutzereingaben.

## <a name="patterns-of-data-transfer"></a>Muster der zu übertragenden Daten

Es ist häufig erforderlich, Daten zwischen Seiten zu teilen &mdash; um Daten auf eine navigiert Seite zu übertragen, und damit eine Seite Daten an die Seite zurückgibt, die Sie aufgerufen hat. Es gibt verschiedene Techniken, um dieses Ziel erreichen.

### <a name="constructor-arguments"></a>Konstruktorargumente

Wenn Sie zu einer neuen Seite zu navigieren, ist es möglich, die Page-Klasse mit einem Konstruktorargument zu instanziieren, die auf der Seite, um sich selbst zu initialisieren können. Dies wird im Beispiel " [**schoolandstudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) " veranschaulicht. Es ist auch möglich, dass auf der Seite navigiert die `BindingContext` von der Seite festgelegt wird, die zu der Seite navigiert.

### <a name="properties-and-method-calls"></a>Eigenschaften und Methodenaufrufe

Die übrigen Daten übertragungsbeispiele untersuchen das Problem für die Übergabe von Informationen zwischen Seiten, wenn eine Seite zu einer anderen Seite navigiert und wieder zurück. In diesen Diskussionen navigiert die *Start* Seite zur *Infoseite* und muss initialisierte Informationen auf die *Infoseite* übertragen. Die Seite *Info* erhält zusätzliche Informationen vom Benutzer und überträgt die Informationen auf die *Start* Seite.

Die *Start* Seite kann problemlos auf die öffentlichen Methoden und Eigenschaften auf der Seite " *Info* " zugreifen, sobald Sie diese Seite instanziiert. Auf der Seite " *Info* " können Sie auch auf öffentliche Methoden und Eigenschaften auf der *Start* Seite zugreifen, aber es kann schwierig sein, einen guten Zeitpunkt dafür zu wählen. Im [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) -Beispiel wird dies in der `OnDisappearing` Überschreibung durchführt. Ein Nachteil ist, dass die *Infoseite* den Typ der *Start* Seite kennen muss.

### <a name="messagingcenter"></a>MessagingCenter

Die xamarin. Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) -Klasse stellt eine andere Möglichkeit für zwei Seiten dar, miteinander zu kommunizieren. Nachrichten werden durch eine Zeichenfolge identifiziert und können von jedem Objekt begleitet werden.

Ein Programm, das Nachrichten von einem bestimmten Typ empfangen möchte, muss Sie mithilfe von [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) abonnieren und eine Rückruffunktion angeben. Später kann ein Abonnement durch Aufrufen von [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)gekündigt werden. Die Rückruffunktion empfängt jede vom angegebenen Typ gesendete Nachricht mit dem angegebenen Namen, der durch die [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) -Methode gesendet wird.

Das [**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) -Programm zeigt, wie Daten mithilfe des Messaging Centers übertragen werden. Dies erfordert jedoch wieder, dass die *Infoseite* den Typ der *Start* Seite kennt.

### <a name="events"></a>Events

Das Ereignis ist ein seit langem bewährten Ansatz für eine Klasse, Informationen zu einer anderen Klasse zu senden, ohne den Klasse Typ zu kennen. Im [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) -Beispiel definiert die *Info* -Klasse ein Ereignis, das ausgelöst wird, wenn die Informationen bereit sind. Es gibt jedoch keinen geeigneten Speicherort für die *Start* Seite, um den Ereignishandler zu trennen.

### <a name="the-app-class-intermediary"></a>Der Vermittler der App-Klasse

Das [**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) -Beispiel zeigt, wie Sie auf Eigenschaften zugreifen können, die in der `App`-Klasse sowohl auf der *Start* Seite als auch auf der Seite *Info* definiert sind Dies ist eine gute Lösung, aber im nächste Abschnitt wird beschrieben, etwas besser.

### <a name="switching-to-a-viewmodel"></a>Um ein "ViewModel" wechseln

Wenn Sie ein ViewModel für die Informationen verwenden, können auf der *Start* Seite und der *Infoseite* die Instanz der Informations Klasse gemeinsam genutzt werden. Dies wird im [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) -Beispiel veranschaulicht.

### <a name="saving-and-restoring-page-state"></a>Speichern und Wiederherstellen des seitenspezifischen Status

Der `App`-Klassen Vermittler oder der ViewModel-Ansatz ist ideal, wenn die Anwendung Informationen speichern muss, wenn das Programm in den Standbymodus wechselt, während die *Infoseite* aktiv ist. Dies wird im [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) -Beispiel veranschaulicht.

## <a name="saving-and-restoring-the-navigation-stack"></a>Speichern und Wiederherstellen von dem Navigationsstapel

Im Allgemeinen sollte ein mehrseitigen Programm, das in den Ruhezustand versetzt zur selben Seite navigieren, wenn sie wiederhergestellt wird. Dies bedeutet, dass ein solches Programm auf den Inhalt im Navigationsstapel speichern soll. In diesem Abschnitt wird die Automatisierung dieses Prozesses in einer Klasse, die für diesen Zweck entwickelten veranschaulicht. Diese Klasse ruft auch die einzelnen Seiten, um ihnen das Speichern und Wiederherstellen ihrer Seitenzustand gewähren.

Die [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek definiert eine Schnittstelle mit dem Namen [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) , die Klassen implementieren können, um Elemente im `Properties` Wörterbuch zu speichern und wiederherzustellen.

Die [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) -Klasse in der **xamarin. formsbook. Toolkit** -Bibliothek wird von `Application`abgeleitet. Anschließend können Sie Ihre `App` Klasse von `MultiPageRestorableApp` ableiten und einige Verwaltungsaufgaben ausführen.

[**Stackrestoredemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) veranschaulicht die Verwendung von `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Etwa eine realen app

Das [**Notetaker**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) -Beispiel nutzt auch `MultiPageRestorableApp` und ermöglicht die Eingabe und Bearbeitung von Notizen, die im `Properties` Wörterbuch gespeichert werden.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 24 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Kapitel 24 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Modale Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md)
