---
title: "Erstellen ein Schriftbild an überwachen"
description: "Dieses Handbuch erläutert, wie einen benutzerdefinierte Überwachung Gesicht-Dienst für Android Dach implementiert wird. Eine schrittweise Anleitung werden für erstellen eine reduzierte digitale Überwachungsfenster Gesicht Dienst, gefolgt von weiteren Code zum Erstellen einer Zifferblatt Analog-Stil Watch bereitgestellt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fb3a2a9e60bda2a99a719bf75d23c29d42a94bdb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-watch-face"></a>Erstellen ein Schriftbild an überwachen

_Dieses Handbuch erläutert, wie einen benutzerdefinierte Überwachung Gesicht-Dienst für Android Dach implementiert wird. Eine schrittweise Anleitung werden für erstellen eine reduzierte digitale Überwachungsfenster Gesicht Dienst, gefolgt von weiteren Code zum Erstellen einer Zifferblatt Analog-Stil Watch bereitgestellt._

## <a name="overview"></a>Übersicht 

In dieser exemplarischen Vorgehensweise wird ein grundlegende Überwachung Gesicht-Dienst erstellt, um die Grundlagen der Erstellung einer benutzerdefinierten Android Dach überwachen-Symbol zu veranschaulichen. Die anfängliche Überwachung Gesicht Dienst zeigt eine einfache digitale Überwachung, in dem die aktuelle Uhrzeit in Stunden und Minuten angezeigt: 

