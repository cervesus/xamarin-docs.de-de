---
title: Begrüßungsbildschirm
description: Eine Android-App nimmt einige Zeit in Betrieb, insbesondere wenn die APP zum ersten Mal auf einem Gerät gestartet wird. Ein Begrüßungsbildschirm kann den Startstatus für den Benutzer oder das Branding anzeigen.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/02/2019
ms.openlocfilehash: 8f225df47b299ae4748c3a3fea586f277e14213d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028715"
---
# <a name="splash-screen"></a>Begrüßungsbildschirm

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)

_Eine Android-App nimmt einige Zeit in Betrieb, insbesondere wenn die APP zum ersten Mal auf einem Gerät gestartet wird. Ein Begrüßungsbildschirm kann den Startstatus für den Benutzer oder das Branding anzeigen._

## <a name="overview"></a>Übersicht

Eine Android-App kann einige Zeit in Anspruch nehmen, insbesondere beim ersten Ausführen der APP auf einem Gerät (manchmal auch als _Kaltstart_bezeichnet). Der Begrüßungsbildschirm zeigt möglicherweise den Startstatus für den Benutzer an, oder es werden Brandinginformationen angezeigt, um die Anwendung zu identifizieren und höher zu Stufen.

In diesem Leitfaden wird eine Technik zum Implementieren eines Begrüßungs Bildschirms in einer Android-Anwendung erläutert. Dabei werden die folgenden Schritte behandelt:

1. Erstellen einer drawable-Ressource für den Begrüßungsbildschirm.

2. Definieren eines neuen Designs, mit dem die drawable-Ressource angezeigt wird.

3. Hinzufügen einer neuen Aktivität zur Anwendung, die als Begrüßungsbildschirm verwendet wird, der von dem im vorherigen Schritt erstellten Design definiert wird.

