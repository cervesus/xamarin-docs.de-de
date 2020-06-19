---
title: 'title: "Anpassen eines WebView-Elements" description: "Das WebView-Element in Xamarin.Forms ist eine Ansicht, die Web- und HTML-Inhalte in Ihrer App anzeigt.'
description: 'In diesem Artikel wird erläutert, wie Sie einen benutzerdefinierten Renderer erstellen, der WebView erweitert, um das Aufrufen von C#-Code über JavaScript zuzulassen." ms.prod: xamarin ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 03/31/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/31/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c83742896af4a22bcff327df82c1b14ff983bb2
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138969"
---
# <a name="customizing-a-webview"></a>Anpassen eines WebView-Elements

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-hybridwebview)

_Das `WebView`-Element in Xamarin.Forms ist eine Ansicht, die Web- und HTML-Inhalte in Ihrer App anzeigt. In diesem Artikel wird erläutert, wie Sie einen benutzerdefinierten Renderer erstellen, der `WebView` erweitert, um das Aufrufen von C#-Code über JavaScript zuzulassen._

Jede Xamarin.Forms-Ansicht verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. Wenn ein [`WebView`](xref:Xamarin.Forms.WebView)-Objekt durch eine Xamarin.Forms-App unter iOS gerendert wird, wird die Klasse `WkWebViewRenderer` instanziiert, wodurch wiederum ein natives `WkWebView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `WebViewRenderer`-Klasse ein natives `WebView`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `WebViewRenderer`-Klasse ein natives `WebView`-Steuerelement. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Rendererbasisklassen und native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`View`](xref:Xamarin.Forms.View)-Objekt und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](hybridwebview-images/webview-classes.png "Relationship Between the WebView Class and its Implementing Native Classes")

Der Renderingprozess kann genutzt werden, um Anpassungen für die Plattform zu implementieren, indem für eine [`WebView`](xref:Xamarin.Forms.WebView)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#create-the-hybridwebview) Sie das benutzerdefinierte Steuerelement `HybridWebView`.
1. [Verarbeiten](#consume-the-hybridwebview) Sie das `HybridWebView`-Element in Xamarin.Forms.
1. [Erstellen](#create-the-custom-renderer-on-each-platform) Sie einen benutzerdefinierten Renderer für das `HybridWebView`-Objekt auf jeder Plattform.

Im Folgenden werden alle Elemente zum Implementieren eines `HybridWebView`-Renderers behandelt, der das Xamarin.Forms-[`WebView`](xref:Xamarin.Forms.WebView)-Element erweitert, damit C#-Code über JavaScript aufgerufen werden kann. Die `HybridWebView`-Instanz wird verwendet, um eine HTML-Seite anzuzeigen, die den Benutzer auffordert, seinen Namen einzugeben. Wenn der Benutzer auf die HTML-Schaltfläche klickt, ruft eine JavaScript-Funktion ein C#-`Action`-Objekt auf, das den Benutzernamen enthält.

Weitere Informationen zum Aufrufen von C#-Code über JavaScript finden Sie unter [Aufrufen von C#-Code über JavaScript](#invoke-c-from-javascript). Weitere Informationen über HTML-Seiten finden Sie unter [Erstellen der Webseite](#create-the-web-page).

> [!NOTE]
> Ein [`WebView`](xref:Xamarin.Forms.WebView)-Element kann eine JavaScript-Funktion über C# aufrufen und ein beliebiges Ergebnis an den aufrufenden C#-Code zurückgeben. Weitere Informationen hierzu finden Sie unter [Aufrufen von JavaScript](~/xamarin-forms/user-interface/webview.md#invoking-javascript).

## <a name="create-the-hybridwebview"></a>Erstellen eines HybridWebView-Steuerelements

Das benutzerdefiniertes Steuerelement `HybridWebView` kann erstellt werden, indem die [`WebView`](xref:Xamarin.Forms.WebView)-Klasse als Unterklasse erstellt wird:

```csharp
public class HybridWebView : WebView
{
    Action<string> action;

    public static readonly BindableProperty UriProperty = BindableProperty.Create(
        propertyName: "Uri",
        returnType: typeof(string),
        declaringType: typeof(HybridWebView),
        defaultValue: default(string));

    public string Uri
    {
        get { return (string)GetValue(UriProperty); }
        set { SetValue(UriProperty, value); }
    }