[![Zifferblatt digitale Watch](creating-a-watchface-images/01-initial-face.png "Beispiel-Screenshot, der dem Zifferblatt der anfänglichen digitale Watch")](creating-a-watchface-images/01-initial-face.png#lightbox)

Nachdem diese Zifferblatt digitale Watch entwickelt und getestet wird, wird mehr Code um das upgrade auf eine ganz Zifferblatt mit drei Händen analogen Watch anspruchsvolle hinzugefügt: 

[![Zifferblatt analogen Watch](creating-a-watchface-images/02-example-watchface.png "Beispiel-Screenshot, der dem Zifferblatt der endgültigen analogen Watch")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Sehen Sie sich Gesicht Services gebündelt und als Teil einer Abnutzung-app installiert werden. In den folgenden Beispielen `MainActivity` enthält lediglich den Code aus der Abnutzung app-Vorlage, damit der Überwachung Gesicht Dienst verpackt und als Teil der app auf die intelligente Überwachung bereitgestellt werden kann. Aktiviert ist, wird diese app ausschließlich als Mittel zum Abrufen des Überwachung Gesicht-Diensts in der Abnutzung Gerät (oder -Emulator) geladen dienen zum Debuggen und testen. 

## <a name="requirements"></a>Anforderungen

Um eine Überwachung Gesicht-Dienst zu implementieren, ist Folgendes erforderlich:

-   Android 5.0.x (API-Ebene 21) oder höher auf dem Abnutzung Gerät oder Emulator.

-   Die [Xamarin Android Dach Unterstützungsbibliotheken](https://www.nuget.org/packages/Xamarin.Android.Wear) muss das Xamarin.Android-Projekt hinzugefügt werden. 

Obwohl Android 5.0 ist die minimale API für die Implementierung eines Dienstes zum Überwachen der Gesicht Android 5.1 oder höher wird empfohlen. Android Dach Geräten mit Android 5.1 (API-22) oder höher zulassen Abnutzung apps zu steuern, was auf dem Bildschirm angezeigt wird, während das Gerät im Energiesparmodus befindet *ambient* Modus. Wenn das Gerät verlässt LP- *ambient* -Modus befindet sich im *interaktive* Modus. Weitere Informationen über diese Modi finden Sie unter [behalten Ihre App sichtbar](https://developer.android.com/training/wearables/apps/always-on.html). 


## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen ein neues Android Dach Projekt mit der Bezeichnung **WatchFace** (Weitere Informationen zum Erstellen von neuer Xamarin.Android-Projekten finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dialogfeld "Neues Projekt"](creating-a-watchface-images/03-wear-project-vs-sml.png "Dach App auswählen, klicken Sie im Dialogfeld "Neues Projekt"")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Dialogfeld "Neues Projekt"](creating-a-watchface-images/03-wear-project-xs-sml.png "Dach App auswählen, klicken Sie im Dialogfeld "Neues Projekt"")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


Legen Sie den Paketnamen auf `com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Packen Sie die namenseinstellung](creating-a-watchface-images/04-package-name-vs.png "legen den Paketnamen com.xamarin.watchface fest")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Packen Sie die namenseinstellung](creating-a-watchface-images/04-package-name-xs.png "legen den Paketnamen com.xamarin.watchface fest")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Darüber hinaus einen Bildlauf nach unten, und aktivieren Sie die **INTERNET** und **WAKE_LOCK** Berechtigungen: 

[![Erforderliche Berechtigungen für](creating-a-watchface-images/05-required-permissions-vs.png "Berechtigungen INTERNET aktivieren "und" WAKE_LOCK")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Legen Sie die mindestens Android-Version auf **Android 5.1 (API-Ebene 22)**. Zudem ermöglichen die **Internet** und **WakeLock** Berechtigungen:

[![Erforderliche Berechtigungen für](creating-a-watchface-images/05-required-permissions-xs.png "Berechtigungen Internet aktivieren "und" WakeLock")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Im nächsten Schritt heruntergeladen [preview.png](creating-a-watchface-images/preview.png) &ndash; dies werden hinzugefügt werden, um die **Drawables** Ordner weiter unten in dieser exemplarischen Vorgehensweise.


## <a name="add-the-xamarin-android-wear-package"></a>Fügen Sie das Xamarin Android Abnutzung-Paket

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Starten Sie den NuGet-Paket-Manager (in Visual Studio mit der Maustaste **Verweise** in der **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete verwalten...** ). Das Projekt aktualisieren, um die aktuellste stabile Version des **Xamarin.Android.Wear**: 

[![Hinzufügen von NuGet Package Manager](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.Android.Wear Paket hinzufügen")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Als Nächstes If **Xamarin.Android.Support.v13** ist installiert, deinstallieren Sie es:

[![Entfernen von NuGet Package Manager](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 entfernen")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Starten Sie den NuGet-Paket-Manager (in Visual Studio für Mac, mit der Maustaste **Pakete** in der **Lösung Bereich** , und wählen Sie **Pakete hinzufügen...** ). Das Projekt aktualisieren, um die aktuellste stabile Version des **Xamarin.Android.Wear**: 

[![Hinzufügen von NuGet Package Manager](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.Android.Wear Paket hinzufügen")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Erstellen und Ausführen der app auf einem Abnutzung Gerät oder Emulator (Weitere Informationen hierzu finden Sie unter der [Einstieg](~/android/wear/get-started/index.md) Handbuch). Der folgende Bildschirm für die app sollte auf dem Gerät Abnutzung angezeigt werden:

[![App-Screenshot](creating-a-watchface-images/08-app-screen.png "App-Bildschirm auf Abnutzung Gerät")](creating-a-watchface-images/08-app-screen.png#lightbox)

An diesem Punkt verfügt grundlegende Abnutzung-app nicht Überwachungsfenster Gesicht Funktionalität, da es keinen noch eine Überwachung Gesicht dienstimplementierung bereitstellt. Dieser Dienst wird als Nächstes hinzugefügt werden. 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android Abnutzung implementiert beobachten gesichtsabbildungen über die `CanvasWatchFaceService` Klasse. `CanvasWatchFaceService` stammt aus `WatchFaceService`, die selbst stammt aus `WallpaperService` wie im folgenden Diagramm dargestellt: 

[![Vererbung Diagramm](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService Vererbung-Diagramm")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` enthält eine geschachtelte `CanvasWatchFaceService.Engine`; instanziiert einen `CanvasWatchFaceService.Engine` -Objekt, das von dem Zifferblatt der Uhr zeichnen die eigentliche Arbeit übernimmt. `CanvasWatchFaceService.Engine` stammt aus `WallpaperService.Engine` wie in der Abbildung oben gezeigt. 

In diesem Diagramm nicht dargestellt ist eine `Canvas` , `CanvasWatchFaceService` verwendet für das Zeichnen von dem Zifferblatt der Uhr &ndash; dies `Canvas` über übergeben der `OnDraw` Methode wie unten beschrieben. 

In den folgenden Abschnitten wird ein benutzerdefinierte Überwachung Gesicht-Dienst erstellt werden, durch die folgenden Schritte: 

1.  Definieren Sie eine Klasse mit dem Namen `MyWatchFaceService` , stammt aus `CanvasWatchFaceService`. 

2.  In `MyWatchFaceService`, erstellen Sie eine geschachtelte Klasse mit dem Namen `MyWatchFaceEngine` , stammt aus `CanvasWatchFaceService.Engine`. 

3.  In `MyWatchFaceService`, implementieren Sie eine `CreateEngine` -Methode, die instanziiert `MyWatchFaceEngine` und gibt ihn zurück.

4.  In `MyWatchFaceEngine`, implementieren die `OnCreate` Methode zum Erstellen der Überwachung Gesicht-Formatvorlage, und führen Sie alle anderen Initialisierungsaufgaben. 

5.  Implementieren der `OnDraw` Methode `MyWatchFaceEngine`. Diese Methode wird aufgerufen, wenn dem Zifferblatt der Uhr neu gezeichnet werden muss (d. h. *für ungültig erklärt*). `OnDraw` ist die Methode für die Überwachung Gesicht Elemente wie Stunde, Minute und Sekunde Hände zeichnet (und zeichnet). 

6.  Implementieren der `OnTimeTick` Methode `MyWatchFaceEngine`. 
    `OnTimeTick` wird mindestens einmal pro Minute (in der Umgebung und interaktiven Modus) oder wenn das Datum/Uhrzeit geändert wurde aufgerufen. 

Weitere Informationen zu `CanvasWatchFaceService`, finden Sie unter den Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API-Dokumentation.
Auf ähnliche Weise [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) erläutert die tatsächliche Implementierung von dem Zifferblatt der Uhr.


### <a name="add-the-canvaswatchfaceservice"></a>Fügen Sie der CanvasWatchFaceService hinzu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Fügen Sie eine neue Datei namens **MyWatchFaceService.cs** (in Visual Studio mit der Maustaste **WatchFace** in der **Projektmappen-Explorer**, klicken Sie auf **hinzufügen > Neues Element...** , und wählen Sie **Klasse**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Fügen Sie eine neue Datei namens **MyWatchFaceService.cs** (in Visual Studio für Mac, mit der Maustaste die **WatchFace** Projekt, klicken Sie auf **hinzufügen > neuen Datei...** , und wählen Sie **leere Klasse**). 

-----

Ersetzen Sie den Inhalt dieser Datei durch den folgenden Code ein: 

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (abgeleitet von `CanvasWatchFaceService`) ist "Hauptprogramm" der Schriftart überwachen. `MyWatchFaceService` nur eine Methode implementiert `OnCreateEngine`, die instanziiert und gibt eine `MyWatchFaceEngine` Objekt (`MyWatchFaceEngine` stammt aus `CanvasWatchFaceService.Engine`). Die instanziierte `MyWatchFaceEngine` Objekt zurückgegeben werden muss, als eine `WallpaperService.Engine`. Das Kapseln von `MyWatchFaceService` Objekt an den Konstruktor übergeben wird. 

`MyWatchFaceEngine` ist die tatsächliche Überwachungsfenster Gesicht Implementierung &ndash; es enthält den Code, der dem Zifferblatt der Uhr zeichnet. Er verarbeitet auch Systemereignisse wie Bildschirm Änderungen (ambient/interaktiven Modi Bildschirm ausschalten, usw.). 

 
### <a name="implement-the-engine-oncreate-method"></a>Implementieren Sie die Engine OnCreate-Methode

Die `OnCreate` Methode initialisiert dem Zifferblatt der Uhr. Fügen Sie das folgende Feld zu `MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

Dies `Paint` Objekt wird verwendet, um die aktuelle Uhrzeit auf dem Zifferblatt der Uhr zu zeichnen. Fügen Sie die folgende Methode hinzu `MyWatchFaceEngine`: 

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` wird aufgerufen, kurz nachdem `MyWatchFaceEngine` gestartet wird. Er richtet die `WatchFaceStyle` (, die steuert, wie das Gerät Abnutzung mit dem Benutzer interagiert) und instanziiert die `Paint` -Objekt, das verwendet wird, zeigt den Zeitpunkt an. 

Der Aufruf von `SetWatchFaceStyle` bewirkt Folgendes:

1.  Legt *Peek-Modus* zu `PeekModeShort`, die bewirkt, dass Benachrichtigungen als kleine "Peek" Karten auf der Anzeige angezeigt werden. 

2.  Legt die Hintergrundfarbe Sichtbarkeit auf `Interruptive`, die bewirkt, dass des Hintergrunds einer Peek-Karte, nur kurz angezeigt werden soll, wenn es sich um eine Ausführung einer Benachrichtigung darstellt.

3.  Deaktiviert die Standardzeit für die Benutzeroberfläche von System aus, damit dem Zifferblatt der benutzerdefinierten Watch stattdessen zeigt den Zeitpunkt an kann auf dem Zifferblatt der Uhr gezeichnet werden.

Weitere Informationen zu diesen und anderen Überwachungsfenster Gesicht Style-Optionen finden Sie unter den Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API-Dokumentation.

Nach dem `SetWatchFaceStyle` abgeschlossen ist, `OnCreate` instanziiert die `Paint` Objekt (`hoursPaint`) und legt die Farbe Weiß und die Textgröße auf 48 Pixel ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) muss angegeben werden, in Pixel). 


### <a name="implement-the-engine-ondraw-method"></a>Implementieren Sie die Engine OnDraw-Methode

Die `OnDraw` Methode ist möglicherweise die wichtigste `CanvasWatchFaceService.Engine` Methode &ndash; ist die Methode tatsächlich zeichnet Gesicht Elemente wie z. B. Ziffern beobachten und Uhrzeiger Gesicht. Im folgenden Beispiel wird eine Uhrzeit-Zeichenfolge auf dem Zifferblatt der Uhr gezeichnet.
Fügen Sie die folgende Methode hinzu `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str, 
        (float)(frame.Left + 70), 
        (float)(frame.Top  + 80), hoursPaint);
}
```

Wenn Android aufruft `OnDraw`, übergibt sie ein `Canvas` Instanz und die Grenzen, die in der Schriftart gezeichnet werden kann. Im obigen Beispiel `DateTime` wird verwendet, um die aktuelle Uhrzeit in Stunden und Minuten (im 12-Stunden-Format) zu berechnen. Die resultierende Uhrzeit-Zeichenfolge der Zeichenfläche gezeichnet wird, mithilfe der `Canvas.DrawText` Methode. Die Zeichenfolge wird 70 Pixel über aus dem linken Rand und 80 Pixel nach unten vom oberen Rand angezeigt. 

Weitere Informationen zu den `OnDraw` -Methode finden Sie unter den Android [OnDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) API-Dokumentation.


### <a name="implement-the-engine-ontimetick-method"></a>Implementieren Sie die Engine OnTimeTick-Methode

Android in regelmäßigen Abständen ruft der `OnTimeTick` Methode, um die Zeit angezeigt, indem Sie dem Zifferblatt der Uhr zu aktualisieren. Es wird zur aufgerufen, mindestens einmal pro Minute (im ambient und interaktiven Modus), oder wenn die Datums-/Uhrzeitwerten oder Timezone geändert haben. Fügen Sie die folgende Methode hinzu `MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Diese Implementierung der `OnTimeTick` ruft einfach `Invalidate`. Die `Invalidate` Methode Zeitpläne `OnDraw` auf dem Zifferblatt der Uhr neu gezeichnet werden. 

Weitere Informationen zu den `OnTimeTick` -Methode finden Sie unter den Android [OnTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API-Dokumentation.

 
## <a name="register-the-canvaswatchfaceservice"></a>Registrieren der CanvasWatchFaceService

`MyWatchFaceService` muss registriert werden, der **AndroidManifest.xml** der zugeordneten Abnutzung app. Fügen Sie zu diesem Zweck den folgenden XML-Code, um die `<application>` Abschnitt: 

```xml
<service 
    android:name="watchface.MyWatchFaceService" 
    android:label="Xamarin Sample" 
    android:allowEmbedded="true" 
    android:taskAffinity="" 
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data 
        android:name="android.service.wallpaper" 
        android:resource="@xml/watch_face" />
    <meta-data 
        android:name="com.google.android.wearable.watchface.preview" 
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

Diese XML-Ausgabe führt Folgendes aus:

1.  Legt die `android.permission.BIND_WALLPAPER` Berechtigung. Diese Berechtigung ermöglicht die Überwachungsfenster Gesicht-Berechtigung so ändern Sie das Hintergrundbild System, auf dem Gerät. Beachten Sie, die durch diese Berechtigung werden, in festgelegt muss der `<service>` Abschnitt anstatt in der äußeren `<application>` Abschnitt. 

2.  Definiert eine `watch_face` Ressource. Diese Ressource ist eine kurze XML-Datei, die deklariert eine `wallpaper` Ressource (diese Datei wird im nächsten Abschnitt erstellt werden). 

3.  Deklariert eine zeichenbaren Bildtyp namens `preview` , von dem Überwachungsfenster Picker-Auswahlbildschirm angezeigt wird. 

4.  Enthält eine `intent-filter` zu wissen, dass Android lassen `MyWatchFaceSevice` wird das Anzeigen von einem Gesicht überwachen. 

Somit ist den Code für die grundlegende `WatchFace` Beispiel. Der nächste Schritt besteht darin die notwendigen Ressourcen hinzuzufügen.

 
## <a name="add-resource-files"></a>Hinzufügen von Ressourcendateien

Vor dem Ausführen des Diensts überwachen können, müssen Sie fügen die **Watch_face** Ressource und das Vorschaubild. Erstellen Sie zunächst eine neue XML-Datei am **Resources/xml/watch_face.xml** und Ersetzen Sie den Inhalt durch folgendes XML: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Legen Sie diese Datei Buildaktion auf **AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Buildvorgang](creating-a-watchface-images/10-android-resource-vs.png "Satz Buildaktion auf AndroidResource")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Buildvorgang](creating-a-watchface-images/10-android-resource-xs.png "Satz Buildaktion auf AndroidResource")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Diese Ressourcendatei definiert eine einfache `wallpaper` -Element, das für die Überwachung Schriftart verwendet wird. 

