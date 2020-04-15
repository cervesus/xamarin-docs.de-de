---
title: Zusammenfassung von Kapitel 24. Seitennavigation
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 24. Seitennavigation'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "77130837"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Zusammenfassung von Kapitel 24. Seitennavigation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

Viele Anwendungen bestehen aus mehreren Seiten, zwischen denen der Benutzer navigiert. Die Anwendung verfügt immer über eine *Haupt*seite oder *Start*seite, von der aus der Benutzer zu anderen Seiten navigiert, die in einem Stapel verwaltet werden, um zu ihnen zurückkehren zu können. Zusätzliche Navigationsoptionen werden in [**Kapitel 25, „Seitenvarianten“** ](chapter25.md), behandelt.

## <a name="modal-pages-and-modeless-pages"></a>Modale Seiten und Seiten ohne Modus

`VisualElement` definiert eine [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation)-Eigenschaft vom Typ [`INavigation`](xref:Xamarin.Forms.INavigation), die die folgenden zwei Methoden zum Navigieren zu einer neuen Seite enthält:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Beide Methoden akzeptieren eine `Page`-Instanz als Argument und geben ein `Task`-Objekt zurück. Mit den folgenden zwei Methoden wird zur vorherigen Seite zurücknavigiert:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Wenn die Benutzeroberfläche über eine eigene Schaltfläche **Zurück** verfügt (wie bei Android- und Windows Phone-Telefonen), muss die Anwendung diese Methoden nicht aufrufen.

Obwohl diese Methoden über jedes `VisualElement` verfügbar sind, werden Sie in der Regel von der `Navigation`-Eigenschaft der aktuellen `Page`-Instanz aufgerufen.

Anwendungen verwenden generell modale Seiten, wenn der Benutzer vor der Rückkehr zur vorherigen Seite Informationen auf der Seite angeben muss. Seiten ohne Modus werden manchmal auch als „nicht modal“ oder *hierarchisch* bezeichnet. Nichts auf einer Seite mach sie selbst als modal oder nicht modal kenntlich. Dies wird stattdessen von der Methode gesteuert, die für die Navigation zu der Seite verwendet wird. Um auf allen Plattformen zu funktionieren, muss eine modale Seite ihre eigene Benutzeroberfläche für die Rückkehr zur vorherigen Seite bereitstellen.

