---
title: Xamarin.Forms-WebView
description: In diesem Artikel erläutert, wie Sie die Xamarin.Forms-WebView-Klasse verwenden, um lokale darzustellen oder Netzwerk-Web-Inhalte und Dokumente für Benutzer.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 26fbe6af639c67a94408605ba456bb3a100d2355
ms.sourcegitcommit: 3d39bafe4c56b15cbb695b1f7f02b926e1033f58
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155252"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms-WebView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView) ist eine Ansicht zum Anzeigen von Web-und HTML-Inhalt in Ihrer APP:

![Im App-Browser](webview-images/in-app-browser.png)

## <a name="content"></a>Inhalt

`WebView` unterstützt die folgenden Inhaltstypen:

- HTML-& CSS-Websites &ndash; WebView bietet vollständige Unterstützung für Websites, die mit HTML & CSS geschrieben wurden, einschließlich JavaScript-Unterstützung
- Dokumente &ndash; weil WebView mit nativen Komponenten auf jeder Plattform implementiert ist, kann WebView Dokumente anzeigen, die auf jeder Plattform sichtbar sind. Das heißt, PDF-Dateien unter iOS und Android arbeiten.
- HTML-Zeichen folgen &ndash; WebView kann HTML-Zeichen folgen aus dem Arbeitsspeicher anzeigen.
- Lokale Dateien &ndash; WebView können jeden der oben in der APP eingebetteten Inhaltstypen darstellen.

> [!NOTE]
> die `WebView` unter Windows unterstützt keine Silverlight-, Flash-oder ActiveX-Steuerelemente, auch wenn Sie von Internet Explorer auf dieser Plattform unterstützt werden.

### <a name="websites"></a>Websites

Legen Sie die [`Source`](xref:Xamarin.Forms.WebViewSource) -Eigenschaft des `WebView`auf eine Zeichen folgen-URL fest, um eine Website aus dem Internet anzuzeigen:

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URLs müssen vollständig mit dem angegebenen Protokoll gebildet werden (d. h. sie müssen "http://" oder "https://" vorangestellt ist).

#### <a name="ios-and-ats"></a>iOS und ATS

Seit Version 9 lässt iOS nur Ihre Anwendung zur Kommunikation mit Servern, die optimale Sicherheit standardmäßig zu implementieren. Werte müssen in `Info.plist` festgelegt werden, um die Kommunikation mit unsicheren Servern zu ermöglichen.

> [!NOTE]
> Wenn Ihre Anwendung eine Verbindung mit einer unsicheren Website erfordert, sollten Sie die Domäne immer als Ausnahme mithilfe von `NSExceptionDomains` eingeben, anstatt sie vollständig mithilfe `NSAllowsArbitraryLoads`zu deaktivieren. `NSAllowsArbitraryLoads` sollten nur in extrem Notfällen verwendet werden.

Das folgende Beispiel veranschaulicht, wie Sie eine bestimmte Domäne (in diesem Fall xamarin.com) zu aktivieren, ATS-Anforderungen umgehen:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
    ...
</key>
```

Es ist eine bewährte Methode, die nur einige Domänen ATS, und Sie können vertrauenswürdige Websites zu verwenden, über die zusätzliche Sicherheit in nicht vertrauenswürdigen Domänen cloudspeicherkosten, und umgehen. Das folgende Beispiel veranschaulicht die weniger sichere Methode zum Deaktivieren von ATS für die app aus:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
    ...
</key>
```

Weitere Informationen zu diesem neuen Feature in ios 9 finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md) .

### <a name="html-strings"></a>HTML-Zeichenfolgen

Wenn Sie eine im Code dynamisch definierte HTML-Zeichenfolge präsentieren möchten, müssen Sie eine Instanz von [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource)erstellen:

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![WebView mit HTML-Zeichenfolge](webview-images/html-string.png)

Im obigen Code wird `@` verwendet, um den HTML-Code als [ausführlichen Zeichenfolgenliterals](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)zu markieren, was bedeutet, dass die meisten Escapezeichen ignoriert werden

