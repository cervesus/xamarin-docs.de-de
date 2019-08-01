---
title: Xamarin.Forms-modale Seiten
description: Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde. In diesem Artikel wird das Navigieren zu modalen Seiten veranschaulicht.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: cd29e284c45bfe59633dde924e27d8022e8416ba
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645925"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms-modale Seiten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-modal)

_Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde. In diesem Artikel wird das Navigieren zu modalen Seiten veranschaulicht._

In diesem Artikel werden die folgenden Themen behandelt:

- [Ausführen der Navigation:](#Performing_Navigation) Veröffentlichen von Seiten auf dem modalen Stapel, Entfernen von Seiten aus dem modalen Stapel per Pop, Deaktivieren der Schaltfläche „Zurück“ und Animieren von Seitenübergängen.
- [Übergeben von Daten beim Navigieren:](#Passing_Data_when_Navigating) Übergeben von Daten über einen Seitenkonstruktor und einen `BindingContext`.

## <a name="overview"></a>Übersicht

Eine modale Seite kann jeder von Xamarin.Forms unterstützte [Seiten](~/xamarin-forms/user-interface/controls/pages.md)-Typ sein. Wenn eine modale Seite angezeigt werden soll, überträgt die Anwendung diese Seite per Push in den modalen Stapel, wo sie dann zur aktiven Seite wird. Dieser Vorgang wird im folgenden Diagramm veranschaulicht:

![](modal-images/pushing.png "Übertragen von Seiten auf den modalen Stapel")

Um zu vorherigen Seite zurückzukehren, entfernt die Anwendung die aktuelle Seite per Pop vom modalen Stapel, und die neue oberste Seite wird zur aktiven Seite. Dieser Vorgang wird im folgenden Diagramm veranschaulicht:

![](modal-images/popping.png "Entfernen von Seiten per Pop aus dem modalen Stapel")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Ausführen der Navigation

Modale Navigationsmethoden werden von der Eigenschaft [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) für einen beliebigen [`Page`](xref:Xamarin.Forms.Page)-Typ verfügbar gemacht. Diese Methode ermöglicht das [Übertragen von modalen Seiten per Push](#Pushing_Pages_to_the_Modal_Stack) auf den modalen Stapel und das [Entfernen von modalen Seiten per Pop](#Popping_Pages_from_the_Modal_Stack) aus dem modalen Stapel.

Die Eigenschaft [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) macht zudem die Eigenschaft [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) verfügbar, über welche die modalen Seiten im modalen Stapel abgerufen werden können. Es gibt jedoch kein Konzept für die modale Stapelbearbeitung oder das Entfernen per Pop, um bei der modalen Navigation zur Stammseite zurückzukehren. Grund dafür ist, dass diese Vorgänge auf den zugrunde liegenden Plattformen nicht allgemein unterstützt werden.

> [!NOTE]
> Für die Durchführung einer modalen Seitennavigation ist keine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz erforderlich.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Übertragen von Seiten per Push auf den modalen Stapel

Wenn Sie zur `ModalPage` navigieren möchten, muss die [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*)-Methode für die Eigenschaft [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) der aktuellen Seite aufgerufen werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Dies bewirkt, dass die `ModalPage`-Instanz per Push an den modalen Stapel übertragen wird, wo sie zur aktiven Seite wird, wenn in den [`ListView`](xref:Xamarin.Forms.ListView) ein Element auf der `MainPage`-Instanz ausgewählt wurde. Die `ModalPage`-Instanz wird in den folgenden Screenshots dargestellt:

![](modal-images/modalpage.png "Modale Seite – Beispiel")

Wenn [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) aufgerufen wird, treten die folgenden Ereignisse auf:

- Bei der Seite, die `PushModalAsync` aufruft, wird [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) überschrieben, wenn nicht Android als Plattform zugrunde liegt.
- Bei der Seite, zu der navigiert wird, wird die Überschreibung von [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) aufgerufen.
- Aufgabe `PushAsync` wird abgeschlossen.

Die genaue Reihenfolge, in der diese Ereignisse auftreten, ist jedoch plattformabhängig. Weitere Informationen hierzu finden Sie in [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) im Xamarin.Forms-Buch von Charles Petzold.

> [!NOTE]
> Aufrufe von Überschreibungen von [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) und [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) können nicht als garantierte Anzeichen für eine Seitennavigation behandelt werden. Unter iOS beispielsweise wird die Überschreibung von `OnDisappearing` auf der aktiven Seite aufgerufen, wenn die Anwendung beendet wird.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Seiten zum Entfernen per Pop aus dem modalen Stapel

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop vom modalen Stapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt.

Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `ModalPage`-Instanz die [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Dadurch wird die `ModalPage`-Instanz aus dem modalen Stapel entfernt, und die neue oberste Seite wird zur aktiven Seite. Wenn [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) aufgerufen wird, treten die folgenden Ereignisse auf:

- Bei der Seite, die `PopModalAsync` aufruft, wird [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) überschrieben.
- Bei der Seite, an die zurückgegeben werden soll, wird [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) überschrieben, wenn nicht Android als Plattform zugrunde liegt.
- Die `PopModalAsync`-Aufgabe wird zurückgegeben.

Die genaue Reihenfolge, in der diese Ereignisse auftreten, ist jedoch plattformabhängig. Weitere Informationen hierzu finden Sie in [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) im Xamarin.Forms-Buch von Charles Petzold.

### <a name="disabling-the-back-button"></a>Deaktivieren der Schaltfläche „Zurück“

Unter Android kann der Benutzer jederzeit zur vorherigen Seite zurückkehren, indem er die Standardschaltfläche *Zurück* auf dem Gerät drückt. Wenn der Benutzer von der modalen Seite dazu aufgefordert wird, vor dem Verlassen der Seite eine eigenständige Aufgabe abzuschließen, muss die Anwendung die Schaltfläche *Zurück* deaktivieren. Dies kann durch die Überschreibung der [`Page.OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed)-Methode auf der modalen Seite erreicht werden. Weitere Informationen hierzu finden Sie in [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) im Xamarin.Forms-Buch von Charles Petzold.

### <a name="animating-page-transitions"></a>Animieren von Seitenübergängen

Die Eigenschaft [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) jeder Seite bietet auch überschriebene Push- und Pop-Methoden, die einen `boolean`-Parameter beinhalten, der steuert, ob eine Seitenanimation während des Navigierens angezeigt wird. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Wird der `boolean`-Parameter auf `false` festgelegt, wird die Seitenübergangsanimation deaktiviert. Wird der Parameter auf `true` festgelegt, wird die Seitenübergangsanimation aktiviert. Vorausgesetzt sie wird von der zugrunde liegenden Plattform unterstützt. Bei Push- und Pop-Methoden ohne diesen Parameter wird die Animation standardmäßig aktiviert.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Übergeben von Daten beim Navigieren

Beim Navigieren kann es manchmal erforderlich sein, dass Daten an eine andere Seite übergeben werden. Dies kann auf zwei Arten erfolgen. Durch das Übergeben von Daten durch einen Seitenkonstruktor oder durch Festlegen des [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der neuen Seite auf die Daten. Beide werden nun nacheinander erläutert.

### <a name="passing-data-through-a-page-constructor"></a>Übergeben von Daten durch einen Seitenkonstruktor

Die einfachste Art, beim Navigieren Daten an eine andere Seite zu übergeben, ist die Verwendung eines Seitenkonstruktors. Dies wird in dem folgenden Codebeispiel gezeigt:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Dieser Code erstellt eine `MainPage`-Instanz, mit Angabe von aktuellem Datum und aktueller Uhrzeit im ISO8601-Format.

Die `MainPage`-Instanz empfängt die Daten durch einen Konstruktorparameter wie im folgenden Codebeispiel zu sehen ist:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Durch Festlegen der [`Label.Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaft werden die Daten dann auf der Seite angezeigt.

### <a name="passing-data-through-a-bindingcontext"></a>Übergeben von Daten mithilfe von BindingContext

Ein alternativer Ansatz zum Übergeben von Daten an eine andere Seite während der Navigation ist das Festlegen des [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der neuen Seite auf die Daten. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Dieser Code legt den [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der `DetailPage`-Instanz auf die `Contact`-Instanz fest und navigiert dann zur `DetailPage`.

Die `DetailPage` nutzt dann Datenbindung, um die `Contact`-Instanzdaten anzuzeigen, wie im folgenden XAML-Codebeispiel zu sehen ist:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Das folgende Codebeispiel zeigt, wie die Datenbindung in C# ausgeführt werden kann:

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Die Daten werden dann durch eine Reihe von [`Label`](xref:Xamarin.Forms.Label)-Steuerelementen auf der Seite angezeigt.

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/index.md) (Datenbindungsgrundlagen).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Navigieren zu modalen Seiten veranschaulicht. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.


## <a name="related-links"></a>Verwandte Links

- [Seitennavigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modale (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-modal)
- [PassingData (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)
