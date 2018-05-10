---
title: Erstellen von HTML-Ansichten mit Razor-Vorlagen
description: " Verwenden eine Vollbild-Webseite zum Rendern von HTML kann eine einfache und effektive Möglichkeit zum Rendern komplexer Formatierung auf eine Weise plattformübergreifende, insbesondere, wenn Sie bereits die HTML, Javascript und CSS aus einem Websiteprojekt verfügen."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 48d7778bf3225401f2819909ae6be320cfa881e3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="building-html-views-using-razor-templates"></a>Erstellen von HTML-Ansichten mit Razor-Vorlagen

Mobile Entwicklung weltweit bezieht sich die Begriff "Hybrid-app" in der Regel zu einer Anwendung, in dem einige (oder alle) der Bildschirme, die als HTML-Seiten in einem gehosteten Web-Viewer-Steuerelement dargestellt.

Es gibt einige entwicklungsumgebungen, mit denen Sie erstellen Ihrer mobilen app vollständig in HTML und Javascript, jedoch diese apps von Leistungsproblemen Leiden können, wenn komplexe Verarbeitungsvorgänge oder Benutzeroberflächeneffekte ausführen möchten, und sind auch in der Plattform beschränkt. Funktionen, die sie zugreifen können.

Xamarin bietet das beste aus beiden Welten, insbesondere, wenn HTML Razor-vorlagenmoduls nutzen. Mit Xamarin erhalten Sie die Flexibilität plattformübergreifende vorlagenbasierte HTML Ansichten erstellen, die Javascript und CSS verwenden, aber auch haben vollständigen Zugriff auf die zugrunde liegenden Plattform-APIs und die schnelle Verarbeitung mithilfe von c#.

Dieses Dokument erläutert, wie mithilfe der Razor-vorlagenmoduls erstellen HTML + Javascript + CSS-Sichten, die auf mobilen Plattformen mit Xamarin verwendet werden können.

## <a name="using-web-views-programmatically"></a>Programmgesteuertes Verwenden von Webansichten

Bevor wir Razor Informationen in diesem Abschnitt wird von behandelt Webansichten direkt – anzeigen von HTML-Inhalt verwenden insbesondere HTML-Inhalte, die innerhalb einer Anwendung generiert wird.

Xamarin bietet vollständigen Zugriff auf die zugrunde liegenden Plattform-APIs auf IOS- und Android-Geräten, daher ist es einfach zu erstellen und Anzeigen von HTML mithilfe von c#. Die grundlegende Syntax für jede Plattform ist unten dargestellt.

### <a name="ios"></a>iOS

Anzeigen von HTML in einem Steuerelement UIWebView in Xamarin.iOS verwendet auch nur ein paar Codezeilen:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Finden Sie unter der [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) Know-how für die Weitere Informationen zur Verwendung des UIWebView-Steuerelements.

### <a name="android"></a>Android

Anzeigen von HTML in einem WebView-Steuerelement mit Xamarin.Android erfolgt in nur wenigen Codezeilen:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Finden Sie unter der [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) Know-how für die Weitere Informationen zur Verwendung der WebView-Steuerelement.

### <a name="specifying-the-base-directory"></a>Angeben des Basisverzeichnisses

Auf beiden Plattformen ist ein Parameter, der angibt, das Basisverzeichnis für die HTML-Seite vorhanden. Dies ist der Speicherort auf dem Gerät-Dateisystem, das zum Auflösen relativer Verweise auf Ressourcen wie Bilder und CSS-Dateien verwendet wird. Z. B. wie RFID-Transponder

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

Diese Dateien finden Sie unter: **style.css**, **monkey.jpg** und **jscript.js**. Das Basisverzeichnis-Einstellung informiert Webansicht, wo sich diese Dateien befinden, damit sie in die Seite geladen werden können.

#### <a name="ios"></a>iOS

Die Vorlagenausgabe wird in iOS durch den folgenden C#-Code dargestellt:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Das Basisverzeichnis ist angegeben, als `NSBundle.MainBundle.BundleUrl` die bezieht sich auf das Verzeichnis, das in die Anwendung installiert wird. Alle Dateien in der **Ressourcen** Ordner werden auf diesen Speicherort kopiert, z. B. die **style.css** Datei, die hier dargestellt:

 ![iPhoneHybrid-Lösung](images/image1_240x163.png)