> [!NOTE]
> Abhängig vom Layout, dem das `WebView` untergeordnet ist, kann es erforderlich sein, die `WidthRequest`-und `HeightRequest` Eigenschaften der [`WebView`](xref:Xamarin.Forms.WebView) festzulegen, um den HTML-Inhalt anzuzeigen. Dies ist z. b. in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout)erforderlich.

### <a name="local-html-content"></a>Lokale HTML-Inhalt

WebView kann Inhalte aus HTML-, CSS- und Javascript innerhalb der app eingebettet. Beispiel:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamarin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Beachten Sie, dass die Schriftarten, die in der oben genannten CSS angegeben müssen für jede Plattform angepasst werden, da nicht alle Plattformen dieselben Schriftarten enthält.

Zum Anzeigen von lokalem Inhalt mithilfe eines `WebView`müssen Sie die HTML-Datei wie jede andere öffnen und dann den Inhalt als Zeichenfolge in die Eigenschaft `Html` eines `HtmlWebViewSource`laden. Weitere Informationen zum Öffnen von Dateien finden Sie unter [Arbeiten mit Dateien](~/xamarin-forms/data-cloud/data/files.md).

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von lokalen Inhalten auf jeder Plattform:

![Anzeigen von lokalem Inhalt in WebView](webview-images/local-content.png)

Obwohl die erste Seite geladen wurde, hat der `WebView` nicht wissen, woher der HTML stammt. Das ist ein Problem bei Seiten, die auf lokale Ressourcen zu verweisen. Beispiele für wann das passieren kann, sind beim lokalen Seiten-Link, der auf jede andere, einer Seite ist eine separate JavaScript-Datei verwenden oder eine Seite enthält zu einer CSS-Stylesheet links.  

Um dieses Problem zu beheben, müssen Sie die `WebView` angeben, wo Dateien im Dateisystem zu finden sind. Legen Sie dazu die `BaseUrl`-Eigenschaft auf dem vom `WebView`verwendeten `HtmlWebViewSource` fest.

Da das Dateisystem auf jedem der Betriebssysteme unterschiedlich ist, müssen Sie ermitteln dieser URL auf jeder Plattform. Xamarin. Forms macht die `DependencyService` zum Auflösen von Abhängigkeiten zur Laufzeit auf jeder Plattform verfügbar.

Um die `DependencyService`zu verwenden, definieren Sie zunächst eine Schnittstelle, die auf jeder Plattform implementiert werden kann:

```csharp
public interface IBaseUrl { string Get(); }
```

Beachten Sie, bis die Schnittstelle auf jeder Plattform implementiert wird, die die app nicht ausgeführt werden. Vergewissern Sie sich im allgemeinen Projekt, dass Sie die `BaseUrl` mit dem `DependencyService`festlegen:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Implementierungen der Schnittstelle für jede Plattform müssen bereitgestellt werden.

#### <a name="ios"></a>iOS

Unter IOS sollte sich der Webinhalt im Stammverzeichnis des Projekts oder im **Ressourcen** Verzeichnis mit der Buildaktion *bundleresource*befinden, wie unten gezeigt:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Lokale Dateien unter IOS](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Lokale Dateien unter IOS](webview-images/ios-xs.png)

-----