[![Beispiel-xamarin-Logo-Begrüßungsbildschirm, gefolgt vom APP-Bildschirm](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Diese Anleitung setzt voraus, dass die Anwendung auf Android-API-Ebene 21 oder höher ausgerichtet ist. Die Anwendung muss außerdem über die nuget-Pakete **xamarin. Android. Support. v4** und **xamarin. Android. Support. V7. AppCompat** verfügen, die dem Projekt hinzugefügt werden.

Sämtlicher Code und XML in diesem Handbuch finden Sie möglicherweise im [SplashScreen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen) -Beispiel Projekt für dieses Handbuch.

## <a name="implementing-a-splash-screen"></a>Implementieren eines Begrüßungs Bildschirms

Die schnellste Möglichkeit zum Rendering und zum Anzeigen des Begrüßungs Bildschirms ist das Erstellen eines benutzerdefinierten Designs und das Anwenden auf eine Aktivität, die den Begrüßungsbildschirm anzeigt. Wenn die Aktivität gerendert wird, wird das Design geladen, und die drawable-Ressource (auf die durch das Design verwiesen wird) wird auf den Hintergrund der Aktivität angewendet. Bei dieser Vorgehensweise entfällt die Notwendigkeit, eine Layoutdatei zu erstellen.

Der Begrüßungsbildschirm wird als Aktivität implementiert, die das Marken drawable-Element anzeigt, sämtliche Initialisierungen ausführt und alle Aufgaben startet. Nachdem die APP gestartet wurde, startet die Aktivität des Begrüßungs Bildschirms die Hauptaktivität und entfernt sich selbst aus dem Anwendungs-Back-Stack.

### <a name="creating-a-drawable-for-the-splash-screen"></a>Erstellen eines drawable-Element für den Begrüßungsbildschirm

Im Begrüßungsbildschirm wird im Hintergrund der Aktivität des Begrüßungs Bildschirms ein XML-drawable angezeigt. Zum Anzeigen des Bilds muss ein Bitmap-Bild (z. b. png oder JPG) verwendet werden.

Die Beispielanwendung definiert eine drawable namens **splash_screen. XML**. Diese drawable verwendet eine [Ebenenliste](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) , um das Begrüßungsbildschirm Bild in der Anwendung zu zentrieren, wie im folgenden XML-Code dargestellt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash_logo"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

In diesem `layer-list` wird das Begrüßungs Bild auf eine Hintergrundfarbe festgelegt, die durch die `@color/splash_background`-Ressource angegeben wird. Die Beispielanwendung definiert diese Farbe in der Datei **Resources/Values/Color. XML** :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <color name="splash_background">#FFFFFF</color>
</resources>
```

Weitere Informationen zu `Drawable` Objekten finden Sie [in der Google-Dokumentation unter Android drawable](https://developer.android.com/reference/android/graphics/drawable/Drawable).

### <a name="implementing-a-theme"></a>Implementieren eines Designs

Um ein benutzerdefiniertes Design für die Aktivität des Begrüßungs Bildschirms zu erstellen, bearbeiten Sie die Datei **Values/Styles. XML** , oder fügen Sie Sie hinzu, und erstellen Sie ein neues `style`-Element für den Begrüßungsbildschirm. Eine Beispieldatei " **values. XML** " wird unten mit einem `style` mit dem Namen " **mytheme. Splash**" angezeigt:

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
    <item name="android:windowContentOverlay">@null</item>  
    <item name="android:windowActionBar">true</item>  
  </style>
</resources>
```

**Mytheme. Splash** ist sehr sparsam &ndash; es den Hintergrund des Fensters deklariert, die Titelleiste explizit aus dem Fenster entfernt und deklariert, dass es sich um einen voll Bildschirm handelt. Wenn Sie einen Begrüßungsbildschirm erstellen möchten, mit dem die Benutzeroberfläche der APP emuliert wird, bevor die Aktivität das erste Layout auffüllt, können Sie in der Format Definition `windowContentOverlay` statt `windowBackground`. In diesem Fall müssen Sie auch das drawable-Element von **splash_screen. XML** so ändern, dass es eine Emulation ihrer Benutzeroberfläche anzeigt.

### <a name="create-a-splash-activity"></a>Erstellen einer Begrüßungs Aktivität

Nun benötigen wir eine neue Aktivität für Android zu starten, die über unser Begrüßungs Bild verfügt und alle Start Tasks ausführt. Der folgende Code ist ein Beispiel für eine vollständige Splash-Screen-Implementierung:

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

`SplashActivity` verwendet explizit das Design, das im vorherigen Abschnitt erstellt wurde, wobei das Standarddesign der Anwendung überschrieben wird.
Es ist nicht erforderlich, ein Layout in `OnCreate` zu laden, da das Design einen drawable als Hintergrund deklariert.

Es ist wichtig, das `NoHistory=true`-Attribut so festzulegen, dass die Aktivität aus dem Hintergrund Stapel entfernt wird. Wenn Sie verhindern möchten, dass der Startvorgang von der Schaltfläche "zurück" abgebrochen wird, können Sie auch `OnBackPressed` außer Kraft setzen und keine Aktion ausführen:

```csharp
public override void OnBackPressed() { }
```

Die Start Arbeit wird in `OnResume`asynchron ausgeführt. Dies ist erforderlich, damit die Start Arbeit die Darstellung des Startbildschirms nicht verlangsamt oder verzögert. Wenn die Arbeit abgeschlossen ist, wird `SplashActivity` gestartet `MainActivity` und der Benutzer kann mit der Interaktion mit der APP beginnen.

Diese neue `SplashActivity` wird als Start Programm Aktivität für die Anwendung festgelegt, indem das `MainLauncher`-Attribut auf `true`festgelegt wird. Da `SplashActivity` jetzt die Start Programm Aktivität ist, müssen Sie `MainActivity.cs`bearbeiten und das `MainLauncher`-Attribut aus `MainActivity`entfernen:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>Querformat

Der Begrüßungsbildschirm, der in den vorherigen Schritten implementiert wurde, wird im hoch-und Querformat ordnungsgemäß angezeigt. In einigen Fällen ist es jedoch erforderlich, separate Begrüßungs Bildschirme für hoch-und Querformat (z. b. wenn das Begrüßungs Bild voll Bildschirm ist) zu haben.

Zum Hinzufügen eines Begrüßungs Bildschirms für den Querformat führen Sie die folgenden Schritte aus:

1. Fügen Sie im Ordner **Resources/drawable** die Landscape-Version des Begrüßungsbildschirm Bilds hinzu, das Sie verwenden möchten. In diesem Beispiel ist **splash_logo_land. png** die Landscape-Version des Logos, das in den obigen Beispielen verwendet wurde (es wird eine weiße Beschriftung anstelle von Blue verwendet).

2. Erstellen Sie im Ordner **Resources/drawable** eine Landscape-Version der zuvor definierten `layer-list` drawable (z. b **. splash_screen_land. XML**). Legen Sie in dieser Datei den bitmappfad auf die Querformat Version des Begrüßungsbildschirm Bilds fest. Im folgenden Beispiel verwendet **splash_screen_land. XML** **splash_logo_land. png**:

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

3. Erstellen Sie den Ordner " **Resources/Values-Land** ", falls er nicht bereits vorhanden ist.

4. Fügen Sie die Dateien **Colors. XML** und **Style. XML** zu **Values-Land** hinzu (diese können aus den vorhandenen **Werten/Colors. XML** -und **Values/Style. XML** -Dateien kopiert und geändert werden).

5. Ändern Sie **Values-Land/Style. XML** so, dass es die Landscape-Version der drawable für `windowBackground`verwendet. In diesem Beispiel wird **splash_screen_land. XML** verwendet:

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

6. Ändern Sie **Values-Land/Colors. XML** , um die Farben zu konfigurieren, die Sie für die Querformat Version des Begrüßungs Bildschirms verwenden möchten. In diesem Beispiel wird die Begrüßungs Hintergrundfarbe für den Querformat in blau geändert:

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

7. Erstellen Sie die APP erneut, und führen Sie Sie aus. Drehen Sie das Gerät in den Querformat, während der Begrüßungsbildschirm weiterhin angezeigt wird. Der Begrüßungsbildschirm ändert sich in die Landscape-Version:

    [![Drehung des Begrüßungs Bildschirms in den Querformat](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)

Beachten Sie, dass die Verwendung eines Begrüßungs Bildschirms im Querformat nicht immer eine nahtlose Umgebung bereitstellt. Standardmäßig wird die APP von Android im Hochformat gestartet und in den Querformat versetzt, auch wenn sich das Gerät bereits im Querformat befindet. Wenn die APP gestartet wird, während sich das Gerät im Querformat befindet, stellt das Gerät den Begrüßungsbildschirm für Hochformat kurz her und animiert dann die Drehung vom Hochformat auf den Begrüßungsbildschirm des quer Bildes. Leider findet dieser erste Querformat Übergang statt, auch wenn `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` in den Flags der Begrüßungs Aktivität angegeben ist. Die beste Möglichkeit, diese Einschränkung zu umgehen, besteht darin, ein einzelnes Begrüßungsbildschirm Bild zu erstellen, das im hoch-und Querformat ordnungsgemäß gerendert wird.

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung wurde eine Möglichkeit zum Implementieren eines Begrüßungs Bildschirms in einer xamarin. Android-Anwendung erörtert. Dies gilt insbesondere für das Anwenden eines benutzerdefinierten Designs auf die Start Aktivität.

## <a name="related-links"></a>Verwandte Links

- [SplashScreen (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)
- [drawable für Ebenenliste](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [Material Entwurfsmuster-Startbildschirme](https://material.io/design/communication/launch-screen.html#usage)