Mit dem [**ModelessAndModal**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal)-Beispiel können Sie den Unterschied zwischen Seiten ohne Modus und modalen Seiten untersuchen. Jede Anwendung, die Seitennavigation verwendet, muss ihre Startseite an den [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Konstruktor übergeben, in der Regel in der `App`-Klasse des Programms. Ein Vorteil besteht darin, dass Sie kein `Padding` mehr auf der Seite für iOS festlegen müssen.

Sie werden feststellen, dass die [`Title`](xref:Xamarin.Forms.Page.Title)-Eigenschaft der Seite für Seiten ohne Modus angezeigt wird. Die Tablet- und Desktopplattformen iOS, Android und Windows stellen alle ein Benutzeroberflächenelement bereit, um zur vorherigen Seite zurückzukehren. Natürlich verfügen Android- und Windows Phone-Geräte über eine Standardschaltfläche **Zurück**, um zurückzuwechseln.

Bei modalen Seiten wird der `Title` der Seite nicht angezeigt, und es wird kein Benutzeroberflächenelement bereitgestellt, um zur vorherigen Seite zurückzukehren. Obwohl Sie die Standardschaltfläche **Zurück** von Android und Windows Phone verwenden können, um zur vorherigen Seite zurückzukehren, muss die modale Seite auf den anderen Plattformen ihren eigenen Mechanismus für die Rückkehr bereitstellen.

### <a name="animated-page-transitions"></a>Animierte Seitenübergänge

Alternative Versionen der verschiedenen Navigationsmethoden werden mit einem zweiten booleschen Argument bereitgestellt, das Sie auf `true` festlegen, wenn der Seitenübergang eine Animation beinhalten soll:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Die Standardmethoden für die Seitennavigation enthalten die Animation jedoch standardmäßig, sodass diese nur für die Navigation zu einer bestimmten Seite beim Start (wie am Ende dieses Kapitels erläutert) oder bei der Bereitstellung Ihrer eigenen Eingangsanimation (wie in [**Kapitel 22, „Animation“** ](chapter22.md), erläutert) nützlich sind.

### <a name="visual-and-functional-variations"></a>Visuelle und funktionelle Variationen

`NavigationPage` umfasst zwei Eigenschaften, die Sie festlegen können, wenn Sie die Klasse in Ihrer `App`-Methode instanziieren:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` umfasst außerdem vier angefügte bindbare Eigenschaften, die sich auf die spezifische Seite auswirken, für die sie festgelegt wurden:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) und [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) und [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) und [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) funktionieren nur unter iOS.
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) und [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) funktionieren nur unter iOS und Android.

### <a name="exploring-the-mechanics"></a>Untersuchen der Mechanismen

Die Methoden für die Seitennavigation sind alle asynchron und sollten mit `await` verwendet werden. Der Abschluss zeigt nicht an, dass die Seitennavigation abgeschlossen wurde, sondern nur, dass es sicher ist, den Seitennavigationsstapel zu untersuchen.

Wenn von einer Seite zu einer anderen navigiert wird, erhält die erste Seite in der Regel einen Aufruf ihrer [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing)-Methode, und die zweite Seite erhält einen Aufruf ihrer [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)-Methode. Ähnlich erhält, wenn von einer Seite zu einer anderen Seite zurückgewechselt wird, die erste Seite einen Aufruf ihrer `OnDisappearing`-Methode, und die zweite Seite erhält in der Regel einen Aufruf ihrer `OnAppearing`-Methode. Die Reihenfolge dieser Aufrufe (und der Abschluss der asynchronen Methoden, die die Navigation aufruft) ist plattformabhängig. Die Verwendung des Worts „in der Regel“ in den beiden voranstehenden Aussagen ist der modalen Seitennavigation von Android geschuldet, wo es diese Methodenaufrufe nicht gibt.

Auch weisen Aufrufe der Methoden `OnAppearing` und `OnDisappearing` nicht notwendigerweise auf Seitennavigation hin.

Die `INavigation`-Schnittstelle umfasst zwei Sammlungseigenschaften, die es Ihnen gestatten, den Navigationsstapel zu untersuchen:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) vom Typ `IReadOnlyList<Page>` für den Stapel ohne Modus.
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) vom Typ `IReadOnlyList<Page>` für den modalen Stapel.

Am sichersten ist es, aus der `Navigation`-Eigenschaft der `NavigationPage` auf diese Stapel zuzugreifen (bei der es sich um die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft der `App`-Klasse handelt sollte). Diese Stapel lassen sich nur sicher untersuchen, nachdem die asynchronen Methoden für die Seitennavigation abgeschlossen wurden. Die [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage)-Eigenschaft der `NavigationPage` gibt nicht die aktuelle Seite an, wenn die aktuelle Seite eine modale Seite ist, gibt aber stattdessen die letzte Seite ohne Modus an.

Im [**SinglePageNavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation)-Beispiel können Sie die Seitennavigation und die Stapel sowie die gültigen Typen von Seitennavigation untersuchen:

- Eine Seite ohne Modus kann zu einer anderen Seite ohne Modus oder einen modalen Seite navigieren.
- Eine modale Seite kann nur zu einer anderen modalen Seite navigieren.

### <a name="enforcing-modality"></a>Erzwingen der Modalität

Eine Anwendung verwendet eine modale Seite, wenn es notwendig ist, Informationen vom Benutzer zu erhalten. Der Benutzer sollte daran gehindert werden, zur vorherigen Seite zurückzukehren, bis diese Informationen bereitgestellt wurden. Unter iOS ist es einfach, eine Schaltfläche **Zurück** bereitzustellen und diese nur zu aktivieren, wenn der Benutzer mit der Seite fertig ist. Bei Android- und Windows Phone-Geräten sollte die Anwendung jedoch die [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed)-Methode außer Kraft setzen und `true` zurückgeben, wenn das Programm die Schaltfläche **Zurück** selbst verarbeitet hat, wie im [**ModalEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement)-Beispiel gezeigt.

Im [**MvvmEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement)-Beispiel wird veranschaulicht, wie dies in einem MVVM-Szenario funktioniert.

## <a name="navigation-variations"></a>Navigationsvariationen

Wenn man mehrmals zu einer bestimmten modalen Seite navigieren kann, sollte sie die Informationen behalten, damit der Benutzer die Informationen bearbeiten kann, anstatt sie erneut eingeben zu müssen. Dies können Sie erzielen, indem Sie die spezifische Instanz der modalen Seite beibehalten, doch ein besserer Ansatz (insbesondere unter iOS) besteht in der Erhaltung der Informationen in einem Ansichtsmodell.

### <a name="making-a-navigation-menu"></a>Erstellen eines Navigationsmenüs

Das [**ViewGalleryType**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType)-Beispiel veranschaulicht die Verwendung einer `TableView` zum Auflisten von Menüelementen. Jedes Element ist einem `Type`-Objekt für eine bestimmte Seite zugeordnet. Wenn dieses Element ausgewählt wird, instanziiert das Programm die Seite und navigiert dorthin.

[![Dreifacher Screenshot des Ansichtskatalogtyps](images/ch24fg21-small.png "TableView mit aufgelisteten Menüelementen")](images/ch24fg21-large.png#lightbox "TableView mit aufgelisteten Menüelementen")

Das [**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst)-Beispiel unterscheidet sich geringfügig dahingehend, dass das Menü anstelle von Typen Instanzen jeder Seite enthält. Dies hilft dabei, die Informationen von jeder Seite zu erhalten, aber alle Seiten müssen beim Programmstart instanziiert werden.

### <a name="manipulating-the-navigation-stack"></a>Bearbeiten des Navigationsstapels

[**StackManipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) veranschaulicht mehrere Funktionen, die von `INavigation` definiert werden, mit denen Sie den Navigationsstapel auf strukturierte Weise bearbeiten können:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) und [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) mit optionaler Animation.

### <a name="dynamic-page-generation"></a>Dynamische Seitengenerierung

Das [**BuildAPage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage)-Beispiel veranschaulicht das Erstellen einer Seite zur Laufzeit, basierend auf Benutzereingaben.

## <a name="patterns-of-data-transfer"></a>Datenübertragungsmuster

Es ist häufig erforderlich, Daten zwischen Seiten gemeinsam zu nutzen – um Daten auf eine navigierte Seite zu übertragen, und damit eine Seite Daten an die Seite zurückgeben kann, von der aus sie aufgerufen wurde. Es gibt mehrere Methoden, um dies zu erreichen.

### <a name="constructor-arguments"></a>Konstruktorargumente

Beim Navigieren zu einer neuen Seite ist es möglich, die Seitenklasse (Page) mit einem Konstruktorargument zu instanziieren, das es der Seite ermöglicht, sich selbst zu initialisieren. Dies wird im Beispiel [**SchoolAndStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents)-Beispiel veranschaulicht. Es ist auch möglich, den `BindingContext` der navigierten Seite den von der Seite festlegen zu lassen, von der aus die Navigation erfolgt ist.

### <a name="properties-and-method-calls"></a>Eigenschaften und Methodenaufrufe

Die verbleibenden Datenübertragungsbeispiele untersuchen das Problem der Übergabe von Informationen zwischen Seiten, wenn eine Seite zu einer anderen Seite navigiert und zurück. In diesen Erläuterungen wird von der *Start*seite zur *Info*seite navigiert, und es müssen initialisierte Informationen an die *Info*seite übertragen werden. Die *Info*seite ruft zusätzliche Informationen vom Benutzer ab und überträgt die Informationen an die *Start*seite.

Die *Start*seite kann problemlos auf öffentliche Methoden und Eigenschaften auf der *Info*seite zugreifen, sobald sie diese Seite instanziiert hat. Die *Info*seite kann auch auf öffentliche Methoden und Eigenschaften auf der *Start*seite zugreifen, doch die Auswahl eines geeigneten Zeitpunkts dafür kann schwierig sein. Das [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1)-Beispiel führt dies in seiner `OnDisappearing`-Außerkraftsetzung aus. Ein Nachteil ist, dass die *Info*seite den Typ der *Start*seite kennen muss.

### <a name="messagingcenter"></a>MessagingCenter

Die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)-Klasse von Xamarin.Forms bietet zwei Seiten eine andere Möglichkeit, miteinander zu kommunizieren. Nachrichten werden durch eine Textzeichenfolge identifiziert und können von einem beliebigen Objekt begleitet werden.

Ein Programm, das Nachrichten eines bestimmten Typs empfangen möchte, muss sie mithilfe von [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) abonnieren und eine Rückruffunktion angeben. Später kann es das Abonnement durch Aufrufen von [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) kündigen. Die Rückruffunktion empfängt alle gesendeten Nachrichten des angegebenen Typs mit dem angegebenen Namen, die über die [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Methode gesendet werden.

Das [**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2)-Programm veranschaulicht, wie Daten mithilfe des Messaging Centers übertragen werden, aber auch dies setzt voraus, dass die *Info*seite den Typ der *Start*seite kennt.

### <a name="events"></a>Ereignisse

Das Ereignis ist ein herkömmlicher Ansatz für eine Klasse, um Informationen an eine andere Klasse zu senden, ohne den Typ der Klasse zu kennen. Im [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3)-Beispiel definiert die *Info*-Klasse ein Ereignis, das sie auslöst, wenn die Informationen bereit sind. Es gibt jedoch keinen geeigneten Ort für die *Start*seite, um den Ereignishandler zu trennen.

### <a name="the-app-class-intermediary"></a>Der App-Klassenvermittler

Das [**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4)-Beispiel zeigt, wie Sie auf Eigenschaften zugreifen, die in der `App`-Klasse sowohl von der *Start*seite als auch von der *Info*seite definiert sind. Dies ist eine gute Lösung, aber im nächsten Abschnitt wird etwas besseres beschrieben.

### <a name="switching-to-a-viewmodel"></a>Wechseln zu einem ViewModel

Wenn Sie ein ViewModel für die Informationen verwenden, können die *Start*seite und die *Info*seite die Instanz der Informationsklasse gemeinsam nutzen. Dies wird im [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5)-Beispiel demonstriert.

### <a name="saving-and-restoring-page-state"></a>Speichern und Wiederherstellen des Seitenzustands

Der `App`-Klassenvermittler oder der ViewModel-Ansatz sind ideal, wenn die Anwendung Informationen speichern muss, wenn das Programm in den Standbymodus wechselt, während die *Info*seite aktiv ist. Dies wird im [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6)-Beispiel veranschaulicht.

## <a name="saving-and-restoring-the-navigation-stack"></a>Speichern und Wiederherstellen des Navigationsstapels

Im Allgemeinen sollte ein mehrseitiges Programm, das in den Standbymodus wechselt, zur selben Seite navigieren, wenn es wiederhergestellt wird. Dies bedeutet, dass ein solches Programm den Inhalt des Navigationsstapels speichern sollte. In diesem Abschnitt wird gezeigt, wie sich dieser Prozess in einer zu diesem Zweck entworfenen Klasse automatisieren lässt. Diese Klasse ruft auch die einzelnen Seiten auf, damit diese Ihren Seitenzustand speichern und wiederherstellen können.

Die [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek definiert eine Schnittstelle namens [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs), die Klassen implementieren können, um Elemente im `Properties`-Wörterbuch zu speichern und wiederherzustellen.

Die [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs)-Klasse in der **Xamarin.FormsBook.Toolkit**-Bibliothek wird von `Application` abgeleitet. Sie können dann Ihre `App`-Klasse von `MultiPageRestorableApp` ableiten und einige Verwaltungsaufgaben ausführen.

Das [**StackRestoreDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo)-Beispiel veranschaulicht die Verwendung von `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Fast schon eine realitätsnahe App

Das [**Notetaker**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker)-Beispiel verwendet auch `MultiPageRestorableApp` und ermöglicht die Eingabe und Bearbeitung von Notizen, die im `Properties`-Wörterbuch gespeichert werden.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 24 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Kapitel 24 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Modale Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md)