Wenn noch nicht geschehen, laden Sie [preview.png](creating-a-watchface-images/preview.png). Installieren Sie ihn unter **Resources/drawable/preview.png**. Achten Sie darauf, dass Sie diese Datei zum Hinzufügen der `WatchFace` Projekt. Diese Vorschau wird für den Benutzer in der Auswahl einer Überwachung Gesicht auf dem Gerät Abnutzung angezeigt. Um ein Vorschaubild für eigene Zifferblatt Watch zu erstellen, können Sie einen Screenshot der Schriftart überwachen nutzen, während er ausgeführt wird. (Weitere Informationen zu den ersten Schritten Screenshots von Abnutzung-Geräten finden Sie unter [dauert Screenshots](~/android/wear/deploy-test/debug-on-device.md#screenshots)). 


## <a name="try-it"></a>Probieren Sie es aus!

Erstellen und Bereitstellen der app auf dem Gerät Abnutzung. Daraufhin sollte der Bildschirm Abnutzung app wie zuvor angezeigt werden. Führen Sie Folgendes ein, um die neue Schriftart für die Überwachung zu aktivieren: 

1.  Streichen Sie nach rechts, bis Sie den Hintergrund des Bildschirms überwachen finden Sie unter. 

2.  Berühren Sie und halten Sie an einer beliebigen Stelle im Hintergrund des Bildschirms für zwei Sekunden.

3.  Streichen Sie nach von links nach rechts, um die verschiedenen Überwachungsfenster Gesichter zu durchsuchen. 

4.  Wählen Sie die **Xamarin Beispiel** beobachten Gesicht (auf der rechten Seite dargestellt): 

    [![Auswahl einer Watchface](creating-a-watchface-images/11-watchface-picker.png "Wischen Zifferblatt der Xamarin-Beispiel Watch suchen")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Tippen Sie auf die **Xamarin Beispiel** beobachten Fläche, um es auszuwählen. 

Dadurch werden das Zifferblatt der Überwachung des Geräts für die benutzerdefinierte Überwachung Gesicht bisher implementierten nutzungsdiensts Abnutzung geändert: 

[![Zifferblatt digitale Watch](creating-a-watchface-images/12-digital-watchface.png "benutzerdefinierte digitale Überwachung auf Abnutzung Gerät ausgeführt")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Dies ist ein Schriftbild an relativ einfach gehalten überwachen, da die Implementierung der app so minimal ist (z. B. dieser umfasst jedoch nicht überwachen Gesicht Hintergrund und ihn nicht aufrufen `Paint` Anti-Aliasing-Methoden, um die Darstellung zu verbessern). Es jedoch die minimalen-Funktionalität implementieren, die zum Erstellen eines benutzerdefinierten Überwachungsfenster Gesichts erforderlich ist. 

Im nächsten Abschnitt werden diese Zifferblatt Watch auf eine komplexere Implementierung aktualisiert werden. 

 
## <a name="upgrading-the-watch-face"></a>Aktualisieren von dem Zifferblatt der Uhr

Im weiteren Verlauf dieser exemplarischen Vorgehensweise `MyWatchFaceService` wird aktualisiert, um ein Zifferblatt Analog-Stil Watch anzuzeigen, und es wird erweitert, um weitere Funktionen zu unterstützen. Die folgenden Funktionen werden auf dem Zifferblatt der aktualisierten Watch erstellen hinzugefügt: 

1.  Gibt die Zeit mit analogen Stunde, Minute und Sekunde Hände.

2.  Antwortet auf Änderungen in der Sichtbarkeit.

3.  Reagiert auf die Unterschiede zwischen der ambient-Modus und im interaktiven Modus. 

4.  Liest die Eigenschaften des zugrunde liegenden Abnutzung Geräts an.

5.  Aktualisiert automatisch die Zeit, wenn eine Änderung der Zeitzone stattfindet. 

Vor der Implementierung der folgenden codeänderungen, herunterladen [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), entpacken Sie es, und verschieben Sie die entzippt PNG-Dateien auf **Ressourcen und Ausgaben möglich** (Überschreiben der vorherigen **preview.png**). Fügen Sie die neuen PNG-Dateien, die `WatchFace` Projekt.


### <a name="update-engine-features"></a>Aktualisieren Sie die Funktionen des Datenbankmoduls

Der nächste Schritt besteht Upgrade **MyWatchFaceService.cs** an eine Implementierung, die einer analogen Uhr Gesicht zeichnet und unterstützt neue Funktionen. Ersetzen Sie den Inhalt des **MyWatchFaceService.cs** mit der analoge Version des Codes Überwachungsfenster Gesicht in [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (können Ausschneiden und Einfügen von dieser Quelle in das vorhandene  **MyWatchFaceService.cs**). 

Diese Version des **MyWatchFaceService.cs** vorhandenen Methoden mehr Code hinzugefügt, und umfasst zusätzliche überschriebene Methoden, um weitere Funktionen hinzuzufügen. Die folgenden Abschnitte enthalten eine geführte Tour durch den Quellcode einfügen.

#### <a name="oncreate"></a>OnCreate

Die aktualisierte **OnCreate** Methode konfiguriert das Überwachungsfenster Gesicht Format wie zuvor, enthält aber einige zusätzliche Schritte erforderlich: 

1.  Legt das Hintergrundbild auf den **Xamarin_background** Ressource, die befindet **Resources/drawable-hdpi/xamarin_background.png**. 

2.  Initialisiert `Paint` Objekte für das Zeichnen von Hand Stunde, Minute Hand und zweite Zeiger.

3.  Initialisiert ein `Paint` -Objekt zum Zeichnen der Teilstriche Stunde entlang der Kante der Fläche überwachen. 

4.  Erstellt einen Zeitgeber, ruft der `Invalidate` (Neuzeichnen)-Methode, damit der zweite Zeiger pro Sekunde neu gezeichnet wird. Beachten Sie, dass dieser Zeitgeber erforderlich ist. da `OnTimeTick` Aufrufe `Invalidate` nur einmal pro Minute.

In diesem Beispiel enthält nur einen **xamarin_background.png** image; Sie möchten jedoch ein anderes Hintergrundbild für jede Dichte Bildschirm erstellen, die Ihre benutzerdefinierte Überwachung Gesicht unterstützt. 

#### <a name="ondraw"></a>OnDraw

Die aktualisierte **OnDraw** Methode zeichnet eine analoge-Stil überwachen Gesicht mithilfe der folgenden Schritte: 

1.  Ruft die aktuelle Uhrzeit, die jetzt beibehalten wird ein `time` Objekt. 

2.  Bestimmt die Grenzen der Zeichenoberfläche und seinen Mittelpunkt an.

3.  Zeichnet den Hintergrund, skaliert, um das Gerät zu passen, wenn der Hintergrund gezeichnet wird.

4.  Zeichnet zwölf *Ticks* auf dem Zifferblatt der Uhr (Dies entspricht der Stunden auf dem Uhrenzifferblatt). 

5.  Berechnet den Winkel, Drehung und Länge für jede Hand überwachen.

6.  Zeichnet jeden Hand oberflächlich überwachen. Beachten Sie, dass der zweite Zeiger nicht gezeichnet wird, wenn die Überwachung im ambient-Modus befindet. 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Diese Methode wird aufgerufen, um Sie zu informieren `MyWatchFaceEngine` zu den Eigenschaften des Geräts Abnutzung (z. B. ambient Low-Bit-Modus und Burn-in-Schutz). In `MyWatchFaceEngine`, überprüft diese Methode nur für die Low-bit-ambient-Modus (im ambient niedrigwertigste Bit-Modus, der Bildschirm unterstützt weniger Bits für jede Farbe). 

Weitere Informationen zu dieser Methode finden Sie unter den Android [OnPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API-Dokumentation.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Diese Methode wird aufgerufen, wenn das Gerät Abnutzung betritt oder verlässt ambient-Modus. In der `MyWatchFaceEngine` Implementierung deaktiviert dem Zifferblatt der Uhr Anti-Aliasing an, wenn es im ambient-Modus befindet. 

Weitere Informationen zu dieser Methode finden Sie unter den Android [OnAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API-Dokumentation.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Diese Methode wird aufgerufen, wenn die Überwachung wird sichtbar oder ausgeblendet. In `MyWatchFaceEngine`, diese Methode Register/hebt die Registrierung des Zeitzone Empfängers (unten beschrieben) gemäß den Sichtbarkeitszustand. 

Weitere Informationen zu dieser Methode finden Sie unter den Android [OnVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API-Dokumentation.

 
### <a name="time-zone-feature"></a>Zeitzone-Funktion

Die neue **MyWatchFaceService.cs** enthält auch Funktionen zum Aktualisieren der aktuellen Zeit, wenn die Zeitzone Änderungen (z. B. unterwegs zwischen Zeitzonen). Kurz vor dem Ende **MyWatchFaceService.cs**, eine Änderung einer Zone `BroadcastReceiver` definiert ist, verarbeitet die beabsichtigte Objekte Timezone geändert: 

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

Die `RegisterTimezoneReceiver` und `UnregisterTimezoneReceiver` Methoden werden aufgerufen, indem Sie die `OnVisibilityChanged` Methode. 
`UnregisterTimezoneReceiver` wird aufgerufen, wenn in der Sichtbarkeitszustand des dem Zifferblatt der Uhr geändert ist ausgeblendet. Wenn dem Zifferblatt der Uhr wieder sichtbar ist `RegisterTimezoneReceiver` aufgerufen wird (finden Sie unter der `OnVisibilityChanged` Methode). 

Das Modul `RegisterTimezoneReceiver` Methode deklariert einen Handler für diese Zeitzone des Empfängers `Receive` Ereignis; dieser Handler aktualisiert die `time` Objekt mit der Zeit immer eine Zeitzone überschritten wird: 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Ein Beabsichtigter Filter wird erstellt und registriert für den Empfänger Zeitzone:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

Die `UnregisterTimezoneReceiver` Methode hebt die Registrierung des Empfängers Zeitzone:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Führen Sie die Verbesserte Überwachung-Oberfläche

Erstellen und Bereitstellen der app auf dem Gerät Abnutzung erneut aus. Überwachungsfenster Gesicht Auswahl als vor dem Zifferblatt der Überwachung auswählen. In der Vorschau der Auswahl einer Überwachung wird auf der linken Seite angezeigt, und neue Überwachung Schriftart wird auf der rechten Seite angezeigt:

[![Zifferblatt analogen Watch](creating-a-watchface-images/13-analog-watchface.png "verbessert analogen Fläche in der Auswahl und auf Geräten")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

In dieser Abbildung wird der zweite Zeiger einmal pro Sekunde verschoben. Wenn Sie diesen Code auf einem Gerät Abnutzung ausführen, wird der zweite Zeiger gelangt der Apple Watch ambient-Modus ausgeblendet.

 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde eine benutzerdefinierte Android Dach Watchface implementiert und getestet. Die `CanvasWatchFaceService` und `CanvasWatchFaceService.Engine` Klassen wurden eingeführt, und die wichtigen Methoden der Modulklasse zum Erstellen eines einfachen digitale Überwachungsfenster Gesichts implementiert wurden. Diese Implementierung wurde mit mehr Funktionalität zum Erstellen einer analogen Uhr Zifferblatt aktualisiert, sodass zusätzliche Methoden wurden implementiert, um die Verarbeitung von Änderungen auf Sichtbarkeit, die ambient-Modus und die Unterschiede in den Eigenschaften des Geräts. Schließlich wurde ein Zeitzone broadcast Empfänger implementiert, damit die Überwachung die Zeit automatisch aktualisiert, wann eine Zeitzone überschritten wird. 


## <a name="related-links"></a>Verwandte Links

- [Überwachungsfenster Flächen erstellen](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace-Beispiel](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
