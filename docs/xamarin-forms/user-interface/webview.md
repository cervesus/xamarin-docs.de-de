---
title: Xamarin.FormsWebView
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms WebView-Klasse verwenden, um Benutzern lokale oder Netzwerk-Webinhalte und-Dokumente zu präsentieren.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9b07e044e55f99a7a183e55c566bf59dbd082655
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198170"
---
# <a name="xamarinforms-webview"></a>Xamarin.FormsWebView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)ist eine Ansicht für die Anzeige von Web-und HTML-Inhalt in Ihrer APP:

![Im App-Browser](webview-images/in-app-browser.png)

## <a name="content"></a>Content

`WebView`unterstützt die folgenden Inhaltstypen:

- HTML & CSS Websites &ndash; WebView bietet vollständige Unterstützung für Websites, die mithilfe von HTML-& CSS geschrieben wurden, einschließlich JavaScript-Unterstützung
- Dokumente &ndash; , da WebView mit nativen Komponenten auf jeder Plattform implementiert ist, kann WebView Dokumente anzeigen, die auf jeder Plattform sichtbar sind. Dies bedeutet, dass PDF-Dateien unter IOS und Android funktionieren.
- HTML-Zeichen folgen WebView kann HTML-Zeichen folgen im Arbeits &ndash; Speicher anzeigen.
- Lokale Dateien &ndash; WebView kann jeden der oben in der APP eingebetteten Inhaltstypen darstellen.

> [!NOTE]
> `WebView`unter Windows unterstützt Silverlight, Flash oder ActiveX-Steuerelemente nicht, auch wenn Sie von Internet Explorer auf dieser Plattform unterstützt werden.

### <a name="websites"></a>Websites

Legen Sie die `WebView` -Eigenschaft des s auf eine Zeichen folgen-URL fest, um eine Website aus dem Internet anzuzeigen [`Source`](xref:Xamarin.Forms.WebViewSource) :

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URLs müssen vollständig mit dem angegebenen Protokoll formatiert sein (d. h., es muss "http://" oder "https://" vorangesteht).

#### <a name="ios-and-ats"></a>IOS und ATS

Seit Version 9 ermöglicht IOS nur der Anwendung die Kommunikation mit Servern, die standardmäßig die Best Practice-Sicherheit implementieren. Werte müssen in festgelegt werden `Info.plist` , um die Kommunikation mit unsicheren Servern zu ermöglichen.

> [!NOTE]
> Wenn Ihre Anwendung eine Verbindung mit einer unsicheren Website erfordert, sollten Sie die Domäne immer als Ausnahme mit eingeben, `NSExceptionDomains` anstatt sie vollständig mithilfe von zu deaktivieren `NSAllowsArbitraryLoads` . `NSAllowsArbitraryLoads`sollte nur in extrem Notfällen verwendet werden.

Im folgenden wird veranschaulicht, wie Sie eine bestimmte Domäne (in diesem Fall xamarin.com) zum Umgehen von ATS-Anforderungen aktivieren:

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

Es empfiehlt sich, nur einige Domänen für die Umgehung von ATS zu aktivieren, sodass Sie vertrauenswürdige Sites verwenden können, während Sie von der zusätzlichen Sicherheit auf nicht vertrauenswürdigen Domänen profitieren. Im folgenden wird die weniger sichere Methode zum Deaktivieren von ATS für die APP veranschaulicht:

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

### <a name="html-strings"></a>HTML-Zeichen folgen

Wenn Sie eine im Code dynamisch definierte HTML-Zeichenfolge präsentieren möchten, müssen Sie eine Instanz von erstellen [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) :

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

Im obigen Code `@` wird verwendet, um den HTML-Code als [ausführlichen Zeichenfolgenliterals](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)zu markieren, was bedeutet, dass die meisten Escapezeichen ignoriert werden

