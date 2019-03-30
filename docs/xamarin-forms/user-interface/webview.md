---
title: Xamarin.Forms-WebView
description: In diesem Artikel erläutert, wie Sie die Xamarin.Forms-WebView-Klasse verwenden, um lokale darzustellen oder Netzwerk-Web-Inhalte und Dokumente für Benutzer.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2019
ms.openlocfilehash: 658ce23b0aaced8e195461a485f3e846900c2026
ms.sourcegitcommit: 236a346838c421c7d8951f50abbf4f5365559372
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58641451"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms-WebView

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)

[`WebView`](xref:Xamarin.Forms.WebView) ist eine Ansicht zum Anzeigen von Web- und HTML-Inhalt in Ihrer app. Im Gegensatz zu `OpenUri`, der den Benutzer an dem Webbrowser auf dem Gerät akzeptiert `WebView` zeigt die HTML-Inhalt in Ihrer app.

![](webview-images/in-app-browser.png "In App-Browser")

## <a name="content"></a>Content

`WebView` unterstützt die folgenden Arten von Inhalten:

- HTML und CSS-Websites &ndash; WebView bietet vollständige Unterstützung für Websites mit HTML und CSS, einschließlich Unterstützung für JavaScript geschrieben.
- Dokumente &ndash; da WebView mit nativen Komponenten auf jeder Plattform implementiert wird, ist WebView mit Dokumenten, die auf jeder Plattform angezeigt werden kann. Das heißt, PDF-Dateien unter iOS und Android arbeiten.
- HTML-Zeichenfolgen &ndash; WebView kann HTML-Zeichenfolgen aus dem Arbeitsspeicher anzeigen.
- Lokale Dateien &ndash; WebView kann einer der oben aufgeführten Inhaltstypen eingebettet in der app darstellen.

> [!NOTE]
> `WebView` für Windows unterstützt Silverlight, Flash oder alle ActiveX-Steuerelemente, nicht, selbst wenn sie von Internet Explorer auf dieser Plattform unterstützt werden.

### <a name="websites"></a>Websites

Um eine Website aus dem Internet anzuzeigen, legen die `WebView`des [ `Source` ](xref:Xamarin.Forms.WebViewSource) Eigenschaft, um einen Zeichenfolgen-URL:

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URLs müssen vollständig mit dem angegebenen Protokoll gebildet werden (d. h. sie müssen "http://" oder "https://" vorangestellt ist).

#### <a name="ios-and-ats"></a>iOS und ATS

Seit Version 9 lässt iOS nur Ihre Anwendung zur Kommunikation mit Servern, die optimale Sicherheit standardmäßig zu implementieren. Werte müssen festgelegt werden, `Info.plist` eine Kommunikation mit unsicheren Servern zu ermöglichen.

> [!NOTE]
> Wenn Ihre Anwendung eine Verbindung mit einer nicht sicheren Website erfordert, sollten Sie immer die Domäne eingeben, als eine Ausnahme mit `NSExceptionDomains` deaktivieren, dass ATS vollständig mit `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` sollte nur im Notfall extrem verwendet werden.

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

Finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md) für Weitere Informationen zu diesem neuen Feature in iOS 9.

### <a name="html-strings"></a>HTML-Zeichenfolgen

Wenn Sie eine Zeichenfolge mit HTML, die dynamisch im Code definierte präsentieren möchten, müssen Sie zum Erstellen einer Instanz von [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "WebView anzeigen HTML-Zeichenfolge")

Im obigen Code `@` gekennzeichnet, den HTML-Code als Zeichenfolge literal, d. h. die üblichen Escapezeichen ignoriert werden.

### <a name="local-html-content"></a>Lokale HTML-Inhalt

WebView kann Inhalte aus HTML-, CSS- und Javascript innerhalb der app eingebettet. Zum Beispiel:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
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

Mit Anzeige lokalen Content eine `WebView`, müssen Sie wie jede andere HTML-Datei zu öffnen, und klicken Sie dann laden den Inhalt als Zeichenfolge in die `Html` Eigenschaft eine `HtmlWebViewSource`. Weitere Informationen zum Öffnen von Dateien, finden Sie unter [arbeiten mit Dateien](~/xamarin-forms/app-fundamentals/files.md).

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von lokalen Inhalten auf jeder Plattform:

![](webview-images/local-content.png "WebView Anzeigen von lokalen Inhalten")

Obwohl es sich bei die erste Seite geladen wurde, die `WebView` hat keine Kenntnis der Ursprung des HTML-Codes aus. Das ist ein Problem bei Seiten, die auf lokale Ressourcen zu verweisen. Beispiele für wann das passieren kann, sind beim lokalen Seiten-Link, der auf jede andere, einer Seite ist eine separate JavaScript-Datei verwenden oder eine Seite enthält zu einer CSS-Stylesheet links.  

Um dieses Problem zu beheben, müssen Sie teilen das `WebView` , wo die Dateien im Dateisystem zu finden. Sie tun, indem die `BaseUrl` Eigenschaft der `HtmlWebViewSource` ein, die die `WebView`.

