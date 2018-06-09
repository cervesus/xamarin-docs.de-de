---
title: Xamarin.Forms WebView
description: Dieser Artikel beschreibt wie die Xamarin.Forms-WebView-Klasse verwenden, um lokale darzustellen oder Netzwerk-Webinhalte und Dokumente an Benutzer.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: df004bd2a580e48137162d28ca3974521266ae7a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245643"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms WebView

[WebView](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) ist eine Ansicht zum Anzeigen von Web- und HTML-Inhalte in Ihrer app. Im Gegensatz zu `OpenUri`, was den Benutzer in dem Webbrowser auf dem Gerät in Anspruch nimmt `WebView` zeigt den HTML-Inhalt in Ihrer app.

Dieses Handbuch besteht aus den folgenden Abschnitten:

- **[Inhalt](#Content)**  &ndash; WebView unterstützt verschiedene Inhaltsquellen, einschließlich u. a. eingebettete HTML-Webseiten und HTML-Zeichenfolgen.
- **[Navigation](#Navigation)**  &ndash; WebView bietet Unterstützung für eine bestimmte Seite navigieren, und kehren.
- **[Ereignisse](#Events)**  &ndash; überwachen und reagieren auf Aktionen, die vom Benutzer in der Webansicht.
- **[Leistung](#Performance)**  &ndash; erfahren Sie mehr über die Leistungsmerkmale WebView auf jeder Plattform.
- **[Berechtigungen](#Permissions)**  &ndash; erfahren Sie, wie zum Festlegen von Berechtigungen, damit WebView in Ihrer app funktioniert.
- **[Layout](#Layout)**  &ndash; WebView stellt einige sehr bestimmten Anforderungen an die es wie verteilt werden. Erfahren Sie, wie Sie sicherstellen, dass WebView ordnungsgemäß angezeigt:

![](webview-images/in-app-browser.png "In App-Browser")

## <a name="content"></a>Inhalt

WebView bietet Unterstützung für die folgenden Typen von Inhalten:

- HTML- & CSS-Websites &ndash; WebView bietet vollständige Unterstützung für Websites, die mit HTML und CSS, einschließlich der Unterstützung von JavaScript geschrieben.
- Dokumente &ndash; da WebView mit systemeigenen Komponenten auf jeder Plattform implementiert wird, kann WebView für das Anzeigen von Dokumenten, die auf jeder Plattform sichtbar sind. Das heißt, PDF-Dateien auf IOS- und Android arbeiten.
- HTML-Zeichenfolgen &ndash; WebView kann HTML-Zeichenfolgen aus dem Arbeitsspeicher anzeigen.
- Lokale Dateien &ndash; WebView kann keines der oben aufgeführten Inhaltstypen eingebettet in der app darstellen.

> [!NOTE]
> `WebView` unter Windows unterstützt nicht Silverlight, Flash oder alle ActiveX-Steuerelemente, auch wenn sie von Internet Explorer auf dieser Plattform unterstützt werden.

### <a name="websites"></a>Websites

Wenn eine Website aus dem Internet anzeigen zu können, legen Sie die `WebView`des [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) Eigenschaft, um einen Zeichenfolgen-URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URLs müssen mit dem angegebenen Protokoll vollständig gebildet werden (d. h. muss "http://" oder "https://" vorangestellt ist).

#### <a name="ios-and-ats"></a>iOS und ATS

Seit Version 9 ermöglicht iOS nur es Ihrer Anwendung zur Kommunikation mit Servern, die optimale Sicherheit standardmäßig zu implementieren. Werte müssen festgelegt werden, `Info.plist` zur Kommunikation mit unsicheren Servern.

> [!NOTE]
> Wenn Ihre Anwendung eine Verbindung mit der eine unsichere Website erfordert, sollten Sie immer die Domäne eingeben, als eine Ausnahme mit `NSExceptionDomains` anstatt durch das ATS deaktivieren vollständig mit `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` sollte nur im Notfall Extremsituationen verwendet werden.

Im folgenden veranschaulicht, wie eine bestimmte Domäne (in diesem Fall xamarin.com), um ATS Anforderungen zu umgehen:

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
```

Es wird empfohlen, nur einige Domänen umgehen ATS, und Sie können vertrauenswürdige Websites verwenden, beim profitiert, der zusätzlichen Sicherheits in nicht vertrauenswürdigen Domänen zu aktivieren. Das folgende Beispiel zeigt die weniger sichere Methode ATS für die app zu deaktivieren:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md) Weitere Informationen zu dieser neuen Funktion in iOS 9.

### <a name="html-strings"></a>HTML-Zeichenfolgen

Wenn Sie eine Zeichenfolge mit HTML, die dynamisch im Code definierte bereitstellen möchten, müssen Sie zum Erstellen einer Instanz des [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "WebView Anzeigen von HTML-Zeichenfolge")

Im obigen Code `@` dient zum Kennzeichnen des HTML-Codes als Zeichenfolge-literal, d. h. die üblichen Escapezeichen ignoriert werden.

### <a name="local-html-content"></a>Lokale HTML-Inhalt

WebView kann Inhalte von HTML, CSS und Javascript in der app eingebettet. Zum Beispiel:

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

Beachten Sie, dass die Schriftarten, die in den oben genannten CSS-Code angegeben, da nicht jeder Plattform die gleichen Schriftarten gibt es für jede Plattform angepasst werden müssen.

Zum Anzeigen lokaler Inhalt mithilfe einer `WebView`, müssen Sie zum Öffnen der HTML-Datei wie jede andere, und Laden Sie den Inhalt als Zeichenfolge in der `Html` Eigenschaft ein `HtmlWebViewSource`. Weitere Informationen zu Dateien öffnen können, finden Sie unter [arbeiten mit Dateien](~/xamarin-forms/app-fundamentals/files.md).

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von lokalen Inhalten auf jeder Plattform:

![](webview-images/local-content.png "WebView Anzeigen von lokalen Inhalten")

Obwohl es sich bei die erste Seite geladen wurden, die `WebView` hat keine Kenntnis der, in der HTML-Code stammt. Das ist ein Problem beim Umgang mit Seiten, die auf lokale Ressourcen zu verweisen. Wenn das passieren kann zählen beim lokalen Seiten Verknüpfung, die auf jede andere, eine Seite wird die Verwendung von einer separaten JavaScript-Datei, oder eine Seite enthält zu einer CSS-Stylesheet links.  

Um dieses Problem zu beheben, müssen Sie die `WebView` , wo Sie Dateien im Dateisystem zu finden. Dies tun, indem Sie die Einstellung der `BaseUrl` Eigenschaft auf die `HtmlWebViewSource` verwendet werden, indem Sie die `WebView`.

Da das Dateisystem auf jedem der Betriebssysteme unterschiedlich ist, müssen Sie bestimmen die URL für jede Plattform. Xamarin.Forms stellt die `DependencyService` zum Auflösen von Abhängigkeiten zur Laufzeit auf jeder Plattform.

Verwenden der `DependencyService`, definieren Sie zunächst eine Schnittstelle, die auf jeder Plattform implementiert werden kann:

```csharp
public interface IBaseUrl { string Get(); }
```

Beachten Sie, bis die Schnittstelle für jede Plattform implementiert wird, wird die app nicht ausgeführt. Im Allgemeinen befinden, stellen Sie sicher, dass Sie daran denken, legen Sie die `BaseUrl` mithilfe der `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Implementierung der Schnittstelle für jede Plattform müssen dann bereitgestellt werden.

#### <a name="ios"></a>iOS

Bei iOS kann der Webinhalt sollten im Stammverzeichnis des Projekts gespeichert oder **Ressourcen** Verzeichnis mit Buildvorgang *BundleResource*, wie unten gezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Lokale Dateien unter iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Lokale Dateien unter iOS")

-----

Die `BaseUrl` sollte auf den Pfad des Haupt-Pakets festgelegt werden:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Auf Android-, platzieren Sie HTML, CSS und Bilder in den Ordner "Assets" mit Buildvorgang *AndroidAsset* wie nachstehend gezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Lokale Dateien unter Android")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](webview-images/android-xs.png "Lokale Dateien unter Android")

-----

Auf Android-Geräten die `BaseUrl` sollte festgelegt werden, um `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

Auf Android-Geräten Dateien in der **Bestand** Ordner kann auch über die aktuellen Android-Kontext, die durch verfügbar gemacht wird, zugegriffen werden die `MainActivity.Instance` Eigenschaft:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Für Projekte der universellen Windows-Plattform (UWP), platzieren Sie HTML, CSS und Bilder im Projektstammverzeichnis mit den Buildvorgang festgelegt *Content*.

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

WebView unterstützt die Navigation über mehrere Methoden und Eigenschaften, die sie zur Verfügung:

- **GoForward()** &ndash; Wenn `CanGoForward` ist "true", Aufrufen von `GoForward` zur nächsten Seite unter der besuchte vorwärts navigiert.
- **GoBack()** &ndash; Wenn `CanGoBack` ist "true", Aufrufen von `GoBack` wird der zuletzt besuchten Seite navigieren.
- **CanGoBack** &ndash; `true` treten Seiten navigieren Sie zurück zur `false` Wenn der Browser auf den Start-URL ist.
- **CanGoForward** &ndash; `true` Wenn der Benutzer rückwärts navigiert und vorwärts zu einer Seite, die bereits besucht wurde.

In Seiten `WebView` Multitouch-Gesten nicht unterstützt. Sie müssen unbedingt sicherstellen, dass der Inhalt ist Mobilgeräte optimierte und angezeigt wird, ohne die Notwendigkeit, vergrößern/verkleinern.

Es kommt häufig bei Anwendungen, die einen Link in einer `WebView`, statt den Browser des Geräts. In solchen Situationen es ist sinnvoll, normale Navigation zu ermöglichen, aber wenn die Benutzer-Treffer zu sichern, während Sie sich auf den ersten Link muss die app anzuzeigende normalen app zurückgeben.

Verwenden Sie die integrierte Methoden und Eigenschaften, um dieses Szenario zu ermöglichen.

Starten Sie, indem Sie die Seite für die Browseransicht erstellen:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In unserem Code-Behind:

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

Das ist alles!

![](webview-images/in-app-browser.png "WebView-Navigationsschaltflächen")

## <a name="events"></a>Ereignisse

WebView löst zwei Ereignisse, um Hilfestellungen reagieren auf Änderungen in den Status aus:

- **Navigieren in** &ndash; Ereignis ausgelöst, wenn die WebView beginnt mit dem Laden einer neuen Seite.
- **Navigiert** &ndash; Ereignis ausgelöst, wenn die Seite geladen ist und die Navigation wurde beendet.

Wenn sich abzeichnet, Webseiten, auf denen eine lange dauert, Laden verwenden, erwägen Sie, die diese Ereignisse um zu implementieren eine Statusanzeige. Zum Beispiel sieht der XAML-Code wie folgt:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
  <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

Die zwei Ereignishandler:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

Daraus ergibt sich die folgende Ausgabe (geladen):

![](webview-images/loading-start.png "Navigieren in WebView-Beispiel für Ereignis")

Vollständig geladen:

![](webview-images/loading-end.png "WebView navigiert, Ereignis-Beispiel")

## <a name="performance"></a>Leistung

Neue haben gesehen, jeweils die gängigen Webbrowser Technologien wie Hardware, Rendering- und JavaScript-Kompilierung Accelerated zu übernehmen. Leider aufgrund von sicherheitseinschränkungen, die meisten dieser Weiterentwicklungen waren nicht verfügbar in der die iOS-Equaivalent von `WebView`, `UIWebView`. Xamarin.Forms `WebView` verwendet `UIWebView`. Wenn dies ein Problem aufgetreten ist, müssen Sie einem benutzerdefinierten Renderer zu schreiben, die verwendet werden `WKWebView`, diese unterstützt die Leistung zu verbessern. Beachten Sie, dass `WKWebView` wird nur auf iOS 8 und höher unterstützt.

WebView unter Android in der Standardeinstellung ist etwa so schnell wie den integrierten Browser.

Die [uwp-WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) Microsoft Edge-Renderingmodul verwendet. Desktop- und Tablet-Geräte sollten die gleiche Leistung als mit dem Edge-Browser selbst angezeigt.

## <a name="permissions"></a>Berechtigungen

In der Reihenfolge für `WebView` funktioniert, Sie müssen sicherstellen, dass die Berechtigungen für jede Plattform festgelegt sind. Beachten Sie, dass auf manchen Plattformen `WebView` funktioniert im Debugmodus befindet, jedoch nicht, wenn für die Veröffentlichung erstellt. Ist, dass einige Berechtigungen, wie z. B. für den Zugriff auf das Internet auf Android-Geräten für Mac im Debugmodus standardmäßig von Visual Studio festgelegt sind.

- **Universelle Windows-Plattform** &ndash; erfordert die Funktion Internet (Client & Server) Netzwerkinhalte angezeigt.
- **Android** &ndash; erfordert `INTERNET` nur, wenn Sie Inhalt über das Netzwerk anzeigen. Lokale Inhalte erfordern keine besonderen Berechtigungen.
- **iOS** &ndash; benötigt keine besonderen Berechtigungen.

## <a name="layout"></a>Layout

Im Gegensatz zu den meisten anderen Xamarin.Forms Ansichten `WebView` erfordert, dass `HeightRequest` und `WidthRequest` werden angegeben, wenn in StackLayout oder RelativeLayout enthalten. Wenn Sie ein Failover auf diese Eigenschaften geben die `WebView` wird nicht gerendert.

Die folgenden Beispiele veranschaulichen Layouts, mit denen zu arbeiten, führen Rendering `WebView`s:

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

AbsoluteLayout *ohne* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Raster *ohne* WidthRequest & HeightRequest. Raster ist einer der wenigen Layouts, die keine benötigt angeforderte Höhe und Breite angeben.:

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


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit WebView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView-(Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
