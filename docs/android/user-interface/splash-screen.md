---
title: Begrüßungsbildschirm
description: Eine Android-app dauert einige Zeit, zu starten, insbesondere, wenn die app zunächst auf einem Gerät gestartet wird. Anzeige eines Begrüßungsbildschirms möglicherweise Start angezeigt Bearbeitung an den Benutzer "oder" branding an einrichten.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/06/2018
ms.openlocfilehash: b28dba9031840b312868e2ebc45e348a390d3b12
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675049"
---
# <a name="splash-screen"></a>Begrüßungsbildschirm

_Eine Android-app dauert einige Zeit, zu starten, insbesondere, wenn die app zunächst auf einem Gerät gestartet wird. Anzeige eines Begrüßungsbildschirms möglicherweise Start angezeigt Bearbeitung an den Benutzer "oder" branding an einrichten._


## <a name="overview"></a>Übersicht

Eine Android-app dauert einige Zeit gestartet werden, vor allem bei der erstmaligen die app auf einem Gerät ausgeführt wird (Dies wird manchmal nur als eine _Kaltstart_). Kann den Begrüßungsbildschirm anzuzeigen. Starten Sie den Status für den Benutzer aus, oder branding-Informationen, um zu ermitteln und fördern die Anwendung zeigt.

Dieses Handbuch behandelt ein Verfahren zum Implementieren eines Begrüßungsbildschirms in einer Android-Anwendung. Hierin sind die folgenden Schritte aus:

1.  Erstellen einer drawable-Ressource für den Begrüßungsbildschirm.

2.  Definieren ein neues Design, das die drawable-Ressource angezeigt wird.

3.  Hinzufügen einer neuen Aktivität zur Anwendung, die als Begrüßungsbildschirm definiert durch das Design, das im vorherigen Schritt erstellten verwendet werden.