Der Buildvorgang für alle statischen Inhaltsdateien muss **BundleResource**:

 ![iOS-Projekt erstellen, Aktion: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android erfordert auch ein Basisverzeichnis, die als Parameter übergeben werden, wenn html-Zeichenfolgen in einer Webansicht angezeigt werden.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Die Zeichenfolge **file:///android_asset/** bezieht sich auf den Ordner "Android Assets" in Ihrer app angezeigt, hier enthält die **style.css** Datei.

 ![AndroidHybrid-Lösung](images/image3_240x167.png)

Der Buildvorgang für alle statischen Inhaltsdateien muss **AndroidAsset**.

 ![Android-Projekt erstellen, Aktion: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Aufrufen von c# aus HTML und Javascript

Wenn eine html-Seite in eine Webansicht geladen wird, behandelt die Links und Formulare, als wäre die Seite von einem Server geladen wurde. Dies bedeutet, dass wenn der Benutzer klickt auf einen Link oder ein Formular übermittelt Webansicht versucht, navigieren Sie auf das angegebene Ziel.

Wenn die Verknüpfung mit einem externen Server (z. B. google.com) wurde versucht Webansicht, laden die externe Website (vorausgesetzt, es gibt eine Internetverbindung).

```html
<a href="http://google.com/">Google</a>
```

Wenn der Link relative ist versucht Webansicht, dass der Inhalt aus dem Basis-Verzeichnis zu laden. Natürlich ist keine Verbindung zum Netzwerk erforderlich, damit dies funktioniert, wie der Inhalt in der app auf dem Gerät gespeichert ist.

```html
<a href="somepage.html">Local content</a>
```

Formularaktionen führen Sie die gleiche Regel.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Sie sind nicht zum Hosten von eines Webservers auf dem Client sehr; Allerdings können Sie verwenden den gleichen Server Kommunikation Techniken in heutigen reaktionsfähiges Design Patterns eingesetzt Services über HTTP GET aufgerufen und asynchron verarbeiten von Antworten durch das Ausgeben von Javascript (oder aufrufende Javascript ist bereits in der Webansicht gehostet). Dadurch können Sie problemlos übergeben von Daten aus den HTML-Code wieder in C#-Code für die Verarbeitung und Anzeige der Ergebnisse der HTML-Seite zurück.

IOS und Android bieten einen Mechanismus zum Anwendungscode diese Navigationsereignisse abzufangen, sodass app-Code (falls erforderlich) reagieren kann. Diese Funktion ist entscheidend, um Hybrid-apps erstellen, da sie nativen Code mit Webansicht interagieren können.

#### <a name="ios"></a>iOS

Das ShouldStartLoad-Ereignis für die Webansicht in iOS kann überschrieben werden, damit kann der Anwendungscode eine Anfrage Navigation (z. B. ein Link) behandelt. Die Methodenparameter bereitstellen alle Informationen.

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

und weisen Sie den Ereignishandler:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Unter Android einfach Unterklasse WebViewClient und dann Code implementieren, so reagieren Sie auf der navigationsanforderung.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

ein, und legen Sie den Client für die Webansicht:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Aufrufen von Javascript in c#

C#-Code kann sagt eine Webansicht, um eine neue HTML-Seite zu laden, Javascript auch innerhalb der aktuell angezeigten Seite ausführen. Gesamte Blöcke von Javascript-Code mithilfe von C#-Zeichenfolgen erstellt und ausgeführt werden können oder Sie können die Methodenaufrufe an bereits verfügbar ist, auf der Seite über Javascript hochverfügbares `script` Tags.

#### <a name="android"></a>Android

Erstellen Sie den Javascript-Code, um ausgeführt werden, und klicken Sie dann mit Präfix "Javascript:" und weisen Sie Webansicht diese Zeichenfolge geladen:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS Webansichten bieten eine Methode speziell zum Aufrufen von Javascript:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Zusammenfassung

In diesem Abschnitt wurde eingeführt, die Funktionen von der Web-Steuerelemente unter Android und iOS, mit denen uns mit Xamarin hybridanwendungen erstellen, einschließlich:

-  Die Fähigkeit, laden HTML aus Zeichenfolgen, die im Code generiert,
-  Die Möglichkeit, lokale Dateien (CSS, Javascript, Bilder oder andere HTML-Dateien), verweisen
-  Die Fähigkeit zum Abfangen von Anforderungen der Navigation in C#-Code,
-  Die Fähigkeit, Javascript in C#-Code aufrufen.


Im nächste Abschnitt führt Razor, dem vereinfacht, den HTML-Code zum Verwenden in Hybrid-apps zu erstellen.

## <a name="what-is-razor"></a>Was ist Razor?

Razor ist ein Datenvorlagen-Modul, das mit ASP.NET MVC, ursprünglich zum auf dem Server ausgeführt und generieren HTML an Webbrowser versorgt werden eingeführt wurde.

Razor-vorlagenmoduls erweitert die standard-HTML-Syntax mit c#, damit Sie das Layout Express und CSS-Stylesheets und Javascript problemlos integrieren können. Die Vorlage kann eine Modellklasse verweisen, erfolgen kann jeder beliebige benutzerdefinierte Typ und dessen Eigenschaften zugegriffen werden können, direkt aus der Vorlage. Einer der Hauptvorteile ist die Möglichkeit, HTML und C#-Syntax problemlos zu kombinieren.

Razor-Vorlagen sind nicht auf eine serverseitige Verwendung beschränkt, sie können auch im Xamarin-apps enthalten. Mithilfe von Razor-Vorlagen sowie die Möglichkeit, um Webansichten programmatisch zu arbeiten, können anspruchsvolle plattformübergreifende hybridanwendungen mit Xamarin erstellt werden sollen.

### <a name="razor-template-basics"></a>Grundlagen der Razor-Vorlage

Razor-Vorlagendateien haben eine **cshtml** Dateierweiterung. Sie können zu einem Xamarin-Projekt aus dem Abschnitt Textvorlagen im hinzugefügt werden die **neue Datei** Dialogfeld:

 ![Neue Datei - Razor-Vorlage](images/image5_400x201.png)

Eine einfache Razor-Vorlage ( **RazorView.cshtml**) wird unten gezeigt.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Beachten Sie die folgenden Unterschiede gegenüber eine normale HTML-Datei ein:

-  Die `@` Symbol hat eine besonderen Bedeutung in Razor-Vorlagen – Es gibt an, dass der folgende Ausdruck c# ausgewertet werden soll.
- `@model` Richtlinie wird immer als erste Zeile einer Razor-Vorlagendatei.
-  Die `@model` Richtlinie sollte einen Typ folgen. In diesem Beispiel wird eine einfache Zeichenfolge an der Vorlage übergeben wird, aber dies ist möglicherweise eine benutzerdefinierte Klasse.
-  Wenn `@Model` verwiesen wird während des gesamten der Vorlage, die er bietet eines Verweis auf das Objekt, die Vorlage übergeben wird, wenn sie generiert werden (in diesem Beispiel werden eine Zeichenfolge).
-  Die IDE generiert automatisch die partielle Klasse für Vorlagen (Dateien mit der **cshtml** Erweiterung). Sie können diesen Code anzeigen, jedoch sollte nicht bearbeitet werden.
 ![RazorView.cshtml](images/image6_125x34.png) die partielle Klasse heißt RazorView zur Übereinstimmung mit den Dateinamen der cshtml-Vorlage. Es ist diesen Namen, die zum Verweisen auf die Vorlage in C#-Code verwendet wird.
- `@using` -Anweisungen können auch am oberen Rand einer Razor-Vorlage, um zusätzliche Namespaces enthalten, die einbezogen werden.


Die endgültige HTML-Ausgabe kann dann durch den folgenden C#-Code generiert werden. Beachten Sie, dass wir geben Sie das Modell, um eine Zeichenfolge "Hello World" dem in der gerenderten Vorlagenausgabe integriert wird.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

So sieht die Ausgabe in eine Webansicht auf dem iOS-Simulators und des Android-Emulator dargestellt:

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Weitere Razor-Syntax

In diesem Abschnitt aus, den wir einige grundlegende Razor-Syntax, um Ihnen beim Einstieg helfen einführen verwenden. In den Beispielen in diesem Abschnitt die folgende Klasse mit Daten auffüllen und Razor anzeigen:

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

#### <a name="displaying-model-properties"></a>Anzeigen von Modelleigenschaften

Wenn das Modell eine Klasse mit Eigenschaften ist, werden sie einfach in der Razor-Vorlage referenziert, wie in diesem Beispielvorlage dargestellt:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Dies kann in eine Zeichenfolge, die mithilfe des folgenden Codes gerendert werden:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Die endgültige Ausgabe wird in einer Webansicht auf dem iOS-Simulators und des Android-Emulator hier gezeigt:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>C#-Anweisungen

Komplexere c# können in der Vorlage, wie das Modell die Updates und die Berechnung Alter in diesem Beispiel enthalten sein:

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

Sie können komplexe einzeilige C#-Ausdrücke (z. B. Formatierung Alter) schreiben, indem Sie den Code mit `@()`.

Mehrere C#-Anweisungen geschrieben werden können, von den umgebenden mit `@{}`.

#### <a name="if-else-statements"></a>If-else-Anweisungen

Codeverzweigungen ausgedrückt werden können, mit `@if` wie in diesem Vorlagenbeispiel dargestellt.

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

Schleifenkonstrukte wie `foreach` kann auch hinzugefügt werden. Die `@` Präfix kann in der Schleifenvariablen verwendet werden soll ( `@food` in diesem Fall) er in HTML gerendert werden soll.

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

Die Ausgabe der oben genannten Vorlage wie auf dem iOS-Simulators und des Android-Emulator ausgeführt wird:

 ![Rupert X Monkey](images/image9_520x277.png)

In diesem Abschnitt wurden die Grundlagen der Verwendung von Razor-Vorlagen zum Rendern des einfachen schreibgeschützten Ansichten behandelt. Im nächste Abschnitt erläutert die vollständige apps Razor zu erstellen, die Benutzereingaben akzeptieren und Zusammenwirken von Javascript in der HTML-Ansicht und c# können.

## <a name="using-razor-templates-with-xamarin"></a>Mithilfe von Razor-Vorlagen mit Xamarin

In diesem Abschnitt wird erläutert, wie Build eigene hybridanwendung mithilfe der Lösung-Projektvorlagen in Visual Studio für Mac verwenden. Es stehen drei Vorlagen aus der **Datei > Neu > Projektmappe...**  Fenster:

- **Android > App > Android WebView-Anwendung**
- **iOS > App > WebView-Anwendung**
- **ASP.NET MVC-Projekt**



Die **neue Projektmappe** Fenster sieht wie folgt für iPhone und Android-Projekte: die Beschreibung der Lösung auf der rechten Seite hervorgehoben, Unterstützung für die Razor-vorlagenmoduls.

 ![Erstellen von iPhone und Android-Lösungen](images/image13_1139x959.png)

Beachten Sie, die Sie problemlos hinzufügen, können eine **cshtml** Razor-Vorlage, um *alle* vorhandenen Xamarin-Projekt, es ist nicht erforderlich, diese Lösungsvorlagen verwenden. iOS-Projekte erfordern ein Storyboard mit Razor verwenden; einfach ein UIWebView-Steuerelement programmgesteuert zu einer beliebigen Ansicht hinzufügen, und Sie können Razor-Vorlagen in C#-Code gesamte rendern.

Nachfolgend finden Sie den Inhalt für die Lösung von Vorlage für iPhone und Android-Projekte:

 ![iPhone und Vorlagen für Android](images/image10_428x310.png)

Die Vorlagen bieten Ihnen Ready to Go-Infrastruktur zum Laden einer Razor-Vorlage mit einem Daten-Modellobjekt, Benutzereingaben zu verarbeiten und an den Benutzer über Javascript kommunizieren.

Die wichtigen Teile der Lösung sind:

-  Statischer Inhalt, z. B. die **style.css** Datei.
-  Razor cshtml-Vorlagendateien wie **RazorView.cshtml** .
-  Modellieren von Klassen, die in den Razor-Vorlagen, wie z. B. verwiesen werden **ExampleModel.cs** .
-  Die plattformspezifischen-Klasse, die Webansicht erstellt und rendert die Vorlage an, wie z. B. die `MainActivity` unter Android und die `iPhoneHybridViewController` unter iOS.


Im folgende Abschnitt wird erläutert, wie die Projekte zusammenarbeiten.

### <a name="static-content"></a>Statischer Inhalt

Statischer Inhalt umfasst die CSS-Stylesheets, Bilder, Javascript-Dateien oder andere Inhalte, die aus verknüpft oder verweist auf eine HTML-Datei wird in einer Webansicht angezeigt werden kann.

Die Vorlagenprojekte enthalten einen minimalen Stylesheet um statischen Inhalt in der Hybrid-app einschließen veranschaulichen. Die CSS-Stylesheet wird in der Vorlage wie folgt auf die verwiesen wird:

```html
<link rel="stylesheet" href="style.css" />
```

Sie können dem Stylesheet und Javascript-Dateien, die Sie, einschließlich Frameworks wie JQuery benötigen hinzufügen.

### <a name="razor-cshtml-templates"></a>Razor-Cshtml-Vorlagen

Die Vorlage enthält eine Razor **cshtml** -Datei, die Code aus, um die Datenkommunikation zwischen HTML/Javascript und c# unterstützen vorab geschrieben hat. Auf diese Weise können Sie anspruchsvolle Hybrid-apps, die nicht nur schreibgeschützte Daten aus dem Modell anzeigen, aber auch annehmen von Benutzereingaben in HTML und übergibt ihn dann, in C#-Code für die Verarbeitung oder Speicher zurück erstellen.

#### <a name="rendering-the-template"></a>Rendern der Vorlage

Aufrufen der `GenerateString` in einer Vorlage rendert HTML für die Anzeige in einer Webansicht bereit. Wenn die Vorlage ein Modell verwendet, sollte sie vor dem Rendern angegeben. Dieses Diagramm veranschaulicht, wie Rendering – nicht funktioniert, die die statischen Ressourcen von Webansicht zur Laufzeit mithilfe des angegebenen Basisverzeichnisses finden Sie die angegebenen Dateien aufgelöst werden.

 ![Razor-Flussdiagramm](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Aufrufen von C#-Code aus der Vorlage

Kommunikation mit einer gerenderten Webansicht Rückrufe zu C#-erfolgt durch Festlegen der URL für die Webansicht, und klicken Sie dann die Anforderung in c# um die native Anforderung zu verarbeiten, ohne erneutes Laden der Webansicht abfangen.

Ein Beispiel kann in der Behandlung RazorViews Schaltfläche angezeigt werden. Die Schaltfläche "" hat die folgenden HTML-Code:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

Die `InvokeCSharpWithFormValues` Javascript-Funktion liest alle Werte aus dem HTML-Formular und legt die `location.href` für die Webansicht:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

Dies versucht, navigieren Sie in der Webansicht an eine URL mit einem benutzerdefinierten Schema (z. b. `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Wenn die systemeigene Webansicht dieser navigationsanforderung verarbeitet, haben wir die Möglichkeit, die sie nicht abfangen. In iOS erfolgt dies durch die Behandlung der UIWebView HandleShouldStartLoad-Ereignisses. In der Android-Geräten wir einfach die Unterklasse der WebViewClient im Formular verwendet, und überschreiben Sie ShouldOverrideUrlLoading.

Im Rahmen der diese zwei Navigation Interceptors ist im Wesentlichen identisch.

Überprüfen Sie zunächst die URL, die Webansicht versucht wird, zu laden, und wenn sie das benutzerdefinierte Schema nicht gestartet (`hybrid:`), ermöglichen die Navigation wie gewohnt.

Für die benutzerdefinierte URL-Schema, alles in der URL zwischen dem Schema und die "?" ist der Methodenname (in diesem Fall "UpdateLabel") verarbeitet werden. Alles, was in der Abfragezeichenfolge wird als Parameter für den Methodenaufruf behandelt:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` In diesem Beispiel wird nur eine minimale Menge Zeichenfolgenmanipulation im Textbox-Parameter ("c# besagt, dass" in der Zeichenfolge vorangestellt wird), und klicken Sie dann einen Rückruf an die Webansicht.

Nach dem Verarbeiten der URL, bricht die Methode die Navigation ab, sodass Webansicht nicht versucht, Fertig stellen, um die benutzerdefinierte URL navigieren.

#### <a name="manipulating-the-template-from-c"></a>Bearbeiten die Vorlage in c#

Kommunikation mit einer gerenderten HTML-Webansicht aus c# erfolgt durch Aufrufen von Javascript in der Webansicht. Bei iOS kann dies erfolgt durch Aufrufen von `EvaluateJavascript` auf die UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

Auf Android-Geräten Javascript ausgerufen werden in der Webansicht durch Laden den JavaScript-Code als URL mithilfe der `"javascript:"` URL-Schema:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Erstellen eine app wirklich hybride

Diese Vorlagen nehmen Sie keine von systemeigenen Steuerelementen auf jeder Plattform verwenden – der gesamte Bildschirm mit einer einzelnen Webansicht gefüllt ist.

HTML kann sich hervorragend für die Erstellung von Prototypen und die Arten von Aufgaben anzeigen im Web am besten geeignet ist z. B. rich-Text und dynamisches Layout. HTML und Javascript – einen Bildlauf durch lange Listen mit Daten, jedoch nicht alle Tasks geeignet sind führt z. B. mit besser unter Android native Benutzeroberflächen-Steuerelemente, z. B. (UITableView für iOS) oder ListView.

Die Webansichten in der Vorlage können problemlos mit plattformspezifischen Steuerelemente – einfach bearbeiten verbessert werden die **MainStoryboard.storyboard** im iOS-Designer oder der **Resources/layout/Main.axml** auf Android-Geräten.

### <a name="razortodo-sample"></a>RazorTodo-Beispiel

Die [RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) -Repository enthält zwei separate Lösungen, um die Unterschiede zwischen einer vollständig HTML-driven-app und eine app, die mit systemeigenen Steuerelementen HTML kombiniert dargestellt:

-  **RazorTodo** -vollständig HTML-driven app mithilfe von Razor-Vorlagen.
-  **RazorNativeTodo** - systemeigenen Listenansicht-Steuerelemente für IOS- und Android verwendet jedoch den Bearbeitungsbildschirm mit HTML und Razor zeigt.


Führen Sie diese Xamarin-apps auf IOS- und Android-Geräten mit portablen Klassenbibliotheken (PCLs) gemeinsamen Code z. B. die Datenbank- und modellsicherheitseinstellungen Klassen freigeben. Razor **cshtml** Vorlagen können auch in der PCL enthalten sein, damit sie problemlos über Plattformen hinweg gemeinsam sind.

Beide Beispiel-apps integrieren Twitter gemeinsame Nutzung und -Sprach-APIs von die systemeigene Plattform, die veranschaulichen, dass eine hybridanwendungen mit Xamarin weiterhin Zugriff auf die zugrunde liegenden Funktionalität vom Vorlage gesteuerte HTML Razor-Ansichten verfügen.

Die **RazorTodo** app HTML Razor-Vorlagen für die Liste und Bearbeiten von Ansichten verwendet. Dies bedeutet, dass wir können die app nahezu vollständig in einer freigegebenen Portable Klassenbibliothek erstellen (einschließlich der Datenbank und **cshtml** Razor-Vorlagen). Die folgenden Screenshots zeigen die IOS- und Android-apps.

 ![RazorTodo](images/Both_700x290.png)

Die **RazorNativeTodo** app verwendet eine HTML-Razor-Vorlage für die Bearbeitungsansicht, aber eine systemeigene bildlauffähigen Liste auf jeder Plattform implementiert. Dies bietet eine Reihe von Vorteilen, einschließlich:

-  Leistung - verwenden Sie die systemeigene Bildlauf Steuerelemente Virtualisierung um sicherzustellen, dass schnelle, smooth Durchführen eines Bildlaufs selbst bei sehr lange Listen mit Daten.
-  Systemeigene Leistung - plattformspezifischen Benutzeroberflächenelemente sind leicht aktiviert werden, z. B. das Durchführen eines Bildlaufs Fast indexunterstützung in iOS und Android.


Ein wichtiger Vorteil der Erstellung mit Xamarin Hybrid-apps ist, beginnen Sie mit einer vollständig HTML-driven Benutzeroberfläche (z. B. im ersten Beispiel) und Clientplattform-spezifische Funktionen (wie in der zweiten Beispiel dargestellt) im Bedarfsfall hinzugefügt werden können. Die systemeigene Liste Bildschirme und die HTML-Razor bearbeiten Sie Bildschirme auf beide iOS, und Android sind unten aufgeführt.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Funktionen von der Web-Steuerelemente verfügbar auf IOS- und Android, die das Erstellen von erleichtern erläutert hybridanwendungen.

Er erläutert klicken Sie dann die Razor-vorlagenmoduls und die Syntax, die verwendet werden kann, um HTML-Daten einfach im Xamarin-apps mit generieren. **Cshtml** Razor-Vorlagendateien. Auch wird die Visual Studio für Mac-Projektmappenvorlagen, mit denen Sie schnell beginnen, Erstellen von hybridanwendungen mit Xamarin beschrieben.

Schließlich führte RazorTodo Proben, die diesen veranschaulichen Webansichten mit native Benutzeroberflächen und APIs zu kombinieren.

### <a name="related-links"></a>Verwandte Links

- [RazorTodo-Beispiel](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 - Razor-Ansichtsmodul (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
