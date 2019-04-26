---
title: Webansicht
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: ae0b67de5856e6baef9a4989a93e65ead2854a62
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61310631"
---
# <a name="web-view"></a>Webansicht

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) können Sie Ihre eigenen Fenster für die Anzeige von Webseiten erstellen (oder sogar einen vollständigen Browser entwickeln). In diesem Tutorial erstellen Sie eine einfache [`Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/)
die anzeigen, und Navigieren auf Webseiten.

Erstellen eines neuen Projekts mit dem Namen **HelloWebView**.

Open **Resources/Layout/Main.axml** und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Da diese Anwendung über das Internet zugreifen werden, müssen Sie hinzufügen, dass die erforderlichen Berechtigungen für die Android-Manifestdatei. Öffnen Sie die Eigenschaften des Projekts, um die Berechtigungen angeben, die Ihre Anwendung benötigt wird, ausgeführt werden. Aktivieren der `INTERNET` Berechtigung wie folgt:

![Festlegen der INTERNET-Berechtigung im Android-Manifest](web-view-images/01-set-internet-permissions.png)

Öffnen Sie nun **"mainactivity.cs"** und Hinzufügen eines über die Richtlinie für den Webkit:

```csharp
using Android.Webkit;
```

Am oberen Rand der `MainActivity` Klasse, deklarieren Sie eine [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Objekt:

```csharp
WebView web_view;
```

Wenn die **WebView** wird aufgefordert, eine URL zu laden, er wird standardmäßig delegieren die Anforderung an den Standardbrowser. Damit die **WebView** Laden Sie die URL (und nicht als Standardbrowser), müssen Sie eine Unterklasse `Android.Webkit.WebViewClient` und überschreiben die `ShouldOverriderUrlLoading` Methode. Eine Instanz von diesem benutzerdefinierten `WebViewClient` wird bereitgestellt, um die `WebView`. Zu diesem Zweck fügen die folgende geschachtelte `HelloWebViewClient` in Klasse `MainActivity`:

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

Wenn `ShouldOverrideUrlLoading` gibt `false`, signalisiert für Android, die die aktuelle `WebView` Instanz verarbeitet die Anforderung und, das ist keine weitere Aktion erforderlich. 

Wenn Sie mit der API-Ebene 24 oder höher Anzielen, verwenden Sie die Überladung von `ShouldOverrideUrlLoading` , akzeptiert eine `IWebResourceRequest` für das zweite Argument anstelle von einer `string`:

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

Als Nächstes verwenden Sie den folgenden Code für die [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
Methode:

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

Dadurch wird das Element initialisiert [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) durch den aus der [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) Layout und ermöglicht JavaScript für die [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) mit [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (finden Sie unter den [aufrufen C\# aus JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) Rezept für Informationen zur Verwendung von C-Aufruf\# Funktionen von JavaScript). Zum Schluss wird eine anfängliche Webseite mit geladen [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Erstellen Sie die App, und führen Sie sie aus. Eine einfache Webseite-Viewer-Anwendung sollte wie im folgenden Screenshot dargestellt angezeigt werden:

[![Beispiel Anzeigen einer WebView-App](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Behandelt die **wieder** Schaltfläche drücken, fügen Sie die folgenden using-Anweisung:

```csharp
using Android.Views;
```

Fügen Sie die folgende Methode innerhalb der `HelloWebView` Aktivität:

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

Diese [`OnKeyDown(int, KeyEvent)`](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent))
Callback-Methode wird aufgerufen, wenn eine Schaltfläche geklickt wird, während die Aktivität ausgeführt wird. Die Bedingung in verwendet die [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) zu überprüfen, ob die Taste gedrückt werden die **wieder** Schaltfläche und gibt an, ob die [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) tatsächlich kann Navigieren zurück (falls es sich um einen Verlauf hat). Wenn beide Bedingungen erfüllt sind, und klicken Sie dann die [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) -Methode aufgerufen wird, navigiert die zurück ein Schritt in der [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Verlauf. Zurückgeben von `true` gibt an, dass das Ereignis behandelt wurde. Wenn diese Bedingung nicht erfüllt ist, wird das Ereignis an das System gesendet.

Führen Sie die Anwendung erneut aus. Sie sollten jetzt in der Lage, folgen Links aus, und navigieren Sie über die Seitenverlauf zurück:

[![Beispiel-Screenshot, der zurück-Taste in Aktion](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Verwandte Links

- [Call C# from JavaScript (Aufrufen von C#-Code über JavaScript)](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
