---
title: Implementieren eines HybridWebView-Steuerelements
description: In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement HybridWebView erstellen können. Dadurch wird veranschaulicht, wie Sie plattformspezifische Websteuerelemente erweitern können, sodass diese zulassen, dass C#-Code über JavaScript aufgerufen werden kann.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/19/2018
ms.openlocfilehash: 997b3e8a8f847ae08eea7e022e7b3424d0fddd8d
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233938"
---
# <a name="implementing-a-hybridwebview"></a>Implementieren eines HybridWebView-Steuerelements

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)

_Benutzerdefinierte Xamarin.Forms-Benutzeroberflächen-Steuerelemente sollten von der View-Klasse abgeleitet werden, die verwendet wird, um Layout und Steuerelemente einer Anzeige hinzuzufügen. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes HybridWebView-Steuerelement erstellen können. Dadurch wird veranschaulicht, wie Sie plattformspezifische Websteuerelemente erweitern können, sodass diese zulassen, dass C#-Code über JavaScript aufgerufen werden kann._

Jede Xamarin.Forms-Ansicht verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`View`](xref:Xamarin.Forms.View)-Objekts durch eine Xamarin.Forms-Anwendung wird in iOS die `ViewRenderer`-Klasse instanziiert, wodurch wiederum ein natives `UIView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `ViewRenderer`-Klasse ein `View`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `ViewRenderer`-Klasse ein natives `FrameworkElement`-Steuerelement. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Renderer Base Classes and Native Controls (Rendererbasisklassen und native Steuerelemente)](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`View`](xref:Xamarin.Forms.View)-Objekt und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](hybridwebview-images/view-classes.png "Beziehungen zwischen der View-Klasse und den nativen Klassen, die diese implementieren")

Der Renderingprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für eine [`View`](xref:Xamarin.Forms.View)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Der Prozess dafür sieht wie folgt aus:

1. [Erstellen](#Creating_the_HybridWebView) Sie das benutzerdefinierte Steuerelement `HybridWebView`.
1. [Nutzen](#Consuming_the_HybridWebView) Sie das `HybridWebView`-Objekt von Xamarin.Forms.
1. [Erstellen](#Creating_the_Custom_Renderer_on_each_Platform) Sie einen benutzerdefinierten Renderer für das `HybridWebView`-Objekt auf jeder Plattform.

Im folgenden wird auf jeden dieser Punkte ausführlicher eingegangen, um zu veranschaulichen, wie Sie einen `HybridWebView`-Renderer implementieren können, der die plattformspezifischen Websteuerelemente erweitert, damit C#-Code über JavaScript aufgerufen werden kann. Die `HybridWebView`-Instanz wird verwendet, um eine HTML-Seite anzuzeigen, die den Benutzer auffordert, seinen Namen einzugeben. Wenn der Benutzer auf die HTML-Schaltfläche klickt, ruft eine JavaScript-Funktion ein C#-`Action`-Objekt auf, das den Benutzernamen enthält.

Weitere Informationen zum Aufrufen von C#-Code über JavaScript finden Sie im Abschnitt [Aufrufen von C#-Code über JavaScript](#Invoking_C_from_JavaScript). Weitere Informationen zu HTML-Seiten finden Sie im Abschnitt [Erstellen einer Webseite](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Erstellen eines HybridWebView-Steuerelements

Das benutzerdefinierte Steuerelement `HybridWebView` kann erstellt werden, indem Sie die [`View`](xref:Xamarin.Forms.View)-Klasse wie in folgendem Codebeispiel als Unterklasse verwenden:

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

Das benutzerdefinierte Steuerelement `HybridWebView` wird im .NET Standard-Bibliotheksprojekt erstellt und definiert die folgende API für das Steuerelement:

- Eine `Uri`-Eigenschaft, die die Adresse der zu ladenden Webseite angibt.
- Eine `RegisterAction`-Methode, die ein `Action`-Objekt beim Steuerelement registriert. Die registrierte Aktion wird über JavaScript aufgerufen, das in der HTML-Datei enthalten ist, auf die die `Uri`-Eigenschaft verweist.
- Eine `CleanUp`-Methode, die den Verweis auf das registrierte `Action`-Objekt entfernt.
- Eine `InvokeAction`-Methode, die das registrierte `Action`-Objekt aufruft. Diese Methode wird von einem benutzerdefinierten Renderer in jedem plattformspezifischen Projekt aufgerufen.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Nutzen eines HybridWebView-Steuerelements

Sie können auf das benutzerdefinierte Steuerelement `HybridWebView` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das benutzerdefinierte Steuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `HybridWebView` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf das benutzerdefinierte Steuerelement zu verweisen.

Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `HybridWebView` von einer C#-Seite genutzt werden kann:

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

Die `HybridWebView`-Instanz wird verwendet, um ein natives Websteuerelement auf jeder Plattform anzuzeigen. Deren `Uri`-Eigenschaft wird auf eine HTML-Datei festgelegt, die in jedem plattformspezifischen Projekt gespeichert ist und die vom nativen Websteuerelement angezeigt wird. Die gerenderte HTML-Datei fordert den Benutzer auf, seinen Namen anzugeben. Dabei ruft eine JavaScript-Funktion ein C#-`Action`-Objekt auf, nachdem der Benutzer auf die HTML-Schaltfläche geklickt hat.

Das `HybridWebViewPage`-Objekt registriert die Aktion, die über JavaScript aufgerufen werden soll. Dies wird in folgendem Codebeispiel veranschaulicht:

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

Diese Aktion ruft die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))-Methode auf, um ein modale Popupelement anzuzeigen, das den Namen anzeigt, der auf der HTML-Seite eingegeben wurde, die von der `HybridWebView`-Instanz angezeigt wurde.

Ein benutzerdefinierter Renderer kann jetzt jedem Anwendungsprojekt hinzugefügt werden, um plattformspezifische Websteuerelemente zu erweitern, sodass diese zulassen, dass C#-Code über JavaScript aufgerufen wird.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `ViewRenderer<T1,T2>`-Klasse, die das benutzerdefinierte Steuerelement rendert. Das erste Typargument sollte das benutzerdefinierte Steuerelement sein, für das der Renderer erstellt werden soll. In diesem Fall ist das `HybridWebView`. Das zweite Typargument sollte das native Steuerelement sein, das die benutzerdefinierte Ansicht implementiert.
1. Überschreiben Sie die `OnElementChanged`-Methode, die das benutzerdefinierte Steuerelement rendert, und schreiben Sie Logik, um dieses anzupassen. Die Methode wird aufgerufen, wenn das entsprechende benutzerdefinierte Xamarin.Forms-Steuerelement erstellt wird.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Bei den meisten Xamarin.Forms-Elementen ist das Angeben eines benutzerdefinierten Renderers in jedem Plattformprojekt optional. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet. Benutzerdefinierte Renderer sind jedoch in jedem Plattformprojekt erforderlich, wenn ein [View](xref:Xamarin.Forms.View)-Element gerendert wird.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](hybridwebview-images/solution-structure.png "Projektzuständigkeiten beim benutzerdefinierten Steuerelement HybridWebView")

Das benutzerdefinierte Steuerelement `HybridWebView` wird von plattformspezifischen Rendererklassen gerendert, die alle von der `ViewRenderer`-Klasse für jede Plattform abgeleitet werden. Das führt dazu, dass jedes benutzerdefinierte `HybridWebView`-Steuerelement mit plattformspezifischen Websteuerelementen gerendert wird. Dies wird in folgenden Screenshots veranschaulicht:

![](hybridwebview-images/screenshots.png "HybridWebView auf jeder Plattform")

Die `ViewRenderer`-Klasse macht die `OnElementChanged`-Methode verfügbar, die bei der Erstellung des benutzerdefinierten Xamarin.Forms-Steuerelements aufgerufen wird, um das entsprechende native Websteuerelement zu rendern. Diese Methode akzeptiert einen `ElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element dar, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element, an das der Renderer angefügt *ist*. In der Beispielanwendung ist die `OldElement`-Eigenschaft `null`, und die `NewElement`-Eigenschaft enthält einen Verweis auf die `HybridWebView`-Instanz.

