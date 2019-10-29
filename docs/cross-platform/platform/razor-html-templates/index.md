---
title: Erstellen von HTML-Ansichten mithilfe von Razor-Vorlagen
description: " Die Verwendung einer Vollbild-Webseite zum Rendering von HTML kann eine einfache und effektive Möglichkeit zum Rendering komplexer Formatierungen auf plattformübergreifende Weise darstellen, insbesondere dann, wenn Sie bereits über HTML, JavaScript und CSS aus einem Website Projekt verfügen."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: davidortinau
ms.author: daortin
ms.date: 07/24/2018
ms.openlocfilehash: 5f1b1345f9abbf891cfbea6e45a8ed2abd7c0dac
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014604"
---
# <a name="building-html-views-using-razor-templates"></a>Erstellen von HTML-Ansichten mithilfe von Razor-Vorlagen

In der mobilen Entwicklungsumgebung bezieht sich der Begriff "Hybrid-app" in der Regel auf eine Anwendung, die einige (oder alle) Ihrer Bildschirme als HTML-Seiten in einem gehosteten Web-Viewer-Steuerelement darstellt.

Es gibt einige Entwicklungsumgebungen, in denen Sie Ihre Mobile App vollständig in HTML und JavaScript erstellen können. Allerdings können diese apps von Leistungsproblemen leiden, wenn Sie versuchen, komplexe Verarbeitungs-oder Benutzeroberflächen Effekte zu erreichen, und auch auf der Plattform eingeschränkt sind. Features, auf die Sie zugreifen können.

Xamarin bietet das Beste aus beiden Welten, insbesondere bei Verwendung der Razor-HTML-Vorlagen-Engine. Mit xamarin können Sie plattformübergreifende, auf Vorlagen basierende HTML-Ansichten erstellen, die JavaScript und CSS verwenden, aber auch vollständigen Zugriff auf die zugrunde liegenden Plattform-APIs und C#die schnelle Verarbeitung mithilfe von haben.

In diesem Dokument wird erläutert, wie Sie die Razor-Vorlagen-Engine zum Erstellen von HTML-und JavaScript-und CSS-Ansichten verwenden, die auf mobilen Plattformen mit xamarin verwendet werden können.

## <a name="using-web-views-programmatically"></a>Programm gesteuertes Verwenden von Webansichten

Bevor wir uns mit Razor vertraut machen, wird in diesem Abschnitt erläutert, wie Sie Webansichten verwenden, um HTML-Inhalte direkt anzuzeigen – insbesondere HTML-Inhalte, die in einer APP generiert werden

Xamarin bietet vollständigen Zugriff auf die zugrunde liegenden Plattform-APIs unter IOS und Android, sodass es einfach ist, HTML mithilfe C#von zu erstellen und anzuzeigen. Die grundlegende Syntax für jede Plattform ist unten dargestellt.

### <a name="ios"></a>iOS