    public void RegisterAction(Action<string> callback)
    {
        action = callback;
    }

    public void Cleanup()
    {
        action = null;
    }

    public void InvokeAction(string data)
    {
        if (action == null || data == null)
        {
            return;
        }
        action.Invoke(data);
    }
}
```

Das benutzerdefinierte Steuerelement `HybridWebView` wird im .NET Standard-Bibliotheksprojekt erstellt und definiert die folgende API für das Steuerelement:

- Eine `Uri`-Eigenschaft, die die Adresse der zu ladenden Webseite angibt.
- Eine `RegisterAction`-Methode, die ein `Action`-Objekt beim Steuerelement registriert. Die registrierte Aktion wird über JavaScript aufgerufen, das in der HTML-Datei enthalten ist, auf die die `Uri`-Eigenschaft verweist.
- Eine `CleanUp`-Methode, die den Verweis auf das registrierte `Action`-Objekt entfernt.
- Eine `InvokeAction`-Methode, die das registrierte `Action`-Objekt aufruft. Diese Methode wird von einem benutzerdefinierten Renderer in jedem Plattformprojekt aufgerufen.

## <a name="consume-the-hybridwebview"></a>Verwenden des HybridWebView-Steuerelements

Sie können auf das benutzerdefinierte Steuerelement `HybridWebView` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das benutzerdefinierte Steuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `HybridWebView` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,40,0,0">
    <local:HybridWebView x:Name="hybridWebView"
                         Uri="index.html" />
</ContentPage>

```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf das benutzerdefinierte Steuerelement zu verweisen.

Im folgenden Codebeispiel wird veranschaulicht, wie das benutzerdefinierte Steuerelement `HybridWebView` von einer C#-Seite genutzt werden kann:

```csharp
public HybridWebViewPageCS()
{
    var hybridWebView = new HybridWebView
    {
        Uri = "index.html"
    };
    // ...
    Padding = new Thickness(0, 40, 0, 0);
    Content = hybridWebView;
}
```

Die `HybridWebView`-Instanz wird verwendet, um ein natives Websteuerelement auf jeder Plattform anzuzeigen. Deren `Uri`-Eigenschaft wird auf eine HTML-Datei festgelegt, die in jedem Plattformprojekt gespeichert und vom nativen Websteuerelement angezeigt wird. Die gerenderte HTML-Datei fordert den Benutzer auf, seinen Namen anzugeben. Dabei ruft eine JavaScript-Funktion ein C#-`Action`-Objekt auf, nachdem der Benutzer auf die HTML-Schaltfläche geklickt hat.

Das `HybridWebViewPage`-Objekt registriert die Aktion, die über JavaScript aufgerufen werden soll. Dies wird in folgendem Codebeispiel veranschaulicht:

```csharp
public partial class HybridWebViewPage : ContentPage
{
    public HybridWebViewPage()
    {
        // ...
        hybridWebView.RegisterAction(data => DisplayAlert("Alert", "Hello " + data, "OK"));
    }
}
```

Diese Aktion ruft die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))-Methode auf, um ein modale Popupelement anzuzeigen, das den Namen anzeigt, der auf der HTML-Seite eingegeben wurde, die von der `HybridWebView`-Instanz angezeigt wurde.

Ein benutzerdefinierter Renderer kann jetzt jedem Anwendungsprojekt hinzugefügt werden, um Plattformwebsteuerelemente zu erweitern, sodass diese zulassen, dass C#-Code über JavaScript aufgerufen wird.

## <a name="create-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `WkWebViewRenderer`-Klasse unter iOS bzw. der `WebViewRenderer`-Klasse unter Android und UWP, die das benutzerdefinierte Steuerelement rendert.
1. Überschreiben Sie die `OnElementChanged`-Methode, die das [`WebView`](xref:Xamarin.Forms.WebView)-Element rendert, und schreiben Sie Logik, um diese anzupassen. Diese Methode wird aufgerufen, wenn ein `HybridWebView`-Objekt erstellt wird.
1. Fügen Sie ein `ExportRenderer`-Attribut zur benutzerdefinierten Rendererklasse oder der Datei *AssemblyInfo.cs* hinzu, um festzulegen, dass sie zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet wird. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Bei den meisten Xamarin.Forms-Elementen ist das Angeben eines benutzerdefinierten Renderers in jedem Plattformprojekt optional. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet. Benutzerdefinierte Renderer sind jedoch in jedem Plattformprojekt erforderlich, wenn ein [View](xref:Xamarin.Forms.View)-Element gerendert wird.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](hybridwebview-images/solution-structure.png "HybridWebView Custom Renderer Project Responsibilities")