> [!NOTE]
> Es kann erforderlich sein, die-Eigenschaft und die-Eigenschaft von festzulegen `WidthRequest` , damit `HeightRequest` [`WebView`](xref:Xamarin.Forms.WebView) der HTML-Inhalt angezeigt wird. Dies hängt vom Layout ab, von dem untergeordnet `WebView` ist. Dies ist z. b. in einer erforderlich [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

### <a name="local-html-content"></a>Lokaler HTML-Inhalt

In WebView können Inhalte aus HTML, CSS und JavaScript angezeigt werden, die in der APP eingebettet sind. Beispiel:

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

Sum

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Beachten Sie, dass die im obigen CSS angegebenen Schriftarten für jede Plattform angepasst werden müssen, da nicht jede Plattform die gleichen Schriftarten aufweist.

Zum Anzeigen von lokalem Inhalt mit einem `WebView` müssen Sie die HTML-Datei wie jede andere öffnen und dann den Inhalt als Zeichenfolge in die- `Html` Eigenschaft eines laden `HtmlWebViewSource` . Weitere Informationen zum Öffnen von Dateien finden Sie unter [Arbeiten mit Dateien](~/xamarin-forms/data-cloud/data/files.md).

Die folgenden Screenshots zeigen das Ergebnis der Anzeige von lokalem Inhalt auf den einzelnen Plattformen:

![Anzeigen von lokalem Inhalt in WebView](webview-images/local-content.png)

Obwohl die erste Seite geladen wurde, `WebView` hat nicht wissen, woher das HTML stammt. Dies ist ein Problem beim Umgang mit Seiten, die auf lokale Ressourcen verweisen. Beispiele dafür sind, wann lokale Seiten miteinander verknüpft werden können, eine Seite verwendet eine separate JavaScript-Datei oder eine Seite mit einem CSS-Stylesheet.  

Um dieses Problem zu beheben, müssen Sie angeben, `WebView` wo Dateien im Dateisystem zu finden sind. Dies geschieht durch Festlegen der- `BaseUrl` Eigenschaft für den, der `HtmlWebViewSource` von verwendet wird `WebView` .

Da das Dateisystem auf jedem der Betriebssysteme anders ist, müssen Sie diese URL auf jeder Plattform bestimmen. Xamarin.Formsmacht den `DependencyService` zum Auflösen von Abhängigkeiten zur Laufzeit auf jeder Plattform verfügbar.

Um das zu verwenden `DependencyService` , definieren Sie zunächst eine Schnittstelle, die auf jeder Plattform implementiert werden kann:

```csharp
public interface IBaseUrl { string Get(); }
```

Beachten Sie, dass die APP nicht ausgeführt wird, bis die Schnittstelle auf jeder Plattform implementiert wird. Stellen Sie im Common-Projekt sicher, dass Sie die `BaseUrl` mit dem Festlegen `DependencyService` :

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Es müssen Implementierungen der-Schnittstelle für jede Plattform bereitgestellt werden.

#### <a name="ios"></a>iOS

Unter IOS sollte sich der Webinhalt im Stammverzeichnis des Projekts oder im **Ressourcen** Verzeichnis mit der Buildaktion *bundleresource*befinden, wie unten gezeigt:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Lokale Dateien unter IOS](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Lokale Dateien unter IOS](webview-images/ios-xs.png)

-----

Muss `BaseUrl` auf den Pfad des Haupt Bündels festgelegt werden:

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

Unter Android sollte auf `BaseUrl` festgelegt werden `"file:///android_asset/"` :

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

Unter Android können auf Dateien im Ordner " **Assets** " auch über den aktuellen Android-Kontext zugegriffen werden, der von der-Eigenschaft verfügbar gemacht wird `MainActivity.Instance` :

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Platzieren Sie in universelle Windows-Plattform (UWP)-Projekten HTML-, CSS-und Bilddateien im Stammverzeichnis des Projekts, und legen Sie die Buildaktion auf *Content*fest.

Muss `BaseUrl` auf festgelegt werden `"ms-appx-web:///"` :

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