Da das Dateisystem auf jedem der Betriebssysteme unterschiedlich ist, müssen Sie ermitteln dieser URL auf jeder Plattform. Xamarin.Forms macht die `DependencyService` zum Auflösen von Abhängigkeiten zur Laufzeit auf jeder Plattform.

Verwenden der `DependencyService`, definieren Sie zunächst eine Schnittstelle, die auf jeder Plattform implementiert werden kann:

```csharp
public interface IBaseUrl { string Get(); }
```

Beachten Sie, bis die Schnittstelle auf jeder Plattform implementiert wird, die die app nicht ausgeführt werden. Im gemeinsamen Projekt sicher, dass Sie daran denken, legen Sie die `BaseUrl` mithilfe der `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Implementierungen der Schnittstelle für jede Plattform müssen bereitgestellt werden.

#### <a name="ios"></a>iOS

Unter iOS, sollten die Webinhalte im Stammverzeichnis des Projekts gespeichert werden oder **Ressourcen** Verzeichnis mit Buildaktion *BundleResource*, wie unten dargestellt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](webview-images/ios-vs.png "Lokale Dateien unter iOS")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](webview-images/ios-xs.png "Lokale Dateien unter iOS")

-----

Die `BaseUrl` sollte auf den Pfad des Bündels main festgelegt werden:

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

Unter Android, platzieren Sie HTML, CSS und Bilder im Ordner "Assets" mit Buildaktion *AndroidAsset* wie unten dargestellt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](webview-images/android-vs.png "Lokale Dateien unter Android")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](webview-images/android-xs.png "Lokale Dateien unter Android")

-----

Unter Android die `BaseUrl` sollte festgelegt werden, um `"file:///android_asset/"`:

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

Unter Android werden Dateien mit einer der **Assets** Ordner kann auch über die aktuelle Android-Kontext, der verfügbar gemacht werden, zugegriffen werden die `MainActivity.Instance` Eigenschaft:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Projekte der universellen Windows-Plattform (UWP), setzen Sie HTML, CSS und Bilder in das Stammverzeichnis des Projekts für den Buildaktion festgelegt *Content*.

Die `BaseUrl` sollte festgelegt werden, um `"ms-appx-web:///"`:

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

- **GoForward()** &ndash; Wenn `CanGoForward` ist "true", Aufrufen von `GoForward` wechselt, weiter zur nächsten Seite besucht.
- **GoBack()** &ndash; Wenn `CanGoBack` ist "true", Aufrufen von `GoBack` zur letzten besuchte Seite navigiert.
- **CanGoBack** &ndash; `true` treten Seiten navigieren Sie zurück zur `false` Wenn der Browser auf die Start-URL ist.
- **CanGoForward** &ndash; `true` Wenn der Benutzer rückwärts navigiert ist und Schritt weitergehen, um eine Seite, die bereits besucht wurde.

Seiten `WebView` Multitouch-Gesten nicht unterstützt. Es ist wichtig, um sicherzustellen, dass der Inhalt Mobilgeräte optimierte und angezeigt wird, ohne die Notwendigkeit, zoomen.

Es ist üblich für Anwendungen, um einen Link in zeigen eine `WebView`, statt die der Browser des Geräts. In diesen Fällen erhalten sie eignet sich zum normalen Navigation zu ermöglichen, aber, wenn die Benutzer-Treffer sichern, während sie auf den Link, der gestartet werden, sollte die app auf die normalen app-Ansicht zurückgeben.

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

![](webview-images/in-app-browser.png "WebView-Navigationsschaltflächen")

## <a name="events"></a>Ereignisse

WebView löst die folgenden Ereignisse ein, damit Sie auf statusänderungen reagieren können:

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) – Ereignis wird ausgelöst, wenn die Webansicht beginnt mit dem Laden einer neuen Seite.
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) – Ereignis ausgelöst, wenn die Seite geladen wird und die Navigation wurde beendet.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested) – Ereignis wird ausgelöst, wenn eine Anforderung erfolgt, um den aktuellen Inhalt erneut zu laden.

Die [ `WebNavigatingEventArgs` ](xref:Xamarin.Forms.WebNavigatingEventArgs) Objekt an, die mit der [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) Ereignis verfügt über vier Eigenschaften:

- `Cancel` – Gibt an, ob zum Abbrechen der Navigation.
- `NavigationEvent` – das Navigationsereignis, das ausgelöst wurde.
- `Source` – Das Element, das die Navigation ausgeführt.
- `Url` – das Navigationsziel.

Die [ `WebNavigatedEventArgs` ](xref:Xamarin.Forms.WebNavigatedEventArgs) Objekt an, die mit der [ `Navigated` ](xref:Xamarin.Forms.WebView.Navigated) Ereignis verfügt über vier Eigenschaften:

- `NavigationEvent` – das Navigationsereignis, das ausgelöst wurde.
- `Result` – Beschreibt das Ergebnis der Navigation mithilfe einer [ `WebNavigationResult` ](xref:Xamarin.Forms.WebNavigationResult) -Enumerationsmember. Gültige Werte sind `Cancel`, `Failure`, `Success` und `Timeout`.
- `Source` – Das Element, das die Navigation ausgeführt.
- `Url` – das Navigationsziel.

Wenn Sie Webseiten, die eine lange Zeit zum Laden verwenden voraussichtlich, sollten Sie verwenden die [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) und [ `Navigated` ](xref:Xamarin.Forms.WebView.Navigated) Ereignisse an einen Statusindikator zu implementieren. Zum Beispiel:

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

![](webview-images/loading-start.png "Beispiel WebView navigieren")

Fertig geladen:

![](webview-images/loading-end.png "WebView navigiert, Beispiel")

## <a name="reloading-content"></a>Das erneute Laden Inhalt

[`WebView`](xref:Xamarin.Forms.WebView) verfügt über eine `Reload` -Methode, die verwendet werden kann, um den aktuellen Inhalt erneut zu laden:

```csharp
var webView = new WebView();
...
webView.Reload();
```

Wenn die `Reload` Methode wird aufgerufen, die `ReloadRequested` Ereignis ausgelöst wird, gibt an, dass eine Anforderung gestellt wurde, um den aktuellen Inhalt erneut zu laden.

## <a name="performance"></a>Leistung

Jetzt übernehmen die gängigen Webbrowsern Technologien, wie Rendering und JavaScript-Kompilierung hardwarebeschleunigt. Unter iOS, standardmäßig die Xamarin.Forms `WebView` wird implementiert, indem die `UIWebView` -Klasse, und viele dieser Technologien sind in dieser Implementierung nicht verfügbar. Allerdings eine Anwendung kann sich für mit dem iOS- `WkWebView` Klasse, um die Xamarin.Forms implementieren `WebView`, diese unterstützt die Leistung zu verbessern. Dies kann erreicht werden, indem Sie den folgenden Code zum Hinzufügen der **"AssemblyInfo.cs"** Datei im iOS-Plattform-Projekt für die Anwendung:

```csharp
// Opt-in to using WkWebView instead of UIWebView.
[assembly: ExportRenderer(typeof(WebView), typeof(Xamarin.Forms.Platform.iOS.WkWebViewRenderer))]
```

`WebView` unter Android in der Standardeinstellung ist etwa so schnell wie den integrierten Browser ein.

Die [UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) verwendet die Microsoft Edge-Rendering-Engine. Desktop und Tablet-Geräte sollten die gleiche Leistung wie die Verwendung des Edge-Browsers selbst angezeigt werden.

## <a name="permissions"></a>Berechtigungen

In der Reihenfolge für `WebView` um arbeiten, Sie müssen sicherstellen, dass die Berechtigungen für jede Plattform festgelegt sind. Beachten Sie, dass auf einigen Plattformen `WebView` funktioniert im Debugmodus befindet, aber nicht, wenn Sie zur Veröffentlichung erstellt. Das ist da einige Berechtigungen, wie die für den Internetzugriff unter Android werden standardmäßig von Visual Studio für Mac im Debugmodus festgelegt sind.

- **UWP** &ndash; die Internet (Client & Server)-Funktion erfordert, bei der Anzeige von Netzwerk-Inhalt.
- **Android** &ndash; erfordert `INTERNET` nur, wenn Inhalt über das Netzwerk anzeigen. Lokale Inhalte erfordern keine besonderen Berechtigungen.
- **iOS** &ndash; benötigt keine besonderen Berechtigungen.

## <a name="layout"></a>Layout

Im Gegensatz zu den meisten anderen Xamarin.Forms-Ansichten können `WebView` erfordert, dass `HeightRequest` und `WidthRequest` werden angegeben, wenn in einem StackLayout oder RelativeLayout enthalten. Wenn Sie nicht an den Eigenschaften, die `WebView` werden nicht gerendert.

Die folgenden Beispiele veranschaulichen die Layouts, die zu arbeiten, führen Rendering `WebView`s:

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

Von "AbsoluteLayout" *ohne* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Raster *ohne* WidthRequest & HeightRequest. Raster ist eines der einige Layouts, die keine erfordert angeforderte Höhe und Breite angeben.:

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

[`WebView`](xref:Xamarin.Forms.WebView) bietet die Möglichkeit zum Aufrufen einer JavaScript-Funktion von C#, und ein Ergebnis zurückgeben, mit dem aufrufenden C# Code. Dies erfolgt mit der [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) -Methode, die in der im folgenden Beispiel gezeigt wird die [WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) Beispiel:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

Die [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Methode wertet das JavaScript, das als Argument angegeben ist, und gibt alle Ergebnis als eine `string`. In diesem Beispiel die `factorial` JavaScript-Funktion wird aufgerufen, womit die Fakultät der `number` daher. Diese Funktion wird definiert, in der lokalen HTML-Code JavaScript-Datei, die die [ `WebView` ](xref:Xamarin.Forms.WebView) lädt und wird im folgenden Beispiel dargestellt:

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

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit WebView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
