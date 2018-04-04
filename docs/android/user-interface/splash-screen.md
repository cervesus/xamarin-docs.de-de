---
title: Begrüßungsbildschirm
description: Eine Android-app nimmt einige Zeit in gestartet wird, insbesondere wenn die app zunächst auf einem Gerät gestartet wird. Ein Begrüßungsbildschirm möglicherweise Start angezeigt Bearbeitung an den Benutzer "oder" an, dass branding einrichten.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: f34a3ee44b604bf0b82faf77769f3c2844e6460f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="splash-screen"></a>Begrüßungsbildschirm

_Eine Android-app nimmt einige Zeit in gestartet wird, insbesondere wenn die app zunächst auf einem Gerät gestartet wird. Ein Begrüßungsbildschirm möglicherweise Start angezeigt Bearbeitung an den Benutzer "oder" an, dass branding einrichten._


## <a name="overview"></a>Übersicht

Eine Android-app benötigt einige Zeit, um gestartet wird, insbesondere bei der ersten Mal auf einem Gerät die app ausgeführt wird (Dies wird manchmal als eine _Kaltstart_). Der Begrüßungsbildschirm möglicherweise angezeigt Fortschritt für den Benutzer starten oder branding-Informationen, um zu identifizieren, und stufen die Anwendung möglicherweise angezeigt.

Dieses Handbuch beschreibt eine Technik, um einen Begrüßungsbildschirm in einer Android-Anwendung zu implementieren. Es umfasst die folgenden Schritte aus:

1.  Erstellen eine zeichenbare Ressource für den Begrüßungsbildschirm.

2.  Definieren ein neues Design, das die zeichenbare Ressource angezeigt werden.

3.  Hinzufügen einer neuen Aktivität der Anwendung, die als durch das Design, das im vorherigen Schritt erstellten definierten Begrüßungsbildschirm verwendet werden.

[![Beispiel für Xamarin-Logo-Splash-Bildschirm gefolgt von app-Bildschirm](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>Anforderungen

Dieses Handbuch setzt voraus, dass die Anwendung für Android-API-Ebene 15 (Android 4.0.3) diejenige oder höher. Die Anwendung zudem müssen die **Xamarin.Android.Support.v4** und **Xamarin.Android.Support.v7.AppCompat** NuGet-Pakete, die dem Projekt hinzugefügt.

Alle Code und XML-Code in diesem Handbuch finden Sie in der [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen) Beispielprojekt für dieses Handbuchs.


## <a name="implementing-a-splash-screen"></a>Implementieren einen Begrüßungsbildschirm

Die schnellste Möglichkeit zum Rendern und den Begrüßungsbildschirm angezeigt wird, erstellen ein benutzerdefiniertes Design, und wenden Sie es auf eine Aktivität, die den Begrüßungsbildschirm weist. Wenn die Aktivität gerendert wird, lädt das Design und wendet die zeichenbare Ressource (Verweis durch das Design) auf den Hintergrund der Aktivität. Dieser Ansatz vermeidet die Notwendigkeit zum Erstellen einer Layoutdatei.

Der Begrüßungsbildschirm wird als eine Aktivität, die das Branding zeigt implementiert zeichenbaren, führt alle Initialisierungen und startet alle Aufgaben. Sobald die app neu gestartet wurde, wird der Begrüßungsbildschirm Aktivität startet die Hauptaktivität und selbst aus der Anwendung Back-Stapel entfernt wird.


### <a name="creating-a-drawable-for-the-splash-screen"></a>Erstellen eine zeichenbaren für den Begrüßungsbildschirm

Der Begrüßungsbildschirm wird eine XML-Datei im Hintergrund des Begrüßungsbildschirms Aktivität zeichenbaren angezeigt. Es ist erforderlich, mit der ein Bitmapbild (z. B. eine PNG- oder JPG) für das Bild anzuzeigen.

In diesem Handbuch verwenden wir eine [Ebenenliste](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) zur Mitte der Begrüßungsbildschirms in der Anwendung. Der folgende Codeausschnitt zeigt ein Beispiel für eine `drawable` Ressource mit einer `layer-list`:

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

Dies `layer-list` wird das Bild des Begrüßungsbildschirms center **"Splash.png"** in einem Hintergrundthread gemäß der `@color/splash_background` Ressource.
Legen Sie diese Datei in die **Ressourcen und Ausgaben möglich** Ordner (z. B. **Resources/drawable/splash_screen.xml**).

Nachdem der Begrüßungsbildschirm zeichenbaren erstellt wurde, besteht der nächste Schritt erstellen Sie ein Design für den Begrüßungsbildschirm.


### <a name="implementing-a-theme"></a>Implementieren ein Design

Um ein benutzerdefiniertes Design für den Begrüßungsbildschirm Aktivität zu erstellen, bearbeiten (oder hinzufügen) die Datei **values/styles.xml** und erstellen Sie ein neues `style` -Element für den Begrüßungsbildschirm. Ein Beispiel für **values/style.xml** Datei wird unten gezeigt, mit einem `style` mit dem Namen **MyTheme.Splash**:

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

**MyTheme.Splash** ist sehr spartan &ndash; Fenster Hintergrund deklariert, explizit entfernt die Titelleiste aus dem Fenster und deklariert, dass es Vollbildmodus ist. Wenn Sie einen Begrüßungsbildschirm erstellen, die die Benutzeroberfläche der app emuliert, bevor die Aktivität die erste Layout vergrößert möchten, können Sie `windowContentOverlay` statt `windowBackground` in Ihre Formatdefinition. In diesem Fall müssen Sie auch ändern der **splash_screen.xml** zeichenbaren, sodass er eine Emulation von der Benutzeroberfläche angezeigt.


### <a name="create-a-splash-activity"></a>Erstellen einer Splash-Aktivität

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

`SplashActivity` explizit verwendet das Design, das im vorherigen Abschnitt erstellt wurde, das Standarddesign der Anwendung überschreiben.
Besteht keine Notwendigkeit, laden ein Layout in `OnCreate` wie das Design zeichenbaren als Hintergrund deklariert.

Es ist wichtig, legen Sie die `NoHistory=true` Attribut, damit die Aktivität aus dem Back-Stapel entfernt wird. Sie können auch überschreiben, um zu verhindern, dass die Schaltfläche "zurück" durch das Abbrechen des Startprozess, `OnBackPressed` und keine weiteren Aktionen erforderlich:

```csharp
public override void OnBackPressed() { }
```

Die Start-Arbeit erfolgt asynchron in `OnResume`. Dies ist erforderlich, damit die Arbeit Start nicht verlangsamt oder die Darstellung des Startbildschirms verzögert. Nach Abschluss die Arbeit `SplashActivity` startet `MainActivity` und der Benutzer kann die Interaktion mit der app beginnen.

Diese neue `SplashActivity` als die startprogrammaktivität für die Anwendung festgelegt werden, durch Festlegen der `MainLauncher` -Attribut `true`. Da `SplashActivity` ist jetzt die startprogrammaktivität, die Sie bearbeiten müssen `MainActivity.cs`, und entfernen Sie die `MainLauncher` -Attribut aus `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```


## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert eine Möglichkeit, einen Begrüßungsbildschirm in einer Anwendung Xamarin.Android implementieren; ein benutzerdefiniertes Design zuweisen, nämlich die Start-Aktivität.


## <a name="related-links"></a>Verwandte Links

- [Begrüßungsbildschirm (Beispiel)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Layer-List Drawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Material Entwurfsmuster - Startbildschirme](https://www.google.com/design/spec/patterns/launch-screens.html)
