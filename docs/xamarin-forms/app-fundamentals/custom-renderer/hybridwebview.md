---
title: Implementieren eine HybridWebView
description: "Xamarin.Forms benutzerdefinierte Benutzeroberflächen-Steuerelemente müssen vom View-Klasse abgeleitet werden, die verwendet wird, um Layouts und Steuerelemente auf dem Bildschirm zu platzieren. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes HybridWebView-Steuerelement, die veranschaulicht, wie der plattformspezifischen Websteuerelemente um C#-Code, der aufgerufen wird, werden von JavaScript zuzulassen, zu verbessern."
ms.topic: article
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: ef016d963f710ff54fc57b5e6e57181df030c8f6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="implementing-a-hybridwebview"></a>Implementieren eine HybridWebView

_Xamarin.Forms benutzerdefinierte Benutzeroberflächen-Steuerelemente müssen vom View-Klasse abgeleitet werden, die verwendet wird, um Layouts und Steuerelemente auf dem Bildschirm zu platzieren. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes HybridWebView-Steuerelement, die veranschaulicht, wie der plattformspezifischen Websteuerelemente um C#-Code, der aufgerufen wird, werden von JavaScript zuzulassen, zu verbessern._

Jede Ansicht Xamarin.Forms verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) einer Xamarin.Forms-Anwendung in iOS, gerendert wird die `ViewRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `UIView` Steuerelement. Auf der Android-Plattform die `ViewRenderer` Klasse instanziiert einen `View` Steuerelement. Auf Windows Phone und die universelle Windows-Plattform (UWP) die `ViewRenderer` Klasse instanziiert ein systemeigenes `FrameworkElement` Steuerelement. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) und das entsprechende systemeigene Steuerelemente, die ihn implementieren:

![](hybridwebview-images/view-classes.png "Beziehung zwischen der Ansichtsklasse und die implementierenden Native Klassen")

Des Renderingprozesses kann verwendet werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_HybridWebView) der `HybridWebView` benutzerdefiniertes Steuerelement.
1. [Nutzen](#Consuming_the_HybridWebView) die `HybridWebView`von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das `HybridWebView` auf jeder Plattform.

Jedes Element wird nun wiederum implementieren besprochen werden eine `HybridWebView` Renderer an, der der plattformspezifischen Websteuerelemente um C#-Code, der aufgerufen wird, werden von JavaScript zuzulassen, erhöht. Die `HybridWebView` Instanz wird verwendet, um eine HTML-Seite angezeigt, die den Benutzer auffordert, deren Namen eingeben. Rufen Sie dann, wenn der Benutzer eine HTML-Schaltfläche klickt, eine JavaScript-Funktion wird eine C#- `Action` , die ein Popupfenster mit dem Benutzernamen anzeigt.

Weitere Informationen über den Prozess aufrufen c# aus JavaScript finden Sie unter [aufrufen c# aus JavaScript](#Invoking_C_from_JavaScript). Weitere Informationen zu der HTML-Seite, finden Sie unter [erstellen auf der Webseite](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Erstellen die HybridWebView

Die `HybridWebView` Unterklasse benutzerdefiniertes Steuerelement erstellt werden die [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die `HybridWebView` benutzerdefiniertes Steuerelement wird im Projekt portablen Klassenbibliothek (PCL) erstellt und die folgende API für das Steuerelement definiert:

- Ein `Uri` Eigenschaft, die die Adresse der Webseite zu ladende angibt.
- Ein `RegisterAction` -Methode, die registriert eine `Action` mit dem Steuerelement. Die registrierte Aktion wird aufgerufen, aus JavaScript enthalten sind, in der HTML-Datei verwiesen wird, über die `Uri` Eigenschaft.
- Ein `CleanUp` -Methode, die den Verweis auf den registrierten entfernt `Action`.
- Ein `InvokeAction` -Methode, die den registrierten ruft `Action`. Diese Methode wird ein benutzerdefinierter Renderer in jedem Projekt plattformspezifischen aufgerufen werden.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Nutzen die HybridWebView

Die `HybridWebView` benutzerdefiniertes Steuerelement im verwiesen werden kann in XAML PCL-Projekt durch einen Namespace für den Speicherort der deklarieren und verwenden das Namespacepräfix für das benutzerdefinierte Steuerelement. Im folgenden Codebeispiel wird veranschaulicht wie die `HybridWebView` benutzerdefiniertes Steuerelement genutzt werden kann, durch die entsprechende Verwendung von XAML-Seite:

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

Die `local` nichts Namespacepräfix kann benannt werden. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details des benutzerdefinierten Steuerelements übereinstimmen. Nach der Namespace deklariert ist, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

Im folgenden Codebeispiel wird veranschaulicht wie die `HybridWebView` benutzerdefiniertes Steuerelement genutzt werden kann, um eine C#-Seite:

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

Die `HybridWebView` Instanz wird verwendet, um einen einheitlichen Websteuerelement auf jeder Plattform anzuzeigen. Es wurde `Uri` Eigenschaftensatz in eine HTML-Datei, die in jedem Projekt plattformspezifischen gespeichert ist, und die vom systemeigenen Websteuerelement angezeigt. Das gerenderte HTML fordert den Benutzer auf ihren Namen eingeben, mit einer JavaScript-Funktion aufrufen einer C#- `Action` als Antwort auf eine HTML-Schaltfläche klicken.

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

Diese Aktion ruft die [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Methode, um ein modales Popup anzuzeigen, in dem zeigt den Namen eingegeben haben, in die HTML-Seite angezeigt, die `HybridWebView` Instanz.

Ein benutzerdefinierter Renderer kann jetzt jeder Anwendung-Projekt, um plattformspezifische Websteuerelemente zu erhöhen, indem Sie C#-Code aus JavaScript aufgerufen werden ermöglicht hinzugefügt werden.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `ViewRenderer<T1,T2>` -Klasse, die das benutzerdefinierte Steuerelement gerendert wird. Erste Typargument muss das benutzerdefinierte Steuerelement-Renderer, in diesem Fall ist `HybridWebView`. Das zweite Typargument sollten das systemeigene Steuerelement sein, das die benutzerdefinierte Ansicht implementiert werden sollen.
1. Überschreiben Sie die `OnElementChanged` -Methode, die benutzerdefinierte Logik für Steuerelement und schreiben, um ihn anpassen rendert. Diese Methode wird aufgerufen, wenn das entsprechende benutzerdefinierte Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des benutzerdefinierten Steuerelements mit Xamarin.Forms verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> **Hinweis**: für die meisten Xamarin.Forms Elemente ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Basisklasse für das Steuerelement verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in jede plattformprojekt beim Rendern einer [Ansicht](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Element.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](hybridwebview-images/solution-structure.png "HybridWebView benutzerdefinierten Renderer Projekt Zuständigkeiten")

Die `HybridWebView` plattformspezifischen Renderingklassen, die alle abgeleitet benutzerdefiniertes Steuerelement gerendert wird die `ViewRenderer` Klasse für jede Plattform. Dies führt in den einzelnen `HybridWebView` benutzerdefiniertes Steuerelement mit plattformspezifischen-Websteuerelemente gerendert werden, wie in den folgenden Screenshots dargestellt:

![](hybridwebview-images/screenshots.png "HybridWebView auf jeder Plattform")

Die `ViewRenderer` -Klasse verfügbar macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das benutzerdefinierte Steuerelement mit Xamarin.Forms erstellt wird, um das entsprechende systemeigene Websteuerelement zu rendern. Diese Methode nimmt ein `ElementChangedEventArgs` Parameter, enthält `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, den Renderer *ist* angefügt sind, bzw. In der beispielanwendung der `OldElement` -Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `HybridWebView` Instanz.

