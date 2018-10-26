---
title: Implementieren einer HybridWebView
description: In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes HybridWebView-Steuerelement, das zeigt, wie zur Verbesserung der plattformspezifischen Websteuerelemente um C#-Code aufgerufen werden von JavaScript zu ermöglichen.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/19/2018
ms.openlocfilehash: aa060bd16bc0220f6a6026106ff6c8d786daebc1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105037"
---
# <a name="implementing-a-hybridwebview"></a>Implementieren einer HybridWebView

_Xamarin.Forms benutzerdefinierte Steuerelemente der Benutzeroberfläche sollte von der Ansichtsklasse abgeleitet werden, die verwendet wird, Layouts und Steuerelemente auf dem Bildschirm platziert. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes HybridWebView-Steuerelement, das zeigt, wie zur Verbesserung der plattformspezifischen Websteuerelemente um C#-Code aufgerufen werden von JavaScript zu ermöglichen._

Alle Xamarin.Forms-Ansicht ist eine begleitende-Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `View` ](xref:Xamarin.Forms.View) einer Xamarin.Forms-Anwendung unter iOS gerendert wird die `ViewRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `UIView` Steuerelement. Auf der Android-Plattform die `ViewRenderer` Klasse instanziiert ein `View` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `ViewRenderer` Klasse instanziiert ein systemeigenes `FrameworkElement` Steuerelement. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `View` ](xref:Xamarin.Forms.View) und die entsprechenden systemeigenen Steuerelemente, die sie implementieren:

![](hybridwebview-images/view-classes.png "Beziehung zwischen der Ansichtsklasse und seine Implementierung systemeigenen Klassen")

Das Rendern zu kann verwendet werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `View` ](xref:Xamarin.Forms.View) auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_HybridWebView) der `HybridWebView` benutzerdefiniertes Steuerelement.
1. [Nutzen](#Consuming_the_HybridWebView) der `HybridWebView`von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) des benutzerdefinierten Renderers für das `HybridWebView` auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer `HybridWebView` Renderer an, der verbessert die plattformspezifische Websteuerelemente um C#-Code aufgerufen werden von JavaScript zu ermöglichen. Die `HybridWebView` Instanz wird verwendet, um eine HTML-Seite angezeigt, das den Benutzer auf ihren Namen eingeben. Klicken Sie dann klickt der Benutzer eine HTML-Schaltfläche, JavaScript Aufrufen einer Funktion wird eine C#- `Action` , die ein Popup mit dem Benutzernamen anzeigt.

Weitere Informationen über den Prozess für aufrufende C#-Code in JavaScript finden Sie unter [Aufrufen von c# aus JavaScript](#Invoking_C_from_JavaScript). Weitere Informationen zu der HTML-Seite, finden Sie unter [erstellen die Webseite](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Erstellen die HybridWebView

Die `HybridWebView` benutzerdefiniertes Steuerelement erstellt werden kann, indem Unterklassen der [ `View` ](xref:Xamarin.Forms.View) Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die `HybridWebView` benutzerdefiniertes Steuerelement wird in .NET Standard Library-Projekt erstellt und definiert die folgende API für das Steuerelement:

- Ein `Uri` Eigenschaft, der angibt, die Adresse der Website geladen werden.
- Ein `RegisterAction` -Methode, die registriert eine `Action` mit dem Steuerelement. Wird die registrierte Aktion aufgerufen werden, aus JavaScript enthalten, die in der HTML-Datei über die `Uri` Eigenschaft.
- Ein `CleanUp` Methode, die den Verweis auf den registrierten entfernt `Action`.
- Ein `InvokeAction` -Methode, die den registrierten ruft `Action`. Diese Methode wird ein benutzerdefinierter Renderer in jedem Projekt plattformspezifischen aufgerufen werden.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Nutzen die HybridWebView

Die `HybridWebView` benutzerdefiniertes Steuerelement kann verwiesen werden in XAML in .NET Standard Library-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix des benutzerdefinierten Steuerelements. Das folgende Codebeispiel zeigt die `HybridWebView` benutzerdefiniertes Steuerelement kann von einer XAML-Seite verwendet werden:

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

Die `local` Namespacepräfix kann eine beliebige Bezeichnung. Allerdings die `clr-namespace` und `assembly` -Werte müssen die Details des benutzerdefinierten Steuerelements entsprechen. Sobald der Namespace deklariert wird, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

Das folgende Codebeispiel zeigt die `HybridWebView` benutzerdefiniertes Steuerelement durch einen C# -Code genutzt werden kann:

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

Die `HybridWebView` Instanz wird verwendet, um ein native Websteuerelement auf jeder Plattform anzuzeigen. Sie verfügt über `Uri` -Eigenschaftensatz auf einer HTML-Datei, in den einzelnen plattformspezifischen Projekten gespeichert, und die durch das native Steuerelement angezeigt. Der gerenderte HTML-Code fordert den Benutzer auf ihren Namen eingeben, mit einer JavaScript-Funktion aufrufen einer C#- `Action` als Reaktion auf einen Klick auf eine HTML-Schaltfläche.

Die `HybridWebViewPage` registriert die Aktion, die aus JavaScript aufgerufen werden, wie im folgenden Codebeispiel gezeigt:

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

Diese Aktion ruft die [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) Methode, um ein modales Popup anzuzeigen, der den Namen eingegeben haben, in die HTML-Seite angezeigt, enthält die `HybridWebView` Instanz.

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt plattformspezifische Websteuerelementen zu verbessern, indem C#-Code aus JavaScript aufgerufen werden können hinzugefügt werden.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `ViewRenderer<T1,T2>` -Klasse, die das benutzerdefinierte Steuerelement rendert. Das erste Argument des Typs muss das benutzerdefinierte Steuerelement, in diesem Fall der Renderer ist `HybridWebView`. Das zweite Typargument muss das native Steuerelement, das die benutzerdefinierte Ansicht implementiert wird.
1. Überschreiben der `OnElementChanged` -Methode, die benutzerdefinierte Steuerelement und Schreiben-Logik, um es anzupassen rendert. Diese Methode wird aufgerufen, wenn die entsprechenden benutzerdefinierten Xamarin.Forms-Steuerelements erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms-Elemente ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse des Steuerelements verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in dem jedes plattformprojekt beim Rendern einer [Ansicht](xref:Xamarin.Forms.View) Element.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](hybridwebview-images/solution-structure.png "Zuständigkeiten von HybridWebView benutzerdefinierten Renderer-Projekt")

Die `HybridWebView` benutzerdefiniertes Steuerelement gerendert wird, durch das plattformspezifische, alle abgeleiteten Renderingklassen der `ViewRenderer` -Klasse für jede Plattform. Dies führt in jeder `HybridWebView` benutzerdefiniertes Steuerelement mit plattformspezifischen Websteuerelemente gerendert wird, wie in den folgenden Screenshots gezeigt:

![](hybridwebview-images/screenshots.png "HybridWebView auf jeder Plattform")

Die `ViewRenderer` -Klasse macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das benutzerdefinierte Steuerelement mit Xamarin.Forms erstellt wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert eine `ElementChangedEventArgs` Parameter mit `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, die den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, die den Renderer *ist* angefügt wird, bzw. In diesem Beispiel die `OldElement` Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `HybridWebView` Instanz.