Das benutzerdefinierte Steuerelement `HybridWebView` wird von Plattformrendererklassen gerendert, die unter iOS von der `WkWebViewRenderer`-Klasse und unter Android und UWP von der `WebViewRenderer`-Klasse abgeleitet werden. Dadurch werden wie in den folgenden Screenshots gezeigt alle benutzerdefinierten `HybridWebView`-Steuerelemente mit nativen Websteuerelementen gerendert:

![](hybridwebview-images/screenshots.png "HybridWebView on each Platform")

Die Klassen `WkWebViewRenderer` und `WebViewRenderer` machen die Methode `OnElementChanged` verfügbar, die aufgerufen wird, wenn das benutzerdefinierte Xamarin.Forms-Steuerelement erstellt wird, um das entsprechende native Websteuerelement zu rendern. Diese Methode akzeptiert einen `VisualElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element dar, an das der Renderer angefügt *ist*. In der Beispielanwendung ist die `OldElement`-Eigenschaft `null`, und die `NewElement`-Eigenschaft enthält einen Verweis auf die `HybridWebView`-Instanz.

In allen Plattformrendererklassen dient eine überschriebene Version der `OnElementChanged`-Methode zur Anpassung nativer Websteuerelemente. Ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird, kann über die Eigenschaft `Element` abgerufen werden.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen des zu rendernden benutzerdefinierten Xamarin.Forms-Steuerelements und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird Folgendes behandelt: die Struktur der Webseite, die von jedem nativen Websteuerelement geladen wird, der Prozess zum Aufrufen von C# über JavaScript und die entsprechende Implementierung in jeder benutzerdefinierten Plattformrendererklasse.

### <a name="create-the-web-page"></a>Erstellen der Webseite

Das folgende Codebeispiel veranschaulicht die Webseite, die vom benutzerdefinierten Steuerelement `HybridWebView` angezeigt wird:

```html
<html>
<body>
    <script src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
    <h1>HybridWebView Test</h1>
    <br />
    Enter name: <input type="text" id="name">
    <br />
    <br />
    <button type="button" onclick="javascript: invokeCSCode($('#name').val());">Invoke C# Code</button>
    <br />
    <p id="result">Result:</p>
    <script type="text/javascript">function log(str) {
            $('#result').text($('#result').text() + " " + str);
        }

        function invokeCSCode(data) {
            try {
                log("Sending Data:" + data);
                invokeCSharpAction(data);
            }
            catch (err) {
                log(err);
            }
        }</script>