Eine überschriebene Version von den `OnElementChanged` Methode in jeder Rendererklasse plattformspezifischen bietet die Möglichkeit zum vom systemeigenen Web Control Instanziierung sowie die Anpassung ausgeführt werden. Die `SetNativeControl` Methode sollte verwendet werden, um das systemeigene Websteuerelement instanziieren und diese Methode wird auch des Steuerelementverweis auf die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

Unter bestimmten Umständen die `OnElementChanged` Methode kann mehrfach aufgerufen werden. Daher muss zum Verhindern von Speicherverlusten geachtet werden bei der Instanziierung eines neuen native Steuerelements. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

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

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control` `null` lautet. Ein Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Dieser Ansatz hilft ein benutzerdefinierten Renderers für leistungsstarke erstellen, das von Arbeitsspeicherverlusten nicht beeinträchtigt werden.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: der Typname, der das benutzerdefinierte Xamarin.Forms-Steuerelement gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

Den folgenden Abschnitten werden die Struktur der Webseite, die von jeder systemeigene-Websteuerelement, der Prozess zum aufrufenden c# aus JavaScript und die Implementierung dieses in jeder benutzerdefinierten Renderers Clientplattform-spezifische Klasse geladen werden.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Erstellen auf der Webseite

Im folgenden Codebeispiel wird veranschaulicht, die Webseite, die angezeigt wird, die `HybridWebView` benutzerdefiniertes Steuerelement:

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

Die Webseite ermöglicht einem Benutzer, deren Namen eingeben einer `input` Element, und bietet eine `button` Element, das C#-Code beim aufgerufen wird. Der Prozess zum erreichen Sie dies lautet wie folgt:

- Wenn der Benutzer klickt auf die `button` Element, die `invokeCSCode` JavaScript-Funktion aufgerufen wird, mit dem Wert von der `input` Element an die Funktion übergeben wird.
- Die `invokeCSCode` Funktionsaufrufe, die `log` Funktion zum Anzeigen der Daten, es an die C# sendet- `Action`. Er ruft dann die `invokeCSharpAction` aufzurufende Methode die C#- `Action`, übergeben des Parameters erhaltene der `input` Element.

Die `invokeCSharpAction` JavaScript-Funktion ist nicht auf der Webseite definiert und wird von einzelnen benutzerdefinierten Renderers hinein injiziert.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Aufrufen von c# aus JavaScript

Der Prozess zum aufrufenden c# aus JavaScript ist auf den verschiedenen Plattformen identisch:

- Die benutzerdefinierte Renderer erstellt eine systemeigene Websteuerelement und lädt die HTML-Datei, die gemäß der `HybridWebView.Uri` Eigenschaft.
- Sobald die Webseite geladen wird, der benutzerdefinierte Renderer fügt die `invokeCSharpAction` JavaScript-Funktion in der Webseite.
- Wenn der Benutzer gibt seinen Namen, und klicken Sie auf den HTML-Code `button` Element, das `invokeCSCode` Funktion wird aufgerufen, die wiederum ruft die `invokeCSharpAction` Funktion.
- Die `invokeCSharpAction` Funktion aufgerufen wird, eine Methode in den benutzerdefinierten Renderer wiederum ruft die `HybridWebView.InvokeAction` Methode.
- Die `HybridWebView.InvokeAction` Methode ruft die registrierten `Action`.

In den folgenden Abschnitten werden erläutert, wie dieser Vorgang für jede Plattform implementiert wird.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

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

Die `HybridWebViewRenderer` Klasse lädt die Webseite, die im angegebenen der `HybridWebView.Uri` Eigenschaft in ein natives [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) -Steuerelement, und die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt. Um, sobald der Benutzer seinen Namen gibt und klickt auf den HTML-Code `button` Element, das `invokeCSharpAction` JavaScript-Funktion ausgeführt wird, mit der `DidReceiveScriptMessage` Methode wird aufgerufen, nachdem eine Nachricht, auf der Webseite empfangen wird. Diese Methode ruft seinerseits die `HybridWebView.InvokeAction` -Methode, die die registrierte Aktion aus, um das Popup anzuzeigen aufruft.

Diese Funktionalität wird folgendermaßen erreicht:

- Vorausgesetzt, dass die `Control` Eigenschaft `null`, werden die folgenden Vorgänge ausgeführt:
  - Ein [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) Instanz erstellt, wodurch Ausgeben von Meldungen und Räumen Benutzerskripts, die in eine Webseite.
  - Ein [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) Instanz erstellt, um das Einfügen der `invokeCSharpAction` JavaScript-Funktion in der Webseite nach dem Laden der Webseite.
  - Die [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Methode fügt die [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) Instanz mit dem Inhalt Controller.
  - Die [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Methode fügt einen Nachrichtenhandler ein Skript mit dem Namen `invokeAction` auf die [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) -Instanz, die JavaScript-Funktion bewirkt `window.webkit.messageHandlers.invokeAction.postMessage(data)` in allen definiert werden Frames in allen Webansichten, die nutzt die `WKUserContentController` Instanz.
  - Ein [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) Instanz erstellt, mit der [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) Instanz, der als Content Domänencontroller festgelegt.
  - Ein [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) Steuerelement instanziiert wird, und die `SetNativeControl` Methode wird aufgerufen, um einen Verweis auf weisen die `WKWebView` die Steuerung an die `Control` Eigenschaft.
- Vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:
  - Die [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Methode lädt die angegebenen HTML-Datei der `HybridWebView.Uri` Eigenschaft. Der Code gibt an, dass die Datei, in gespeichert ist der `Content` -Ordner des Projekts. Sobald die Webseite angezeigt wird, die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt werden.
- Wenn das Element den Renderer Änderungen zugeordnet ist:
  - Ressourcen werden freigegeben.

> [!NOTE]
> **Hinweis**: die `WKWebView` Klasse wird nur in iOS 8 und höher unterstützt.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Das folgende Codebeispiel zeigt die benutzerdefinierten Renderers für das Android-Plattform:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
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
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

Die `HybridWebViewRenderer` Klasse lädt die Webseite, die im angegebenen der `HybridWebView.Uri` Eigenschaft in ein natives [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) -Steuerelement, und die `invokeCSharpAction` JavaScript-Funktion ist in der Webseite eingefügt, nachdem die Webseite geladen wurde, mit der `InjectJS` Methode. Sobald der Benutzer seinen Namen gibt und klickt auf den HTML-Code `button` Element, das `invokeCSharpAction` JavaScript-Funktion ausgeführt wird. Diese Funktionalität wird folgendermaßen erreicht:

- Vorausgesetzt, dass die `Control` Eigenschaft `null`, werden die folgenden Vorgänge ausgeführt:
  - Ein systemeigenes [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Instanz erstellt wird, und JavaScript in das Steuerelement aktiviert ist.
  - Die `SetNativeControl` Methode wird aufgerufen, um einen Verweis auf das systemeigene zuweisen [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) die Steuerung an die `Control` Eigenschaft.
- Vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:
  - Die [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Methode fügt ein neues `JSBridge` Instanz die WebView-JavaScript-Kontext, benennen es in die Hauptframe `jsBridge`. Dadurch können Methoden in der `JSBridge` Klasse über JavaScript zugegriffen werden.
  - Die [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Methode lädt die angegebenen HTML-Datei der `HybridWebView.Uri` Eigenschaft. Der Code gibt an, dass die Datei, in gespeichert ist der `Content` -Ordner des Projekts.
  - Die `InjectJS` Methode wird aufgerufen, um das Einfügen der `invokeCSharpAction` JavaScript-Funktion in der Webseite.
- Wenn das Element den Renderer Änderungen zugeordnet ist:
  - Ressourcen werden freigegeben.

Wenn die `invokeCSharpAction` JavaScript-Funktion ausgeführt wird, ruft es die wiederum die `JSBridge.InvokeAction` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

Die Klasse ableiten muss `Java.Lang.Object`, und Methoden, die an JavaScript verfügbar gemacht werden müssen ergänzt werden, mit der `[JavascriptInterface]` und `[Export]` Attribute. Aus diesem Grund, wenn die `invokeCSharpAction` JavaScript-Funktion wird in die Webseite eingefügt und wird ausgeführt, er ruft die `JSBridge.InvokeAction` Methode aufgrund ergänzt wird, mit der `[JavascriptInterface]` und `[Export("invokeAction")]` Attribute. Wiederum die `InvokeAction` Methode ruft die `HybridWebView.InvokeAction` Methode wird aufgerufen, die registrierte Aktion aus, um das Popup anzuzeigen.

> [!NOTE]
> **Hinweis**: Projekten, in denen die `[Export]` Attribut umfasst einen Verweis auf `Mono.Android.Export`, oder ein Compilerfehler führt.

Beachten Sie, dass die `JSBridge` -Klasse verwaltet eine `WeakReference` auf die `HybridWebViewRenderer` Klasse. Dies ist zu vermeiden, erstellen einen Zirkelbezug zwischen den beiden Klassen. Weitere Informationen finden Sie unter [schwache Verweise](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) auf MSDN.

> [!IMPORTANT]
> Auf Android Oreo sicher, dass das Android-Manifest legt die **Ziel Android-Version** auf **automatische**. Andernfalls führt Ausführen dieses Codes zu dem Fehler Nachricht "InvokeCSharpAction ist nicht definiert".

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Erstellen von benutzerdefinierten Renderers für Windows Phone und universelle Windows-Plattform

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für Windows Phone-und uwp:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.WinPhone81
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

Die `HybridWebViewRenderer` Klasse lädt die Webseite, die im angegebenen der `HybridWebView.Uri` Eigenschaft in ein natives `WebView` -Steuerelement, und die `invokeCSharpAction` JavaScript-Funktion ist in der Webseite eingefügt, nachdem die Webseite geladen wurde, mit der `WebView.InvokeScriptAsync` Methode. Um, sobald der Benutzer seinen Namen gibt und klickt auf den HTML-Code `button` Element, das `invokeCSharpAction` JavaScript-Funktion ausgeführt wird, mit der `OnWebViewScriptNotify` Methode wird aufgerufen, nachdem eine Benachrichtigung, auf der Webseite empfangen wird. Diese Methode ruft seinerseits die `HybridWebView.InvokeAction` -Methode, die die registrierte Aktion aus, um das Popup anzuzeigen aufruft.

Diese Funktionalität wird folgendermaßen erreicht:

- Vorausgesetzt, dass die `Control` Eigenschaft `null`, werden die folgenden Vorgänge ausgeführt:
  - Die `SetNativeControl` Methode wird aufgerufen, um ein neues systemeigenes instanziieren `WebView` steuern und weisen Sie einen Verweis hinzu, um die `Control` Eigenschaft.
- Vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:
  - Ereignishandler für das `NavigationCompleted` und `ScriptNotify` Ereignisse registriert werden. Die `NavigationCompleted` Ereignis wird ausgelöst, wenn entweder das systemeigene `WebView` Steuerelement nach dem Laden des aktuellen Inhalts oder wenn die Navigation fehlgeschlagen ist. Die `ScriptNotify` Ereignis wird ausgelöst, wenn der Inhalt in der systemeigenen `WebView` Steuerelement nutzt JavaScript zur Festlegung eine Zeichenfolge an die Anwendung übergeben. Der Webseite ausgelöst der `ScriptNotify` Ereignis durch Aufrufen von `window.external.notify` beim Übergeben einer `string` Parameter.
  - Die `WebView.Source` Eigenschaftensatz an den URI der HTML-Datei, die von angegeben wird die `HybridWebView.Uri` Eigenschaft. Der Code wird davon ausgegangen, dass die Datei, in gespeichert ist der `Content` -Ordner des Projekts. Sobald die Webseite angezeigt wird, die `NavigationCompleted` Ereignis wird ausgelöst, und die `OnWebViewNavigationCompleted` Methode aufgerufen wird. Die `invokeCSharpAction` JavaScript-Funktion wird dann in die Webseite mit injiziert der `WebView.InvokeScriptAsync` -Methode bereitgestellt, die die Navigation erfolgreich abgeschlossen.
- Wenn das Element den Renderer Änderungen zugeordnet ist:
  - Ereignisse werden gekündigt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat zum Erstellen eines benutzerdefinierten Renderers für gezeigt eine `HybridWebView` benutzerdefiniertes Steuerelement, die veranschaulicht, wie der plattformspezifischen Websteuerelemente um C#-Code, der aufgerufen wird, werden von JavaScript zuzulassen, zu verbessern.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererHybridWebView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [C# aus JavaScript aufrufen](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