Das native Websteuerelement sollte bei der überschriebenen Version der `OnElementChanged`-Methode in jeder plattformspezifischen Rendererklasse instanziiert und angepasst werden. Mit der `SetNativeControl`-Methode sollte das native Websteuerelement instanziiert und die Steuerelementreferenz der Eigenschaft `Control` zugewiesen werden. Zusätzlich kann ein Verweis auf das zu rendernde Xamarin.Forms-Steuerelement über die `Element`-Eigenschaft abgerufen werden.

In einigen Fällen kann die `OnElementChanged`-Methode mehrmals aufgerufen werden. Daher müssen Sie bei der Instanziierung eines nativen Steuerelements sorgfältig vorgehen, um Arbeitsspeicherverluste zu vermeiden. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control` `null` lautet. Ein Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Mit diesem Ansatz kann ein leistungsstarker benutzerdefinierter Renderer erstellt werden, der nicht durch Speicherverluste beeinträchtigt wird.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen des zu rendernden benutzerdefinierten Xamarin.Forms-Steuerelements und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird Folgendes besprochen: die Struktur der Webseite, die von jedem nativen Websteuerelement geladen wird, der Prozess zum Aufrufen von C# über JavaScript und die entsprechende Implementierung in jeder plattformspezifischen benutzerdefinierten Rendererklasse.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Erstellen der Webseite

Das folgende Codebeispiel veranschaulicht die Webseite, die vom benutzerdefinierten Steuerelement `HybridWebView` angezeigt wird:

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
          log(err);
    }
}
</script>
</body>
</html>
```

