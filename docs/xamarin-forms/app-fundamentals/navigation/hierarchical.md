---
title: Hierarchische Navigation
description: In diesem Artikel wird gezeigt, wie die NavigationPage-Klasse verwendet werden kann, um die Navigation in einem Stapel von LIFO-Seiten (Last In, First Out) auszuführen.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: a0a58cf05c97221a73cd0784b7859bb9c84cef86
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "38994675"
---
# <a name="hierarchical-navigation"></a>Hierarchische Navigation

_Die NavigationPage-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von Page-Objekten. In diesem Artikel wird gezeigt, wie die NavigationPage-Klasse verwendet werden kann, um die Navigation in einem Stapel von Seiten auszuführen._

Für den Wechsel von einer auf die andere Seite überträgt eine Anwendung, wie im folgenden Diagramm dargestellt, eine neue Seite mithilfe von Push in den Navigationsstapel, wo sie dann zur aktiven Seite wird:

![](hierarchical-images/pushing.png "Pushen einer Seite auf den Navigationsstapel")

Um zu vorherigen Seite zurückzukehren, entfernt die Anwendung die aktuelle Seite per Pop aus dem Navigationsstapel, und die neue oberste Seite wird zur aktiven Seite. Dieser Vorgang wird in dem folgenden Diagramm veranschaulicht:

![](hierarchical-images/popping.png "Eine Seite per Pop aus dem Navigationsstapel entfernen")

Navigationsmethoden werden von der Eigenschaft [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) für einen beliebigen [`Page`](xref:Xamarin.Forms.Page)-Typ verfügbar gemacht. Diese Methoden bieten die Möglichkeit, Seiten per Push auf den Navigationsstapel zu übertragen, Seiten per Pop aus dem Navigationsstapel zu entfernen und die Stapelbearbeitung durchzuführen.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Ausführen der Navigation

