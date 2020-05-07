---
title: Xamarin. Forms-WebView
description: In diesem Artikel wird erläutert, wie Sie die xamarin. Forms-WebView-Klasse verwenden, um Benutzern lokale oder Netzwerk-Webinhalte und-Dokumente zu präsentieren.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2020
ms.openlocfilehash: 31b705a51e405285cc5eaae391dd0794bfacfd9c
ms.sourcegitcommit: 443ecd9146fe2a7bbb9b5ab6d33c835876efcf1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82852491"
---
# <a name="xamarinforms-webview"></a>Xamarin. Forms-WebView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)ist eine Ansicht für die Anzeige von Web-und HTML-Inhalt in Ihrer APP:

![Im App-Browser](webview-images/in-app-browser.png)

## <a name="content"></a>Inhalt

`WebView`unterstützt die folgenden Inhaltstypen:

- HTML & CSS Websites &ndash; WebView bietet vollständige Unterstützung für Websites, die mithilfe von HTML-& CSS geschrieben wurden, einschließlich JavaScript-Unterstützung
- Dokumente &ndash; , da WebView mit nativen Komponenten auf jeder Plattform implementiert ist, kann WebView Dokumente anzeigen, die auf jeder Plattform sichtbar sind. Dies bedeutet, dass PDF-Dateien unter IOS und Android funktionieren.
- HTML- &ndash; Zeichen folgen WebView kann HTML-Zeichen folgen im Arbeitsspeicher anzeigen.
- Lokale Dateien &ndash; WebView kann jeden der oben in der APP eingebetteten Inhaltstypen darstellen.

> [!NOTE]
> `WebView`unter Windows unterstützt Silverlight, Flash oder ActiveX-Steuerelemente nicht, auch wenn Sie von Internet Explorer auf dieser Plattform unterstützt werden.

### <a name="websites"></a>Websites

Legen Sie die-Eigenschaft des `WebView`s auf eine Zeichen folgen [`Source`](xref:Xamarin.Forms.WebViewSource) -URL fest, um eine Website aus dem Internet anzuzeigen:

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URLs müssen vollständig mit dem angegebenen Protokoll formatiert sein (d. h., es muss "http://" oder "https://" vorangesteht).

#### <a name="ios-and-ats"></a>IOS und ATS

Seit Version 9 ermöglicht IOS nur der Anwendung die Kommunikation mit Servern, die standardmäßig die Best Practice-Sicherheit implementieren. Werte müssen in `Info.plist` festgelegt werden, um die Kommunikation mit unsicheren Servern zu ermöglichen.

> [!NOTE]
> Wenn Ihre Anwendung eine Verbindung mit einer unsicheren Website erfordert, sollten Sie die Domäne immer als Ausnahme mit `NSExceptionDomains` eingeben, anstatt sie vollständig mithilfe `NSAllowsArbitraryLoads`von zu deaktivieren. `NSAllowsArbitraryLoads`sollte nur in extrem Notfällen verwendet werden.

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

Im obigen Code `@` wird verwendet, um den HTML-Code als [ausführlichen Zeichenfolgenliterals](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)zu markieren, was bedeutet, dass die meisten Escapezeichen ignoriert werden

> [!NOTE]
> Es kann erforderlich sein, `WidthRequest` die-Eigenschaft `HeightRequest` und die- [`WebView`](xref:Xamarin.Forms.WebView) Eigenschaft von festzulegen, damit der HTML-Inhalt angezeigt `WebView` wird. Dies hängt vom Layout ab, von dem untergeordnet ist. Dies ist z. b. in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)erforderlich.

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

Zum Anzeigen von lokalem Inhalt `WebView`mit einem müssen Sie die HTML-Datei wie jede andere öffnen und dann den Inhalt als Zeichenfolge in die `Html` -Eigenschaft eines laden `HtmlWebViewSource`. Weitere Informationen zum Öffnen von Dateien finden Sie unter [Arbeiten mit Dateien](~/xamarin-forms/data-cloud/data/files.md).

Die folgenden Screenshots zeigen das Ergebnis der Anzeige von lokalem Inhalt auf den einzelnen Plattformen:

![Anzeigen von lokalem Inhalt in WebView](webview-images/local-content.png)

Obwohl die erste Seite geladen wurde, `WebView` hat nicht wissen, woher das HTML stammt. Dies ist ein Problem beim Umgang mit Seiten, die auf lokale Ressourcen verweisen. Beispiele dafür sind, wann lokale Seiten miteinander verknüpft werden können, eine Seite verwendet eine separate JavaScript-Datei oder eine Seite mit einem CSS-Stylesheet.  

Um dieses Problem zu beheben, müssen Sie angeben `WebView` , wo Dateien im Dateisystem zu finden sind. Dies geschieht durch Festlegen der `BaseUrl` -Eigenschaft für `HtmlWebViewSource` den, der `WebView`von verwendet wird.