Auf der Webseite kann ein Benutzer seinen Namen in ein `input`-Element eingeben. Dort steht auch ein `button`-Element zur Verfügung, das C#-Code aufruft, wenn darauf geklickt wird. Der Prozess dafür sieht wie folgt aus:

- Wenn der Benutzer auf das `button`-Element klickt, wird die JavaScript-Funktion `invokeCSCode` aufgerufen, und der Wert des `input`-Elements wird an die Funktion übergeben.
- Die `invokeCSCode`-Funktion ruft die `log`-Funktion auf, um die Daten anzuzeigen, die sie an das C#-`Action`-Objekt sendet. Dann ruft sie die `invokeCSharpAction`-Methode auf, um das C#-`Action`-Objekt aufzurufen, und übergibt den Parameter, den sie vom `input`-Element erhalten hat.

Die JavaScript-Funktion `invokeCSharpAction` ist auf der Webseite nicht definiert, sondern wird vom jeweiligen benutzerdefinierten Renderer eingefügt.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Aufrufen von C#-Code über JavaScript

Der Prozess zum Aufrufen von C#-Code über JavaScript ist für jede Plattform gleich:

- Der benutzerdefinierte Renderer erstellt ein natives Websteuerelement und lädt die HTML-Datei, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird.
- Sobald die Webseite geladen wurde, fügt der benutzerdefinierte Renderer die JavaScript-Funktion `invokeCSharpAction` auf der Webseite ein.
- Wenn der Benutzer seinen Namen angibt und auf das HTML-Element `button` klickt, wird die `invokeCSCode`-Funktion aufgerufen, die wiederum die `invokeCSharpAction`-Funktion aufruft.
- Die `invokeCSharpAction`-Funktion ruft eine Methode im benutzerdefinierten Renderer auf, die wiederum die `HybridWebView.InvokeAction`-Methode aufruft.
- Die `HybridWebView.InvokeAction`-Methode ruft das registrierte `Action`-Objekt auf.

In den folgenden Abschnitten wird besprochen, wir dieser Prozess auf der entsprechenden Plattform implementiert wird.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

Die `HybridWebViewRenderer`-Klasse lädt die Webseite, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird, in ein natives [`WKWebView`](xref:WebKit.WKWebView)-Steuerelement, und die JavaScript-Funktion `invokeCSharpAction` wird auf der Webseite eingefügt. Sobald der Benutzer seinen Namen angibt und auf das HTML-`button`-Element klickt, wird die JavaScript-Funktion `invokeCSharpAction` ausgeführt und die `DidReceiveScriptMessage`-Methode wird aufgerufen, nachdem eine Meldung der Webseite empfangen wurde. Dann ruft diese Methode wiederum die `HybridWebView.InvokeAction`-Methode auf, die die registrierte Aktion aufruft, um das Popupelement anzuzeigen.

Diese Funktionalität erreichen sie folgendermaßen:

- Wenn die `Control`-Eigenschaft `null` ist, werden die folgenden Vorgänge durchgeführt:
  - Eine [`WKUserContentController`](xref:WebKit.WKUserContentController)-Instanz wird erstellt, durch die das Posten von Meldungen und Einfügen von Benutzerskripts auf der Webseite möglich ist.
  - Eine [`WKUserScript`](xref:WebKit.WKUserScript)-Instanz wird erstellt, um die JavaScript-Funktion `invokeCSharpAction` auf der Webseite einzufügen, nachdem die Seite geladen wurde.
  - Die [`WKUserContentController.AddUserScript`](xref:WebKit.WKUserContentController.AddUserScript(WebKit.WKUserScript))-Methode fügt dem ContentController-Objekt die [`WKUserScript`](xref:WebKit.WKUserScript)-Instanz hinzu.
  - Die [`WKUserContentController.AddScriptMessageHandler`](xref:WebKit.WKUserContentController.AddScriptMessageHandler(WebKit.IWKScriptMessageHandler,System.String))-Methode fügt der [`WKUserContentController`](xref:WebKit.WKUserContentController)-Instanz einen Skriptmeldungshandler mit dem Namen `invokeAction` hinzu, wodurch die JavaScript-Funktion `window.webkit.messageHandlers.invokeAction.postMessage(data)` in allen Frames in allen Webansichten definiert wird, die die `WKUserContentController`-Instanz verwenden.
  - Eine [`WKWebViewConfiguration`](xref:WebKit.WKWebViewConfiguration)-Instanz wird erstellt, und die [`WKUserContentController`](xref:WebKit.WKUserContentController)-Instanz wird als ContentController festgelegt.
  - Ein [`WKWebView`](xref:WebKit.WKWebView)-Steuerelement wird instanziiert, und die `SetNativeControl`-Methode wird aufgerufen, um der `Control`-Eigenschaft einen Verweis auf das `WKWebView`-Steuerelement hinzuzufügen.
- Wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, werden die folgenden Vorgänge durchgeführt:
  - Die [`WKWebView.LoadRequest`](xref:WebKit.WKWebView.LoadRequest(Foundation.NSUrlRequest))-Methode lädt die HTML-Datei, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird. Der Code gibt an, dass die Datei im `Content`-Ordner des Projekts gespeichert wird. Wenn die Webseite angezeigt wird, wird die JavaScript-Funktion `invokeCSharpAction` auf dieser eingefügt.
- Wenn sich das Element ändert, an das der Renderer angefügt ist, geschieht Folgendes:
  - Ressourcen werden freigegeben.

> [!NOTE]
> Die `WKWebView`-Klasse wird nur unter iOS 8 oder höher unterstützt.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavascriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                webView.SetWebViewClient(new JavascriptWebViewClient($"javascript: {JavascriptFunction}"));
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl($"file:///android_asset/Content/{Element.Uri}");
            }
        }
    }
}
```

Die `HybridWebViewRenderer`-Klasse lädt die Webseite, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird, in ein natives [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)-Steuerelement, und die JavaScript-Funktion `invokeCSharpAction` wird auf der Webseite mit der `OnPageFinished`-Überschreibung in der `JavascriptWebViewClient`-Klasse eingefügt, nachdem die Webseite vollständig geladen wurde:

```csharp
public class JavascriptWebViewClient : WebViewClient
{
    string _javascript;