Bei der hierarchischen Navigation wird die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse verwendet, um durch einen Stapel von [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten zu navigieren. Auf den folgenden Screenshots werden die Komponenten der `NavigationPage` auf den jeweiligen Plattformen veranschaulicht:

![](hierarchical-images/navigationpage-components.png "NavigationPage-Komponenten")

Das Layout einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse ist plattformabhängig:

- Unter iOS finden Sie die Navigationsleiste ebenfalls am oberen Rand der Seite, wo ein Titel zu sehen ist. Dort befindet sich auch eine Schaltfläche *Zurück*, mit der zurück zur vorherigen Seite navigiert werden kann.
- Unter Android finden Sie die Navigationsleiste ebenfalls am oberen Rand der Seite, wo ein Titel sowie ein Symbol zu sehen. Dort befindet sich auch eine Schaltfläche *Zurück*, mit der zurück zur vorherigen Seite navigiert werden kann. Das Symbol wird im `[Activity]`-Attribut definiert, das die `MainActivity`-Klasse im plattformspezifischen Android-Projekt ergänzt.
- Auf der Universellen Windows-Plattform wird auf der Navigationsleiste ganz oben auf der Seite ein Titel angezeigt.

Auf allen Plattformen wird der Wert der [`Page.Title`](xref:Xamarin.Forms.Page.Title)-Eigenschaft als Seitentitel angezeigt.

> [!NOTE]
> Es wird empfohlen, die `NavigationPage`-Klasse nur mit `ContentPage`-Instanzen aufzufüllen.

### <a name="creating-the-root-page"></a>Erstellen der Stammseite

Die erste Seite, die zu einem Navigationsstapel hinzugefügt wird, wird als *Stammseite* der Anwendung bezeichnet. Das folgende Codebeispiel zeigt, wie dies erreicht wird:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Dies bewirkt, dass die `Page1Xaml` [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite und zur Stammseite der Anwendung wird. Dies wird im folgenden Screenshot veranschaulicht:

![](hierarchical-images/mainpage.png "Stammseite des Navigationsstapels")

> [!NOTE]
> Die [`RootPage`](xref:Xamarin.Forms.NavigationPage.RootPage)-Eigenschaft einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz ermöglicht den Zugriff auf die erste Seite im Navigationsstapel.

### <a name="pushing-pages-to-the-navigation-stack"></a>Pushen von Seiten auf den Navigationsstapel

Für die Navigation zur `Page2Xaml` muss die [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*)-Methode für die Eigenschaft [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) der aktuellen Seite aufgerufen werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Dies bewirkt, dass die `Page2Xaml`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird. Dies wird im folgenden Screenshot veranschaulicht:

![](hierarchical-images/secondpage.png "Seite, die per Push auf den Navigationsstapel übertragen wird")

Wenn die Methode [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) aufgerufen wird, treten die folgenden Ereignisse auf:

- Bei der Seite, die `PushAsync` aufruft, wird [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) überschrieben.
- Bei der Seite, zu der navigiert wird, wird die Überschreibung von [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) aufgerufen.
- Aufgabe `PushAsync` wird abgeschlossen.

Die genaue Reihenfolge, in der diese Ereignisse auftreten, ist jedoch plattformabhängig. Weitere Informationen hierzu finden Sie in [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) im Xamarin.Forms-Buch von Charles Petzold.

> [!NOTE]
> Aufrufe von Überschreibungen von [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) und [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) können nicht als garantierte Anzeichen für eine Seitennavigation behandelt werden. Unter iOS beispielsweise wird die Überschreibung von `OnDisappearing` auf der aktiven Seite aufgerufen, wenn die Anwendung beendet wird.

### <a name="popping-pages-from-the-navigation-stack"></a>Eine Seite per Pop aus dem Navigationsstapel entfernen

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt.

Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `Page2Xaml`-Instanz die [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Dadurch wird die `Page2Xaml`-Instanz von dem Navigationsstapel entfernt, und die neue oberste Seite wird zur aktiven Seite. Wenn die Methode [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) aufgerufen wird, treten die folgenden Ereignisse auf:

- Bei der Seite, die `PopAsync` aufruft, wird [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) überschrieben.
- Für die Seite, die zurückgegeben wird, wird die Überschreibung von [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) aufgerufen.
- Die `PopAsync`-Aufgabe wird zurückgegeben.

Die genaue Reihenfolge, in der diese Ereignisse auftreten, ist jedoch plattformabhängig. Weitere Informationen hierzu finden Sie in [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) im Xamarin.Forms-Buch von Charles Petzold.

Die Eigenschaft [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) jeder Seite stellt zusätzlich zu den [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*)- und [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methoden eine [`PopToRootAsync`](xref:Xamarin.Forms.NavigationPage.PopToRootAsync)-Methode bereit, wie im folgenden Codebeispiel dargestellt:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Diese Methode entfernt alle Klassen außer die [`Page`](xref:Xamarin.Forms.Page)-Klasse aus dem Navigationsstapel, deshalb wird die Stammseite der Anwendung zur aktiven Seite.

### <a name="animating-page-transitions"></a>Animieren von Seitenübergängen

Die Eigenschaft [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) jeder Seite bietet auch überschriebene Push- und Pop-Methoden, die einen `boolean`-Parameter beinhalten, der steuert, ob eine Seitenanimation während des Navigierens angezeigt wird. Dies wird im folgenden Codebeispiel veranschaulicht:

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

Wird der `boolean`-Parameter auf `false` festgelegt, wird die Seitenübergangsanimation deaktiviert. Wird der Parameter auf `true` festgelegt, wird die Seitenübergangsanimation aktiviert. Vorausgesetzt sie wird von der zugrunde liegenden Plattform unterstützt. Bei Push- und Pop-Methoden ohne diesen Parameter wird die Animation standardmäßig aktiviert.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Übergeben von Daten beim Navigieren

Beim Navigieren kann es manchmal erforderlich sein, dass Daten an eine andere Seite übergeben werden. Dies kann auf zwei Arten erfolgen: Durch das Übergeben von Daten durch einen Seitenkonstruktor oder durch Festlegen des [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der neuen Seite auf die Daten. Beide werden nun nacheinander erläutert.

### <a name="passing-data-through-a-page-constructor"></a>Übergeben von Daten durch einen Seitenkonstruktor

Die einfachste Art, beim Navigieren Daten an eine andere Seite zu übergeben, ist die Verwendung eines Seitenkonstruktors. Dies wird in dem folgenden Codebeispiel gezeigt:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Dieser Code erstellt eine `MainPage`-Instanz, mit Angabe von aktuellem Datum und aktueller Uhrzeit im ISO8601-Format, das in einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz umschlossen ist.

Die `MainPage`-Instanz empfängt die Daten durch einen Konstruktorparameter wie in dem folgenden Codebeispiel gezeigt:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Durch Festlegen der [`Label.Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaft werden die Daten, wie in den folgenden Screenshots dargestellt, dann auf der Seite angezeigt:

![](hierarchical-images/passing-data-constructor.png "Daten, die durch einen Seitenkonstruktor übergeben werden")

### <a name="passing-data-through-a-bindingcontext"></a>Übergeben von Daten mithilfe von BindingContext

Ein alternativer Ansatz zum Übergeben von Daten an eine andere Seite während der Navigation ist das Festlegen der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der neuen Seite auf die Daten. Dies wird im folgenden Codebeispiel veranschaulicht:

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

Dieser Code legt die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der `SecondPage`-Instanz auf die `Contact`-Instanz fest und navigiert dann zur `SecondPage`.

Die `SecondPage` nutzt dann Datenbindung, um die `Contact`-Instanzdaten anzuzeigen, wie im folgenden XAML-Codebeispiel zu sehen ist:

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

Das folgende Codebeispiel zeigt, wie die Datenbindung in C# ausgeführt werden kann:

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

Durch mehrere [`Label`](xref:Xamarin.Forms.Label)-Steuerelemente werden die Daten, wie in den folgenden Screenshots dargestellt, dann auf der Seite angezeigt:

![](hierarchical-images/passing-data-bindingcontext.png "Daten, die durch eine BindingContext-Eigenschaft übergeben werden")

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Bearbeiten des Navigationsstapels

Die Eigenschaft [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) macht die Eigenschaft [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) verfügbar, über welche die Seiten im Navigationsstapel abgerufen werden können. Während Xamarin.Forms den Zugriff auf den Navigationsstapel verwaltet, stellt die `Navigation`-Eigenschaft die [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*)- und [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*)-Methoden zum Bearbeiten des Stapels bereit, indem Seiten eingefügt oder entfernt werden.

Die Methode [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) fügt eine angegebene Seite noch vor einer vorhandenen angegebenen Seite in den Navigationsstapel ein, so wie in diesem Diagramm gezeigt:

![](hierarchical-images/insert-page-before.png "Pushen einer Seite auf den Navigationsstapel")

Im folgenden Diagramm ist dargestellt, wie die [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*)-Methode die angegebene Seite aus dem Navigationsstapel entfernt:

![](hierarchical-images/remove-page.png "Eine Seite aus dem Navigationsstapel entfernen")

Mit diesen Methoden kann eine benutzerdefinierte Navigation durchgeführt werden, z. B. das Ersetzen einer Anmeldeseite durch eine neue Seite, gefolgt von einer erfolgreichen Anmeldung. Dieses Szenario wird im folgenden Codebeispiel veranschaulicht:

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

Vorausgesetzt, dass die Anmeldeinformationen des Benutzers richtig sind, wird die `MainPage`-Instanz noch vor der aktuellen Seite in den Navigationsstapel eingefügt. Die [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methode entfernt daraufhin die aktuelle Seite aus dem Navigationsstapel, und die Instanz `MainPage` wird zur aktiven Seite.

## <a name="displaying-views-in-the-navigation-bar"></a>Anzeigen von Ansichten in der Navigationsleiste

Jede [`View`](xref:Xamarin.Forms.View)-Klasse von Xamarin.Forms kann in der Navigationsleiste einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) angezeigt werden. Dafür muss die angefügte [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)-Eigenschaft für `View` festgelegt werden. Die angefügte Eigenschaft kann für eine beliebige [`Page`](xref:Xamarin.Forms.Page)-Klasse festgelegt werden. Wenn die `Page` per Push auf eine `NavigationPage` übertragen wird, respektiert die `NavigationPage` den Wert der Eigenschaft.

Im folgenden Beispiel, das aus dem [Title View-Beispiel (Titelansicht)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/) von Xamarin stammt, wird dargestellt, wie die angefügte [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)-Eigenschaft von XAML festgelegt wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="NavigationPageTitleView.TitleViewPage">
    <NavigationPage.TitleView>
        <Slider HeightRequest="44" WidthRequest="300" />
    </NavigationPage.TitleView>
    ...
</ContentPage>
```

Hier der entsprechende C#-Code:

```csharp
public class TitleViewPage : ContentPage
{
    public TitleViewPage()
    {
        var titleView = new Slider { HeightRequest = 44, WidthRequest = 300 };
        NavigationPage.SetTitleView(this, titleView);
        ...
    }
}
```

Dies führt dazu, dass eine [`Slider`](xref:Xamarin.Forms.Slider)-Klasse in der Navigationsleiste auf der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) angezeigt wird:

[![Slider-TiteView](hierarchical-images/titleview-small.png "Slider-TiteView")](hierarchical-images/titleview-large.png#lightbox "Slider-TiteView")

> [!IMPORTANT]
> Viele Ansichten werden nicht in der Navigationsleiste angezeigt, es sei denn, die Größe der Ansicht wird mit den Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) angegeben. Alternativ kann die Ansicht auch in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse umschlossen werden, wobei die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) auf die entsprechenden Werte festgelegt sind.

Da die [`Layout`](xref:Xamarin.Forms.Layout)-Klasse von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet wird, kann die angefügte [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)-Eigenschaft festgelegt werden, um eine Layoutklasse anzuzeigen, die mehrere Ansichten enthält. Unter iOS und auf der Universellen Windows-Plattform (UWP) kann die Höhe der Navigationsleiste nicht geändert werden. Wenn die in der Navigationsleiste angezeigte Ansicht also die Standardgröße der Navigationsleiste übersteigt, wird sie abgeschnitten. Die Höhe der Navigationsleiste kann unter Android jedoch geändert werden. Legen Sie dazu die bindbare [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty)-Eigenschaft auf die Variable `double` fest, mit der die neue Höhe dargestellt wird. Weitere Informationen finden Sie unter [Setting the Navigation Bar Height on a NavigationPage (Festlegen der Höhe der Navigationsleiste auf eine NavigationPage)](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight).

Alternativ können Sie eine erweiterte Navigationsleiste vorschlagen, indem Sie einige Inhaltselemente in die Navigationsleiste platzieren und einige in eine Ansicht am oberen Rand des Seiteninhalts, für den Sie die Farbe entsprechend der Navigationsleiste anpassen. Zusätzlich können Sie unter iOS die Trennlinie und den Schatten am unteren Rand der Navigationsleiste entfernen, indem Sie die bindbare [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty)-Eigenschaft auf `true` festlegen. Weitere Informationen finden Sie unter [Hiding the Navigation Bar Separator on a NavigationPage (Ausblenden der Trennlinie der Navigationsleiste auf eine NavigationPage)](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar).

> [!NOTE]
> Die Eigenschaften [`BackButtonTitle`](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty), [`Title`](xref:Xamarin.Forms.Page.Title), [`TitleIcon`](xref:Xamarin.Forms.NavigationPage.TitleIconProperty) und [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) können Werte definieren, die einen Bereich in der Navigationsleiste einnehmen. Obwohl sich die Größe der Navigationsleiste je nach Plattform und Bildschirmgröße unterscheidet, führt das Festlegen all dieser Eigenschaften zu Konflikten, da nicht genügend Platz vorhanden ist. Versuchen Sie also nicht, eine Kombination dieser Eigenschaften zu nutzen, sondern legen Sie nur die `TitleView`-Eigenschaft fest, um Ihr gewünschtes Design für die Navigationsleiste zu realisieren.

### <a name="limitations"></a>Einschränkungen

Es gibt mehrere Einschränkungen, die Sie beachten sollten, wenn Sie eine [`View`](xref:Xamarin.Forms.View)-Klasse in der Navigationsleiste einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) anzeigen:

- Unter iOS werden Ansichten, die in der Navigationsleiste einer `NavigationPage` platziert sind, an einer anderen Position angezeigt, je nachdem, ob große Titel erlaubt sind. Weitere Informationen zum Aktivieren großer Titel finden Sie unter [Große Titel anzeigen](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title).
- Unter Android können Ansichten nur in Apps, die app-compat nutzen, in der Navigationsleiste einer `NavigationPage` platziert werden.
- Es wird empfohlen, große und komplexe Ansichten wie [`ListView`](xref:Xamarin.Forms.ListView) und [`TableView`](xref:Xamarin.Forms.TableView) in der Navigationsleiste einer `NavigationPage` zu platzieren.

## <a name="related-links"></a>Verwandte Links

- [Seitennavigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchical Navigation (Hierarchische Navigation: Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [TitleView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/)
- [How to Create a Sign In Screen Flow in Xamarin.Forms (Xamarin University Video) (Erstellen eines Anmeldebildschirmflows in Xamarin.Forms (Video von Xamarin University): Beispiel)](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [How to Create a Sign In Screen Flow in Xamarin.Forms (Xamarin University Video) (Erstellen eines Anmeldebildschirmflows in Xamarin.Forms (Video von Xamarin University))](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