</body>
</html>
```

Auf der Webseite kann ein Benutzer seinen Namen in ein `input`-Element eingeben. Dort steht auch ein `button`-Element zur Verfügung, das C#-Code aufruft, wenn darauf geklickt wird. Der Prozess dafür sieht wie folgt aus:

- Wenn der Benutzer auf das `button`-Element klickt, wird die JavaScript-Funktion `invokeCSCode` aufgerufen, und der Wert des `input`-Elements wird an die Funktion übergeben.
- Die `invokeCSCode`-Funktion ruft die `log`-Funktion auf, um die Daten anzuzeigen, die sie an das C#-`Action`-Objekt sendet. Dann ruft sie die `invokeCSharpAction`-Methode auf, um das C#-`Action`-Objekt aufzurufen, und übergibt den Parameter, den sie vom `input`-Element erhalten hat.

Die JavaScript-Funktion `invokeCSharpAction` ist auf der Webseite nicht definiert, sondern wird vom jeweiligen benutzerdefinierten Renderer eingefügt.

Unter iOS befindet sich diese HTML-Datei mit einem **BundleResource**-Buildvorgang im Inhaltsordner des Plattformprojekts. Unter Android befindet sich diese HTML-Datei mit einem **AndroidAsset**-Buildvorgang im Assets-/Inhaltsordner des Plattformprojekts.

### <a name="invoke-c-from-javascript"></a>Aufrufen von C#-Code über JavaScript

Der Prozess zum Aufrufen von C#-Code über JavaScript ist für jede Plattform gleich:

- Der benutzerdefinierte Renderer erstellt ein natives Websteuerelement und lädt die HTML-Datei, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird.
- Sobald die Webseite geladen wurde, fügt der benutzerdefinierte Renderer die JavaScript-Funktion `invokeCSharpAction` auf der Webseite ein.
- Wenn der Benutzer seinen Namen angibt und auf das HTML-Element `button` klickt, wird die `invokeCSCode`-Funktion aufgerufen, die wiederum die `invokeCSharpAction`-Funktion aufruft.
- Die `invokeCSharpAction`-Funktion ruft eine Methode im benutzerdefinierten Renderer auf, die wiederum die `HybridWebView.InvokeAction`-Methode aufruft.
- Die `HybridWebView.InvokeAction`-Methode ruft das registrierte `Action`-Objekt auf.

In den folgenden Abschnitten wird besprochen, wir dieser Prozess auf der entsprechenden Plattform implementiert wird.

### <a name="create-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : WkWebViewRenderer, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        public HybridWebViewRenderer() : this(new WKWebViewConfiguration())
        {
        }

        public HybridWebViewRenderer(WKWebViewConfiguration config) : base(config)
        {
            userController = config.UserContentController;
            var script = new WKUserScript(new NSString(JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
            userController.AddUserScript(script);
            userController.AddScriptMessageHandler(this, "invokeAction");
        }

        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                userController.RemoveAllUserScripts();
                userController.RemoveScriptMessageHandler("invokeAction");
                HybridWebView hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }

            if (e.NewElement != null)
            {
                string filename = Path.Combine(NSBundle.MainBundle.BundlePath, $"Content/{((HybridWebView)Element).Uri}");
                LoadRequest(new NSUrlRequest(new NSUrl(filename, false)));
            }
        }

        public void DidReceiveScriptMessage(WKUserContentController userContentController, WKScriptMessage message)
        {
            ((HybridWebView)Element).InvokeAction(message.Body.ToString());
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

Die `HybridWebViewRenderer`-Klasse lädt die Webseite, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird, in ein natives [`WKWebView`](xref:WebKit.WKWebView)-Steuerelement, und die JavaScript-Funktion `invokeCSharpAction` wird auf der Webseite eingefügt. Sobald der Benutzer seinen Namen angibt und auf das HTML-`button`-Element klickt, wird die JavaScript-Funktion `invokeCSharpAction` ausgeführt und die `DidReceiveScriptMessage`-Methode wird aufgerufen, nachdem eine Meldung der Webseite empfangen wurde. Dann ruft diese Methode wiederum die `HybridWebView.InvokeAction`-Methode auf, die die registrierte Aktion aufruft, um das Popupelement anzuzeigen.

Diese Funktionalität erreichen sie folgendermaßen:

- Der Rendererkonstruktor erstellt ein `WkWebViewConfiguration`-Objekt und ruft dessen [`WKUserContentController`](xref:WebKit.WKUserContentController)-Objekt ab. Das `WkUserContentController`-Objekt ermöglicht das Posten von Nachrichten und das Injizieren von Benutzerskripts in eine Webseite.
- Der Rendererkonstruktor erstellt ein [`WKUserScript`](xref:WebKit.WKUserScript)-Objekt, das die JavaScript-Funktion `invokeCSharpAction` in die Webseite injiziert, nachdem die Webseite geladen wurde.
- Der Rendererkonstruktor ruft die [`WKUserContentController.AddUserScript`](xref:WebKit.WKUserContentController.AddUserScript(WebKit.WKUserScript))-Methode auf, um das [`WKUserScript`](xref:WebKit.WKUserScript)-Objekt zum Inhaltscontroller hinzuzufügen.
- Der Rendererkonstruktor ruft die [`WKUserContentController.AddScriptMessageHandler`](xref:WebKit.WKUserContentController.AddScriptMessageHandler(WebKit.IWKScriptMessageHandler,System.String))-Methode auf, um einen Skriptmeldungshandler namens `invokeAction` zum [`WKUserContentController`](xref:WebKit.WKUserContentController)-Objekt hinzuzufügen, wodurch die JavaScript-Funktion `window.webkit.messageHandlers.invokeAction.postMessage(data)` in allen `WebView`-Instanzen in allen Frames definiert wird, die das `WKUserContentController`-Objekt verwenden.
- Wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, werden die folgenden Vorgänge durchgeführt:
  - Die [`WKWebView.LoadRequest`](xref:WebKit.WKWebView.LoadRequest(Foundation.NSUrlRequest))-Methode lädt die HTML-Datei, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird. Der Code gibt an, dass die Datei im `Content`-Ordner des Projekts gespeichert wird. Wenn die Webseite angezeigt wird, wird die JavaScript-Funktion `invokeCSharpAction` auf dieser eingefügt.
- Ressourcen werden freigegeben, wenn das Element geändert wird, an das der Renderer angefügt ist.
- Das Xamarin.Forms-Element wird bereinigt, wenn der Renderer entfernt wird.

> [!NOTE]
> Die `WKWebView`-Klasse wird nur unter iOS 8 oder höher unterstützt.

Darüber hinaus muss **Info.plist** aktualisiert werden, damit folgende Werte enthalten sind:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

### <a name="create-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : WebViewRenderer
    {
        const string JavascriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<WebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                ((HybridWebView)Element).Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.SetWebViewClient(new JavascriptWebViewClient(this, $"javascript: {JavascriptFunction}"));
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl($"file:///android_asset/Content/{((HybridWebView)Element).Uri}");
            }
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

Die `HybridWebViewRenderer`-Klasse lädt die Webseite, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird, in ein natives [`WebView`](xref:Android.Webkit.WebView)-Steuerelement, und die JavaScript-Funktion `invokeCSharpAction` wird auf der Webseite mit der `OnPageFinished`-Überschreibung in der `JavascriptWebViewClient`-Klasse eingefügt, nachdem die Webseite vollständig geladen wurde:

```csharp
public class JavascriptWebViewClient : FormsWebViewClient
{
    string _javascript;