Eine außer Kraft gesetzte Version von der `OnElementChanged` -Methode, in den einzelnen plattformspezifischen rendererklassen an, ist der Ort für die Instanziierung der systemeigenen Web-Steuerelement und die Anpassung auszuführen. Die `SetNativeControl` Methode sollte verwendet werden, um das native Steuerelement zu instanziieren und diese Methode weist auch steuerelementreferenz der `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

In einigen Fällen die `OnElementChanged` -Methode mehrere Male aufgerufen werden kann. Aus diesem Grund, um Speicherverluste zu verhindern, Sorgfalt bei der Instanziierung eines neuen nativen Steuerelements. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

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

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control` `null` lautet. Ein Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Dieser Ansatz hilft, einen leistungsfähigen benutzerdefinierten Renderer erstellen, der durch Speicherverluste beeinträchtigt nicht.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: den Typnamen des benutzerdefinierten Xamarin.Forms-Steuerelements gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Den folgenden Abschnitten werden die Struktur der Webseite, die von jeder systemeigene Websteuerelement, den Prozess zum aufrufenden c# aus JavaScript und der Implementierung in jeder benutzerdefinierten Renderer plattformspezifische Klasse geladen werden.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Erstellen die Webseite

Das folgende Codebeispiel zeigt die Webseite, die angezeigt wird, die `HybridWebView` benutzerdefiniertes Steuerelement:

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

