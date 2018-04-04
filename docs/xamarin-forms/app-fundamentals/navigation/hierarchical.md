---
title: Hierarchische Navigation
description: Die NavigationPage-Klasse bietet die hierarchische Navigation, in denen der Benutzer Seiten vorwärts und rückwärts nach Bedarf navigieren kann. Die Klasse implementiert die Navigation als einen Stapel Last in, First Out (LIFO) von Page-Objekten. In diesem Artikel veranschaulicht, wie die NavigationPage-Klasse, die zum Ausführen der Navigation in einem Stapel von Seiten verwendet wird.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: afaf0c702cdba1ba9c5d2c9d158501c50501f910
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="hierarchical-navigation"></a>Hierarchische Navigation

_Die NavigationPage-Klasse bietet die hierarchische Navigation, in denen der Benutzer Seiten vorwärts und rückwärts nach Bedarf navigieren kann. Die Klasse implementiert die Navigation als einen Stapel Last in, First Out (LIFO) von Page-Objekten. In diesem Artikel veranschaulicht, wie die NavigationPage-Klasse, die zum Ausführen der Navigation in einem Stapel von Seiten verwendet wird._

In diesem Artikel werden die folgenden Themen erläutert:

- [Ausführen von Navigation](#Performing_Navigation) – erstellen Sie die Seite "Root", Seiten auf Navigationsstapel übertragen, Seiten aus dem Navigationsbereich Stapel herunternehmen und Seitenübergänge zu animieren.
- [Übergeben von Daten beim Navigieren](#Passing_Data_when_Navigating) : Übergeben von Daten über einen Seitenkonstruktor, und eine `BindingContext`.
- [Bearbeiten von Navigationsstapel](#Manipulating_the_Navigation_Stack) – bearbeiten den Stapel durch Einfügen, oder Entfernen von Seiten.

## <a name="overview"></a>Übersicht

Um von einer Seite in eine andere zu verschieben, wird eine Anwendung push eine neue Seite im Navigationsbereich Stapel, in dem sie die aktive Seite werden wie im folgenden Diagramm dargestellt:

![](hierarchical-images/pushing.png "Eine Seite an die Navigationsstapel weitergegeben werden.")

Um zurück zur vorherigen Seite zurückzukehren, wird die Anwendung die aktuelle Seite Navigationsstapel angezeigt, und die oberste neue Seite wird die aktive Seite, wie im folgenden Diagramm dargestellt:

![](hierarchical-images/popping.png "Eine Seite aus dem Navigationsbereich Stapel herunternehmen")

Navigationsmethoden werden verfügbar gemacht, indem Sie die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft für ein beliebiges [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) abgeleitete Typen. Diese Methoden bieten die Möglichkeit, Seiten auf den Navigationsstapel in pop Seiten aus Navigationsstapel zu übertragen und stapelbearbeitung auszuführen.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Ausführen von Navigation

Bei der hierarchischen Navigation wird die [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Klasse verwendet, um durch einen Stapel von [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Objekten zu navigieren. Die folgenden Screenshots zeigen die Hauptkomponenten der `NavigationPage` auf jeder Plattform:

![](hierarchical-images/navigationpage-components.png "NavigationPage-Komponenten")

Das Layout einer [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) ist abhängig von der Plattform:

- Bei iOS kann eine Navigationsleiste am oberen Rand der Seite, die einen Titel anzeigt und dessen, vorhanden ist eine *wieder* Schaltfläche, die zur vorherigen Seite zurückgibt.
- Auf Android-Geräten eine Navigationsleiste am oberen Rand der Seite, einen Titel, ein Symbol anzeigt, vorhanden ist und ein *wieder* Schaltfläche, die zur vorherigen Seite zurückgibt. Das Symbol wird definiert, der `[Activity]` -Attribut, das ergänzt die `MainActivity` Klasse im Android plattformspezifische Projekt.
- Auf Windows Phone existiert eine Navigationsleiste am oberen Rand der Seite, die einen "Titel" anzeigt. Windows Phone verfügt nicht über die *wieder* Schaltfläche auf der Navigationsleiste, da eine auf dem Bildschirm *wieder* Schaltfläche am unteren Rand des Bildschirms vorhanden ist.

Auf allen Plattformen, den Wert der [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) Eigenschaft wird als Titel der Seite angezeigt.

> [!NOTE]
> Es wird empfohlen, die eine `NavigationPage` aufgefüllt werden soll, mit `ContentPage` nur Instanzen.

### <a name="creating-the-root-page"></a>Erstellen die Seite "Root"

Die erste Seite, die zu einem Navigationsstapel hinzugefügt wird, wird als *Stammseite* der Anwendung bezeichnet. Das folgende Codebeispiel zeigt, wie dies erreicht wird:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Dies bewirkt, dass die `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanz, wo der aktiven Seite und die Seite "Root" der Anwendung wird im Navigationsbereich Stapel verschoben werden. Dies wird in den folgenden Screenshots veranschaulicht:

![](hierarchical-images/mainpage.png "Seite "Root" der Navigationsstapel")

> [!NOTE]
> Die [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) Eigenschaft von einem [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz ermöglicht den Zugriff auf die erste Seite im Navigationsstapel.

### <a name="pushing-pages-to-the-navigation-stack"></a>Seiten an Navigationsstapel weitergegeben werden.

Zu navigieren `Page2Xaml`, es ist erforderlich, das Aufrufen der [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) Methode für die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) -Eigenschaft der aktuellen Seite, wie im folgenden Codebeispiel wird:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Dies bewirkt, dass die `Page2Xaml`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird. Dies wird in den folgenden Screenshots veranschaulicht:

![](hierarchical-images/secondpage.png "Seite zu leisten Navigationsstapel")

Wenn die [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) Methode wird aufgerufen, die folgenden Ereignisse auftreten:

- Die aufrufende Seite `PushAsync` hat seine [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) Außerkraftsetzung aufgerufen.
- Die Seite, zu dem navigiert verfügt über seine [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) Außerkraftsetzung aufgerufen.
- Die `PushAsync` Aufgabe abgeschlossen ist.

Allerdings ist die genaue Reihenfolge, in der diese Ereignisse auftreten, plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charless Xamarin.Forms Buch.

> [!NOTE]
> Aufrufe von der [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) und [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) Außerkraftsetzungen nicht als garantierte Anzeichen Seitennavigation behandelt werden. Bei iOS kann z. B. die `OnDisappearing` Außerkraftsetzung für die aktive Seite aufgerufen wird, wenn die Anwendung beendet wird.

### <a name="popping-pages-from-the-navigation-stack"></a>Abholvorgänge Seiten aus dem Navigationsstapel

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt.

Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `Page2Xaml`-Instanz die [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Dadurch wird die `Page2Xaml`-Instanz von dem Navigationsstapel entfernt, und die neue oberste Seite wird zur aktiven Seite. Wenn die [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Methode wird aufgerufen, die folgenden Ereignisse auftreten:

- Die aufrufende Seite `PopAsync` hat seine [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) Außerkraftsetzung aufgerufen.
- Die Rückgabe an Seite verfügt über seine [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) Außerkraftsetzung aufgerufen.
- Die `PopAsync` Aufgabe zurück.

Allerdings ist die genaue Reihenfolge, in der diese Ereignisse auftreten, plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charless Xamarin.Forms Buch.

Als auch [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) und [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Methoden, die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft Rand jeder Seite bietet auch eine [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Diese Methode nimmt alle bis auf den Stamm [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) deaktiviert Navigationsstapel, sodass die Stammseite der Anwendung die aktive Seite.

### <a name="animating-page-transitions"></a>Animieren Seitenübergänge

Die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft Rand jeder Seite enthält zudem außer Kraft gesetzte Push und pop Methoden wie eine `boolean` Parameter, der steuert, ob zum Anzeigen einer Seite Animation während der Navigation, wie im folgenden Code gezeigt. Beispiel:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Festlegen der `boolean` Parameter `false` deaktiviert die Animation Seite Übergang beim Festlegen des Parameters auf `true` können Sie die Seite Übergangsanimationen, vorausgesetzt, dass sie von der zugrunde liegenden Plattform unterstützt wird. Die Push "und" Pop-Methoden, die dieser Parameter hat jedoch aktiviert die Animation standardmäßig.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Übergeben von Daten beim Navigieren

Manchmal ist es erforderlich, für eine Seite, um Daten während der Navigation zu einer anderen Seite zu übergeben. Zwei Techniken, um dies zu erreichen, werden Daten über einen Seitenkonstruktor, und legen der neuen Seite bestanden [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) an den Daten. Jede wird jetzt der Reihe nach erläutert werden.

### <a name="passing-data-through-a-page-constructor"></a>Übergeben von Daten über eine Seite-Konstruktor

Die einfachste Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite ist über Konstruktorparameter Seite, die im folgenden Codebeispiel gezeigt wird:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Dieser Code erstellt ein `MainPage` Instanz, in das aktuelle Datum und die Uhrzeit im ISO8601-Format übergeben, wird die eingebunden in eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz.

Die `MainPage` Instanz empfängt die Daten über einen Konstruktorparameter aus, wie im folgenden Codebeispiel gezeigt:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Die Daten werden dann auf der Seite angezeigt, durch Festlegen der [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaft, die in den folgenden Screenshots dargestellt:

![](hierarchical-images/passing-data-constructor.png "Daten, die über eine Seite-Konstruktor übergeben")

### <a name="passing-data-through-a-bindingcontext"></a>Übergeben von Daten über ein BindingContext

Eine alternative Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite wird durch Festlegen der neuen Seite [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) auf die Daten, wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

In diesem Code wird die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) von der `SecondPage` -Instanz, auf die `Contact` Instanz, und wechselt anschließend zum der `SecondPage`.

Die `SecondPage` verwendet dann die Datenbindung zum Anzeigen der `Contact` Instanzdaten, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Im folgenden Codebeispiel wird veranschaulicht, wie die Datenbindung in c# durchgeführt werden kann:

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

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
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

Die Daten werden dann auf der Seite angezeigt, durch eine Reihe von [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) steuert, wie in den folgenden Screenshots dargestellt:

![](hierarchical-images/passing-data-bindingcontext.png "Daten, die über eine BindingContext übergeben")

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Bearbeiten von Navigationsstapel

Die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft macht eine [ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) Eigenschaft aus der die Seiten im Navigationsstapel abgerufen werden können. Während Xamarin.Forms Zugriff auf die Navigationsstapel verwaltet die `Navigation` Eigenschaft ermöglicht die [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) und [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) Methoden zum Bearbeiten der Stapel von einfügen Seiten oder entfernen.

Die [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) Methode fügt eine angegebene Seite im Navigationsstapel vor einer vorhandenen angegebenen Seite, wie im folgenden Diagramm dargestellt:

![](hierarchical-images/insert-page-before.png "Einfügen einer Seite im Navigationsstapel")

Die [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) Methode entfernt die angegebene Seite aus dem Navigationsstapel, wie im folgenden Diagramm dargestellt:

![](hierarchical-images/remove-page.png "Entfernen eine Seite aus dem Navigationsstapel")

Mithilfe dieser Methoden kann eine benutzerdefinierte Navigation erzielen Sie, wie eine Anmeldeseite mit einer neuen Seite, die eine erfolgreiche Anmeldung nach ersetzen. Im folgenden Codebeispiel wird dieses Szenario veranschaulicht:

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

Vorausgesetzt, dass die Anmeldeinformationen des Benutzers korrekt ist, werden die `MainPage` Instanz Navigationsstapel vor der aktuellen Seite eingefügt wird. Die [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Methode entfernt die aktuelle Seite Navigationsstapel, klicken Sie dann mit der `MainPage` Instanz als die aktive Seite.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie mithilfe der [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Klasse, um die Navigation in einem Stapel von Seiten auszuführen. Diese Klasse bietet die hierarchische Navigation, in denen der Benutzer Seiten vorwärts und rückwärts nach Bedarf navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Objekten.


## <a name="related-links"></a>Verwandte Links

- [Seitennavigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchische (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Vorgehensweise: Erstellen einer Anmeldung im Bildschirm-Fluss in Xamarin.Forms (Xamarin-University-Video) Beispiel](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Vorgehensweise: Erstellen einer Anmeldung im Bildschirm-Fluss in Xamarin.Forms (Xamarin-University-Video)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