WebView unterstützt die Navigation durch verschiedene Methoden und Eigenschaften, die es zur Verfügung stellt:

- **GoForward ()** &ndash; Wenn `CanGoForward` true ist, wird `GoForward` bei Aufrufen von navigiert vorwärts zur nächsten besuchten Seite aufgerufen.
- **GoBack ()** &ndash; Wenn `CanGoBack` den Wert true hat, `GoBack` wird der Aufruf von zur letzten besuchten Seite navigiert.
- **CanGoBack** &ndash; `true`, wenn Seiten vorhanden sind, zu denen navigiert werden soll, `false` Wenn der Browser an der Start-URL steht.
- **CanGoForward** &ndash; , `true` Wenn der Benutzer rückwärts navigiert ist und mit einer bereits besuchten Seite fortfahren kann.

Innerhalb von Seiten `WebView` unterstützt keine Multitouch-Gesten. Es ist wichtig sicherzustellen, dass der Inhalt Mobil optimiert ist und ohne Zoom Vorgang angezeigt wird.

Es ist üblich, dass Anwendungen einen Link in einem `WebView` anstelle des Geräte Browsers anzeigen. In diesen Situationen ist es hilfreich, die normale Navigation zuzulassen. wenn der Benutzer jedoch zurückkommt, während er sich auf der Start Verknüpfung befindet, sollte die APP zur normalen App-Ansicht zurückkehren.

Verwenden Sie die integrierten Navigationsmethoden und-Eigenschaften, um dieses Szenario zu aktivieren.

Erstellen Sie zunächst die Seite für die Browseransicht:

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

## <a name="events"></a>Events

WebView löst die folgenden Ereignisse aus, um Sie bei der Reaktion auf Zustandsänderungen zu unterstützen:

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)das –-Ereignis wird ausgelöst, wenn die WebView mit dem Laden einer neuen Seite beginnt.
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated)–-Ereignis, das ausgelöst wird, wenn die Seite geladen und die Navigation beendet wurde.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested)das –-Ereignis wird ausgelöst, wenn eine Anforderung zum erneuten Laden des aktuellen Inhalts erfolgt.

Das [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) Objekt, das das [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) Ereignis begleitet, verfügt über vier Eigenschaften:

- `Cancel`– Gibt an, ob die Navigation abgebrochen werden soll.
- `NavigationEvent`– Das aufgelöbene Navigations Ereignis.
- `Source`– das Element, das die Navigation ausgeführt hat.
- `Url`– das Navigations Ziel.

