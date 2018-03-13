---
title: Webansicht
ms.topic: article
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 0d418786a7364946e4e20100157fa0907b66deeb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="web-view"></a>Webansicht

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) können Sie eigene Fenster für die Anzeige von Webseiten zu erstellen (oder selbst entwickeln einen vollständige Browser). In diesem Lernprogramm erstellen Sie eine einfache [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) , die anzeigen und Webseiten navigieren können.

Erstellen Sie ein neues Projekt mit dem Namen **HelloWebView**.

Open **Resources/Layout/Main.axml** , und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Da diese Anwendung im Internet zugreifen, müssen Sie die erforderlichen Berechtigungen für die Android-Manifestdatei hinzufügen. Öffnen Sie die Projekteigenschaften, um anzugeben, welche Berechtigungen Ihre Anwendung erfordert, funktioniert. Aktivieren der `INTERNET` Berechtigung, wie unten dargestellt:

![Die INTERNET-Berechtigung festgelegt in der Android-Manifest.](web-view-images/01-set-internet-permissions.png)

Öffnen Sie jetzt **MainActivity.cs** und fügen Sie eine using-Direktive für den Webkit:

```csharp
using Android.Webkit;
```

Am oberen Rand der `MainActivity` Klasse, deklarieren Sie eine [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Objekt:

```csharp
WebView web_view;
```

Wenn die **WebView** wird aufgefordert, eine URL zu laden, es standardmäßig delegiert die Anforderung an den Standardbrowser. Damit die **WebView** Laden Sie die URL (anstatt den Standardbrowser), müssen Sie eine Unterklasse `Android.Webkit.WebViewClient` und überschreiben die `ShouldOverriderUrlLoading` Methode. Eine Instanz von diesem benutzerdefinierten `WebViewClient` wird bereitgestellt, um die `WebView`. Zu diesem Zweck fügen die folgende geschachtelte `HelloWebViewClient` in Klasse `MainActivity`:

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

Wenn `ShouldOverrideUrlLoading` gibt `false`, wird signalisiert, Android, die der aktuellen `WebView` Instanz verarbeitet die Anforderung und, die keine weiteren Aktionen erforderlich sind. 

Wenn Sie API-Ebene, 24 oder höher abzielen, verwenden Sie die Überladung der `ShouldOverrideUrlLoading` , akzeptiert eine `IWebResourceRequest` für das zweite Argument eine `string`:

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

Als Nächstes verwenden Sie den folgenden Code für die [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) Methode:

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

Dadurch wird das Element initialisiert [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) durch die Version auf dem [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) Layout und ermöglicht JavaScript für die [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) mit [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (finden Sie unter der [aufrufen C\# aus JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript) Rezept für Informationen zum Aufruf C\# JavaScript-Funktionen). Schließlich eine anfängliche Webseite geladen wird [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Erstellen Sie die App, und führen Sie sie aus. Eine einfache Webseite-Viewer-app sollte wie im folgenden Screenshot sehen angezeigt werden:

[![Beispiel der Anzeige von einem WebView-app](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Behandeln der **wieder** Schaltfläche Tastendruck, fügen Sie die folgenden Anweisung:

```csharp
using Android.Views;
```

Als Nächstes fügen Sie die folgende Methode innerhalb der `HelloWebView` Aktivität:

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

Dies [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) Rückrufmethode aufgerufen wird, wenn eine gedrückt, während die Aktivität ausgeführt wird. Die Bedingung innerhalb verwendet die [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) zu überprüfen, ob die Taste gedrückt werden die **wieder** Schaltfläche und gibt an, ob die [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) tatsächlich kann Navigieren zurück (Wenn sie einen Verlauf verfügt). Wenn beide "true", sind die [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) -Methode aufgerufen wird, navigiert die wieder ein Schritt in der [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Verlauf. Zurückgeben von `true` gibt an, dass das Ereignis behandelt wurde. Wenn diese Bedingung nicht erfüllt ist, wird das Ereignis zurück an das System gesendet.

Führen Sie die Anwendung erneut aus. Sie sollten jetzt möglich, folgen Links, und navigieren über die Seitenverlauf zurück:

[![Beispiel Screenshots der Schaltfläche "zurück", in Aktion](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Verwandte Links

- [C# aus JavaScript aufrufen](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
