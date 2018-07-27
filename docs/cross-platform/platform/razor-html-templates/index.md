---
title: Erstellen von HTML-Ansichten mit Razor-Vorlagen
description: " Verwenden einer Vollbild-Webseite zum Rendern von HTML kann eine einfache und effektive Möglichkeit zum Rendern komplexer Formatierung auf eine Weise plattformübergreifende, insbesondere dann, wenn Sie die HTML, Javascript und CSS aus einem Websiteprojekt noch sein."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: 7e569aaddef912d9534e98f2f987ad5dfca8a5a6
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270131"
---
# <a name="building-html-views-using-razor-templates"></a>Erstellen von HTML-Ansichten mit Razor-Vorlagen

In der Entwicklung für mobile Geräte-Welt bezieht sich die Begriff "Hybrid-app" in der Regel zu einer Anwendung, die einige oder alle der Bildschirme als HTML-Seiten in einem gehosteten Web-Viewer-Steuerelement darstellt.

Es gibt einige entwicklungsumgebungen, mit denen Sie erstellen Ihrer mobilen app vollständig in HTML und Javascript, aber diese apps Leistungsprobleme beim Versuch Leiden können, die komplexe Verarbeitung oder Benutzeroberflächeneffekte ausführen und sind auch auf der Plattform eingeschränkt Features, die sie zugreifen können.

Xamarin bietet das beste aus beiden Welten, insbesondere, wenn die Razor-HTML-Vorlagen-Engine nutzen. Mit Xamarin können Sie flexibel plattformübergreifende auf Vorlagen basierenden HTML-Ansichten zu erstellen, die Javascript und CSS, aber haben zudem vollständigen Zugriff auf die zugrunde liegenden Plattform-APIs und die schnelle Verarbeitung, die mithilfe von c#.

Dieses Dokument erläutert, wie Sie mit der Razor-Vorlagen-Engine erstellen, HTML und Javascript + CSS-Sichten, die auf mobilen Plattformen mit Xamarin verwendet werden können.

## <a name="using-web-views-programmatically"></a>Programmgesteuertes Verwenden von Webansichten

Bevor wir Razor Informationen wird beschrieben in diesem Abschnitt verwenden Webansichten zum Anzeigen von HTML-Inhalt direkt – insbesondere HTML-Inhalt, der in einer app generiert wird.

Xamarin bietet vollständigen Zugriff auf die zugrunde liegenden Plattform-APIs auf IOS- und Android, daher ist es einfach zu erstellen und mit C# erstellte HTML-Seite anzeigen. Die grundlegende Syntax für jede Plattform ist unten dargestellt.

### <a name="ios"></a>iOS

Anzeigen von HTML in einem Steuerelement UIWebView in Xamarin.iOS akzeptiert auch nur wenige Codezeilen erforderlich:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Finden Sie unter den [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) Know-how für die Weitere Informationen zur Verwendung des UIWebView-Steuerelements.

### <a name="android"></a>Android

Anzeigen von HTML in einem WebView-Steuerelement, das mithilfe von Xamarin.Android erfolgt in nur wenigen Codezeilen erforderlich:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable Javascript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Finden Sie unter den [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) Know-how für die Weitere Informationen zur Verwendung der WebView-Steuerelement.

### <a name="specifying-the-base-directory"></a>Angeben des Basisverzeichnisses

Es gibt einen Parameter, der angibt, das Basisverzeichnis für die HTML-Seite, auf beiden Plattformen. Dies ist der Speicherort des Geräts-Dateisystem, die zum Auflösen relativer Verweise auf Ressourcen wie Bilder und CSS-Dateien verwendet wird. Z. B. wie tags

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

Diese Dateien finden Sie unter: **"Style.CSS"**, **monkey.jpg** und **jscript.js**. Die Basisverzeichnis-Einstellung weist der Webansicht, wo sich diese Dateien befinden, damit sie auf der Seite geladen werden können.

#### <a name="ios"></a>iOS

Die Vorlagenausgabe wird in iOS durch den folgenden C#-Code dargestellt:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Das Basisverzeichnis angegeben ist, als `NSBundle.MainBundle.BundleUrl` die bezieht sich auf das Verzeichnis, das in die Anwendung installiert wird. Alle Dateien in die **Ressourcen** Ordner werden an diesem Speicherort kopiert, z. B. die **"Style.CSS"** hier gezeigte Datei:

 ![iPhoneHybrid-Lösung](images/image1_240x163.png)