    public JavascriptWebViewClient(string javascript)
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

- Wenn die `Control`-Eigenschaft `null` ist, werden die folgenden Vorgänge durchgeführt:
  - Eine native [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)-Instanz wird erstellt, JavaScript wird auf dem Steuerelement aktiviert und eine `JavascriptWebViewClient`-Instanz wird als Implementierung von `WebViewClient` festgelegt.
  - Die `SetNativeControl`-Methode wird aufgerufen, um der `Control`-Eigenschaft einen Verweis auf das native [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)-Steuerelement zuzuweisen.
- Wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, werden die folgenden Vorgänge durchgeführt:
  - Die [`WebView.AddJavascriptInterface`](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/)-Methode fügt eine neue `JSBridge`-Instanz im Hauptframe des JavaScript-Kontexts des WebView-Objekts hinzu und nennt diese `jsBridge`. So kann auf Methoden in der `JSBridge`-Klasse über JavaScript zugegriffen werden.
  - Die [`WebView.LoadUrl`](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/)-Methode lädt die HTML-Datei, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird. Der Code gibt an, dass die Datei im `Content`-Ordner des Projekts gespeichert wird.
  - In der `JavascriptWebViewClient`-Klasse wird die JavaScript-Funktion `invokeCSharpAction` auf der Webseite eingefügt, sobald die Seite vollständig geladen wurde.
- Wenn sich das Element ändert, an das der Renderer angefügt ist, geschieht Folgendes:
  - Ressourcen werden freigegeben.

Wenn die JavaScript-Funktion `invokeCSharpAction` ausgeführt wird, ruft diese wiederum die `JSBridge.InvokeAction`-Methode auf. Dies wird in folgendem Codebeispiel veranschaulicht:

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer))
    {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

Die Klasse muss von der `Java.Lang.Object`-Basisklasse abgeleitet werden, und die Methoden, die für JavaScript verfügbar gemacht werden, müssen mit den Attributen `[JavascriptInterface]` und `[Export]` versehen werden. Deshalb wird die `JSBridge.InvokeAction`-Methode aufgerufen, wenn die JavaScript-Funktion `invokeCSharpAction` auf der Webseite eingefügt und ausgeführt wird, da diese mit den Attributen `[JavascriptInterface]` und `[Export("invokeAction")]` versehen ist. Dann ruft die `InvokeAction`-Methode wiederum die `HybridWebView.InvokeAction`-Methode auf, die die registrierte Aktion aufruft, um das Popupelement anzuzeigen.

> [!NOTE]
> Projekte, die das `[Export]`-Attribut verwenden, müssen einen Verweis auf `Mono.Android.Export` enthalten. Ansonsten kommt es zu einem Compilerfehler.

Beachten Sie, dass die `JSBridge`-Klasse ein `WeakReference`-Objekt für die `HybridWebViewRenderer`-Klasse enthält. Dadurch wird vermieden, dass es zu einem Zirkelbezug zwischen zwei Klassen kommt. Weitere Informationen finden Sie unter [Schwache Verweise](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) in der Microsoft-Dokumentation.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen des benutzerdefinierten Renderers auf der UWP

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die UWP veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

Die `HybridWebViewRenderer`-Klasse lädt die Webseite, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird, in ein natives `WebView`-Steuerelement, und die JavaScript-Funktion `invokeCSharpAction` wird auf der Webseite mit der `WebView.InvokeScriptAsync`-Methode eingefügt, nachdem die Webseite vollständig geladen wurde. Sobald der Benutzer seinen Namen angibt und auf das HTML-`button`-Element klickt, wird die JavaScript-Funktion `invokeCSharpAction` ausgeführt und die `OnWebViewScriptNotify`-Methode wird aufgerufen, nachdem eine Benachrichtigung der Webseite empfangen wurde. Dann ruft diese Methode wiederum die `HybridWebView.InvokeAction`-Methode auf, die die registrierte Aktion aufruft, um das Popupelement anzuzeigen.

Diese Funktionalität erreichen sie folgendermaßen:

- Wenn die `Control`-Eigenschaft `null` ist, werden die folgenden Vorgänge durchgeführt:
  - Die `SetNativeControl`-Methode wird aufgerufen, um ein neues natives `WebView`-Steuerelement zu instanziieren und der `Control`-Eigenschaft einen Verweis auf dieses Steuerelement zuzuweisen.
- Wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, werden die folgenden Vorgänge durchgeführt:
  - Ereignishandler für die Ereignisse `NavigationCompleted` und `ScriptNotify` werden registriert. Das Ereignis `NavigationCompleted` wird ausgelöst, wenn das native `WebView`-Steuerelement den aktuellen Inhalt vollständig geladen hat oder wenn es beim Navigieren einen Fehler gab. Das Ereignis `ScriptNotify` wird ausgelöst, wenn der Inhalt des nativen `WebView`-Steuerelements JavaScript verwendet, um eine Zeichenfolge an die Anwendung zu übergeben. Die Webseite löst das Ereignis `ScriptNotify` aus, indem sie `window.external.notify` aufruft, während sie einen `string`-Parameter übergibt.
  - Die `WebView.Source`-Eigenschaft wird auf den URI der HTML-Datei festgelegt, die von der `HybridWebView.Uri`-Eigenschaft angegeben wird. Der Code geht davon aus, dass die Datei im `Content`-Ordner des Projekts gespeichert wird. Wenn die Webseite angezeigt wird, wird das Ereignis `NavigationCompleted` ausgelöst und die `OnWebViewNavigationCompleted`-Methode aufgerufen. Die JavaScript-Funktion `invokeCSharpAction` wird dann mit der `WebView.InvokeScriptAsync`-Methode auf der Webseite eingefügt, wenn die Navigation erfolgreich abgeschlossen wurde.
- Wenn sich das Element ändert, an das der Renderer angefügt ist, geschieht Folgendes:
  - Ereignisabonnements werden aufgehoben.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes `HybridWebView`-Steuerelement erstellen können. Dadurch wurde veranschaulicht, wie Sie plattformspezifische Websteuerelemente erweitern können, sodass diese zulassen, dass C#-Code über JavaScript aufgerufen werden kann.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererHybridWebView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [Call C# from JavaScript (Aufrufen von C#-Code über JavaScript)](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
