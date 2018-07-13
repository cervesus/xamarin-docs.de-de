---
title: Xamarin.Forms-modale Seiten
description: Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde. In diesem Artikel veranschaulicht, wie zum modale Seiten zu navigieren.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 44aee8500c7de2ae56b59049368d6025ec49cc5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994816"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms-modale Seiten

_Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde. In diesem Artikel veranschaulicht, wie zum modale Seiten zu navigieren._

In diesem Artikel werden die folgenden Themen behandelt:

- [Ausführen der Navigation](#Performing_Navigation) – Seiten auf den modalen Stapel Abholvorgänge Seiten aus dem modalen Stapel übertragen, deaktivieren Sie die Schaltfläche "zurück" und Seitenübergänge zu animieren.
- [Übergeben von Daten beim Navigieren](#Passing_Data_when_Navigating) : Übergeben von Daten über einen Seitenkonstruktor, und eine `BindingContext`.

## <a name="overview"></a>Übersicht

Eine modale Seite möglich die [Seite](~/xamarin-forms/user-interface/controls/pages.md) von Xamarin.Forms unterstützte Typen. Eine modale Seite angezeigt wird die Anwendung per Push übertragen auf den Stapel modales, in dem sie die aktive Seite, werden wie im folgenden Diagramm dargestellt:

![](modal-images/pushing.png "Eine Seite übertragen an den modalen Stapel")

Um zur vorherigen Seite zurück die Anwendung wird die aktuelle Seite aus dem Stapel modal angezeigt wird, und die neue oberste Seite wird zur aktiven Seite, wie im folgenden Diagramm dargestellt:

![](modal-images/popping.png "Entfernt eine Seite aus dem modalen Stapel")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Ausführen der Navigation

Modale Navigationsmethoden werden von der Eigenschaft [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) für einen beliebigen [`Page`](xref:Xamarin.Forms.Page)-Typ verfügbar gemacht. Diese Methoden bieten die Möglichkeit, [push modale Seiten](#Pushing_Pages_to_the_Modal_Stack) im modalen Stapel und [pop-modale Seiten](#Popping_Pages_from_the_Modal_Stack) aus dem modalen Stapel.

Die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) -Eigenschaft auch verfügbar macht eine [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) Eigenschaft, die aus der die modalen Seiten im modalen Stapel abgerufen werden können. Es gibt jedoch kein Konzept für die modale Stapelbearbeitung oder das Entfernen per Pop, um bei der modalen Navigation zur Stammseite zurückzukehren. Grund dafür ist, dass diese Vorgänge auf den zugrunde liegenden Plattformen nicht allgemein unterstützt werden.

> [!NOTE]
> Für die Durchführung einer modalen Seitennavigation ist keine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz erforderlich.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Seiten auf den modalen Stapel übertragen

Zu navigieren die `ModalPage` es ist erforderlich, rufen Sie die [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) Methode für die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) -Eigenschaft der aktuellen Seite, wie im folgenden Codebeispiel veranschaulicht:

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

Dies bewirkt, dass die `ModalPage` Instanz im modalen Stapel verschoben werden, wo Sie dann zur aktiven Seite, angegeben, dass ein Element ausgewählt wurde die [ `ListView` ](xref:Xamarin.Forms.ListView) auf die `MainPage` Instanz. Die `ModalPage` Instanz wird in den folgenden Screenshots dargestellt:

![](modal-images/modalpage.png "Modale Seite-Beispiel")

Wenn [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) wird aufgerufen, die folgenden Ereignisse auftreten:

- Die Seite "aufrufende `PushModalAsync` hat seine [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) aufgerufen werden, vorausgesetzt, dass die zugrunde liegende Plattform Android nicht außer Kraft setzen.
- Die Seite, zu dem navigiert hat seine [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft setzen, aufgerufen.
- Die `PushAsync` Aufgabe abgeschlossen ist.

Allerdings ist die genaue Reihenfolge an, dass diese Ereignisse treten plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles petzolds Xamarin.Forms Buch.

> [!NOTE]
> Aufrufe von der [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) und [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) Außerkraftsetzungen nicht als garantierte Anzeichen für die Seitennavigation behandelt werden. Unter iOS, z. B. die `OnDisappearing` außer Kraft setzen, wird auf der aktiven Seite aufgerufen, wenn die Anwendung beendet wird.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Abholvorgänge Seiten aus dem modalen Stapel

Zur aktive Seite vom modalen Stapel geholt werden kann, durch Drücken der *wieder* Schaltfläche auf dem Gerät unabhängig von, ob dies eine physische Schaltfläche an das Gerät ist oder eine Schaltfläche auf dem Bildschirm.

Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `ModalPage`-Instanz die [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Dies bewirkt, dass die `ModalPage` Instanz aus dem Stapel modale über die neue oberste Seite als die aktive Seite entfernt werden soll. Wenn [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) wird aufgerufen, die folgenden Ereignisse auftreten:

- Die Seite "aufrufende `PopModalAsync` hat seine [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) außer Kraft setzen, aufgerufen.
- Die zurückgegebene Ergebnis auf Seite verfügt über seine [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) aufgerufen werden, vorausgesetzt, dass die zugrunde liegende Plattform Android nicht außer Kraft setzen.
- Die `PopModalAsync` Aufgabe zurückgibt.

Allerdings ist die genaue Reihenfolge an, dass diese Ereignisse treten plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles petzolds Xamarin.Forms Buch.

### <a name="disabling-the-back-button"></a>Deaktivieren die Schaltfläche "zurück"

Unter Android wird der Benutzer kann jederzeit zur vorherigen Seite durch Drücken des Standards *wieder* Schaltfläche auf dem Gerät. Wenn der modale Seite den Benutzer eine eigenständige Aufgabe abgeschlossen ist, bevor Sie diese Seite verlassen erforderlich ist, muss die Anwendung deaktivieren der *wieder* Schaltfläche. Dies kann erreicht werden, durch Überschreiben der [ `Page.OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) Methode für die modale Seite. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles petzolds Xamarin.Forms Buch.

### <a name="animating-page-transitions"></a>Animieren von Seitenübergänge

Die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) -Eigenschaft jeder Seite bietet auch außer Kraft gesetzte Push- und pop-Methoden, die enthalten eine `boolean` Parameter, der steuert, ob zum Anzeigen einer Seite Animation während der Navigation, wie im folgenden Code gezeigt. Beispiel:

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

Festlegen der `boolean` Parameter `false` deaktiviert die Seitenübergänge Animation, beim Festlegen des Parameters auf `true` können Sie die Seite übergangsanimation, vorausgesetzt, dass sie von der zugrunde liegenden Plattform unterstützt wird. Die Push- und Pop-Methoden, die dieser Parameter hat jedoch aktiviert die Animation sind standardmäßig.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Übergeben von Daten beim Navigieren

Manchmal ist es erforderlich, für eine Seite, um Daten während der Navigation zu einer anderen Seite zu übergeben. Sind zwei Techniken, um dies zu erreichen, durch Übergeben von Daten über einen Seitenkonstruktor, und legen der neuen Seite [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) an den Daten. Nun wird jeder wiederum erläutert werden.

### <a name="passing-data-through-a-page-constructor"></a>Übergeben von Daten über einen Konstruktor für die Seite

Die einfachste Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite ist über Konstruktorparameter Seite, die in der im folgenden Codebeispiel gezeigt wird:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Dieser Code erstellt eine `MainPage` Instanz, in das aktuelle Datum und die Uhrzeit im ISO8601-Format übergeben.

Die `MainPage` Instanz empfängt die Daten über einen Konstruktorparameter, wie im folgenden Codebeispiel gezeigt:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Die Daten werden dann auf der Seite angezeigt, durch Festlegen der [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft.

### <a name="passing-data-through-a-bindingcontext"></a>Übertragen von Daten über eine BindingContext

Eine alternative Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite wird durch Festlegen der neuen Seite [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) auf die Daten, wie im folgenden Codebeispiel gezeigt:

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

Dieser Code legt fest der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von der `DetailPage` -Instanz, auf die `Contact` Instanz, und klicken Sie dann navigiert zu der `DetailPage`.

Die `DetailPage` dann mithilfe der Datenbindung an die `Contact` Instanzdaten, wie in den folgenden XAML-Codebeispiel gezeigt:

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

Im folgenden Codebeispiel wird veranschaulicht, wie die Datenbindung in C# -Code ausgeführt werden kann:

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

Die Daten werden dann auf der Seite angezeigt, durch eine Reihe von [ `Label` ](xref:Xamarin.Forms.Label) Steuerelemente.

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/index.md) (Datenbindungsgrundlagen).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie zum modale Seiten zu navigieren. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.


## <a name="related-links"></a>Verwandte Links

- [Seitennavigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modale (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
