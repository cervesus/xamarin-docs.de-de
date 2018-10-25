---
title: Hierarchische Navigation
description: In diesem Artikel wird veranschaulicht, wie die "NavigationPage"-Klasse, die zum Ausführen der Navigation in einem Stapel von Last in, First Out (LIFO)-Seiten verwendet wird.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: a0a58cf05c97221a73cd0784b7859bb9c84cef86
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "38994675"
---
# <a name="hierarchical-navigation"></a>Hierarchische Navigation

_Die "NavigationPage"-Klasse bietet es sich um eine hierarchische Navigation bereit, in dem der Benutzer können Navigation durch Seiten, und in der Rückwärtsrichtung wie gewünscht ist. Die Klasse implementiert die Navigation als Last in, First Out (LIFO) der Page-Objekte. In diesem Artikel wird veranschaulicht, wie die "NavigationPage"-Klasse, die zum Ausführen der Navigation in einem Stapel von Seiten verwendet wird._

Um von einer Seite in ein anderes zu verschieben, wird eine Anwendung übertragen per push eine neue Seite in den Navigationsstapel, in dem sie die aktive Seite, werden wie im folgenden Diagramm dargestellt:

![](hierarchical-images/pushing.png "Eine Seite übertragen auf den Navigationsstapel")

Um zurück zur vorherigen Seite zurückzukehren, wird die Anwendung die aktuelle Seite, von dem Navigationsstapel angezeigt, und die neue oberste Seite wird die aktive Seite, wie im folgenden Diagramm dargestellt:

![](hierarchical-images/popping.png "Eine Seite, von dem Navigationsstapel entfernt")

Navigationsmethoden werden verfügbar gemacht, indem die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Eigenschaft für jedes beliebige [ `Page` ](xref:Xamarin.Forms.Page) abgeleitete Typen. Diese Methoden bieten die Möglichkeit, um Seiten in den Navigationsstapel, zu Seiten, pop vom Navigationsstapel übertragen und stapelbearbeitung auszuführen.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Ausführen der Navigation

Bei der hierarchischen Navigation wird die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse verwendet, um durch einen Stapel von [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten zu navigieren. Die folgenden Screenshots zeigen die wichtigsten Komponenten von der `NavigationPage` auf jeder Plattform:

![](hierarchical-images/navigationpage-components.png "\"NavigationPage\"-Komponenten")

Das Layout einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) ist abhängig von der Plattform:

- Unter iOS, existiert eine Navigationsleiste am oberen Rand der Seite, die einen Titel anzeigt, und, bei dem, ein *zurück* Schaltfläche, die zur vorherigen Seite zurückgibt.
- Unter Android eine Navigationsleiste am oberen Rand der Seite mit einem Titel, ein Symbol, vorhanden ist und ein *wieder* Schaltfläche, die zur vorherigen Seite zurückgibt. Das Symbol wird definiert, der `[Activity]` -Attribut, das ergänzt die `MainActivity` Klasse im plattformspezifischen Projekt Android.
- Für die universelle Windows-Plattform existiert eine Navigationsleiste am oberen Rand der Seite, die einen Titel anzeigt.

Auf allen Plattformen, den Wert des der [ `Page.Title` ](xref:Xamarin.Forms.Page.Title) Eigenschaft wird als Titel der Seite angezeigt werden.

> [!NOTE]
> Es wird empfohlen, eine `NavigationPage` gefüllt werden soll, mit `ContentPage` nur Instanzen.

### <a name="creating-the-root-page"></a>Erstellen die Seite "Root"

Die erste Seite, die zu einem Navigationsstapel hinzugefügt wird, wird als *Stammseite* der Anwendung bezeichnet. Das folgende Codebeispiel zeigt, wie dies erreicht wird:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Dies bewirkt, dass die `Page1Xaml` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Instanz, auf den Navigationsstapel übertragen werden, wird es zur aktiven Seite und die Stammseite der Anwendung. Dies wird in den folgenden Screenshots gezeigt:

![](hierarchical-images/mainpage.png "Stammseite der Navigationsstapel")

> [!NOTE]
> Die [ `RootPage` ](xref:Xamarin.Forms.NavigationPage.RootPage) Eigenschaft eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz ermöglicht den Zugriff auf die erste Seite im Navigationsstapel.