Da das Dateisystem auf jedem der Betriebssysteme anders ist, müssen Sie diese URL auf jeder Plattform bestimmen. Xamarin. Forms `DependencyService` macht zum Auflösen von Abhängigkeiten zur Laufzeit auf jeder Plattform verfügbar.

Um das `DependencyService`zu verwenden, definieren Sie zunächst eine Schnittstelle, die auf jeder Plattform implementiert werden kann:

```csharp
public interface IBaseUrl { string Get(); }
```

Beachten Sie, dass die APP nicht ausgeführt wird, bis die Schnittstelle auf jeder Plattform implementiert wird. Stellen Sie im Common-Projekt sicher, dass Sie die `BaseUrl` mit dem `DependencyService`festlegen:

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

`BaseUrl` Muss auf den Pfad des Haupt Bündels festgelegt werden:

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

Unter Android `BaseUrl` sollte auf festgelegt werden `"file:///android_asset/"`:

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

Unter Android können auf Dateien im Ordner " **Assets** " auch über den aktuellen Android-Kontext zugegriffen werden, der von der `MainActivity.Instance` -Eigenschaft verfügbar gemacht wird:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Platzieren Sie in universelle Windows-Plattform (UWP)-Projekten HTML-, CSS-und Bilddateien im Stammverzeichnis des Projekts, und legen Sie die Buildaktion auf *Content*fest.

`BaseUrl` Muss auf `"ms-appx-web:///"`festgelegt werden:

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

- **GoForward ()** &ndash; Wenn `CanGoForward` true ist, wird `GoForward` navigiert vorwärts zur nächsten besuchten Seite aufgerufen.
- **GoBack ()** &ndash; Wenn `CanGoBack` true ist, wird `GoBack` der Aufruf von zur letzten besuchten Seite navigiert.
- &ndash; , `true` Wenn Seiten vorhanden sind, zu denen navigiert werden soll `false` **, wenn der** Browser an der Start-URL steht.
- **CanGoForward** &ndash; `true` , wenn der Benutzer rückwärts navigiert ist und zu einer bereits besuchten Seite wechseln kann.

Innerhalb von Seiten `WebView` unterstützt keine Multitouch-Gesten. Es ist wichtig sicherzustellen, dass der Inhalt Mobil optimiert ist und ohne Zoom Vorgang angezeigt wird.

Es ist üblich, dass Anwendungen einen Link in einem `WebView`anstelle des Geräte Browsers anzeigen. In diesen Situationen ist es hilfreich, die normale Navigation zuzulassen. wenn der Benutzer jedoch zurückkommt, während er sich auf der Start Verknüpfung befindet, sollte die APP zur normalen App-Ansicht zurückkehren.

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
- `Result`– Beschreibt das Ergebnis der Navigation mithilfe eines [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) -Enumerationsmembers. Gültige Werte sind `Cancel`, `Failure`, `Success` und `Timeout`.
- `Source`– das Element, das die Navigation ausgeführt hat.
- `Url`– das Navigations Ziel.

Wenn Sie die Verwendung von Webseiten erwarten, die viel Zeit zum Laden benötigen, sollten [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) Sie [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) die-und-Ereignisse verwenden, um einen Status Indikator zu implementieren. Beispiel:

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

[`WebView`](xref:Xamarin.Forms.WebView)verfügt über `Reload` eine-Methode, die zum erneuten Laden des aktuellen Inhalts verwendet werden kann:

```csharp
var webView = new WebView();
...
webView.Reload();
```

Wenn die `Reload` -Methode aufgerufen wird `ReloadRequested` , wird das-Ereignis ausgelöst, das angibt, dass eine Anforderung zum erneuten Laden des aktuellen Inhalts ausgegeben wurde.

## <a name="performance"></a>Leistung

Gängige Webbrowser übernehmen Technologien wie Hardware beschleunigtes Rendering und JavaScript-Kompilierung. Vor xamarin. Forms 4,4 wurde xamarin. Forms `WebView` in ios von der `UIWebView` -Klasse implementiert. Viele dieser Technologien waren in dieser Implementierung jedoch nicht verfügbar. Da xamarin. Forms 4,4 ist, wird xamarin. Forms `WebView` in ios von der `WkWebView` -Klasse implementiert, die schnelleres Durchsuchen unterstützt.

> [!NOTE]
> Unter IOS `WkWebViewRenderer` verfügt über eine Konstruktorüberladung, die ein `WkWebViewConfiguration` -Argument akzeptiert. Dadurch kann der Renderer bei der Erstellung konfiguriert werden.