Die `BaseUrl` sollte auf den Pfad des Haupt Bündels festgelegt werden:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS
{
  public class BaseUrl_iOS : IBaseUrl
  {
    public string Get()
    {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Platzieren Sie unter Android HTML, CSS und Images im Ordner "Assets" mit der Buildaktion " *androidasset* ", wie unten gezeigt:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Lokale Dateien unter Android](webview-images/android-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Lokale Dateien unter Android](webview-images/android-xs.png)

-----

Unter Android sollte die `BaseUrl` auf `"file:///android_asset/"`festgelegt werden:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android
{
  public class BaseUrl_Android : IBaseUrl
  {
    public string Get()
    {
      return "file:///android_asset/";
    }
  }
}
```

Unter Android kann auf Dateien im Ordner " **Assets** " auch über den aktuellen Android-Kontext zugegriffen werden, der durch die `MainActivity.Instance`-Eigenschaft verfügbar gemacht wird:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Platzieren Sie in universelle Windows-Plattform (UWP)-Projekten HTML-, CSS-und Bilddateien im Stammverzeichnis des Projekts, und legen Sie die Buildaktion auf *Content*fest.

Die `BaseUrl` sollte auf `"ms-appx-web:///"`festgelegt werden:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Navigation

WebView unterstützt die Navigation über verschiedene Methoden und Eigenschaften, die es zur Verfügung stellt:

- **GoForward ()** &ndash; wenn `CanGoForward` true ist, wird beim Aufrufen von `GoForward` der nächsten besuchten Seite vorwärts navigiert.
- **GoBack ()** &ndash; wenn `CanGoBack` true ist, wird beim Aufrufen von `GoBack` zur zuletzt besuchten Seite navigiert.
- **&ndash; `true`** , wenn Seiten vorhanden sind, zu denen Sie zurück navigieren können, `false`, wenn der Browser an der Start-URL steht.
- " **CanGoForward** " &ndash; `true`, wenn der Benutzer rückwärts navigiert ist und zu einer bereits besuchten Seite wechseln kann.

Innerhalb von Seiten unterstützt `WebView` keine Multitouch-Gesten. Es ist wichtig, um sicherzustellen, dass der Inhalt Mobilgeräte optimierte und angezeigt wird, ohne die Notwendigkeit, zoomen.

Es kommt häufig vor, dass Anwendungen einen Link in einem `WebView`und nicht im Browser des Geräts anzeigen. In diesen Fällen erhalten sie eignet sich zum normalen Navigation zu ermöglichen, aber, wenn die Benutzer-Treffer sichern, während sie auf den Link, der gestartet werden, sollte die app auf die normalen app-Ansicht zurückgeben.

Verwenden Sie die integrierten Navigationsmethoden und Eigenschaften, um dieses Szenario zu ermöglichen.

Starten Sie, indem Sie die Seite für die Browseransicht erstellen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.InAppBrowserXaml"
             Title="Browser">
    <StackLayout Margin="20">
        <StackLayout Orientation="Horizontal">
            <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="OnBackButtonClicked" />
            <Button Text="Forward" HorizontalOptions="EndAndExpand" Clicked="OnForwardButtonClicked" />
        </StackLayout>
        <!-- WebView needs to be given height and width request within layouts to render. -->
        <WebView x:Name="webView" WidthRequest="1000" HeightRequest="1000" />
    </StackLayout>
</ContentPage>
```

Im Code-Behind:

```csharp
public partial class InAppBrowserXaml : ContentPage
{
    public InAppBrowserXaml(string URL)
    {
        InitializeComponent();
        webView.Source = URL;
    }

    async void OnBackButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoBack)
        {
            webView.GoBack();
        }
        else
        {
            await Navigation.PopAsync();
        }
    }

    void OnForwardButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoForward)
        {
            webView.GoForward();
        }
    }
}
```

Das ist alles!

![WebView-Navigations Schaltflächen](webview-images/in-app-browser.png)

## <a name="events"></a>Ereignisse

WebView löst die folgenden Ereignisse ein, damit Sie auf statusänderungen reagieren können:

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) –-Ereignis, das ausgelöst wird, wenn die WebView beginnt, eine neue Seite zu laden
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) –-Ereignis, das ausgelöst wird, wenn die Seite geladen und die Navigation beendet wurde.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested) –-Ereignis, das ausgelöst wird, wenn eine Anforderung zum erneuten Laden des aktuellen Inhalts erfolgt.

Das [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) Objekt, das das [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) Ereignis begleitet, verfügt über vier Eigenschaften:

- `Cancel` – gibt an, ob die Navigation abgebrochen werden soll.
- `NavigationEvent` – das aufgelöbene Navigations Ereignis.
- `Source` – das Element, das die Navigation ausgeführt hat.
- `Url` – das Navigations Ziel.