### <a name="pushing-pages-to-the-navigation-stack"></a>Seiten auf den Navigationsstapel übertragen

Zu navigieren `Page2Xaml`, es ist erforderlich, rufen Sie die [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) Methode für die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) -Eigenschaft der aktuellen Seite, wie im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Dies bewirkt, dass die `Page2Xaml`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird. Dies wird in den folgenden Screenshots gezeigt:

![](hierarchical-images/secondpage.png "Seite mithilfe von Push auf den Navigationsstapel übertragen")

Wenn die [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) Methode wird aufgerufen, die folgenden Ereignisse auftreten:

- Die Seite "aufrufende `PushAsync` hat seine [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) außer Kraft setzen, aufgerufen.
- Die Seite, zu dem navigiert hat seine [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft setzen, aufgerufen.
- Die `PushAsync` Aufgabe abgeschlossen ist.

Allerdings ist die genaue Reihenfolge, in der diese Ereignisse auftreten, plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles petzolds Xamarin.Forms Buch.

> [!NOTE]
> Aufrufe von der [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) und [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) Außerkraftsetzungen nicht als garantierte Anzeichen für die Seitennavigation behandelt werden. Unter iOS, z. B. die `OnDisappearing` außer Kraft setzen, wird auf der aktiven Seite aufgerufen, wenn die Anwendung beendet wird.

### <a name="popping-pages-from-the-navigation-stack"></a>Abholvorgänge Seiten aus dem Navigationsstapel

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt.

Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `Page2Xaml`-Instanz die [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Dadurch wird die `Page2Xaml`-Instanz von dem Navigationsstapel entfernt, und die neue oberste Seite wird zur aktiven Seite. Wenn die [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) Methode wird aufgerufen, die folgenden Ereignisse auftreten:

- Die Seite "aufrufende `PopAsync` hat seine [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) außer Kraft setzen, aufgerufen.
- Die zurückgegebene Ergebnis auf Seite verfügt über seine [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft setzen, aufgerufen.
- Die `PopAsync` Aufgabe zurückgibt.

Allerdings ist die genaue Reihenfolge, in der diese Ereignisse auftreten, plattformabhängig. Weitere Informationen finden Sie unter [Kapitel 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles petzolds Xamarin.Forms Buch.

Als auch [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) und [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) Methoden, die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) -Eigenschaft jeder Seite bietet auch eine [ `PopToRootAsync` ](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Diese Methode öffnet, alle bis auf den Stamm [ `Page` ](xref:Xamarin.Forms.Page) aus den Navigationsstapel, sodass die Stammseite der Anwendung zur aktiven Seite.

### <a name="animating-page-transitions"></a>Animieren von Seitenübergänge

Die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) -Eigenschaft jeder Seite bietet auch außer Kraft gesetzte Push- und pop-Methoden, die enthalten eine `boolean` Parameter, der steuert, ob zum Anzeigen einer Seite Animation während der Navigation, wie im folgenden Code gezeigt. Beispiel:

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

Festlegen der `boolean` Parameter `false` deaktiviert die Seitenübergänge Animation, beim Festlegen des Parameters auf `true` können Sie die Seite übergangsanimation, vorausgesetzt, dass sie von der zugrunde liegenden Plattform unterstützt wird. Die Push- und Pop-Methoden, die dieser Parameter hat jedoch aktiviert die Animation sind standardmäßig.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Übergeben von Daten beim Navigieren

Manchmal ist es erforderlich, für eine Seite, um Daten während der Navigation zu einer anderen Seite zu übergeben. Zwei Techniken, um dies zu erreichen sind übergeben von Daten über einen Seitenkonstruktor, und legen der neuen Seite [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) an den Daten. Nun wird jeder wiederum erläutert werden.

### <a name="passing-data-through-a-page-constructor"></a>Übergeben von Daten über einen Konstruktor für die Seite

Die einfachste Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite ist über Konstruktorparameter Seite, die in der im folgenden Codebeispiel gezeigt wird:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Dieser Code erstellt eine `MainPage` Instanz, in das aktuelle Datum und die Uhrzeit im ISO8601-Format übergeben werden, die als Wrapper versehen einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz.

Die `MainPage` Instanz empfängt die Daten über einen Konstruktorparameter, wie im folgenden Codebeispiel gezeigt:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Die Daten werden dann auf der Seite angezeigt, durch Festlegen der [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft, die in den folgenden Screenshots dargestellt:

![](hierarchical-images/passing-data-constructor.png "Daten, die über einen Konstruktor für die Seite übergeben")

### <a name="passing-data-through-a-bindingcontext"></a>Übertragen von Daten über eine BindingContext

Eine alternative Methode zum Übergeben von Daten während der Navigation zu einer anderen Seite wird durch Festlegen der neuen Seite [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) auf die Daten, wie im folgenden Codebeispiel gezeigt:

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

Dieser Code legt fest der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von der `SecondPage` -Instanz, auf die `Contact` Instanz, und klicken Sie dann navigiert zu der `SecondPage`.

Die `SecondPage` dann mithilfe der Datenbindung an die `Contact` Instanzdaten, wie in den folgenden XAML-Codebeispiel gezeigt:

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

Im folgenden Codebeispiel wird veranschaulicht, wie die Datenbindung in C# -Code ausgeführt werden kann:

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

Die Daten werden dann auf der Seite angezeigt, durch eine Reihe von [ `Label` ](xref:Xamarin.Forms.Label) steuert, wie in den folgenden Screenshots gezeigt:

![](hierarchical-images/passing-data-bindingcontext.png "Daten, die über eine BindingContext übergeben")

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Bearbeiten von dem Navigationsstapel

Die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Eigenschaft macht eine [ `NavigationStack` ](xref:Xamarin.Forms.INavigation.NavigationStack) Eigenschaft, die aus der die Seiten im Navigationsstapel abgerufen werden können. Während Xamarin.Forms Zugriff auf den Navigationsstapel, verwaltet die `Navigation` Eigenschaft enthält die [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) und [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) Methoden zur Bearbeitung im Stapels durch Einfügen Seiten oder entfernt werden.

Die [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) -Methode fügt eine angegebene Seite in den Navigationsstapel, bevor Sie eine vorhandene angegebene Seite, ein, wie im folgenden Diagramm dargestellt:

![](hierarchical-images/insert-page-before.png "Eine Seite einzufügen im Navigationsstapel.")

Die [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) Methode entfernt die angegebene Seite von dem Navigationsstapel, wie im folgenden Diagramm dargestellt:

![](hierarchical-images/remove-page.png "Eine Seite entfernt von dem Navigationsstapel.")

Diese Methoden ermöglichen es sich um eine benutzerdefinierte Navigations-erzielen Sie, wie z. B. eine Anmeldeseite mit einer neuen Seite, die nach einer erfolgreichen Anmeldung ersetzt. Im folgenden Codebeispiel wird dieses Szenario veranschaulicht:

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

Vorausgesetzt, dass die Anmeldeinformationen des Benutzers korrekt ist, werden die `MainPage` Instanz wird in den Navigationsstapel vor der aktuellen Seite eingefügt. Die [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) Methode entfernt die aktuelle Seite vom Navigationsstapel, klicken Sie dann mit der `MainPage` Instanz zur aktiven Seite zu.

## <a name="displaying-views-in-the-navigation-bar"></a>Anzeigen von Ansichten in der Navigationsleiste

Alle Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View) können angezeigt werden, in der Navigationsleiste auf der ein [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Dies geschieht durch Festlegen der [ `NavigationPage.TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) angefügten Eigenschaft, um eine `View`. Angefügte Eigenschaft festgelegt werden kann, auf einem [ `Page` ](xref:Xamarin.Forms.Page), und wann die `Page` zu leisten ist eine `NavigationPage`, `NavigationPage` wird den Wert der Eigenschaft berücksichtigt.

Das folgende Beispiel stammt aus dem [Title-Viewer-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/), veranschaulicht das Festlegen der [ `NavigationPage.TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) angefügte Eigenschaft in XAML:

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

Hier ist das Äquivalent C# Code:

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

Dies führt zu einem [ `Slider` ](xref:Xamarin.Forms.Slider) in der Navigationsleiste angezeigt wird, auf die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage):

[![Schieberegler TitleView](hierarchical-images/titleview-small.png "Schieberegler TitleView")](hierarchical-images/titleview-large.png#lightbox "Schieberegler TitleView")

> [!IMPORTANT]
> Viele Ansichten nicht in der Navigationsleiste angezeigt, es sei denn, die Größe der Ansicht mit angegeben wird die [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften. Alternativ kann die Ansicht eingebunden werden, einem [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) mit der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften, die auf die entsprechenden Werte festgelegt.

Beachten Sie, dass, weil die [ `Layout` ](xref:Xamarin.Forms.Layout) Klasse leitet sich von der [ `View` ](xref:Xamarin.Forms.View) -Klasse, die [ `TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) angefügte Eigenschaft kann festgelegt werden, um ein Layout anzuzeigen. Klasse, die mehrere Ansichten enthält. Unter iOS und die universelle Windows-Plattform (UWP) die Höhe der Navigationsleiste kann nicht geändert werden, und daher Clipping tritt auf, wenn in die Ansicht angezeigt, in der Navigationsleiste auf größer als die Standardgröße der Navigationsleiste ist. Allerdings unter Android die Höhe der Navigationsleiste kann geändert werden durch Festlegen der [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bindbare Eigenschaft eine `double` , das die neue Höhe darstellt. Weitere Informationen finden Sie unter [Festlegen der Höhe der Leiste Navigation auf einer "NavigationPage"](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight).

Alternativ kann eine erweiterte Navigationsleiste vorgeschlagen werden, platzieren Sie am oberen Rand der Inhalt der Seite, die Sie Farbe entsprechen, auf der Navigationsleiste auf einige des Inhalts in der Navigationsleiste und andere in einer Ansicht. Darüber hinaus unter iOS die Trennlinie und Schattenkopien, die am unteren Rand der Navigationsleiste können entfernt werden durch Festlegen der [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) bindbare Eigenschaft `true`. Weitere Informationen finden Sie unter [ausblenden-Trennzeichens. die Navigation auf einer "NavigationPage"](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar).

> [!NOTE]
> Die [ `BackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty), [ `Title` ](xref:Xamarin.Forms.Page.Title), [ `TitleIcon` ](xref:Xamarin.Forms.NavigationPage.TitleIconProperty), und [ `TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) Eigenschaften können alle definieren. Werte, die Platz auf der Navigationsleiste auf. Während die Größe der Navigation durch die Plattform und der Bildschirmgröße variiert, führt alle diese Eigenschaften festlegen zu Konflikte aufgrund der begrenzten Speicherplatz zur Verfügung. Anstatt zu versuchen, eine Kombination dieser Eigenschaften verwenden, können möglicherweise Sie Design Ihrer gewünschten Navigationsleiste besser erreichen können, indem Sie nur Festlegen der `TitleView` Eigenschaft.

### <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen zu beachten bei der Anzeige einer [ `View` ](xref:Xamarin.Forms.View) in der Navigationsleiste auf der ein [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage):

- Unter iOS, Ansichten, die in der Navigationsleiste des platziert eine `NavigationPage` angezeigt werden, in eine andere Position, je nachdem, ob große Titel aktiviert sind. Weitere Informationen zum Aktivieren der große Titel finden Sie unter [große Titel anzeigen](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title).
- Unter Android werden Ansichten in der Navigationsleiste des Platzieren einer `NavigationPage` können nur ausgeführt werden, in apps, die Anwendungskompatibilität zu verwenden.
- Es wird nicht empfohlen, große und komplexe Ansichten, z. B. platzieren [ `ListView` ](xref:Xamarin.Forms.ListView) und [ `TableView` ](xref:Xamarin.Forms.TableView), in der Navigationsleiste auf der ein `NavigationPage`.

## <a name="related-links"></a>Verwandte Links

- [Seitennavigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchisch (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Titleview enthalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/)
- [Vorgehensweise: Erstellen Sie ein Zeichen im Bildschirm-Fluss in Xamarin.Forms (Xamarin University-Video)-Beispiel](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Vorgehensweise: Erstellen Sie ein Zeichen im Bildschirm-Fluss in Xamarin.Forms (Xamarin University-Video)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- ["NavigationPage"](xref:Xamarin.Forms.NavigationPage)