    public JavascriptWebViewClient(HybridWebViewRenderer renderer, string javascript) : base(renderer)
    {
        _javascript = javascript;
    }

    public override void OnPageFinished(WebView view, string url)
    {
        base.OnPageFinished(view, url);
        view.EvaluateJavascript(_javascript, null);
    }
}
```

Wenn der Benutzer seinen Namen eingibt und auf das HTML-`button`-Element klickt, wird die JavaScript-Funktion `invokeCSharpAction` ausgeführt. Diese Funktionalität erreichen sie folgendermaßen:

- Wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, werden die folgenden Vorgänge durchgeführt:
  - Die `SetWebViewClient`-Methode legt ein neues `JavascriptWebViewClient`-Objekt als Implementierung von `WebViewClient` fest.
  - Die [`WebView.AddJavascriptInterface`](xref:Android.Webkit.WebView.AddJavascriptInterface*)-Methode fügt eine neue `JSBridge`-Instanz im Hauptframe des JavaScript-Kontexts des WebView-Objekts hinzu und nennt diese `jsBridge`. So kann auf Methoden in der `JSBridge`-Klasse über JavaScript zugegriffen werden.
  - Die [`WebView.LoadUrl`](xref:Android.Webkit.WebView.LoadUrl*)-Methode lädt die HTML-Datei, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird. Der Code gibt an, dass die Datei im `Content`-Ordner des Projekts gespeichert wird.
  - In der `JavascriptWebViewClient`-Klasse wird die JavaScript-Funktion `invokeCSharpAction` auf der Webseite eingefügt, sobald die Seite vollständig geladen wurde.
- Ressourcen werden freigegeben, wenn das Element geändert wird, an das der Renderer angefügt ist.
- Das Xamarin.Forms-Element wird bereinigt, wenn der Renderer entfernt wird.

Wenn die JavaScript-Funktion `invokeCSharpAction` ausgeführt wird, ruft diese wiederum die `JSBridge.InvokeAction`-Methode auf. Dies wird in folgendem Codebeispiel veranschaulicht:

```csharp
public class JSBridge : Java.Lang.Object
{
    readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

    public JSBridge(HybridWebViewRenderer hybridRenderer)
    {
        hybridWebViewRenderer = new WeakReference<HybridWebViewRenderer>(hybridRenderer);
    }