Eine Anwendung kann aus Kompatibilitätsgründen die `UIWebView` Verwendung der IOS-Klasse zur Implementierung von xamarin. Forms `WebView`zurückgeben. Dies kann erreicht werden, indem Sie der Datei **AssemblyInfo.cs** im IOS-Platt Form Projekt für die Anwendung den folgenden Code hinzufügen:

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView`unter Android ist standardmäßig so schnell wie der integrierte Browser.

Die [UWP-Webansicht](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) verwendet die Microsoft Edge-Rendering-Engine. Auf Desktop-und Tablet-Geräten sollte die gleiche Leistung wie bei der Verwendung des Edge-Browsers selbst angezeigt werden.

## <a name="permissions"></a>Berechtigungen

Damit Ordnungs `WebView` gemäß funktioniert, müssen Sie sicherstellen, dass für jede Plattform Berechtigungen festgelegt sind. Beachten Sie, dass auf manchen `WebView` Plattformen im Debugmodus funktioniert, aber nicht, wenn Sie für Release erstellt werden. Dies liegt daran, dass einige Berechtigungen, wie z. b. die für den Internet Zugriff unter Android, standardmäßig durch Visual Studio für Mac im Debugmodus festgelegt werden.

- **UWP** &ndash; erfordert die Internet Funktion (Client & Server), wenn Netzwerk Inhalte angezeigt werden.
- **Android** &ndash; erfordert `INTERNET` nur, wenn Inhalt aus dem Netzwerk angezeigt wird. Lokale Inhalte erfordern keine besonderen Berechtigungen.
- **IOS** &ndash; erfordert keine besonderen Berechtigungen.

## <a name="layout"></a>Layout

Im Gegensatz zu den meisten anderen xamarin. `WebView` Forms- `HeightRequest` Ansichten `WidthRequest` erfordert, dass und angegeben werden, wenn Sie in Stacklayout oder relativelayout enthalten sind. Wenn Sie diese Eigenschaften nicht angeben, `WebView` wird nicht angezeigt.

In den folgenden Beispielen werden Layouts veranschaulicht, die zur Funktions `WebView`Weise von Rendern von s führen:

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

[`WebView`](xref:Xamarin.Forms.WebView)bietet die Möglichkeit, eine JavaScript-Funktion aus c# aufzurufen und jedes Ergebnis an den aufrufenden c#-Code zurückzugeben. Dies erfolgt mit der [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) -Methode, die im folgenden Beispiel aus dem [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) -Beispiel gezeigt wird:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

Die [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) -Methode wertet das JavaScript aus, das als-Argument angegeben ist, und gibt jedes `string`Ergebnis als zurück. In diesem Beispiel wird die `factorial` JavaScript-Funktion aufgerufen, die die Fakultät von `number` als Ergebnis zurückgibt. Diese JavaScript-Funktion wird in der lokalen HTML-Datei definiert [`WebView`](xref:Xamarin.Forms.WebView) , die lädt, und wird im folgenden Beispiel gezeigt:

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

Cookies können für einen [`WebView`](xref:Xamarin.Forms.WebView)festgelegt werden, die dann mit der Webanforderung an die angegebene URL gesendet werden. Dies wird durch das hinzu `Cookie` fügen von Objekten `CookieContainer`zu einem erreicht, das dann als Wert der `WebView.Cookies` bindbaren Eigenschaft festgelegt wird. Im folgenden Code wird ein Beispiel hierfür dargestellt:

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

In diesem Beispiel wird dem `Cookie` `CookieContainer` -Objekt ein einzelnes-Objekt hinzugefügt, das dann als Wert der- `WebView.Cookies` Eigenschaft festgelegt wird. Wenn eine [`WebView`](xref:Xamarin.Forms.WebView) Webanforderung an die angegebene URL sendet, wird das Cookie mit der Anforderung gesendet.

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView depreation und App Store-Ablehnung (iTMS-90809)

Ab dem 2020 [werden apps](https://developer.apple.com/news/?id=12232019b) , die weiterhin die veraltete `UIWebView` API verwenden, von Apple abgelehnt. Obwohl xamarin. Forms auf `WKWebView` den Standardwert umgestellt hat, gibt es noch einen Verweis auf das ältere SDK in den xamarin. Forms-Binärdateien. Das aktuelle [IOS](~/ios/deploy-test/linker.md) -linkerverhalten entfernt das nicht, und daher wird die veraltete `UIWebView` API nach wie vor von Ihrer APP referenziert, wenn Sie Sie an den App Store übermitteln.

Eine Vorschauversion des Linkers ist verfügbar, um dieses Problem zu beheben. Um die Vorschau zu aktivieren, müssen Sie ein zusätzliches Argument `--optimize=experimental-xforms-product-type` für den Linker angeben.

Hierfür müssen folgende Voraussetzungen erfüllt sein:

- **Xamarin. Forms 4,5 oder höher**. Xamarin. Forms 4,6 (oder höher) ist erforderlich, wenn Ihre APP Material Visualisierung verwendet.
- **Xamarin. IOS 13.10.0.17 oder höher**. Überprüfen Sie die xamarin. IOS-Version [in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#version-information). Diese Version von xamarin. IOS ist in Visual Studio für Mac 8.4.1 und Visual Studio 16.4.3 enthalten.
- **Entfernen Sie Verweise `UIWebView`auf **. Der `UIWebView` `UIWebView`Code darf keine Verweise auf oder Klassen aufweisen, die verwenden.

Weitere Informationen zum erkennen und entfernen `UIWebView` von Verweisen finden Sie unter " [UIWebView depreation](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)".

### <a name="configure-the-linker"></a>Konfigurieren des Linkers

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Führen Sie die folgenden Schritte aus, damit `UIWebView` der Linker Verweise entfernen muss:

1. Klicken Sie mit **der** &ndash; rechten Maustaste auf Ihr IOS-Projekt, und wählen Sie **Eigenschaften**aus.
1. Navigieren Sie &ndash; **zum Abschnitt IOS-Build, und** wählen Sie den Abschnitt **IOS-Build** aus.
1. **Aktualisieren Sie die zusätzlichen mberührungs-Argumente** &ndash; in den **zusätzlichen mberührungs** -Argumenten `--optimize=experimental-xforms-product-type` fügen Sie dieses Flag hinzu (zusätzlich zu allen Werten, die möglicherweise bereits vorhanden sind). Hinweis: dieses Flag funktioniert zusammen mit dem **linkerverhalten** , das nur auf das **SDK** oder **alle verknüpfen**festgelegt ist. Wenn Sie aus irgendeinem Grund Fehler sehen, wenn Sie das linkerverhalten auf alle festlegen, ist dies wahrscheinlich ein Problem innerhalb des App-Codes oder einer Drittanbieter Bibliothek, die nicht Linker sicher ist. Weitere Informationen zum Linker finden Sie unter [Verknüpfen von xamarin. IOS-apps](~/ios/deploy-test/linker.md).
1. **Aktualisieren Sie alle Buildkonfigurationen** &ndash; verwenden Sie die **Konfigurations** -und **Platt Form** Listen am oberen Rand des Fensters, um alle Buildkonfigurationen zu aktualisieren. Die wichtigste zu Aktualisier dende Konfiguration ist die **Release/iPhone-** Konfiguration, da diese normalerweise zum Erstellen von Builds für die App Store-Übermittlung verwendet wird.

Sie können das Fenster mit dem neuen Flag in diesem Screenshot sehen:

[![Festlegen des Flags im Abschnitt IOS-Build](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Führen Sie die folgenden Schritte aus, damit `UIWebView` der Linker Verweise entfernen muss:

1. Klicken Sie mit &ndash; **der rechten Maustaste auf das IOS-** Projekt, und wählen Sie **Optionen**aus.
1. Navigieren Sie &ndash; **zum Abschnitt IOS-Build, und** wählen Sie den Abschnitt **IOS-Build** aus.
1. **Aktualisieren Sie die zusätzlichen _mberührungs_ -Argumente** &ndash; in den **zusätzlichen _mberührungs_ -Argumenten** fügen Sie dieses Flag `--optimize=experimental-xforms-product-type` hinzu (zusätzlich zu allen Werten, die möglicherweise bereits vorhanden sind). Hinweis: dieses Flag funktioniert zusammen mit dem **linkerverhalten** , das nur auf das **SDK** oder **alle verknüpfen**festgelegt ist. Wenn Sie aus irgendeinem Grund Fehler sehen, wenn Sie das linkerverhalten auf alle festlegen, ist dies wahrscheinlich ein Problem innerhalb des App-Codes oder einer Drittanbieter Bibliothek, die nicht Linker sicher ist. Weitere Informationen zum Linker finden Sie unter [Verknüpfen von xamarin. IOS-apps](~/ios/deploy-test/linker.md).
1. **Aktualisieren Sie alle Buildkonfigurationen** &ndash; verwenden Sie die **Konfigurations** -und **Platt Form** Listen am oberen Rand des Fensters, um alle Buildkonfigurationen zu aktualisieren. Die wichtigste zu Aktualisier dende Konfiguration ist die **Release/iPhone-** Konfiguration, da diese normalerweise zum Erstellen von Builds für die App Store-Übermittlung verwendet wird.

Sie können das Fenster mit dem neuen Flag in diesem Screenshot sehen:

[![Festlegen des Flags im Abschnitt IOS-Build](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

Wenn Sie nun einen neuen (Release) Build erstellen und an den App Store senden, sollten keine Warnungen zu der veralteten API vorliegen.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit WebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
- [UIWebView-Veraltung](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)