Der Buildvorgang für alle Dateien mit statischem Inhalt muss **BundleResource**:

 ![iOS-Projekt erstellen, Aktion: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android erfordert auch ein Basisverzeichnis, die als Parameter übergeben werden, wenn html-Zeichenfolgen in einer Webansicht angezeigt werden.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Die spezielle Zeichenfolge **file:///android_asset/** bezieht sich auf den Ordner "Android Assets" in Ihrer app angezeigt, die hier enthält die **"Style.CSS"** Datei.

 ![AndroidHybrid-Lösung](images/image3_240x167.png)

Der Buildvorgang für alle Dateien mit statischem Inhalt muss **AndroidAsset**.

 ![Android-Projekt erstellen, Aktion: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Aufrufen von c# aus HTML und Javascript

Wenn eine html-Seite in eine Webansicht geladen wird, wird er behandelt die Links und Forms, als wäre die Seite geladen wurde, von einem Server. Dies bedeutet, dass wenn der Benutzer klickt auf einen Link oder ein Formular übermittelt die Webansicht versucht, auf das angegebene Ziel zu navigieren.

Wenn der Link zu einem externen Server (z. B. also z.B. google.com) ist dann versucht die Webansicht, laden die externe Website (vorausgesetzt, dass eine Internetverbindung besteht).

```html
<a href="http://google.com/">Google</a>
```

Wenn der Link relativ ist versucht die Webansicht, laden, dass der Inhalt aus dem Basisverzeichnis. Offensichtlich ist keine Netzwerkverbindung erforderlich, damit dies funktioniert, wie der Inhalt in der app auf dem Gerät gespeichert ist.

```html
<a href="somepage.html">Local content</a>
```

Formularaktionen folgen derselben Regel.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Sie wollen keinen Webserver, auf dem Client hosten; Allerdings können Sie verwenden die gleichen Server Kommunikation Verfahren, die in der heutigen reaktionsfähiges Design Patterns eingesetzt zum Aufrufen von Diensten über HTTP-GET und asynchron verarbeiten von Antworten durch das Ausgeben von Javascript (oder aufrufende Javascript ist bereits in der Webansicht gehostet). Dadurch können Sie problemlos Daten aus den HTML-Code in C#-Code für die Verarbeitung dann anzuzeigen, die die Ergebnisse der HTML-Seite zurück übergeben.

Geben einen Mechanismus zum Anwendungscode, um diese Navigationsereignisse abfangen, damit app-Code reagieren kann (falls erforderlich), iOS und Android. Dieses Feature ist entscheidend für die Erstellung von Hybrid-apps, da sie nativen Code mit der Webansicht interagieren können.

#### <a name="ios"></a>iOS

Die ShouldStartLoad-Ereignis in der Webansicht in iOS kann überschrieben werden, damit Anwendungscode eine navigationsanforderung (z. B. einen Klick Link) verarbeiten kann. Die Methodenparameter bereitstellen alle Informationen.

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

ein, und weisen Sie dann den Ereignishandler:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Unter Android einfach WebViewClient-Unterklasse und klicken Sie dann Code implementieren, auf die navigationsanforderung reagieren.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

ein, und legen Sie dann den Client für die Webansicht:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Aufrufen von Javascript in c#

C#-Code kann weisen eine Webansicht, um eine neue HTML-Seite zu laden, Javascript auch in der aktuell angezeigten Seite ausführen. Gesamte Javascript-Code-Blöcken mit c#-Zeichenfolgen erstellt und ausgeführt werden können, oder Sie können die Methodenaufrufe an bereits verfügbar ist, auf der Seite über Javascript erstellen `script` Tags.

#### <a name="android"></a>Android

Erstellen Sie die Javascript-Code ausgeführt werden, und klicken Sie dann mit Präfix "Javascript:" und weisen Sie die Webansicht, um diese Zeichenfolge zu laden:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS-Webansichten bieten eine Methode speziell für Javascript aufrufen:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Zusammenfassung

In diesem Abschnitt wurden die Funktionen von der Web-Steuerelemente für Android und iOS, mit denen uns die hybridanwendungen mit Xamarin, einschließlich:

-  Die Möglichkeit zum Laden von HTML aus Zeichenfolgen, die in Code generiert werden soll,
-  Die Möglichkeit, lokale Dateien (CSS, Javascript, Bilder oder andere HTML-Dateien) zu verweisen.
-  Die Möglichkeit, das Abfangen von navigationsanforderungen, die in C#-Code,
-  Die Fähigkeit, Javascript aus c#-Code aufgerufen werden soll.


Im nächste Abschnitt wird eingeführt, Razor, dadurch wird es einfach, den HTML-Code verwendet in Hybrid-apps zu erstellen.

## <a name="what-is-razor"></a>Was ist Razor?

Razor ist einer Vorlagen-Engine, die mit ASP.NET MVC, ursprünglich zum Ausführen auf dem Server und zum Generieren von HTML an Webbrowser verarbeitet eingeführt wurde.

Die Razor-Vorlagen-Engine erweitert die standard-HTML-Syntax mit c#, damit Sie das Layout Express und CSS-Stylesheets und Javascript einfach integrieren können. Die Vorlage kann eine Modellklasse verweisen, was kann einen beliebigen benutzerdefinierten Typ, und, dessen Eigenschaften zugegriffen werden können, direkt aus der Vorlage. Eines der wichtigsten Vorteile ist die Möglichkeit, HTML und c#-Syntax problemlos zu kombinieren.

Razor-Vorlagen sind nicht auf eine serverseitige Verwendung beschränkt, sie können auch in Xamarin-apps enthalten sein. Mit Razor-Vorlagen zusammen mit der Möglichkeit für die programmgesteuerte Arbeit mit Web-Ansichten kann komplexe plattformübergreifenden Hybrid-Anwendungen mit Xamarin erstellt werden sollen.

### <a name="razor-template-basics"></a>Grundlagen der Razor-Vorlage

Razor-Vorlagendateien haben eine **.cshtml** Dateierweiterung. Sie können zu einem Xamarin-Projekt aus dem Abschnitt Textvorlagen hinzugefügt werden die **neue Datei** Dialogfeld:

 ![Neue Datei - Razor-Vorlage](images/image5_400x201.png)

Eine einfache Razor-Vorlage ( **RazorView.cshtml**) wird im folgenden dargestellt.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Beachten Sie die folgenden Unterschiede gegenüber eine normale HTML-Datei ein:

-  Die `@` Symbol verfügt über eine besondere Bedeutung in Razor-Vorlagen – gibt an, dass der folgende Ausdruck c# ausgewertet werden soll.
- `@model` Richtlinie wird immer als erste Zeile einer Razor-Vorlagendatei.
-  Die `@model` Richtlinie sollte einen Typ gefolgt werden. In diesem Beispiel wird eine einfache Zeichenfolge an die Vorlage übergeben, aber dies ist möglicherweise benutzerdefinierte Klasse.
-  Wenn `@Model` verwiesen wird, wird die Vorlage ermöglicht es einen Verweis auf das Objekt, die Vorlage übergeben wird, nachdem er generiert wurde (in diesem Beispiel werden eine Zeichenfolge).
-  Die IDE generiert automatisch die partielle Klasse für Vorlagen (Dateien mit der **.cshtml** Erweiterung). Sie können diesen Code anzeigen, aber es sollte nicht bearbeitet werden.
 ![RazorView.cshtml](images/image6_125x34.png) die partielle Klasse heißt RazorView entsprechend den Dateinamen der cshtml-Vorlage. Ist dieser Name, der zum Verweisen auf die Vorlage in c#-Code verwendet wird.
- `@using` -Anweisungen können auch am oberen Rand einer Razor-Vorlage, um zusätzliche Namespaces enthalten, die einbezogen werden.


Die endgültige HTML-Ausgabe kann dann mit dem folgenden C#-Code generiert werden. Beachten Sie, dass wir angeben, dass das Modell, um eine Zeichenfolge "Hello World" der in der gerenderten Ausgabe integriert werden, wird sein.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

So sieht die Ausgabe in eine Webansicht für den iOS-Simulator und Android-Emulator aus:

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Weitere Razor-Syntax

Verwenden in diesem Abschnitt es geht um eine grundlegende Razor-Syntax, um Ihnen beim Einstieg helfen führen es aus. In die Beispielen in diesem Abschnitt die folgende Klasse mit Daten füllen und mithilfe von Razor anzuzeigen:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Alle Beispiele verwenden die folgenden Daten Initialisierungscode

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Anzeigen von Eigenschaften des Modells

Wenn das Modell eine Klasse mit Eigenschaften ist, werden sie einfach in der Razor-Vorlage verwiesen, wie in dieser Beispielvorlage dargestellt:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Dies kann auf eine Zeichenfolge, die mit dem folgenden Code gerendert werden:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Die endgültige Ausgabe ist hier in eine Webansicht für den iOS-Simulator und Android-Emulator dargestellt:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>C#-Anweisungen

Komplexere c# können in der Vorlage, wie z. B. Updates der Modell-Eigenschaft und die Berechnung Alter in diesem Beispiel enthalten sein:

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

Sie können komplexe einzeilige C#-Ausdrücke (wie dem Formatieren von des Alters) schreiben, indem Sie den Code mit `@()`.

Mehrere C#-Anweisungen geschrieben werden können, durch die Umschließung mit `@{}`.

#### <a name="if-else-statements"></a>If-else-Anweisungen

Codeverzweigungen ausgedrückt werden können, mit `@if` wie in diesem Beispiel gezeigt.

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

Schleifenkonstrukte wie `foreach` kann auch hinzugefügt werden. Die `@` Präfixe verwendet werden können, auf die Schleifenvariable ( `@food` in diesem Fall) es in HTML gerendert werden soll.

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

Die Ausgabe der obigen Vorlage ist dargestellt, auf dem iOS-Simulator und Android-Emulator ausgeführt wird:

 ![Rupert X Monkey](images/image9_520x277.png)

In diesem Abschnitt wurden die Grundlagen der Verwendung von Razor-Vorlagen zum Rendern von Ansichten für einfache nur-Lese behandelt. Im nächste Abschnitt erläutert das vollständige apps, die mithilfe von Razor erstellen, die Benutzereingaben akzeptieren und Zusammenwirken von Javascript in der HTML-Ansicht und c#.

## <a name="using-razor-templates-with-xamarin"></a>Verwenden von Razor-Vorlagen mit Xamarin

In diesem Abschnitt wird erläutert, wie Build verwenden, Ihre eigenen hybridanwendung, die mit den Vorlagen in Visual Studio für Mac. Es gibt drei Vorlagen aus der **Datei > Neu > Projektmappe...**  Fenster:

- **Android > App > Android WebView-Anwendung**
- **iOS > App > WebView-Anwendung**
- **ASP.NET MVC-Projekt**



Die **neue Projektmappe** Fenster sieht wie folgt für iPhone und Android-Projekte – die Beschreibung der Lösung auf der rechten Seite werden hervorgehoben, Unterstützung für die Razor-Vorlagen-Engine.

 ![Erstellen von iPhone und Android-Lösungen](images/image13_1139x959.png)

Beachten Sie, die Sie ganz einfach hinzufügen können eine **.cshtml** Razor-Vorlage zum *alle* vorhandene Xamarin-Projekt, es ist nicht erforderlich, diese Lösungsvorlagen verwenden. iOS-Projekten benötigen kein Storyboard Razor entweder zu verwenden. einfach ein UIWebView-Steuerelement programmgesteuert zu einer beliebigen Ansicht hinzufügen, und Sie können Razor-Vorlagen in C#-Code gesamte rendern.

Der Inhalt der Standard-Lösung für iPhone und Android-Projekte werden unten angezeigt:

 ![iPhone und Android-Vorlagen](images/image10_428x310.png)

Die Vorlagen bieten Ihnen sofort umgehend Anwendungsinfrastruktur, laden eine Razor-Vorlage mit einer Data Model-Objekts, Benutzereingaben zu verarbeiten und an den Benutzer über Javascript kommunizieren.

Die wichtigen Teile der Lösung sind:

-  Statische Inhalte wie z. B. die **"Style.CSS"** Datei.
-  Razor-Vorlagendateien .cshtml wie **RazorView.cshtml** .
-  Model-Klassen, die in der Razor-Vorlagen, wie z. B. verwiesen werden **ExampleModel.cs** .
-  Die plattformspezifische-Klasse, die die Webansicht erstellt und rendert die Vorlage, wie z. B. die `MainActivity` unter Android und die `iPhoneHybridViewController` unter iOS.


Im folgende Abschnitt wird erläutert, wie die Projekte funktionieren.

### <a name="static-content"></a>Statischer Inhalt

Statischer Inhalte enthält CSS-Stylesheets, Bilder, Javascript-Dateien oder andere Inhalte, die verknüpft oder verweist auf eine HTML-Datei wird in einer Webansicht angezeigt werden kann.

Die Vorlagenprojekte enthalten einen minimalen Stylesheet veranschaulichen wie statischen Inhalte in der Hybrid-app eingeschlossen werden. Die CSS-Stylesheet wird in der Vorlage wie folgt verwiesen:

```html
<link rel="stylesheet" href="style.css" />
```

Sie können beliebige Stylesheet und die Javascript-Dateien, die Sie, einschließlich Frameworks wie JQuery benötigen hinzufügen.

### <a name="razor-cshtml-templates"></a>Razor-Cshtml-Vorlagen

Die Vorlage enthält eine Razor **.cshtml** -Datei, die bereits Code aus, um die Kommunikation von Daten zwischen der HTML-/Javascript und C# -Code geschrieben hat. Auf diese Weise können Sie Builds, die komplexe Hybrid-apps, die nicht nur schreibgeschützte Daten aus dem Modell anzeigen, aber auch im HTML-Code Benutzereingaben akzeptieren und übergeben sie an der C#-Code zur Verarbeitung oder Speicherung.

#### <a name="rendering-the-template"></a>Rendern von der Vorlage

Aufrufen der `GenerateString` rendert Sie in einer Vorlage HTML-Code für die Anzeige in eine Webansicht bereit. Wenn die Vorlage ein Modell verwendet sollten sie vor dem Rendern bereitgestellt werden. Dieses Diagramm veranschaulicht, wie Rendering – nicht funktioniert, die die statischen Ressourcen, von der Webansicht zur Laufzeit mithilfe des angegebenen Basisverzeichnisses aufgelöst werden, das um die angegebenen Dateien zu finden.

 ![Razor-Flussdiagramm](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Aufrufen von C#-Code aus der Vorlage

Kommunikation von einer gerenderten Webansicht Rückrufe zur C#-erfolgt durch Festlegen der URL für die Webansicht, und klicken Sie dann das Abfangen der Anforderung in C# geschrieben, die native Anforderung zu verarbeiten, ohne das erneute Laden der Webansicht.

Ein Beispiel kann in der Behandlung RazorViews-Schaltfläche angezeigt werden. Die Schaltfläche "hat die folgenden HTML-Code:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

Die `InvokeCSharpWithFormValues` Javascript-Funktion liest alle Werte aus dem HTML-Formular und legt die `location.href` für die Webansicht:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

Dies versucht, zum Navigieren der Webansicht an eine URL mit einem benutzerdefinierten Schema (z. b. `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Wenn die systemeigene Webansicht dieses navigationsanforderung verarbeitet wird, haben wir die Möglichkeit, diese abzufangen. In iOS erfolgt dies durch das Behandeln der UIWebViews HandleShouldStartLoad-Ereignisses. In Android wir einfach die Unterklasse der WebViewClient im Formular verwendet wird, und überschreiben ShouldOverrideUrlLoading.

Die Interna der diese zwei Navigation Interceptors ist im Wesentlichen identisch.

Überprüfen Sie zunächst die URL, die Webansicht versucht, zu laden, und wenn es nicht mit dem benutzerdefinierten Schema gestartet (`hybrid:`), können Sie die Navigation wie gewohnt.

Für die benutzerdefinierte URL-Schema, alles, was in der URL zwischen dem Schema und die "?" ist der Methodenname (in diesem Fall "UpdateLabel") verarbeitet werden. Alles, was in der Abfragezeichenfolge wird als Parameter für den Aufruf der Methode behandelt:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` In diesem Beispiel wird eine minimale Menge Zeichenfolgenmanipulation für den Textbox-Parameter ("C# -Code besagt" die Zeichenfolge voranstellen), und klicken Sie dann einen Rückruf an die Webansicht.

Nach dem Verarbeiten der URL, bricht die Methode, damit die Webansicht nicht versucht, die abgeschlossen wird, um die benutzerdefinierte URL navigieren die Navigation ab.

#### <a name="manipulating-the-template-from-c"></a>Bearbeiten die Vorlage über c#

Kommunikation mit einer gerenderten HTML-Web-Ansicht von C# -Code erfolgt durch Aufrufen von Javascript in der Webansicht. Unter iOS, dies erfolgt durch Aufrufen von `EvaluateJavascript` auf die UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

Unter Android Javascript aufgerufen werden, die Webansicht Laden Sie den JavaScript-Code als eine URL mit der `"javascript:"` URL-Schema:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Eine App vollständig hybride

Diese Vorlagen nehmen Sie keine von systemeigenen Steuerelementen auf jeder Plattform verwenden – der gesamte Bildschirm mit einer einzelnen Web-Ansicht gefüllt ist.

HTML kann sich hervorragend für die Erstellung von Prototypen, und die verschiedene Möglichkeiten zur Anzeige im Web optimiert ist z. B. rich-Text und zum dynamischen Layout. In HTML und Javascript – Durchführen eines Bildlaufs durch lange Listen mit Daten, jedoch nicht alle Tasks geeignet sind führt z. B. unter Android mit nativen UI-Steuerelemente wie z. B. (unter iOS UITableView) oder der ListView.

Die Webansichten in der Vorlage können problemlos mit plattformspezifischen Steuerelementen – einfach bearbeiten verbessert werden die **MainStoryboard.storyboard** in der iOS-Designer oder der **Resources/layout/Main.axml** unter Android.

### <a name="razortodo-sample"></a>RazorTodo-Beispiel

Die [RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) -Repository enthält zwei separate Lösungen, um die Unterschiede zwischen einer vollständig HTML-driven app und eine app, die HTML mit nativen Steuerelementen kombiniert anzuzeigen:

-  **RazorTodo** -vollständig HTML-driven app mit Razor-Vorlagen.
-  **RazorNativeTodo** - systemeigenen Listenansicht-Steuerelemente für iOS und Android verwendet, jedoch zeigt der Bildschirm zum Bearbeiten mit HTML und Razor.


Führen Sie diese Xamarin-apps auf IOS- und Android, Portable Class Libraries (PCLs) zum Freigeben von gemeinsamen Codes wie z. B. die Datenbank- und modellsicherheitseinstellungen Klassen nutzen. Razor **.cshtml** Vorlagen können auch in der PCL enthalten sein, damit diese problemlos plattformübergreifend gemeinsam genutzt werden.

Twitter freigeben und -Sprach-APIs aus der nativen Plattform zeigt, dass die Hybrid-Anwendungen mit Xamarin immer noch Zugriff auf die gesamte zugrunde liegende Funktionalität von Vorlagen gesteuerter HTML Razor-Ansichten haben beide Beispiel-apps zu integrieren.

Die **RazorTodo** app verwendet die HTML-Razor-Vorlagen für die Liste "und" Bearbeiten-Ansichten. Dies bedeutet, dass wir können die app fast vollständig in einer gemeinsamen portablen Klassenbibliothek erstellen (einschließlich der Datenbank und **.cshtml** Razor-Vorlagen). Die folgenden Screenshots zeigen die IOS- und Android-apps.

 ![RazorTodo](images/Both_700x290.png)

Die **RazorNativeTodo** app wird mithilfe einer HTML-Razor-Vorlage für die Bearbeitungsansicht, aber eine Liste der native Bildlauf auf jeder Plattform implementiert. Dies bietet eine Reihe von Vorteilen, einschließlich:

-  Leistung – verwenden Sie die native Bildlauf Steuerelemente Virtualisierung um sicherzustellen, dass schnelle und problemlose scrollen, selbst bei sehr langen Listen mit Daten.
-  Native Benutzeroberfläche - Clientplattform-spezifische UI-Elemente können auf einfache Weise aktiviert werden, z. B. die indexunterstützung schnelle Bildläufe in iOS und Android.


Ein wichtiger Vorteil der Erstellung von Hybrid-apps mit Xamarin ist, beginnen mit einer vollständig HTML-gesteuerten Benutzeroberfläche (z. B. das erste Beispiel) und plattformspezifische Funktionen (wie im zweiten Beispiel gezeigt) im Bedarfsfall hinzugefügt werden können. Die native Liste Bildschirme und HTML Razor Bearbeitungsbildschirme auf iOS und Android werden unten angezeigt.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Funktionen von der Web-Steuerelemente verfügbar unter iOS und Android, die das Erstellen von erleichtern erläutert Hybrid-Anwendungen.

Es erläutert klicken Sie dann die Razor-Vorlagen-Engine und die Syntax, die zum Generieren von HTML einfach im Xamarin-apps mithilfe von verwendet werden kann. **Cshtml** Razor-Vorlagendateien. Es wurde außerdem Visual Studio für Mac, Vorlagen, mit denen Sie schnell Einstieg zur Erstellung von hybridanwendungen mit Xamarin, beschrieben.

Zum Schluss führte RazorTodo Proben, die Webansichten mit nativen Benutzeroberflächen und APIs kombinieren veranschaulicht.

### <a name="related-links"></a>Verwandte Links

- [RazorTodo-Beispiel](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 – Razor-Ansichtsengine (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