    [JavascriptInterface]
    [Export("invokeAction")]
    public void InvokeAction(string data)
    {
        HybridWebViewRenderer hybridRenderer;

        if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget(out hybridRenderer))
        {
            ((HybridWebView)hybridRenderer.Element).InvokeAction(data);
        }
    }
}
```

Die Klasse muss von der `Java.Lang.Object`-Basisklasse abgeleitet werden, und die Methoden, die für JavaScript verfügbar gemacht werden, müssen mit den Attributen `[JavascriptInterface]` und `[Export]` versehen werden. Deshalb wird die `JSBridge.InvokeAction`-Methode aufgerufen, wenn die JavaScript-Funktion `invokeCSharpAction` auf der Webseite eingefügt und ausgeführt wird, da diese mit den Attributen `[JavascriptInterface]` und `[Export("invokeAction")]` versehen ist. Dann ruft diese `InvokeAction`-Methode wiederum die `HybridWebView.InvokeAction`-Methode auf, die die registrierte Aktion aufruft, um das Popupelement anzuzeigen.

> [!IMPORTANT]
> Android-Projekte, die das `[Export]`-Attribut verwenden, müssen einen Verweis auf `Mono.Android.Export` enthalten. Ansonsten kommt es zu einem Compilerfehler.

Beachten Sie, dass die `JSBridge`-Klasse ein `WeakReference`-Objekt für die `HybridWebViewRenderer`-Klasse enthält. Dadurch wird vermieden, dass es zu einem Zirkelbezug zwischen zwei Klassen kommt. Weitere Informationen finden Sie unter [Schwache Verweise](/en-us/dotnet/standard/garbage-collection/weak-references).

### <a name="create-the-custom-renderer-on-uwp"></a>Erstellen des benutzerdefinierten Renderers unter UWP

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die UWP veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : WebViewRenderer
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.WebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri($"ms-appx-web:///Content//{((HybridWebView)Element).Uri}");
            }
        }

        async void OnWebViewNavigationCompleted(Windows.UI.Xaml.Controls.WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            ((HybridWebView)Element).InvokeAction(e.Value);
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

Die `HybridWebViewRenderer`-Klasse lädt die Webseite, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird, in ein natives `WebView`-Steuerelement, und die JavaScript-Funktion `invokeCSharpAction` wird auf der Webseite mit der `WebView.InvokeScriptAsync`-Methode eingefügt, nachdem die Webseite vollständig geladen wurde. Sobald der Benutzer seinen Namen angibt und auf das HTML-`button`-Element klickt, wird die JavaScript-Funktion `invokeCSharpAction` ausgeführt und die `OnWebViewScriptNotify`-Methode wird aufgerufen, nachdem eine Benachrichtigung der Webseite empfangen wurde. Dann ruft diese Methode wiederum die `HybridWebView.InvokeAction`-Methode auf, die die registrierte Aktion aufruft, um das Popupelement anzuzeigen.

Diese Funktionalität erreichen sie folgendermaßen:

- Wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, werden die folgenden Vorgänge durchgeführt:
  - Ereignishandler für die Ereignisse `NavigationCompleted` und `ScriptNotify` werden registriert. Das Ereignis `NavigationCompleted` wird ausgelöst, wenn das native `WebView`-Steuerelement den aktuellen Inhalt vollständig geladen hat oder wenn es beim Navigieren einen Fehler gab. Das Ereignis `ScriptNotify` wird ausgelöst, wenn der Inhalt des nativen `WebView`-Steuerelements JavaScript verwendet, um eine Zeichenfolge an die Anwendung zu übergeben. Die Webseite löst das Ereignis `ScriptNotify` aus, indem sie `window.external.notify` aufruft, während sie einen `string`-Parameter übergibt.
  - Die `WebView.Source`-Eigenschaft wird auf den URI der HTML-Datei festgelegt, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird. Der Code geht davon aus, dass die Datei im `Content`-Ordner des Projekts gespeichert wird. Wenn die Webseite angezeigt wird, wird das Ereignis `NavigationCompleted` ausgelöst und die `OnWebViewNavigationCompleted`-Methode aufgerufen. Die JavaScript-Funktion `invokeCSharpAction` wird dann mit der `WebView.InvokeScriptAsync`-Methode auf der Webseite eingefügt, wenn die Navigation erfolgreich abgeschlossen wurde.
- Das Abonnement des Ereignisses wird nur gekündigt, wenn das Element geändert wird, an das der Renderer angefügt ist.
- Das Xamarin.Forms-Element wird bereinigt, wenn der Renderer entfernt wird.

## <a name="related-links"></a>Verwandte Links

- [HybridWebView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-hybridwebview)
