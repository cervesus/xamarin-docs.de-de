---
title: Webansicht
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: c1fcb16bd40b818b27b57b877534e051a789a6c9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029022"
---
# <a name="xamarinandroid-web-view"></a>Xamarin. Android-Webansicht

mit [`WebView`](xref:Android.Webkit.WebView) können Sie ein eigenes Fenster zum Anzeigen von Webseiten erstellen (oder sogar einen kompletten Browser entwickeln). In diesem Tutorial erstellen Sie eine einfache [`Activity`](xref:Android.App.Activity)
, die Webseiten anzeigen und navigieren können.

Erstellen Sie ein neues Projekt mit dem Namen **hellowebview**.

Öffnen Sie **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Da diese Anwendung auf das Internet zugreift, müssen Sie der Android-Manifest-Datei die entsprechenden Berechtigungen hinzufügen. Öffnen Sie die Eigenschaften des Projekts, um anzugeben, welche Berechtigungen Ihre Anwendung für den Betrieb benötigt. Aktivieren Sie die `INTERNET`-Berechtigung, wie unten dargestellt:

![Festlegen der Internet-Berechtigung im Android-Manifest](web-view-images/01-set-internet-permissions.png)

Öffnen Sie jetzt **MainActivity.cs** , und fügen Sie eine using-Direktive für webkit hinzu:

```csharp
using Android.Webkit;
```

Deklarieren Sie oben in der `MainActivity`-Klasse ein [`WebView`](xref:Android.Webkit.WebView) Objekt:

```csharp
WebView web_view;
```

Wenn die **WebView** aufgefordert wird, eine URL zu laden, wird die Anforderung standardmäßig an den Standardbrowser delegiert. Damit die **WebView** die URL lädt (anstelle des Standard Browsers), müssen Sie die Unterklasse `Android.Webkit.WebViewClient` und die `ShouldOverriderUrlLoading`-Methode überschreiben. Eine Instanz dieses benutzerdefinierten `WebViewClient` wird für die `WebView`bereitgestellt. Fügen Sie zu diesem Zweck die folgende geschachtelte `HelloWebViewClient`-Klasse in `MainActivity`hinzu:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Wenn `ShouldOverrideUrlLoading` `false`zurückgibt, signalisiert es Android, dass die aktuelle `WebView` Instanz die Anforderung verarbeitet hat und keine weitere Aktion erforderlich ist. 

Wenn Sie auf API-Ebene 24 oder höher abzielen, verwenden Sie die Überladung von `ShouldOverrideUrlLoading`, die eine `IWebResourceRequest` für das zweite Argument anstelle eines `string`annimmt:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

Verwenden Sie als nächstes den folgenden Code für die [`OnCreate()`](xref:Android.App.Activity.OnCreate*))-Methode:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Dadurch wird das Element [`WebView`](xref:Android.Webkit.WebView) mit dem Element aus dem [`Activity`](xref:Android.App.Activity) Layout initialisiert und JavaScript für die [`WebView`](xref:Android.Webkit.WebView) mit [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled)
`= true` aktiviert (Weitere Informationen finden Sie in der Anleitung zum [Abrufen von C\# von JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) für Informationen zum Abrufen von C-\# Funktionen aus JavaScript). Zum Schluss wird eine anfängliche Webseite mit [`LoadUrl(String)`](xref:Android.Webkit.WebView)geladen.

Erstellen Sie die App, und führen Sie sie aus. Es sollte eine einfache Web Page Viewer-App angezeigt werden, die im folgenden Screenshot zu sehen ist:

[![Beispiel für die APP, die eine WebView anzeigt](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Fügen Sie die folgende using-Anweisung hinzu, um die Tastenkombination " **zurück** " zu behandeln:

```csharp
using Android.Views;
```

Fügen Sie als nächstes die folgende Methode in die `HelloWebView`-Aktivität ein:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

Diese [`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
die Rückruf Methode wird immer dann aufgerufen, wenn eine Schaltfläche gedrückt wird, während die Aktivität ausgeführt wird. Die-Bedingung in verwendet die [`KeyEvent`](xref:Android.Views.KeyEvent) , um zu überprüfen, ob es sich bei dem gedrückten Schlüssel um die Schaltfläche " **zurück** " handelt und ob die [`WebView`](xref:Android.Webkit.WebView) tatsächlich zurück navigieren kann (wenn Sie über einen Verlauf verfügt) Wenn beide "true" sind, wird die [`GoBack()`](xref:Android.Webkit.WebView.GoBack) -Methode aufgerufen, die einen Schritt im [`WebView`](xref:Android.Webkit.WebView) Verlauf zurück navigiert. Die Rückgabe `true` gibt an, dass das Ereignis behandelt wurde. Wenn diese Bedingung nicht erfüllt wird, wird das Ereignis zurück an das System gesendet.

Führen Sie die Anwendung erneut aus. Sie sollten nun Links verwenden und wieder durch den Seiten Verlauf navigieren können:

[![Beispiel-Screenshots der Schaltfläche "zurück" in Aktion](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Call C# from JavaScript (Aufrufen von C#-Code über JavaScript)](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android. WebKit. WebView](xref:Android.Webkit.WebView)
- [KeyEvent](xref:Android.Webkit.WebView)