Das [`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs) Objekt, das das [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) Ereignis begleitet, verfügt über vier Eigenschaften:

- `NavigationEvent`– Das aufgelöbene Navigations Ereignis.
- `Result`– Beschreibt das Ergebnis der Navigation mithilfe eines- [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) Enumerationsmembers. Gültige Werte sind `Cancel`, `Failure`, `Success` und `Timeout`.
- `Source`– das Element, das die Navigation ausgeführt hat.
- `Url`– das Navigations Ziel.

Wenn Sie die Verwendung von Webseiten erwarten, die viel Zeit zum Laden benötigen, sollten Sie die [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) -und-Ereignisse verwenden, [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) um einen Status Indikator zu implementieren. Beispiel:

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

Die beiden Ereignishandler:

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

Dies führt zu folgender Ausgabe (laden):

![Beispiel für WebView-Navigations Ereignis](webview-images/loading-start.png)

Ladevorgang abgeschlossen:

![Beispiel für WebView-Navigations Ereignis](webview-images/loading-end.png)

## <a name="reloading-content"></a>Inhalt wird erneut geladen

[`WebView`](xref:Xamarin.Forms.WebView)verfügt über eine- `Reload` Methode, die zum erneuten Laden des aktuellen Inhalts verwendet werden kann:

```csharp
var webView = new WebView();
...
webView.Reload();
```

Wenn die- `Reload` Methode aufgerufen wird `ReloadRequested` , wird das-Ereignis ausgelöst, das angibt, dass eine Anforderung zum erneuten Laden des aktuellen Inhalts ausgegeben wurde.

## <a name="performance"></a>Leistung

Gängige Webbrowser übernehmen Technologien wie Hardware beschleunigtes Rendering und JavaScript-Kompilierung. Vor Xamarin.Forms 4,4 Xamarin.Forms `WebView` wurde der von der-Klasse unter IOS implementiert `UIWebView` . Viele dieser Technologien waren in dieser Implementierung jedoch nicht verfügbar. Seit Xamarin.Forms 4,4 Xamarin.Forms `WebView` wird das von der-Klasse unter IOS implementiert `WkWebView` , das schnelleres Durchsuchen unterstützt.

> [!NOTE]
> Unter IOS verfügt über `WkWebViewRenderer` eine Konstruktorüberladung, die ein- `WkWebViewConfiguration` Argument akzeptiert. Dadurch kann der Renderer bei der Erstellung konfiguriert werden.

Eine Anwendung kann `UIWebView` Xamarin.Forms aus Kompatibilitätsgründen zur Verwendung der IOS-Klasse zurückkehren, um zu implementieren `WebView` . Dies kann erreicht werden, indem Sie der Datei **AssemblyInfo.cs** im IOS-Platt Form Projekt für die Anwendung den folgenden Code hinzufügen:

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView`unter Android ist standardmäßig so schnell wie der integrierte Browser.

Die [UWP-Webansicht](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) verwendet die Microsoft Edge-Rendering-Engine. Auf Desktop-und Tablet-Geräten sollte die gleiche Leistung wie bei der Verwendung des Edge-Browsers selbst angezeigt werden.

## <a name="permissions"></a>Berechtigungen

Damit Ordnungs `WebView` gemäß funktioniert, müssen Sie sicherstellen, dass für jede Plattform Berechtigungen festgelegt sind. Beachten Sie, dass auf manchen Plattformen `WebView` im Debugmodus funktioniert, aber nicht, wenn Sie für Release erstellt werden. Dies liegt daran, dass einige Berechtigungen, wie z. b. die für den Internet Zugriff unter Android, standardmäßig durch Visual Studio für Mac im Debugmodus festgelegt werden.

- **UWP** &ndash; erfordert die Internet Funktion (Client & Server), wenn Netzwerk Inhalte angezeigt werden.
- **Android** &ndash; erfordert `INTERNET` nur, wenn Inhalt aus dem Netzwerk angezeigt wird. Lokale Inhalte erfordern keine besonderen Berechtigungen.
- **IOS** &ndash; erfordert keine besonderen Berechtigungen.

## <a name="layout"></a>Layout

Im Gegensatz zu den meisten anderen Xamarin.Forms Sichten `WebView` erfordert, dass `HeightRequest` und `WidthRequest` angegeben werden, wenn Sie in Stacklayout oder relativelayout enthalten sind. Wenn Sie diese Eigenschaften nicht angeben, `WebView` wird nicht angezeigt.

In den folgenden Beispielen werden Layouts veranschaulicht, die zur Funktionsweise von Rendern von `WebView` s führen:

Stacklayout mit widthrequest & "Erhöhung trequest":

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

Relativelayout mit widthrequest & Erhöhung trequest:

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

Raster *ohne* widthrequest & Erhöhung trequest. Raster ist eines der wenigen Layouts, bei dem keine angeforderte Höhe und Breite angegeben werden müssen:

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

[`WebView`](xref:Xamarin.Forms.WebView)bietet die Möglichkeit, eine JavaScript-Funktion aus c# aufzurufen und jedes Ergebnis an den aufrufenden c#-Code zurückzugeben. Dies erfolgt mit der- [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Methode, die im folgenden Beispiel aus dem [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) -Beispiel gezeigt wird:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

Die [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) -Methode wertet das JavaScript aus, das als-Argument angegeben ist, und gibt jedes Ergebnis als zurück `string` . In diesem Beispiel wird die `factorial` JavaScript-Funktion aufgerufen, die die Fakultät von `number` als Ergebnis zurückgibt. Diese JavaScript-Funktion wird in der lokalen HTML-Datei definiert, die [`WebView`](xref:Xamarin.Forms.WebView) lädt, und wird im folgenden Beispiel gezeigt:

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

## <a name="cookies"></a>Cookies

Cookies können für einen festgelegt werden [`WebView`](xref:Xamarin.Forms.WebView) , die dann mit der Webanforderung an die angegebene URL gesendet werden. Dies wird durch das Hinzufügen von `Cookie` Objekten zu einem erreicht `CookieContainer` , das dann als Wert der `WebView.Cookies` bindbaren Eigenschaft festgelegt wird. Im folgenden Code wird ein Beispiel hierfür dargestellt:

```csharp
using System.Net;
using Xamarin.Forms;
// ...

CookieContainer cookieContainer = new CookieContainer();
Uri uri = new Uri("https://dotnet.microsoft.com/apps/xamarin", UriKind.RelativeOrAbsolute);

Cookie cookie = new Cookie
{
    Name = "XamarinCookie",
    Expires = DateTime.Now.AddDays(1),
    Value = "My cookie",
    Domain = uri.Host,
    Path = "/"
};
cookieContainer.Add(uri, cookie);
webView.Cookies = cookieContainer;
webView.Source = new UrlWebViewSource { Url = uri.ToString() };
```

In diesem Beispiel wird dem-Objekt ein einzelnes- `Cookie` Objekt hinzugefügt `CookieContainer` , das dann als Wert der-Eigenschaft festgelegt wird `WebView.Cookies` . Wenn [`WebView`](xref:Xamarin.Forms.WebView) eine Webanforderung an die angegebene URL sendet, wird das Cookie mit der Anforderung gesendet.

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView depreation und App Store-Ablehnung (iTMS-90809)

Ab dem 2020 werden apps, die weiterhin die veraltete API verwenden, von [Apple abgelehnt](https://developer.apple.com/news/?id=12232019b) `UIWebView` . Während Xamarin.Forms auf als Standardwert umgestellt hat `WKWebView` , gibt es noch einen Verweis auf das ältere SDK in den Xamarin.Forms Binärdateien. Das aktuelle [IOS](~/ios/deploy-test/linker.md) -linkerverhalten entfernt das nicht, und daher wird die veraltete `UIWebView` API nach wie vor von Ihrer APP referenziert, wenn Sie Sie an den App Store übermitteln.

Eine Vorschauversion des Linkers ist verfügbar, um dieses Problem zu beheben. Um die Vorschau zu aktivieren, müssen Sie ein zusätzliches Argument `--optimize=experimental-xforms-product-type` für den Linker angeben.

Hierfür müssen folgende Voraussetzungen erfüllt sein:

- ** Xamarin.Forms 4,5 oder höher**. Xamarin.Forms4,6 oder höher ist erforderlich, wenn Ihre APP Material Visualisierung verwendet.
- **Xamarin. IOS 13.10.0.17 oder höher**. Überprüfen Sie die xamarin. IOS-Version [in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#version-information). Diese Version von xamarin. IOS ist in Visual Studio für Mac 8.4.1 und Visual Studio 16.4.3 enthalten.
- **Entfernen Sie Verweise `UIWebView` auf **. Der Code darf keine Verweise auf `UIWebView` oder Klassen aufweisen, die verwenden `UIWebView` .

Weitere Informationen zum erkennen und Entfernen von `UIWebView` Verweisen finden Sie unter " [UIWebView depreation](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)".

### <a name="configure-the-linker"></a>Konfigurieren des Linkers

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Führen Sie die folgenden Schritte aus, damit der Linker Verweise entfernen muss `UIWebView` :

1. **IOS-Projekteigenschaften öffnen** &ndash; Klicken Sie mit der rechten Maustaste auf Ihr IOS-Projekt und wählen Sie **Eigenschaften**
1. **Navigieren Sie zum Abschnitt** &ndash; IOS-Build. Wählen Sie den Abschnitt **IOS-Build** aus.
1. **Aktualisieren der zusätzlichen mtouchscreen-Argumente** &ndash; Fügen Sie in den **zusätzlichen mberührungs-Argumenten** dieses Flag hinzu `--optimize=experimental-xforms-product-type` (zusätzlich zu allen Werten, die möglicherweise bereits vorhanden sind). Hinweis: dieses Flag funktioniert zusammen mit dem **linkerverhalten** , das nur auf das **SDK** oder **alle verknüpfen**festgelegt ist. Wenn Sie aus irgendeinem Grund Fehler sehen, wenn Sie das linkerverhalten auf alle festlegen, ist dies wahrscheinlich ein Problem innerhalb des App-Codes oder einer Drittanbieter Bibliothek, die nicht Linker sicher ist. Weitere Informationen zum Linker finden Sie unter [Verknüpfen von xamarin. IOS-apps](~/ios/deploy-test/linker.md).
1. **Alle Buildkonfigurationen aktualisieren** &ndash; Verwenden Sie die Listen **Konfiguration** und **Plattform** am oberen Rand des Fensters, um alle Buildkonfigurationen zu aktualisieren. Die wichtigste zu Aktualisier dende Konfiguration ist die **Release/iPhone-** Konfiguration, da diese normalerweise zum Erstellen von Builds für die App Store-Übermittlung verwendet wird.

Sie können das Fenster mit dem neuen Flag in diesem Screenshot sehen:

[![Festlegen des Flags im Abschnitt IOS-Build](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Führen Sie die folgenden Schritte aus, damit der Linker Verweise entfernen muss `UIWebView` :

1. Optionen für das **IOS-Projekt öffnen** &ndash; Klicken Sie mit der rechten Maustaste auf Ihr IOS-Projekt, **und wählen Sie**
1. **Navigieren Sie zum Abschnitt** &ndash; IOS-Build. Wählen Sie den Abschnitt **IOS-Build** aus.
1. **Aktualisieren Sie die zusätzlichen _mberührungs_ -Argumente** &ndash; in den **zusätzlichen _mberührungs_ -Argumenten** fügen Sie dieses Flag hinzu `--optimize=experimental-xforms-product-type` (zusätzlich zu allen Werten, die möglicherweise bereits vorhanden sind). Hinweis: dieses Flag funktioniert zusammen mit dem **linkerverhalten** , das nur auf das **SDK** oder **alle verknüpfen**festgelegt ist. Wenn Sie aus irgendeinem Grund Fehler sehen, wenn Sie das linkerverhalten auf alle festlegen, ist dies wahrscheinlich ein Problem innerhalb des App-Codes oder einer Drittanbieter Bibliothek, die nicht Linker sicher ist. Weitere Informationen zum Linker finden Sie unter [Verknüpfen von xamarin. IOS-apps](~/ios/deploy-test/linker.md).
1. **Alle Buildkonfigurationen aktualisieren** &ndash; Verwenden Sie die Listen **Konfiguration** und **Plattform** am oberen Rand des Fensters, um alle Buildkonfigurationen zu aktualisieren. Die wichtigste zu Aktualisier dende Konfiguration ist die **Release/iPhone-** Konfiguration, da diese normalerweise zum Erstellen von Builds für die App Store-Übermittlung verwendet wird.

Sie können das Fenster mit dem neuen Flag in diesem Screenshot sehen:

[![Festlegen des Flags im Abschnitt IOS-Build](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

Wenn Sie nun einen neuen (Release) Build erstellen und an den App Store senden, sollten keine Warnungen zu der veralteten API vorliegen.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit WebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
- [UIWebView-Veraltung](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)