[![Beispiel für Xamarin-Logo-Begrüßungsbildschirm gefolgt von app-Bildschirm](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>Anforderungen

Dieses Handbuch setzt voraus, dass die Anwendung auf Android-API-Ebene 15 (Android 4.0.3) ausgerichtet ist oder höher. Die Anwendung muss auch verfügen die **Xamarin.Android.Support.v4** und **Xamarin.Android.Support.v7.AppCompat** NuGet-Pakete, die dem Projekt hinzugefügt.

Alle Code und XML-Code in diesem Handbuch finden Sie unter den [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen) Beispielprojekt zu diesem Handbuch.


## <a name="implementing-a-splash-screen"></a>Implementieren eines Begrüßungsbildschirms

Die schnellste Möglichkeit zum Rendern und den Begrüßungsbildschirm angezeigt wird, erstellen Sie ein benutzerdefiniertes Design, und klicken Sie auf eine Aktivität, die den Begrüßungsbildschirm weist angewendet. Wenn die Aktivität gerendert wird, lädt das Design und wendet die drawable-Ressource (Verweis durch das Design) in den Hintergrund der Aktivität. Dieser Ansatz vermeidet die Notwendigkeit, erstellen eine Layoutdatei.

Der Begrüßungsbildschirm wird als eine Aktivität, die organisationsspezifischen zeigt implementiert drawable, führt alle Initialisierungen, und alle Aufgaben gestartet. Sobald die app das Bootstrapping durchgeführt wurde, wird der Begrüßungsbildschirm Aktivität startet die Hauptaktivität, und sich selbst aus dem BackStack der Anwendung entfernt.


### <a name="creating-a-drawable-for-the-splash-screen"></a>Erstellen eine Drawable für den Begrüßungsbildschirm

Der Begrüßungsbildschirm wird im Hintergrund des Begrüßungsbildschirms Aktivität drawable XML angezeigt. Es ist erforderlich, ein Bitmapbild (z. B. eine PNG- oder JPG) für das Image zu verwenden, um Sie anzuzeigen.

In diesem Leitfaden verwenden wir eine [Ebenenliste](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) zentrieren Sie das Image des Begrüßungsbildschirms in der Anwendung. Der folgende Codeausschnitt zeigt ein Beispiel für eine `drawable` Ressource mit einer `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

Dies `layer-list` wird das Image des Begrüßungsbildschirms center **"Splash.png"** im Hintergrund gemäß der `@color/splash_background` Ressource. Platzieren Sie diese XML-Datei in die **Ressourcen/drawable** Ordner (z. B. **Resources/drawable/splash_screen.xml**).

Nach der Begrüßungsbildschirm drawable erstellt wurde, werden im nächste Schritt erstellen Sie ein Design für den Begrüßungsbildschirm.


### <a name="implementing-a-theme"></a>Implementieren ein Design

Um ein benutzerdefiniertes Design für den Begrüßungsbildschirm Aktivität zu erstellen, bearbeiten (oder hinzufügen) die Datei **values/styles.xml** und erstellen Sie ein neues `style` -Element für den Begrüßungsbildschirm. Ein Beispiel für **values/style.xml** Datei ist unten dargestellt, mit einem `style` mit dem Namen **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** ist sehr spartan &ndash; deklariert der Fensterhintergrund, explizit der Titelleiste angezeigt wird, aus dem Fenster, und gibt an, dass es sich um Vollbildmodus ist. Wenn Sie die Anzeige eines Begrüßungsbildschirms zu erstellen, die die Benutzeroberfläche der app emuliert, bevor die Aktivität der ersten Layoutübergabe vergrößert möchten, können Sie `windowContentOverlay` statt `windowBackground` in Ihre Style-Definition. In diesem Fall müssen Sie auch ändern der **splash_screen.xml** drawable, sodass eine Emulation von der Benutzeroberfläche angezeigt.


### <a name="create-a-splash-activity"></a>Erstellen Sie eine Splash-Aktivität

Nun benötigen wir eine neue Aktivität für Android zu starten, die unsere Splash-Image und führt alle Startaufgaben aus. Der folgende Code ist ein Beispiel für eine vollständige Splash-Bildschirm-Implementierung:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` explizit verwendet das Design, das im vorherigen Abschnitt wurde das Standarddesign in der Anwendung überschreiben.
Besteht keine Notwendigkeit zum Laden eines Layouts in `OnCreate` wie Designs ein drawable als Hintergrund deklariert.

Es ist wichtig, legen Sie die `NoHistory=true` Attribut, damit die Aktivität aus dem BackStack entfernt wird. Sie können auch überschreiben, um zu verhindern, dass die zurück-Schaltfläche Abbrechen des Startvorgangs, `OnBackPressed` und es sind keine weiteren Aktionen:

```csharp
public override void OnBackPressed() { }
```

Die Startup-Arbeit erfolgt asynchron in `OnResume`. Dies ist erforderlich, damit die Arbeit beim Start nicht verlangsamt oder die Darstellung des Startbildschirms verzögert. Wenn die Arbeit abgeschlossen hat, `SplashActivity` startet `MainActivity` und der Benutzer kann beginnen, mit der app interagieren.

Diese neue `SplashActivity` als der startprogrammaktivität für die Anwendung festgelegt ist, durch Festlegen der `MainLauncher` Attribut `true`. Da `SplashActivity` ist jetzt der startprogrammaktivität, die Sie bearbeiten müssen `MainActivity.cs`, und entfernen Sie die `MainLauncher` -Attribut vom `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>Im Querformat

Der Begrüßungsbildschirm angezeigt, die in den vorherigen Schritten implementiert wird im Hoch-und Querformat richtig angezeigt. In einigen Fällen ist es jedoch notwendig, separate Begrüßungsbildschirme für hoch-und Querformat haben (z. B., wenn das Image des Begrüßungsbildschirms Vollbildmodus ist).

Um einen Begrüßungsbildschirm für im Querformat hinzuzufügen, verwenden Sie die folgenden Schritte aus:

1. In der **Ressourcen/drawable** Ordner hinzufügen die Landschaft-Version, der das Bild des Begrüßungsbildschirms verwenden möchten. In diesem Beispiel **splash_logo_land.png** ist die Version im Querformat das Logo, das verwendet wurde, in den obigen Beispielen (verwendet weißen Nachrichten nicht blau).

2. In der **Ressourcen/drawable** Ordner, erstellen Sie eine Querformat-Version des der `layer-list` drawable, wurde zuvor definiert (z. B. **splash_screen_land.xml**). Legen Sie in dieser Datei den Bitmappfad, auf die Version im Querformat, der das Image des Begrüßungsbildschirms. Im folgenden Beispiel **splash_screen_land.xml** verwendet **splash_logo_land.png**:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
        <color android:color="@color/splash_background"/>
      </item>
      <item>
        <bitmap
            android:src="@drawable/splash_logo_land"
            android:tileMode="disabled"
            android:gravity="center"/>
      </item>
    </layer-list>
    ```

3.  Erstellen der **Ressourcen/Werte-Land** Ordner aus, wenn er nicht bereits vorhanden.

4.  Fügen Sie die Dateien **"Colors.xml"** und **style.xml** zu **Werte zusteuere** (diese kopiert und aus der vorhandenen geändert werden können **values/colors.xml**und **values/style.xml** Dateien).

5.  Ändern Sie **Werte-Land/style.xml** so, dass es die Landschaft-Version von der drawable für verwendet `windowBackground`. In diesem Beispiel **splash_screen_land.xml** wird verwendet:

    ```xml
    <resources>
      <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
      </style>
        <style name="MyTheme" parent="MyTheme.Base">
      </style>
      <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowBackground">@drawable/splash_screen_land</item>
        <item name="android:windowNoTitle">true</item>  
        <item name="android:windowFullscreen">true</item>  
        <item name="android:windowContentOverlay">@null</item>  
        <item name="android:windowActionBar">true</item>  
      </style>
    </resources>
    ```

6.  Ändern Sie **Werte-Land / "Colors.xml"** so konfigurieren Sie die Farben für die Landschaft-Version des Begrüßungsbildschirms verwenden möchten. In diesem Beispiel wird die Hintergrundfarbe des Begrüßungsbildschirms für im Querformat Blau geändert:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#3498DB</color>
    </resources>
    ```

7.  Erstellen Sie und führen Sie die app erneut aus. Drehen Sie das Gerät auf Querformat, während weiterhin der Begrüßungsbildschirm angezeigt wird. Der Begrüßungsbildschirm ändert sich in das Querformat-Version:

    [![Drehung des Begrüßungsbildschirms in das Querformat](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)


Beachten Sie, dass die Verwendung eines Begrüßungsbildschirms im Querformat nicht immer eine nahtlose benutzererfahrung bietet. Standardmäßig werden Android startet die app im Hochformat und geht es um Querformat, selbst wenn das Gerät bereits im Querformatmodus ausgeführt wird. Daher, wenn die app gestartet wird, während das Gerät im Querformat ist, das Gerät kurz zeigt den Begrüßungsbildschirm Hochformat und klicken Sie dann animiert Rotationsachse von das Hochformat auf Querformat Begrüßungsbildschirm. Leider dieser anfänglichen Hochformat, Querformat Übergang erfolgt auch dann, wenn `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` wird in der Aktivität Splash-Flags angegeben. Die beste Möglichkeit, diese Einschränkung umgehen, ist eine einzelne Splash-Bildschirm, das rendert ordnungsgemäß in sowohl hoch-und Querformat zu erstellen.


## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, eine Möglichkeit zum Implementieren eines Begrüßungsbildschirms in einer Xamarin.Android-Anwendung; anwenden, nämlich ein benutzerdefiniertes Design, mit der Startaktivität.


## <a name="related-links"></a>Verwandte Links

- [SplashScreen (Beispiel)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Layer-List-Drawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [Material Design-Muster – Startbildschirme](https://material.io/design/communication/launch-screen.html#usage)