Das [`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs) Objekt, das das [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) Ereignis begleitet, verfügt über vier Eigenschaften:

- `NavigationEvent` – das aufgelöbene Navigations Ereignis.
- `Result` – beschreibt das Ergebnis der Navigation mithilfe eines [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) Enumerationsmembers. Gültige Werte sind `Cancel`, `Failure`, `Success` und `Timeout`.
- `Source` – das Element, das die Navigation ausgeführt hat.
- `Url` – das Navigations Ziel.

Wenn Sie die Verwendung von Webseiten erwarten, die viel Zeit zum Laden benötigen, können Sie die [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) -und [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) Ereignisse verwenden, um eine Statusanzeige zu implementieren. Beispiel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.LoadingLabelXaml"
             Title="Loading Demo">
    <StackLayout>
        <!--Loading label should not render by default.-->
        <Label x:Name="labelLoading" Text="Loading..." IsVisible="false" />
        <WebView HeightRequest="1000" WidthRequest="1000" Source="http://www.xamarin.com" Navigated="webviewNavigated" Navigating="webviewNavigating" />
    </StackLayout>
</ContentPage>
```

Die zwei Ereignishandler:

```csharp
void webviewNavigating(object sender, WebNavigatingEventArgs e)
{
    labelLoading.IsVisible = true;
}

void webviewNavigated(object sender, WebNavigatedEventArgs e)
{
    labelLoading.IsVisible = false;
}
```

Dadurch wird die folgende Ausgabe (geladen):

![Beispiel für WebView-Navigations Ereignis](webview-images/loading-start.png)

Fertig geladen:

![Beispiel für WebView-Navigations Ereignis](webview-images/loading-end.png)

## <a name="reloading-content"></a>Das erneute Laden Inhalt

[`WebView`](xref:Xamarin.Forms.WebView) verfügt über eine `Reload`-Methode, die zum erneuten Laden des aktuellen Inhalts verwendet werden kann:

```csharp
var webView = new WebView();
...
webView.Reload();
```

Wenn die `Reload`-Methode aufgerufen wird, wird das `ReloadRequested` Ereignis ausgelöst, das angibt, dass eine Anforderung zum erneuten Laden des aktuellen Inhalts ausgegeben wurde.

## <a name="performance"></a>Leistung

Gängige Webbrowser übernehmen Technologien wie Hardware beschleunigtes Rendering und JavaScript-Kompilierung. Vor xamarin. Forms 4,4 wurde der xamarin. Forms-`WebView` von der `UIWebView`-Klasse unter IOS implementiert. Viele dieser Technologien waren in dieser Implementierung jedoch nicht verfügbar. Da xamarin. Forms 4,4 ist, wird der xamarin. Forms-`WebView` in ios von der `WkWebView`-Klasse implementiert, die schnelleres Durchsuchen unterstützt.

> [!NOTE]
> Unter IOS verfügt der `WkWebViewRenderer` über eine Konstruktorüberladung, die ein `WkWebViewConfiguration`-Argument akzeptiert. Dadurch kann der Renderer bei der Erstellung konfiguriert werden.

Aus Kompatibilitätsgründen kann eine Anwendung zur Implementierung des xamarin. Forms-`WebView`die IOS-`UIWebView`-Klasse zurückkehren. Dies kann erreicht werden, indem Sie der Datei **AssemblyInfo.cs** im IOS-Platt Form Projekt für die Anwendung den folgenden Code hinzufügen:

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView` unter Android ist standardmäßig so schnell wie der integrierte Browser.

Die [UWP-Webansicht](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) verwendet die Microsoft Edge-Rendering-Engine. Desktop und Tablet-Geräte sollten die gleiche Leistung wie die Verwendung des Edge-Browsers selbst angezeigt werden.

## <a name="permissions"></a>Berechtigungen

Damit `WebView` funktioniert, müssen Sie sicherstellen, dass für jede Plattform Berechtigungen festgelegt sind. Beachten Sie, dass `WebView` auf einigen Plattformen im Debugmodus funktioniert, aber nicht, wenn Sie für die Freigabe erstellt werden. Das ist da einige Berechtigungen, wie die für den Internetzugriff unter Android werden standardmäßig von Visual Studio für Mac im Debugmodus festgelegt sind.

- **UWP** -&ndash; erfordert die Internet Funktion (Client & Server), wenn Netzwerk Inhalte angezeigt werden.
- **Android** -&ndash; erfordert nur `INTERNET`, wenn Inhalt aus dem Netzwerk angezeigt wird. Lokale Inhalte erfordern keine besonderen Berechtigungen.
- **IOS** -&ndash; erfordert keine besonderen Berechtigungen.

## <a name="layout"></a>Layout

Im Gegensatz zu den meisten anderen xamarin. Forms-Ansichten erfordert `WebView`, dass `HeightRequest` und `WidthRequest` angegeben werden, wenn Sie in Stacklayout oder relativelayout enthalten sind. Wenn Sie diese Eigenschaften nicht angeben, wird der `WebView` nicht angezeigt.

In den folgenden Beispielen werden Layouts veranschaulicht, die zum Arbeiten und Rendern von `WebView`s führen:

StackLayout mit WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout mit WidthRequest & HeightRequest:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout *ohne* widthrequest & Erhöhung trequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Raster *ohne* widthrequest & Erhöhung trequest. Raster ist eines der einige Layouts, die keine erfordert angeforderte Höhe und Breite angeben.:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

## <a name="invoking-javascript"></a>Aufrufen von JavaScript

[`WebView`](xref:Xamarin.Forms.WebView) bietet die Möglichkeit, eine JavaScript-Funktion von C#aufzurufen und jedes Ergebnis an den Aufruf C# enden Code zurückzugeben. Dies erfolgt mit der [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) -Methode, die im folgenden Beispiel aus dem [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) -Beispiel gezeigt wird:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

Die [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) -Methode wertet das JavaScript aus, das als-Argument angegeben ist, und gibt jedes Ergebnis als `string`zurück. In diesem Beispiel wird die JavaScript-Funktion `factorial` aufgerufen, die die Fakultät `number` als Ergebnis zurückgibt. Diese JavaScript-Funktion wird in der lokalen HTML-Datei definiert, die der [`WebView`](xref:Xamarin.Forms.WebView) lädt, und wird im folgenden Beispiel gezeigt:

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView depreation und App Store-Ablehnung (iTMS-90809)

Ab dem 2020 werden apps, die weiterhin die veraltete `UIWebView`-API verwenden, von [Apple abgelehnt](https://developer.apple.com/news/?id=12232019b) . Obwohl xamarin. Forms standardmäßig auf `WKWebView` umgestellt wurde, gibt es noch einen Verweis auf das ältere SDK in den xamarin. Forms-Binärdateien. Das aktuelle [IOS](~/ios/deploy-test/linker.md) -linkerverhalten führt dies nicht aus, und als Ergebnis wird die veraltete `UIWebView`-API weiterhin von Ihrer APP referenziert, wenn Sie Sie an den App Store übermitteln.

Eine Vorschauversion des Linkers ist verfügbar, um dieses Problem zu beheben. Um die Vorschau zu aktivieren, müssen Sie dem Linker ein zusätzliches Argument `--optimize=experimental-xforms-product-type` bereitstellen. 

Hierfür müssen folgende Voraussetzungen erfüllt sein:

- **Xamarin. Forms 4,5 oder höher** &ndash; vorab Versionen von xamarin. Forms 4,5 können verwendet werden.
- **Xamarin. IOS 13.10.0.17 oder höher** &ndash; die xamarin. IOS-Version [in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#version-information)überprüfen. Diese Version von xamarin. IOS ist in Visual Studio für Mac 8.4.1 und Visual Studio 16.4.3 enthalten.
- **Entfernen Sie Verweise auf `UIWebView`** &ndash; der Code darf keine Verweise auf `UIWebView` oder Klassen enthalten, die `UIWebView`verwenden.

### <a name="configure-the-linker-preview"></a>Konfigurieren der Linker-Vorschau

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Führen Sie die folgenden Schritte für den Linker aus, um `UIWebView` Verweise zu entfernen:

1. **Öffnen Sie IOS-Projekteigenschaften** &ndash; klicken Sie mit der rechten Maustaste auf Ihr IOS-Projekt, **und wählen Sie**
1. **Navigieren Sie zum Abschnitt IOS-Build,** &ndash; den Abschnitt **IOS-Build** auszuwählen.
1. **Aktualisieren Sie die zusätzlichen mberührungs-Argumente** &ndash; in den **zusätzlichen mberührungs-Argumenten** dieses Flag `--optimize=experimental-xforms-product-type` hinzufügen (zusätzlich zu allen Werten, die möglicherweise bereits vorhanden sind). Hinweis: dieses Flag funktioniert zusammen mit dem **linkerverhalten** , das nur auf das **SDK** oder **alle verknüpfen**festgelegt ist. Wenn Sie aus irgendeinem Grund Fehler sehen, wenn Sie das linkerverhalten auf alle festlegen, ist dies wahrscheinlich ein Problem innerhalb des App-Codes oder einer Drittanbieter Bibliothek, die nicht Linker sicher ist. Weitere Informationen zum Linker finden Sie unter [Verknüpfen von xamarin. IOS-apps](~/ios/deploy-test/linker.md).
1. **Aktualisieren Sie alle Buildkonfigurationen** &ndash; verwenden Sie die **Konfigurations** -und **Platt Form** Listen am oberen Rand des Fensters, um alle Buildkonfigurationen zu aktualisieren. Die wichtigste zu Aktualisier dende Konfiguration ist die **Release/iPhone-** Konfiguration, da diese normalerweise zum Erstellen von Builds für die App Store-Übermittlung verwendet wird.

Sie können das Fenster mit dem neuen Flag in diesem Screenshot sehen:

[![Festlegen des Flags im Abschnitt IOS-Build](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Führen Sie diese Schritte für den Linker aus, um `UIWebView` Verweise zu entfernen.

1. **Öffnen Sie IOS-Projektoptionen** &ndash; klicken Sie mit der rechten Maustaste auf Ihr IOS-Projekt, **und wählen Sie**
1. **Navigieren Sie zum Abschnitt IOS-Build,** &ndash; den Abschnitt **IOS-Build** auszuwählen.
1. **Aktualisieren Sie die zusätzlichen _mberührungs_ -Argumente** &ndash; in den **zusätzlichen _mberührungs_ -Argumenten** dieses Flag `--optimize=experimental-xforms-product-type` hinzufügen (zusätzlich zu allen Werten, die möglicherweise bereits vorhanden sind). Hinweis: dieses Flag funktioniert zusammen mit dem **linkerverhalten** , das nur auf das **SDK** oder **alle verknüpfen**festgelegt ist. Wenn Sie aus irgendeinem Grund Fehler sehen, wenn Sie das linkerverhalten auf alle festlegen, ist dies wahrscheinlich ein Problem innerhalb des App-Codes oder einer Drittanbieter Bibliothek, die nicht Linker sicher ist. Weitere Informationen zum Linker finden Sie unter [Verknüpfen von xamarin. IOS-apps](~/ios/deploy-test/linker.md).
1. **Aktualisieren Sie alle Buildkonfigurationen** &ndash; verwenden Sie die **Konfigurations** -und **Platt Form** Listen am oberen Rand des Fensters, um alle Buildkonfigurationen zu aktualisieren. Die wichtigste zu Aktualisier dende Konfiguration ist die **Release/iPhone-** Konfiguration, da diese normalerweise zum Erstellen von Builds für die App Store-Übermittlung verwendet wird.

Sie können das Fenster mit dem neuen Flag in diesem Screenshot sehen:

[![Festlegen des Flags im Abschnitt IOS-Build](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

Wenn Sie nun einen neuen (Release) Build erstellen und an den App Store senden, sollten keine Warnungen zu der veralteten API vorliegen.

## <a name="related-links"></a>Verwandte Themen

- [Arbeiten mit WebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