Das Anzeigen von HTML in einem UIWebView-Steuerelement in xamarin. IOS erfordert auch nur ein paar Codezeilen:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Weitere Informationen zur Verwendung des UIWebView-Steuer Elements finden Sie unter [IOS UIWebView](https://github.com/xamarin/docs-archive/tree/master/Recipes/ios/content_controls/web_view) -Rezepte.

### <a name="android"></a>Android

Das Anzeigen von HTML in einem WebView-Steuerelement mithilfe von xamarin. Android wird in wenigen Codezeilen durchgeführt:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable JavaScript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Weitere Informationen zur Verwendung des WebView-Steuer Elements finden Sie in den [Android WebView](https://github.com/xamarin/docs-archive/tree/master/Recipes/android/controls/webview) -Rezepten.

### <a name="specifying-the-base-directory"></a>Angeben des Basisverzeichnisses

Auf beiden Plattformen gibt es einen Parameter, der das Basisverzeichnis für die HTML-Seite angibt. Dies ist der Speicherort im Dateisystem des Geräts, mit dem relative Verweise auf Ressourcen wie Bilder und CSS-Dateien aufgelöst werden. Beispielsweise Tags wie

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

Weitere Informationen finden Sie in den folgenden Dateien: **Style. CSS**, **Monkey. jpg** und **JScript. js**. Die Basisverzeichnis Einstellung teilt der Webansicht mit, wo sich diese Dateien befinden, damit Sie in die Seite geladen werden können.

#### <a name="ios"></a>iOS

Die Vorlagen Ausgabe wird in ios mit folgendem C# Code gerendert:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Das Basisverzeichnis wird als `NSBundle.MainBundle.BundleUrl` angegeben, das auf das Verzeichnis verweist, in dem die Anwendung installiert ist. Alle Dateien im Ordner " **Resources** " werden an diesen Speicherort kopiert, z. b. die Datei " **Style. CSS** ", die hier gezeigt wird:

 ![iphonehybrid-Lösung](images/image1_240x163.png)

Die Buildaktion für alle statischen Inhalts Dateien sollte **bundleresource**lauten:

 ![IOS-projektbuildaktion: bundleresource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android erfordert auch ein Basisverzeichnis, das als Parameter übergeben wird, wenn HTML-Zeichen folgen in einer Webansicht angezeigt werden.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Die besondere Zeichenfolge **file:///android_asset/** bezieht sich auf den Ordner Android-Objekte in Ihrer APP, der hier mit der Datei " **Style. CSS** " angezeigt wird.

 ![Androidhybrid-Lösung](images/image3_240x167.png)

Die Buildaktion für alle statischen Inhalts Dateien sollte " **androidasset**" lauten.

 ![Android Project Build Action: androidasset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Aufrufen C# von HTML und JavaScript

Wenn eine HTML-Seite in eine Webansicht geladen wird, werden die Verknüpfungen und Formulare so behandelt, wie dies beim Laden der Seite von einem Server der Fall wäre. Dies bedeutet Folgendes: Wenn der Benutzer auf einen Link klickt oder ein Formular übermittelt, versucht die Webansicht, zum angegebenen Ziel zu navigieren.

Wenn der Link zu einem externen Server (z. b. Google.com) gehört, versucht die Webansicht, die externe Website zu laden (vorausgesetzt, es ist eine Internetverbindung vorhanden).

```html
<a href="http://google.com/">Google</a>
```

Wenn der Link relativ ist, versucht die Webansicht, diesen Inhalt aus dem Basisverzeichnis zu laden. Offensichtlich ist keine Netzwerkverbindung erforderlich, damit dies funktioniert, da der Inhalt in der APP auf dem Gerät gespeichert wird.

```html
<a href="somepage.html">Local content</a>
```

Formular Aktionen folgen derselben Regel.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Sie werden keinen Webserver auf dem Client hosten. Sie können jedoch dieselben Server Kommunikationsverfahren verwenden, die in den heutigen reaktionsfähigen Entwurfsmustern eingesetzt werden, um Dienste über HTTP Get aufzurufen, und Antworten asynchron durch Ausgeben von JavaScript (oder durch das Aufrufen von JavaScript, das bereits in der Webansicht gehostet wird) verarbeiten. Auf diese Weise können Sie problemlos Daten aus dem HTML- C# Code zur Verarbeitung zurück an den Code übergeben und die Ergebnisse dann auf der HTML-Seite wieder anzeigen.

Sowohl IOS als auch Android bieten einen Mechanismus für Anwendungscode, mit dem diese Navigations Ereignisse abgefangen werden, damit app-Code Antworten kann (falls erforderlich). Diese Funktion ist für die Entwicklung von Hybrid-apps von entscheidender Bedeutung, da nativer Code mit der Webansicht interagieren kann.

#### <a name="ios"></a>iOS

Das Ereignis "schuldstartload" in der Webansicht in ios kann überschrieben werden, damit Anwendungscode eine Navigations Anforderung verarbeiten kann (z. b. ein Link Klick). Die Methoden Parameter geben alle Informationen an.

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

und weisen Sie dann den Ereignishandler zu:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Unter nur Unterklasse webviewclient und dann Code implementieren, um auf die Navigations Anforderung zu reagieren.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

und dann den Client in der Webansicht festlegen:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Aufrufen von JavaScript aus C-\#

Zusätzlich zur Anzeige einer Webansicht, um eine neue HTML-Seite C# zu laden, kann Code auch JavaScript in der aktuell angezeigten Seite ausführen. Gesamte JavaScript-Code Blöcke können mithilfe C# von Zeichen folgen erstellt und ausgeführt werden, oder Sie können Methodenaufrufe an JavaScript erstellen, die bereits über`script`Tags auf der Seite verfügbar sind.

#### <a name="android"></a>Android

Erstellen Sie den JavaScript-Code, der ausgeführt werden soll, und weisen Sie ihm dann "JavaScript:" zu, und weisen Sie die Webansicht an, diese Zeichenfolge

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

IOS-Webansichten bieten eine spezielle Methode zum Aufrufen von JavaScript:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Zusammenfassung

In diesem Abschnitt wurden die Features der webansichts-Steuerelemente für Android und IOS vorgestellt, mit denen Sie Hybrid Anwendungen mit xamarin erstellen können, darunter:

- Die Möglichkeit zum Laden von HTML aus Zeichen folgen, die im Code generiert werden,
- Die Möglichkeit, auf lokale Dateien (CSS, JavaScript, Bilder oder andere HTML-Dateien) zu verweisen,
- Die Möglichkeit, Navigationsanforderungen im C# Code abzufangen,
- Die Möglichkeit zum Abrufen von JavaScript C# aus dem Code.

Im nächsten Abschnitt wird Razor vorgestellt, mit dem Sie das HTML-Format für Hybrid-apps ganz einfach erstellen können.

## <a name="what-is-razor"></a>Was ist Razor?

Razor ist eine Vorlagen-Engine, die mit ASP.NET MVC eingeführt wurde, die ursprünglich auf dem Server ausgeführt wurde und HTML-Code generiert, der für Webbrowser bereitgestellt werden soll.

Die Razor-Vorlagen-Engine erweitert die HTML- C# Standard Syntax mit, sodass Sie das Layout Ausdrücken und CSS-Stylesheets und JavaScript problemlos integrieren können. Die Vorlage kann auf eine Modell Klasse verweisen, bei der es sich um beliebige benutzerdefinierte Typen handeln kann und auf deren Eigenschaften direkt von der Vorlage aus zugegriffen werden kann. Einer der Hauptvorteile ist die Möglichkeit, HTML und C# Syntax problemlos zu kombinieren.

Razor-Vorlagen sind nicht auf die serverseitige Verwendung beschränkt, Sie können auch in xamarin-Apps enthalten sein. Die Verwendung von Razor-Vorlagen und die Möglichkeit, mit Webansichten Programm gesteuert zu arbeiten, ermöglicht die Erstellung ausgereifender plattformübergreifender Hybrid Anwendungen mit xamarin.

### <a name="razor-template-basics"></a>Grundlagen zu Razor

Razor-Vorlagen Dateien haben die Dateierweiterung **. cshtml** . Sie können einem xamarin-Projekt aus dem Text Vorlagen Abschnitt im Dialogfeld " **neue Datei** " hinzugefügt werden:

 ![Neue Datei-Razor-Vorlage](images/image5_400x201.png)

Eine einfache Razor-Vorlage ( **razorview. cshtml**) ist unten dargestellt.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Beachten Sie die folgenden Unterschiede zu einer regulären HTML-Datei:

- Das `@` Symbol hat eine besondere Bedeutung in Razor-Vorlagen – es gibt C# an, dass der folgende Ausdruck ausgewertet werden soll.
- `@model`-Direktive wird immer als erste Zeile einer Razor-Vorlagen Datei angezeigt.
- Auf die `@model` Direktive sollte ein Typ folgen. In diesem Beispiel wird eine einfache Zeichenfolge an die Vorlage übermittelt, dabei kann es sich jedoch um eine beliebige benutzerdefinierte Klasse handeln.
- Wenn auf `@Model` in der gesamten Vorlage verwiesen wird, wird ein Verweis auf das Objekt bereitstellt, das bei der Generierung an die Vorlage übermittelt wird (in diesem Beispiel ist es eine Zeichenfolge).
- Die IDE generiert automatisch eine partielle Klasse für Vorlagen (Dateien mit der Erweiterung **. cshtml** ). Sie können diesen Code zwar anzeigen, aber nicht bearbeiten.
 ![razorview. cshtml](images/image6_125x34.png) die partielle Klasse den Namen razorview hat, um dem Namen der Datei. cshtml-Vorlage zu entsprechen. Dieser Name wird verwendet, um auf die Vorlage im C# Code zu verweisen.
- `@using`-Anweisungen können auch am Anfang einer Razor-Vorlage eingefügt werden, um zusätzliche Namespaces einzuschließen.

Die endgültige HTML-Ausgabe kann dann mit folgendem C# Code generiert werden. Beachten Sie, dass das Modell als Zeichenfolge "Hallo Welt" angegeben wird, die in die Ausgabe der gerenderten Vorlage eingebunden wird.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Dies ist die Ausgabe, die in einer Webansicht des IOS-Simulators und Android-Emulator angezeigt wird:

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Weitere Razor-Syntax

In diesem Abschnitt werden einige grundlegende Razor-Syntax vorgestellt, die Ihnen beim Einstieg helfen. In den Beispielen in diesem Abschnitt wird die folgende Klasse mit Daten aufgefüllt und mithilfe von Razor angezeigt:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

In allen Beispielen wird der folgende Daten Initialisierungs Code verwendet:

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Anzeigen von Modell Eigenschaften

Wenn das Modell eine Klasse mit Eigenschaften ist, kann in der Razor-Vorlage leicht auf Sie verwiesen werden, wie in der folgenden Beispiel Vorlage gezeigt:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Dies kann mithilfe des folgenden Codes in eine Zeichenfolge gerendert werden:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Die endgültige Ausgabe wird hier in einer Webansicht des IOS-Simulators und Android-Emulator angezeigt:

 ![Milliard](images/image8_516x160.png)

#### <a name="c-statements"></a>C#Äußerungen

Komplexere Informationen C# können in der Vorlage enthalten sein, wie z. b. die Updates der Modell Eigenschaft und die Alters Berechnung in diesem Beispiel:

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

Sie können komplexe einzeilige C# Ausdrücke (z. b. die Formatierung des Alters) schreiben, indem Sie den Code mit `@()`umschließen.

Sie C# können mehrere-Anweisungen schreiben, indem Sie Sie mit `@{}`umschließen.

#### <a name="if-else-statements"></a>If-else-Anweisungen

Codebranches können mit `@if` ausgedrückt werden, wie in diesem Vorlagen Beispiel gezeigt.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>Schleifen

Schleifen Konstrukte wie `foreach` können ebenfalls hinzugefügt werden. Das `@` Präfix kann für die Schleifenvariable (in diesem Fall `@food`) verwendet werden, um Sie in HTML zu rendereren.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

Die Ausgabe der obigen Vorlage wird im IOS-Simulator und Android-Emulator angezeigt:

 ![Rupert X Affe](images/image9_520x277.png)

In diesem Abschnitt wurden die Grundlagen der Verwendung von Razor-Vorlagen zum Rendering einfacher Schreib geschützter Sichten behandelt. Im nächsten Abschnitt wird erläutert, wie Sie mit Razor umfassendere Apps erstellen können, die Benutzereingaben akzeptieren und zwischen JavaScript in der HTML-Ansicht und C#interagieren können.

## <a name="using-razor-templates-with-xamarin"></a>Verwenden von Razor-Vorlagen mit xamarin

In diesem Abschnitt wird erläutert, wie Sie Ihre eigene Hybrid Anwendung mithilfe der Lösungs Vorlagen in Visual Studio für Mac erstellen. Es gibt drei Vorlagen aus der **Datei > neue > Projekt Mappe...** Fenster:

- **Android-> app > Android WebView-Anwendung**
- **IOS-> app > WebView-Anwendung**
- **ASP.NET-MVC-Projekt**

Das **neue** projektmappenfenster sieht für iPhone-und Android-Projekte wie folgt aus: die Lösungs Beschreibung auf der rechten Seite hebt die Unterstützung für die Razor-Vorlagen-Engine hervor.

 ![Erstellen von iPhone-und Android-Lösungen](images/image13_1139x959.png)

Beachten *Sie, dass Sie einem vorhandenen* xamarin-Projekt problemlos eine **. cshtml** -Razor-Vorlage hinzufügen können. es ist nicht erforderlich, diese Lösungs Vorlagen zu verwenden. für IOS-Projekte ist ein Storyboard nicht erforderlich, um Razor zu verwenden. Fügen Sie einer Ansicht einfach ein UIWebView-Steuerelement Programm gesteuert hinzu, und Sie können Razor C# -Vorlagen vollständig im Code darstellen.

Die Standardvorlagen projektmappeninhalte für iPhone-und Android-Projekte sind unten dargestellt:

 ![iPhone-und Android-Vorlagen](images/image10_428x310.png)

Mit den Vorlagen erhalten Sie eine sofort einsatzbereite Anwendungs Infrastruktur zum Laden einer Razor-Vorlage mit einem Datenmodell Objekt, zum Verarbeiten von Benutzereingaben und zum kommunizieren mit dem Benutzer über JavaScript.

Wichtige Teile der Lösung sind:

- Statischer Inhalt, z. b. die Datei **Style. CSS** .
- Razor. cshtml-Vorlagen Dateien wie **razorview. cshtml** .
- Modellklassen, auf die in den Razor-Vorlagen verwiesen wird, z. b. **ExampleModel.cs** .
- Die plattformspezifische Klasse, die die Webansicht erstellt und die Vorlage rendert, wie z. b. die `MainActivity` unter Android und die `iPhoneHybridViewController` unter IOS.

Im folgenden Abschnitt wird erläutert, wie die Projekte funktionieren.

### <a name="static-content"></a>Statischer Inhalt

Statischer Inhalt umfasst CSS-Stylesheets, Bilder, JavaScript-Dateien oder andere Inhalte, die von einer HTML-Datei, die in einer Webansicht angezeigt wird, verknüpft oder darauf verwiesen werden kann.

Die Vorlagen Projekte enthalten ein minimales Stylesheet, das zeigt, wie statischer Inhalt in ihre Hybrid-App eingeschlossen wird. Auf das CSS-Stylesheet wird in der Vorlage wie folgt verwiesen:

```html
<link rel="stylesheet" href="style.css" />
```

Sie können alle benötigten Stylesheets-und JavaScript-Dateien hinzufügen, einschließlich Frameworks wie jQuery.

### <a name="razor-cshtml-templates"></a>Razor-cshtml-Vorlagen

Die Vorlage enthält eine Razor **. cshtml** -Datei, die über vorab geschriebenen Code zum Kommunizieren von Daten zwischen HTML/JavaScript und C#verfügt. Auf diese Weise können Sie ausgereifte Hybrid-Apps erstellen, die nicht nur schreibgeschützte Daten aus dem Modell anzeigen, sondern auch Benutzereingaben in der HTML-Datei akzeptieren und C# Sie zur Verarbeitung oder Speicherung an den Code zurückgeben.

#### <a name="rendering-the-template"></a>Rendern der Vorlage

Wenn Sie die `GenerateString` für eine Vorlage aufrufen, wird HTML für die Anzeige in einer Webansicht gerendert. Wenn die Vorlage ein Modell verwendet, sollte diese vor dem Rendering bereitgestellt werden. Dieses Diagramm veranschaulicht, wie das Rendering funktioniert – nicht, dass die statischen Ressourcen zur Laufzeit von der Webansicht aufgelöst werden. dabei wird das angegebene Basisverzeichnis verwendet, um nach den angegebenen Dateien zu suchen.

 ![Razor-Flussdiagramm](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Aufrufen C# von Code aus der Vorlage

Die Kommunikation von einer gerenderten Webansicht, die zurück an C# aufgerufen wird, erfolgt durch Festlegen der URL für die Webansicht C# und Abfangen der Anforderung in, um die native Anforderung zu verarbeiten, ohne die Webansicht erneut zu laden.

Ein Beispiel für die Behandlung der Schaltfläche razorview finden Sie unter. Die Schaltfläche verfügt über den folgenden HTML-Code:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

Die `InvokeCSharpWithFormValues` JavaScript-Funktion liest alle Werte aus dem HTML-Formular und legt die `location.href` für die Webansicht fest:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

Dadurch wird versucht, in der Webansicht zu einer URL mit einem benutzerdefinierten Schema zu navigieren (z. b. `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Wenn die native Webansicht diese Navigations Anforderung verarbeitet, haben wir die Möglichkeit, Sie abzufangen. In ios erfolgt dies durch die Behandlung des "-"-Handlers von UIWebView. In Android unterteilen wir einfach den webviewclient, der im Formular verwendet wird, und überschreiben "lastdoverrideurlload".

Die internale dieser beiden navigationinterceptors sind im Wesentlichen identisch.

Überprüfen Sie zunächst die URL, die von der Webansicht geladen werden soll, und wenn Sie nicht mit dem benutzerdefinierten Schema (`hybrid:`) beginnt, können Sie die Navigation wie gewohnt durchführen.

Für das benutzerdefinierte URL-Schema ist alles in der URL zwischen dem Schema und dem "?" der Methodenname, der behandelt werden soll (in diesem Fall "Update Label"). Alles in der Abfrage Zeichenfolge wird als Parameter für den Methodenaufrufe behandelt:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` in diesem Beispiel führt eine minimale Anzahl von Zeichen folgen Manipulationen für den Parameter "Textfeld" ausC# (vorangestellt "sagt" der Zeichenfolge) und ruft dann die Webansicht zurück.

Nachdem die URL verarbeitet wurde, bricht die Methode die Navigation ab, sodass die Webansicht nicht mehr versucht, die Navigation zur benutzerdefinierten URL abzuschließen.

#### <a name="manipulating-the-template-from-c"></a>Bearbeiten der Vorlage aus C-\#

Die Kommunikation mit einer gerenderten HTML C# -Webansicht von erfolgt durch Aufrufen von JavaScript in der Webansicht. Unter IOS erfolgt dies durch Aufrufen von `EvaluateJavascript` in der UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

Unter Android kann JavaScript in der Webansicht aufgerufen werden, indem JavaScript mit dem `"javascript:"` URL-Schema als URL geladen wird:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Echte Hybrid Erstellung einer APP

Diese Vorlagen verwenden keine systemeigenen Steuerelemente für jede Plattform – der gesamte Bildschirm wird mit einer einzelnen Webansicht gefüllt.

HTML eignet sich hervorragend für die Prototyperstellung und zeigt die Arten der Dinge an, mit denen sich das Web am besten eignet, z. b. Rich-Text Allerdings sind nicht alle Aufgaben für HTML und JavaScript geeignet – das Scrollen durch lange Listen mit Daten, z. b. eine bessere Leistung mithilfe von nativen UI-Steuerelementen (z. b. uitableview unter IOS oder ListView unter Android).

Die Webansichten in der Vorlage können problemlos mit plattformspezifischen Steuerelementen ergänzt werden – bearbeiten Sie einfach die Datei " **mainstoryboard. Storyboard** " im IOS-Designer oder " **Resources/Layout/Main. axml** " unter Android.

### <a name="razortodo-sample"></a>Razortodo-Beispiel

Das [razortodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) -Repository enthält zwei separate Lösungen zum Anzeigen der Unterschiede zwischen einer vollständig HTML-gesteuerten APP und einer APP, die HTML mit nativen Steuerelementen kombiniert:

- **Razortodo** : vollständig HTML-gesteuerte App mithilfe von Razor-Vorlagen.
- **Razornativetodo** : verwendet Native Listenansicht-Steuerelemente für IOS und Android, aber zeigt den Bearbeitungs Bildschirm mit HTML und Razor an.

Diese xamarin-Apps können unter IOS und Android ausgeführt werden und verwenden Portable Klassenbibliotheken (pcls), um gemeinsamen Code wie die Datenbank-und Modellklassen gemeinsam zu nutzen. Razor **. cshtml** -Vorlagen können auch in der PCL enthalten sein, sodass Sie problemlos plattformübergreifend freigegeben werden können.

Beide Beispiel-Apps enthalten Twitter-Freigabe-und Text-zu-Sprache-APIs von der systemeigenen Plattform und veranschaulichen, dass Hybrid Anwendungen mit xamarin weiterhin Zugriff auf alle zugrunde liegenden Funktionen aus HTML-Razor-Vorlagen gesteuerten Ansichten haben.

Die **razortodo** -App verwendet HTML-Razor-Vorlagen für die Listen-und Bearbeitungs Ansichten. Dies bedeutet, dass die APP fast vollständig in einer freigegebenen portablen Klassenbibliothek erstellt werden kann (einschließlich der Razor-Vorlagen "Database" und " **. cshtml** "). Die folgenden Screenshots zeigen die IOS-und Android-Apps.

 ![Razortodo](images/Both_700x290.png)

Die **razornativetodo** -App verwendet eine HTML-Razor-Vorlage für die Bearbeitungs Ansicht, implementiert aber eine native scrollliste auf jeder Plattform. Dies bietet eine Reihe von Vorteilen, einschließlich:

- Leistung: die systemeigenen scrollsteuerelemente verwenden die Virtualisierung, um einen schnellen, reibungslosen Bildlauf auch mit sehr langen Listen von Daten sicherzustellen.
- Native Benutzeroberfläche: plattformspezifische Benutzeroberflächen Elemente sind einfach zu aktivieren, wie z. b. die Unterstützung für schnelles Scrollen in IOS und Android.

Ein wichtiger Vorteil beim Aufbau von Hybrid-apps mit xamarin besteht darin, dass Sie mit einer vollständig HTML-gesteuerten Benutzeroberfläche (wie dem ersten Beispiel) beginnen und dann bei Bedarf plattformspezifische Funktionen hinzufügen können (wie im zweiten Beispiel gezeigt). Die systemeigenen Listen Bildschirme und HTML-Razor-Bearbeitungsbildschirme unter IOS und Android sind unten dargestellt.

 ![Razornativetodo](images/BothNative_700x290.png)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Features der webansichts Steuerelemente erläutert, die unter IOS und Android verfügbar sind und das Entwickeln von Hybrid Anwendungen erleichtern.

Anschließend wurde die Razor-Vorlagen-Engine und die Syntax erläutert, die zum einfachen Generieren von HTML in xamarin-apps mit verwendet werden kann. **cshtml** Razor-Vorlagen Dateien. Außerdem wurden die Visual Studio für Mac Lösungs Vorlagen beschrieben, mit denen Sie schnell mit dem Erstellen von Hybrid Anwendungen mit xamarin beginnen können.

Schließlich wurden die razortodo-Beispiele vorgestellt, die veranschaulichen, wie Webansichten mit nativen Benutzeroberflächen und APIs kombiniert werden.

### <a name="related-links"></a>Verwandte Links

- [Razortodo-Beispiel](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3-Razor-Ansichts Modul (Microsoft)](https://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax (Microsoft)](https://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