Die Webseite ermöglicht einem Benutzer, deren Namen eingeben einer `input` -Element, und bietet eine `button` -Element, das C#-Code beim Klicken auf aufgerufen wird. Der Prozess hierfür lautet wie folgt aus:

- Wenn der Benutzer klickt auf die `button` -Element, die `invokeCSCode` JavaScript-Funktion aufgerufen wird, mit dem Wert von der `input` Element an die Funktion übergeben wird.
- Die `invokeCSCode` Funktionsaufrufe der `log` Funktion zum Anzeigen der Daten, es an der C# sendet- `Action`. Es ruft dann die `invokeCSharpAction` aufzurufende Methode der C#- `Action`, die erhaltene Parameter übergeben wird, die `input` Element.

Die `invokeCSharpAction` JavaScript-Funktion ist nicht auf der Webseite definiert und wird in diesen jedes benutzerdefinierten Renderers eingefügt werden.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Aufrufen von c# aus JavaScript

Der Prozess zum aufrufenden c# von JavaScript ist auf jeder Plattform identisch:

- Der benutzerdefinierte Renderer erstellt eine systemeigene Websteuerelement und lädt die HTML-Datei, die gemäß der `HybridWebView.Uri` Eigenschaft.
- Sobald die Webseite geladen wurde, der benutzerdefinierte Renderer fügt die `invokeCSharpAction` JavaScript-Funktion in die Webseite.
- Wenn der Benutzer klickt auf den HTML-Code und gibt seinen Namen wird `button` -Element, das `invokeCSCode` Funktion wird aufgerufen, die wiederum ruft die `invokeCSharpAction` Funktion.
- Die `invokeCSharpAction` -Funktion aufruft, eine Methode in der benutzerdefinierte Renderer, der wiederum ruft die `HybridWebView.InvokeAction` Methode.
- Die `HybridWebView.InvokeAction` Methode ruft den registrierten `Action`.

In den folgenden Abschnitten werden erläutert, wie dieser Prozess auf jeder Plattform implementiert wird.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die iOS-Plattform:

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

Die `HybridWebViewRenderer` -Klasse lädt die Webseite, die im angegebenen die `HybridWebView.Uri` Eigenschaft in ein systemeigenes [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) -Steuerelement, und die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt. Sobald der Benutzer seinen Namen gibt und klickt auf den HTML-Code `button` -Element, das `invokeCSharpAction` JavaScript-Funktion ausgeführt wird, mit der `DidReceiveScriptMessage` Methode aufgerufen wird, nachdem eine Nachricht von der Webseite empfangen wird. Diese Methode wiederum ruft die `HybridWebView.InvokeAction` -Methode, die die registrierte Aktion, um das Popup anzuzeigen aufruft.

Diese Funktion wird wie folgt erreicht:

- Vorausgesetzt, dass die `Control` Eigenschaft `null`, werden die folgenden Vorgänge ausgeführt:
  - Ein [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) -Instanz erstellt wird, wodurch Einreihen von Nachrichten und Einfügen von Benutzerskripts, die in einer Webseite.
  - Ein [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) Instanz wird erstellt, um das Einfügen der `invokeCSharpAction` JavaScript-Funktion in die Webseite nach dem Laden der Webseite.
  - Die [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Methode fügt die [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) Instanz mit dem Content-Controller.
  - Die [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Methode fügt einen Skript-Message-Handler mit dem Namen `invokeAction` auf die [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) -Instanz, die die JavaScript-Funktion bewirkt `window.webkit.messageHandlers.invokeAction.postMessage(data)` in allen definiert werden Frames in allen Webansichten, die verwendet wird, werden die `WKUserContentController` Instanz.
  - Ein [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) Instanz erstellt, mit der [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) Instanz, die als Content Controller festgelegt wird.
  - Ein [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) -Steuerelement wird instanziiert, und die `SetNativeControl` Methode wird aufgerufen, um die Zuweisung von eines Verweis auf die `WKWebView` die Steuerung an die `Control` Eigenschaft.
- Vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird:
  - Die [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Methode lädt die HTML-Datei, die angegeben wird die `HybridWebView.Uri` Eigenschaft. Der Code gibt an, dass die Datei, in gespeichert wird der `Content` -Ordner des Projekts. Sobald die Webseite angezeigt wird, die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt werden.
- Wenn das Element der Renderer Änderungen zugeordnet ist:
  - Ressourcen werden freigegeben.

> [!NOTE]
> Die `WKWebView` Klasse wird nur in iOS 8 und höher unterstützt.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die Android-Plattform:

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

Die `HybridWebViewRenderer` -Klasse lädt die Webseite, die im angegebenen die `HybridWebView.Uri` Eigenschaft in ein systemeigenes [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) -Steuerelement, und die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt, nachdem die Webseite verfügt über das Laden beendet, mit der `OnPageFinished` außer Kraft setzen, der `JavascriptWebViewClient` Klasse:

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

Sobald der Benutzer seinen Namen gibt und klickt auf den HTML-Code `button` -Element, das `invokeCSharpAction` JavaScript-Funktion ausgeführt wird. Diese Funktion wird wie folgt erreicht:

- Vorausgesetzt, dass die `Control` Eigenschaft `null`, werden die folgenden Vorgänge ausgeführt:
  - Ein systemeigenes [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Instanz erstellt wurde, JavaScript, die in das Steuerelement aktiviert ist und ein `JavascriptWebViewClient` Instanz wird festgelegt, wie die Implementierung von `WebViewClient`.
  - Die `SetNativeControl` aufgerufen, um einen Verweis auf das systemeigene weisen [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) die Steuerung an die `Control` Eigenschaft.
- Vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird:
  - Die [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Methode fügt ein neues `JSBridge` Instanz in die Hauptframe der Webansicht-JavaScript-Kontexts, nennen Sie es `jsBridge`. Dadurch können Methoden in der `JSBridge` Klasse über JavaScript zugegriffen werden.
  - Die [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Methode lädt die HTML-Datei, die angegeben wird die `HybridWebView.Uri` Eigenschaft. Der Code gibt an, dass die Datei, in gespeichert wird der `Content` -Ordner des Projekts.
  - In der `JavascriptWebViewClient` -Klasse, die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt, sobald die Seite vollständig geladen wurde.
- Wenn das Element der Renderer Änderungen zugeordnet ist:
  - Ressourcen werden freigegeben.

Wenn die `invokeCSharpAction` JavaScript-Funktion ausgeführt wird, ruft er wiederum die `JSBridge.InvokeAction` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

Die Klasse muss von abgeleitet werden `Java.Lang.Object`, und Methoden, die in JavaScript verfügbar gemacht werden versehen werden müssen, mit der `[JavascriptInterface]` und `[Export]` Attribute. Aus diesem Grund, wenn die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt und ausgeführt wird, ruft der `JSBridge.InvokeAction` Methode aufgrund ergänzt wird die `[JavascriptInterface]` und `[Export("invokeAction")]` Attribute. Im Gegenzug die `InvokeAction` Methode ruft die `HybridWebView.InvokeAction` Methode wird aufgerufen, die registrierte Aktion aus, um das Popup anzuzeigen.

> [!NOTE]
> Projekte, die `[Export]` Attribut muss einen Verweis auf enthalten `Mono.Android.Export`, oder ein Compilerfehler führt.

Beachten Sie, dass die `JSBridge` -Klasse verwaltet eine `WeakReference` auf die `HybridWebViewRenderer` Klasse. Dadurch wird vermieden, erstellen einen Zirkelverweis zwischen den beiden Klassen. Weitere Informationen finden Sie unter [schwache Verweise](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) auf MSDN.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen benutzerdefinierten Renderer auf UWP

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für UWP:

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

Die `HybridWebViewRenderer` -Klasse lädt die Webseite, die im angegebenen die `HybridWebView.Uri` Eigenschaft in ein systemeigenes `WebView` -Steuerelement, und die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt, nachdem die Webseite geladen wurde, mit der `WebView.InvokeScriptAsync` Methode. Sobald der Benutzer seinen Namen gibt und klickt auf den HTML-Code `button` -Element, das `invokeCSharpAction` JavaScript-Funktion ausgeführt wird, mit der `OnWebViewScriptNotify` Methode aufgerufen wird, nachdem eine Benachrichtigung, von der Webseite empfangen wird. Diese Methode wiederum ruft die `HybridWebView.InvokeAction` -Methode, die die registrierte Aktion, um das Popup anzuzeigen aufruft.

Diese Funktion wird wie folgt erreicht:

- Vorausgesetzt, dass die `Control` Eigenschaft `null`, werden die folgenden Vorgänge ausgeführt:
  - Die `SetNativeControl` aufgerufen, um eine neue systemeigene instanziieren `WebView` steuern und weisen Sie einen Verweis, um die `Control` Eigenschaft.
- Vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird:
  - Ereignishandler für die `NavigationCompleted` und `ScriptNotify` Ereignisse registriert werden. Die `NavigationCompleted` Ereignis wird ausgelöst, wenn entweder das systemeigene `WebView` Steuerelement den aktuellen Inhalt Laden abgeschlossen ist oder wenn die Navigation fehlgeschlagen ist. Die `ScriptNotify` Ereignis wird ausgelöst, wenn der Inhalt in den systemeigenen `WebView` Steuerelement verwendet JavaScript zum Übergeben einer Zeichenfolge an die Anwendung. Der Webseite ausgelöst wird die `ScriptNotify` -Ereignis durch einen Aufruf `window.external.notify` beim Übergeben einer `string` Parameter.
  - Die `WebView.Source` -Eigenschaftensatz auf den URI der HTML-Datei, die angegeben wird die `HybridWebView.Uri` Eigenschaft. Der Code wird davon ausgegangen, dass die Datei, in gespeichert wird der `Content` -Ordner des Projekts. Sobald die Webseite angezeigt wird, die `NavigationCompleted` Ereignis wird ausgelöst, und die `OnWebViewNavigationCompleted` Methode wird aufgerufen. Die `invokeCSharpAction` JavaScript-Funktion wird dann in die Webseite mit injiziert den `WebView.InvokeScriptAsync` -Methode bereitgestellt, die die Navigation erfolgreich abgeschlossen wurde.
- Wenn das Element der Renderer Änderungen zugeordnet ist:
  - Ereignisse sind nicht mehr abonniert.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde beschrieben, wie zum Erstellen eines benutzerdefinierten Renderers für eine `HybridWebView` benutzerdefiniertes Steuerelement, das veranschaulicht, wie zur Verbesserung der plattformspezifischen Websteuerelemente um C#-Code aufgerufen werden von JavaScript zu ermöglichen.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererHybridWebView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [C# -Code aus JavaScript aufrufen.](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
