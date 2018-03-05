---
title: Modale Seiten
description: "Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde. Dieser Artikel veranschaulicht, wie zu modalen Seiten zu navigieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: b1e67fe355b9a84cc6832441f06c72dcd4c512ad
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="modal-pages"></a>Modale Seiten

_Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde. Dieser Artikel veranschaulicht, wie zu modalen Seiten zu navigieren._

In diesem Artikel werden die folgenden Themen erläutert:

- [Ausführen von Navigation](#Performing_Navigation) – Seiten auf den modalen Stapel Abholvorgänge Seiten aus dem Stapel modale übertragen, deaktivieren Sie die Schaltfläche "zurück" und Seitenübergänge zu animieren.
- [Übergeben von Daten beim Navigieren](#Passing_Data_when_Navigating) : Übergeben von Daten über einen Seitenkonstruktor, und eine `BindingContext`.

## <a name="overview"></a>Übersicht

Eine modale Seite möglich die [Seite](~/xamarin-forms/user-interface/controls/pages.md) Typen von Xamarin.Forms unterstützt. Um ein modales für die Seitenanzeige wird die Anwendung sie weiterleiten, im modalen Stapel, in dem sie die aktive Seite werden wie im folgenden Diagramm dargestellt:

![](modal-images/pushing.png "Eine Seite an den modalen Stapel weitergegeben werden.")

Um zur vorherigen Seite zurückzukehren die Anwendung wird die aktuelle Seite aus dem Stapel modal angezeigt, und die neue Seite für die oberste wird die aktive Seite wie im folgenden Diagramm dargestellt:

![](modal-images/popping.png "Eine Seite aus dem modale Stapel herunternehmen")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Ausführen von Navigation

Modale Navigationsmethoden werden von der Eigenschaft [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) für einen beliebigen [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Typ verfügbar gemacht. Diese Methoden bieten die Möglichkeit, [push modale Seiten](#Pushing_Pages_to_the_Modal_Stack) im modalen Stapel und [pop modale Seiten](#Popping_Pages_from_the_Modal_Stack) aus dem modale Stapel.

Die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft auch macht eine [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) Eigenschaft aus der die modalen Seiten im modalen Stapel abgerufen werden können. Es gibt jedoch kein Konzept für die modale Stapelbearbeitung oder das Entfernen per Pop, um bei der modalen Navigation zur Stammseite zurückzukehren. Grund dafür ist, dass diese Vorgänge auf den zugrunde liegenden Plattformen nicht allgemein unterstützt werden.

> [!NOTE]
> Ein [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz ist nicht für die Durchführung von modalen Seitennavigation erforderlich.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Seiten auf den modalen Stapel übertragen

Zu navigieren der `ModalPage` es ist notwendig, das Aufrufen der [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) Methode für die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) -Eigenschaft der aktuellen Seite, wie im folgenden Codebeispiel wird:

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

Dies bewirkt, dass die `ModalPage` Instanz im modalen Stapel verschoben werden, wird es der aktiven Seite, angegeben, dass ein Element ausgewählt wurde die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) auf die `MainPage` Instanz. Die `ModalPage` Instanz wird in den folgenden Screenshots dargestellt:

![](modal-images/modalpage.png "Modale Seite-Beispiel")

Wenn [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) aufgerufen wird, die folgenden Ereignisse auftreten:

- Die aufrufende Seite `PushModalAsync` hat seine [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) aufgerufen werden, vorausgesetzt, dass die zugrunde liegende Plattform Android nicht überschreiben.
- Die Seite, zu dem navigiert verfügt über seine [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) Außerkraftsetzung aufgerufen.
- Die `PushAsync` Aufgabe abgeschlossen ist.

Allerdings ist die genaue Reihenfolge an, dass diese Ereignisse treten plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charless Xamarin.Forms Buch.

> [!NOTE]
> Aufrufe von der [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) und [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) Außerkraftsetzungen nicht als garantierte Anzeichen Seitennavigation behandelt werden. Bei iOS kann z. B. die `OnDisappearing` Außerkraftsetzung für die aktive Seite aufgerufen wird, wenn die Anwendung beendet wird.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Abholvorgänge Seiten aus dem Stapel modale

Die aktive Seite kann vom modalen Stapel geholt werden, durch Drücken der *wieder* Schaltfläche auf dem Gerät unabhängig von, ob dies eine Taste auf dem Gerät ist oder eine Schaltfläche auf dem Bildschirm.

Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `ModalPage`-Instanz die [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Dies bewirkt, dass die `ModalPage` Instanz aus dem modale Stapel mit der neuen oberste Seite als die aktive Seite entfernt werden soll. Wenn [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) aufgerufen wird, die folgenden Ereignisse auftreten:

- Die aufrufende Seite `PopModalAsync` hat seine [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) Außerkraftsetzung aufgerufen.
- Die Rückgabe an Seite verfügt über seine [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) aufgerufen werden, vorausgesetzt, dass die zugrunde liegende Plattform Android nicht überschreiben.
- Die `PopModalAsync` Aufgabe zurück.

Allerdings ist die genaue Reihenfolge an, dass diese Ereignisse treten plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charless Xamarin.Forms Buch.

### <a name="disabling-the-back-button"></a>Deaktivieren die Schaltfläche "zurück"

Unter Android und Windows Phone-Benutzer kann stets zur vorherigen Seite durch Drücken des Standards *wieder* Schaltfläche auf dem Gerät. Wenn die modale Seite den Benutzer für Ihre eigenständige Aufgaben vor dem Verlassen der Seite erforderlich ist, muss die Anwendung deaktiviert die *wieder* Schaltfläche. Dies geschieht durch Überschreiben der [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) Methode auf der Seite "gebunden". Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charless Xamarin.Forms Buch.

### <a name="animating-page-transitions"></a>Animieren Seitenübergänge

Die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft Rand jeder Seite enthält zudem außer Kraft gesetzte Push und pop Methoden wie eine `boolean` Parameter, der steuert, ob zum Anzeigen einer Seite Animation während der Navigation, wie im folgenden Code gezeigt. Beispiel:

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

Festlegen der `boolean` Parameter `false` deaktiviert die Animation Seite Übergang beim Festlegen des Parameters auf `true` können Sie die Seite Übergangsanimationen, vorausgesetzt, dass sie von der zugrunde liegenden Plattform unterstützt wird. Die Push "und" Pop-Methoden, die dieser Parameter hat jedoch aktiviert die Animation standardmäßig.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Übergeben von Daten beim Navigieren

Manchmal ist es erforderlich, für eine Seite, um Daten während der Navigation zu einer anderen Seite zu übergeben. Sind zwei Techniken, um dies zu erreichen, durch Übergeben von Daten über einen Seitenkonstruktor, und legen der neuen Seite [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) an den Daten. Jede wird jetzt der Reihe nach erläutert werden.

### <a name="passing-data-through-a-page-constructor"></a>Übergeben von Daten über eine Seite-Konstruktor

Die einfachste Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite ist über Konstruktorparameter Seite, die im folgenden Codebeispiel gezeigt wird:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Dieser Code erstellt eine `MainPage` Instanz, in das aktuelle Datum und die Uhrzeit im ISO8601-Format übergeben.

Die `MainPage` Instanz empfängt die Daten über einen Konstruktorparameter aus, wie im folgenden Codebeispiel gezeigt:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Die Daten werden dann auf der Seite angezeigt, durch Festlegen der [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaft.

### <a name="passing-data-through-a-bindingcontext"></a>Übergeben von Daten über ein BindingContext

Eine alternative Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite wird durch Festlegen der neuen Seite [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) auf die Daten, wie im folgenden Codebeispiel gezeigt:

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

In diesem Code wird die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) von der `DetailPage` -Instanz, auf die `Contact` Instanz, und wechselt anschließend zum der `DetailPage`.

Die `DetailPage` verwendet dann die Datenbindung zum Anzeigen der `Contact` Instanzdaten, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

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

Im folgenden Codebeispiel wird veranschaulicht, wie die Datenbindung in c# durchgeführt werden kann:

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

Die Daten werden dann auf der Seite angezeigt, durch eine Reihe von [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelemente.

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/index.md) (Datenbindungsgrundlagen).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie zu modalen Seiten zu navigieren. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.


## <a name="related-links"></a>Verwandte Links

- [Seitennavigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modale (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
