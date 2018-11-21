---
title: Erstellen eines Zifferblatts für Android Wear 1.0
description: Dieses Handbuch erklärt, wie einen benutzerdefinierte Überwachung gesichtserkennungs-Dienst für Android Wear 1.0 implementiert. Schrittweise Anweisungen zum Erstellen von digitalen sehen Sie sich gesichtserkennungs-Diensts, gefolgt von weiteren Code zum Erstellen einer Analog-Stil watchface Fall eine gekürzte.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/23/2018
ms.openlocfilehash: 067a39838fbfe3f1b33ac0d30b5069366b11e407
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172001"
---
# <a name="creating-a-watch-face"></a>Erstellen eines Zifferblatts

_Dieses Handbuch erklärt, wie einen benutzerdefinierte Überwachung gesichtserkennungs-Dienst für Android Wear 1.0 implementiert. Schrittweise Anweisungen zum Erstellen von digitalen sehen Sie sich gesichtserkennungs-Diensts, gefolgt von weiteren Code zum Erstellen einer Analog-Stil watchface Fall eine gekürzte._

## <a name="overview"></a>Übersicht

In dieser exemplarischen Vorgehensweise wird ein grundlegende Überwachung Face-Dienst erstellt, um die Grundlagen der Erstellung einer benutzerdefinierten 1.0 für Android Wear-Zifferblatt Ihrer Apple Watch veranschaulicht.
Der anfängliche Überwachung gesichtserkennungs-Dienst zeigt eine einfache digitale sehen Sie sich, in dem die aktuelle Uhrzeit in Stunden und Minuten angezeigt:

[![Digitale watchface](creating-a-watchface-images/01-initial-face.png "Beispiel-Screenshot, der das erste digitale watchface")](creating-a-watchface-images/01-initial-face.png#lightbox)

Nachdem diese digitale watchface entwickelt und getestet wird, wird mehr Code zum Upgraden auf eine analoge watchface mit drei Hand anspruchsvolle hinzugefügt:

[![Analoge watchface](creating-a-watchface-images/02-example-watchface.png "Beispiel-Screenshot, der das letzte analoge watchface")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Sehen Sie sich, gesichtserkennungs-Dienste sind zusammengefasst und als Teil einer 1.0 Wear-app installiert. In den folgenden Beispielen `MainActivity` enthält nichts anderes als den Code aus der 1.0 Wear-app-Vorlage, damit der Watch gesichtserkennungs-Dienst verpackt und als Teil der app auf die intelligenten Überwachung bereitgestellt werden kann. Aktiviert ist, dient diese app rein als ein Fahrzeug zum Abrufen des sehen Sie sich gesichtserkennungs-Diensts geladen, in dem 1.0 Wear-Gerät (oder den Emulator) zum Debuggen und testen.

## <a name="requirements"></a>Anforderungen

Um einen Watch gesichtserkennungs-Dienst zu implementieren, ist Folgendes erforderlich:

-   Android 5.0 (API Level 21) oder höher, auf dem Wear-Gerät oder Emulator.

-   Die [Xamarin Android Wear-Unterstützungsbibliotheken](https://www.nuget.org/packages/Xamarin.Android.Wear) muss das Xamarin.Android-Projekt hinzugefügt werden.

Obwohl Android 5.0 die Mindest-API-Ebene ist für die Implementierung eines Watch gesichtserkennungs-Diensts, Android 5.1 oder höher empfohlen. Android Wear-Geräten mit Android 5.1 (API 22) oder höher können Wear-apps zu steuern, was auf dem Bildschirm angezeigt wird, während das Gerät im Energiesparmodus befindet *ambient* Modus. Wenn das Gerät verlässt energiesparenden *ambient* -Modus befindet sich im *interaktive* Modus. Weitere Informationen über diese Modi finden Sie unter [halten Ihre App sichtbar](https://developer.android.com/training/wearables/apps/always-on.html).


## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen Sie ein neues Android Wear-1.0-Projekt namens **WatchFace** (Weitere Informationen zum Erstellen von neuen Xamarin.Android-Projekte finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Dialogfeld "Neues Projekt"](creating-a-watchface-images/03-wear-project-vs-sml.png "Wear-App in das Dialogfeld \"Neues Projekt\" auswählen")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Dialogfeld "Neues Projekt"](creating-a-watchface-images/03-wear-project-xs-sml.png "Wear-App in das Dialogfeld \"Neues Projekt\" auswählen")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


Legen Sie den Paketnamen `com.xamarin.watchface`:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Packen Sie die namenseinstellung](creating-a-watchface-images/04-package-name-vs.png "legen Sie den Paketnamen com.xamarin.watchface")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Packen Sie die namenseinstellung](creating-a-watchface-images/04-package-name-xs.png "legen Sie den Paketnamen com.xamarin.watchface")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Darüber hinaus einen Bildlauf nach unten, und aktivieren die **INTERNET** und **WAKE_LOCK** Berechtigungen:

[![Erforderliche Berechtigungen](creating-a-watchface-images/05-required-permissions-vs.png "INTERNET aktivieren und WAKE_LOCK Berechtigungen")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Legen Sie die Version des Android-Mindestversion auf **Android 5.1 (API-Ebene 22)**.
Darüber hinaus ermöglichen die **Internet** und **WakeLock** Berechtigungen:

[![Erforderliche Berechtigungen](creating-a-watchface-images/05-required-permissions-xs.png "Internet aktivieren und WakeLock Berechtigungen")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Als Nächstes laden [preview.png](creating-a-watchface-images/preview.png) &ndash; wird dies hinzugefügt, die **zeichenbarer Ressourcen** Ordner weiter unten in dieser exemplarischen Vorgehensweise.


## <a name="add-the-xamarinandroid-wear-package"></a>Hinzufügen des Xamarin.Android Wear-Pakets

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Starten Sie den NuGet-Paket-Manager (in Visual Studio mit der Maustaste **Verweise** in die **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete verwalten...** ). Aktualisieren Sie das Projekt auf die neueste stabile Version von **Xamarin.Android.Wear**:

[![Hinzufügen von NuGet-Paket-Manager](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "das Xamarin.Android.Wear-Paket hinzufügen")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Als Nächstes If **Xamarin.Android.Support.v13** ist installiert, deinstallieren Sie es:

[![Entfernen von NuGet-Paket-Manager](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 entfernen")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Starten Sie den NuGet-Paket-Manager (in Visual Studio für Mac, mit der Maustaste **Pakete** in die **Projektmappenbereich** , und wählen Sie **Pakete hinzufügen...** ). Aktualisieren Sie das Projekt auf die neueste stabile Version von **Xamarin.Android.Wear**:

[![Hinzufügen von NuGet-Paket-Manager](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "das Xamarin.Android.Wear-Paket hinzufügen")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Erstellen und Ausführen der app auf einem Wear-Gerät oder Emulator (Weitere Informationen hierzu finden Sie unter den [Einstieg](~/android/wear/get-started/index.md) Handbuch). Den folgende Bildschirm der app sollte auf dem Wear-Gerät angezeigt werden:

[![App-Screenshot](creating-a-watchface-images/08-app-screen.png "Bildschirm \"App\" Wear-Gerät")](creating-a-watchface-images/08-app-screen.png#lightbox)

An diesem Punkt verfügt die grundlegende Wear-app nicht gesichtserkennungs-Funktion sehen Sie sich, da eine Überwachung gesichtserkennungs-dienstimplementierung noch keine bereitgestellt wird. Dieser Dienst wird als Nächstes hinzugefügt werden.


## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android Wear implementiert ansehen Gesichtern anhand der `CanvasWatchFaceService` Klasse. `CanvasWatchFaceService` stammt aus `WatchFaceService`, die wiederum ergibt sich aus `WallpaperService` wie im folgenden Diagramm dargestellt:

[![Diagramm für Vererbung](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService-Diagramm für Vererbung")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` enthält eine geschachtelte `CanvasWatchFaceService.Engine`; es instanziiert einen `CanvasWatchFaceService.Engine` -Objekt, das von dem Zifferblatt Ihrer Apple Watch zeichnen die eigentliche Arbeit übernimmt. `CanvasWatchFaceService.Engine` stammt aus `WallpaperService.Engine` wie in der obigen Abbildung dargestellt.

In diesem Diagramm nicht angezeigt wird eine `Canvas` , `CanvasWatchFaceService` verwendet zum Zeichnen der Zifferblatt Ihrer Apple Watch &ndash; dies `Canvas` über übergeben die `OnDraw` Methode wie unten beschrieben.

In den folgenden Abschnitten wird ein benutzerdefinierte Überwachung gesichtserkennungs-Dienst erstellt werden, mit folgenden Schritten:

1.  Definieren Sie eine Klasse namens `MyWatchFaceService` abgeleitete `CanvasWatchFaceService`.

2.  In `MyWatchFaceService`, erstellen Sie eine verschachtelte Klasse namens `MyWatchFaceEngine` abgeleitete `CanvasWatchFaceService.Engine`.

3.  In `MyWatchFaceService`, implementieren Sie eine `CreateEngine` -Methode, die instanziiert `MyWatchFaceEngine` und gibt ihn zurück.

4.  In `MyWatchFaceEngine`, implementieren die `OnCreate` -Methode zum Erstellen von styl Fläche sehen Sie sich, und alle anderen Initialisierungsaufgaben ausführen.

5.  Implementieren der `OnDraw` -Methode der `MyWatchFaceEngine`. Diese Methode wird aufgerufen, wenn das watchface neu gezeichnet werden muss (d. h. *für ungültig erklärt*). `OnDraw` ist die Methode, die ansehen gesichtserkennungs-Elemente wie Stunde, Minute und Sekunde Hände zeichnet (und zeichnet).

6.  Implementieren der `OnTimeTick` -Methode der `MyWatchFaceEngine`.
    `OnTimeTick` wird mindestens einmal pro Minute (in der Umgebung und interaktiven Modus) oder wenn Datum/Uhrzeit geändert wurde aufgerufen.

Weitere Informationen zu `CanvasWatchFaceService`, finden Sie im Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) -API-Dokumentation.
Auf ähnliche Weise [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) wird erläutert, die tatsächliche Implementierung der dem Zifferblatt Ihrer Apple Watch.


### <a name="add-the-canvaswatchfaceservice"></a>Fügen Sie der CanvasWatchFaceService hinzu.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Fügen Sie eine neue Datei namens **MyWatchFaceService.cs** (in Visual Studio mit der Maustaste **WatchFace** in die **Projektmappen-Explorer**, klicken Sie auf **hinzufügen > Neues Element...** , und wählen Sie **Klasse**).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Fügen Sie eine neue Datei namens **MyWatchFaceService.cs** (in Visual Studio für Mac, mit der Maustaste der **WatchFace** Projekt, klicken Sie auf **hinzufügen > neue Datei...** , und wählen Sie **leere Klasse**).

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

`MyWatchFaceService` (abgeleitet von `CanvasWatchFaceService`) ist eine Anwendung ist "main" von dem Zifferblatt Ihrer Apple Watch. `MyWatchFaceService` implementiert nur eine Methode, `OnCreateEngine`, die instanziiert und gibt eine `MyWatchFaceEngine` Objekt (`MyWatchFaceEngine` ergibt sich aus `CanvasWatchFaceService.Engine`). Das instanziierte `MyWatchFaceEngine` Objekt zurückgegeben werden muss, als eine `WallpaperService.Engine`. Das kapseln `MyWatchFaceService` Objekt wird an den Konstruktor übergeben.

`MyWatchFaceEngine` ist die Implementierung der tatsächlichen Watch Gesicht &ndash; sie enthält den Code, der das watchface zeichnet. Er verarbeitet auch Systemereignisse wie der Bildschirm wird (ambient/interaktiven Modi Bildschirm ausschalten, usw.).


### <a name="implement-the-engine-oncreate-method"></a>Implementieren Sie die Engine OnCreate-Methode

Die `OnCreate` Methode initialisiert die Zifferblatt Ihrer Apple Watch. Fügen Sie das folgende Feld hinzu `MyWatchFaceEngine`:

```csharp
Paint hoursPaint;
```

Dies `Paint` , zeichnen Sie die aktuelle Uhrzeit auf dem Zifferblatt Ihrer Apple Watch Objekt verwendet werden. Fügen Sie die folgende Methode `MyWatchFaceEngine`:

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

`OnCreate` wird aufgerufen, kurz nachdem `MyWatchFaceEngine` gestartet wird. Richtet die `WatchFaceStyle` (die steuert, wie die Wear-Gerät mit dem Benutzer interagiert) und instanziiert die `Paint` -Objekt, das verwendet wird, die Zeit an.

Der Aufruf von `SetWatchFaceStyle` bewirkt Folgendes:

1.  Legt *Peek Modus* zu `PeekModeShort`, der bewirkt, dass Benachrichtigungen als kleine "Peek" Karten auf der Anzeige angezeigt werden.

2.  Legt die Sichtbarkeit der Hintergrund auf `Interruptive`, der bewirkt, dass des Hintergrunds einer Karte Peek, nur kurz angezeigt werden soll, wenn es sich um eine unterbrechende Benachrichtigung darstellt.

3.  Deaktiviert die Standardzeit für die Benutzeroberfläche von System aus, auf dem Zifferblatt Ihrer Apple Watch gezeichnet wird, damit die Zeit von der benutzerdefinierten watchface stattdessen angezeigt werden kann.

Weitere Informationen zu diesen und anderen überwachen Gesicht Style-Optionen finden Sie im Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) -API-Dokumentation.

Nach dem `SetWatchFaceStyle` abgeschlossen ist, `OnCreate` instanziiert die `Paint` Objekt (`hoursPaint`) und legt die Farbe Weiß und die Textgröße auf 48 Pixel fest ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) muss angegeben werden, in Pixel).


### <a name="implement-the-engine-ondraw-method"></a>Implementieren Sie die Engine OnDraw-Methode

Die `OnDraw` Methode ist vielleicht die wichtigste `CanvasWatchFaceService.Engine` Methode &ndash; ist die Methode, die tatsächlich zeichnet gesichtserkennungs-Elementen, z. B. Ziffern ansehen und Uhrzeiger Gesicht.
Im folgenden Beispiel zeichnet sie eine Uhrzeit-Zeichenfolge, auf dem Zifferblatt Ihrer Apple Watch.
Fügen Sie die folgende Methode `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

Wenn Android aufruft `OnDraw`, übergibt ein `Canvas` -Instanz und die Grenzen an, in dem das Gesicht gezeichnet werden kann. Im obigen Codebeispiel `DateTime` wird verwendet, um die aktuelle Uhrzeit in Stunden und Minuten (im 12-Stunden-Format) zu berechnen. Die resultierende Uhrzeitzeichenfolge auf der Leinwand gezeichnet wird, mithilfe der `Canvas.DrawText` Methode. Die Zeichenfolge wird 70 Pixel über aus dem linken Rand und 80 Pixel nach unten aus dem oberen Rand angezeigt.

Weitere Informationen zu den `OnDraw` -Methode finden Sie im Android [OnDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) -API-Dokumentation.


### <a name="implement-the-engine-ontimetick-method"></a>Implementieren Sie die Engine OnTimeTick-Methode

Android in regelmäßigen Abständen ruft der `OnTimeTick` Methode, um die Zeit angezeigt, indem Sie das watchface zu aktualisieren. Es wird zu aufgerufen, mindestens einmal pro Minute (im Ambiente und interaktiven Modus), oder wenn das Datum/Uhrzeit oder Zeitzone geändert haben. Fügen Sie die folgende Methode `MyWatchFaceEngine`:

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Diese Implementierung der `OnTimeTick` ruft einfach `Invalidate`. Die `Invalidate` Methode Zeitpläne `OnDraw` auf dem Zifferblatt Ihrer Apple Watch neu gezeichnet werden.

Weitere Informationen zu den `OnTimeTick` -Methode finden Sie im Android [OnTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) -API-Dokumentation.


## <a name="register-the-canvaswatchfaceservice"></a>Registrieren Sie sich die CanvasWatchFaceService

`MyWatchFaceService` muss registriert werden, der **"androidmanifest.xml"** der zugeordneten Wear-app. Zu diesem Zweck fügen Sie den folgenden XML-Code, um die `<application>` Abschnitt:

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

Dieser XML-Code führt Folgendes aus:

1.  Legt die `android.permission.BIND_WALLPAPER` Berechtigung. Diese Berechtigung ermöglicht die Überwachung Gesicht Dienst die Berechtigung zum Ändern Sie das System Hintergrundbild auf dem Gerät. Beachten Sie, die diese Berechtigung muss, in festgelegt werden der `<service>` Abschnitt und nicht in der äußeren `<application>` Abschnitt.

2.  Definiert eine `watch_face` Ressource. Diese Ressource ist eine kurze XML-Datei, die deklariert eine `wallpaper` Ressourcen (diese Datei wird im nächsten Abschnitt erstellt werden).

3.  Deklariert eine drawable Image mit dem Namen `preview` , von dem Auswahlbildschirm des Watch-Auswahl angezeigt wird.

4.  Enthält eine `intent-filter` können Sie wissen, dass Android `MyWatchFaceService` wird eine Zifferblatt Ihrer Apple Watch angezeigt werden.

Ist den Code für die grundlegende abgeschlossen `WatchFace` Beispiel. Im nächste Schritt werden die erforderlichen Ressourcen hinzufügen.


## <a name="add-resource-files"></a>Hinzufügen von Ressourcendateien

Vor dem Ausführen des Diensts überwachen, müssen Sie Hinzufügen der **Watch_face** Ressource und das Vorschaubild. Erstellen Sie zunächst eine neue XML-Datei am **Resources/xml/watch_face.xml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Legen Sie diese Datei Buildaktion auf **AndroidResource**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Buildvorgang](creating-a-watchface-images/10-android-resource-vs.png "AndroidResource Aktion erstellen")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Buildvorgang](creating-a-watchface-images/10-android-resource-xs.png "AndroidResource Aktion erstellen")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Diese Ressourcendatei definiert eine einfache `wallpaper` -Element, das für das watchface verwendet wird.

Wenn Sie noch nicht geschehen, laden Sie [preview.png](creating-a-watchface-images/preview.png).
Installieren Sie es auf **Resources/drawable/preview.png**. Achten Sie darauf, dass Sie diese Datei zum Hinzufügen der `WatchFace` Projekt. Diese Preview-Bild wird an den Benutzer in der Watch Gesicht Auswahl auf dem Wear-Gerät angezeigt. Um ein Vorschaubild für Ihre eigenen Zifferblatt Ihrer Apple Watch zu erstellen, können Sie einen Screenshot der Zifferblatt Ihrer Apple Watch nutzen, während er ausgeführt wird. (Weitere Informationen zum Abrufen von Screenshots von Wear-Geräten finden Sie unter [und Screenshots](~/android/wear/deploy-test/debug-on-device.md#screenshots)).


## <a name="try-it"></a>Versuch es!

Erstellen und Bereitstellen der app mit dem Wear-Gerät. Daraufhin sollte im Wear-app-Bildschirm wie zuvor angezeigt werden. Gehen Sie zum Aktivieren der neuen Zifferblatt Ihrer Apple Watch:

1.  Streichen Sie nach rechts, bis Sie sehen, dass den Hintergrund des Bildschirms überwachen.

2.  Berühren Sie und halten Sie an einer beliebigen Stelle im Hintergrund des Bildschirms für zwei Sekunden.

3.  Streichen Sie nach von links nach rechts, um die verschiedenen watchfaces zu durchsuchen.

4.  Wählen Sie die **Xamarin-Beispiel** Zifferblatt (auf der rechten Seite dargestellt):

    [![Watchface Auswahl](creating-a-watchface-images/11-watchface-picker.png "Wischen zum Suchen der Xamarin-Beispiel Zifferblatt Ihrer Apple Watch")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Tippen Sie auf die **Xamarin-Beispiel** Zifferblatt, um es auszuwählen.

Dadurch wird das watchface des Geräts Abnutzung des benutzerdefinierten Überwachung Gesicht Diensts bisher implementiert geändert:

[![Digitale watchface](creating-a-watchface-images/12-digital-watchface.png "benutzerdefinierte digitale überwachen auf Wear-Gerät ausgeführt wird")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Dies ist eine relativ einfache watchface, da die app-Implementierung daher minimal ist (z. B. Hintergrund Fläche sehen Sie sich nicht enthalten sind, und er ruft auch nicht `Paint` Anti-Aliasing-Methoden, um die Darstellung zu verbessern).
Jedoch es die Funktionalität auf das Notwendigste implementieren, die zum Erstellen eines benutzerdefinierten Zifferblatts erforderlich ist.

Im nächsten Abschnitt werden diese Zifferblatt Ihrer Apple Watch auf eine komplexere Implementierung aktualisiert.


## <a name="upgrading-the-watch-face"></a>Aktualisieren die Zifferblatt Ihrer Apple watch

Im weiteren Verlauf dieser exemplarischen Vorgehensweise `MyWatchFaceService` wird aktualisiert, sodass eine analoge-Stil watchface anzeigen und es wird erweitert, um weitere Funktionen zu unterstützen. Die folgenden Funktionen werden hinzugefügt, um das aktualisierte watchface zu erstellen:

1.  Gibt die Zeit mit analoge Stunde, Minute und Sekunde Händen.

2.  Reagiert auf Änderungen der Sichtbarkeit.

3.  Reagiert auf Änderungen zwischen Ambiente und interaktiven Modus.

4.  Liest die Eigenschaften des zugrunde liegenden Wear-Gerät.

5.  Aktualisiert automatisch die Zeit, wenn eine Änderung der Zeitzone stattfindet.

Laden Sie vor der Implementierung der folgenden codeänderungen, [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), entpacken Sie es und verschieben Sie die entzippten PNG-Dateien in **Ressourcen/drawable** (Überschreiben der vorherigen **preview.png**). Fügen Sie die neuen PNG-Dateien, die `WatchFace` Projekt.


### <a name="update-engine-features"></a>Update-Engine-Funktionen

Im nächsten Schritt wird die Aktualisierung **MyWatchFaceService.cs** auf eine Implementierung, die eine analoge watchface zeichnet und unterstützt neue Funktionen. Ersetzen Sie den Inhalt der **MyWatchFaceService.cs** mit der analoge Version des Codes Fläche sehen Sie sich im [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (Ausschneiden und Einfügen von dieser Quelle in die vorhandenen können  **MyWatchFaceService.cs**).

Diese Version von **MyWatchFaceService.cs** die vorhandenen Methoden mehr Code hinzugefügt, und umfasst zusätzliche überschriebene Methoden, um weitere Funktionen hinzuzufügen. Die folgenden Abschnitte enthalten eine Einführung in den Quellcode veranschaulichen.

#### <a name="oncreate"></a>OnCreate

Die aktualisierte **OnCreate** Methode konfiguriert des gesichtserkennungs-Stils sehen Sie sich wie zuvor, bietet aber einige zusätzliche Schritte erforderlich:

1.  Legt das Hintergrundbild auf dem **Xamarin_background** Ressource, die befindet **Resources/drawable-hdpi/xamarin_background.png**.

2.  Initialisiert `Paint` Objekte für das Zeichnen, die manuell für Stunde, Minute manuell und zweiten Zeigers.

3.  Initialisiert eine `Paint` -Objekt zum Zeichnen der Teilstriche Stunde entlang der Bildschirmkante dem Zifferblatt Ihrer Apple Watch.

4.  Erstellt einen Timer, Aufrufe der `Invalidate` (Neuzeichnen)-Methode so, dass der zweite Zeiger pro Sekunde neu gezeichnet wird. Beachten Sie, dass dieser Zeitgeber erforderlich ist. da `OnTimeTick` Aufrufe `Invalidate` nur einmal pro Minute.

In diesem Beispiel enthält nur einen **xamarin_background.png** image, jedoch unter Umständen möchten Sie ein anderes Hintergrundbild für jede Dichte Bildschirm erstellen, die Ihre benutzerdefinierten watchface unterstützt.

#### <a name="ondraw"></a>OnDraw

Die aktualisierte **OnDraw** Methode zeichnet eine analoge-Stil watchface verwenden die folgenden Schritte aus:

1.  Ruft die aktuelle Zeit, die jetzt in verwaltet wird eine `time` Objekt.

2.  Bestimmt die Grenzen der Zeichenoberfläche und seinen Mittelpunkt.

3.  Zeichnet den Hintergrund, skaliert, um das Gerät passen, wenn der Hintergrund gezeichnet wird.

4.  Zeichnet zwölf *Ticks* auf dem Zifferblatt der Uhr (entsprechend der Stunden auf dem Uhrenzifferblatt).

5.  Berechnet den Winkel, Drehung und Länge für jede Seite sehen Sie sich an.

6.  Zeichnet jeder Runde ansehen oberflächlich. Beachten Sie, dass der zweite Zeiger nicht gezeichnet wird, wenn die Überwachung im ambient-Modus befindet.


#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Diese Methode wird aufgerufen, um darüber zu informieren `MyWatchFaceEngine` zu den Eigenschaften des Geräts Wear (z. B. Low-Bit-ambient-Modus und Burn-in-Schutz). In `MyWatchFaceEngine`, diese Methode überprüft nur für Low--ambient-Modus Bit (im ambient niedrigen Bit-Modus, der Bildschirm unterstützt weniger Bits für jede Farbe).

Weitere Informationen zu dieser Methode finden Sie im Android [OnPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) -API-Dokumentation.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Diese Methode wird aufgerufen, wenn die Wear-Gerät betritt oder ambient-Modus verlässt. In der `MyWatchFaceEngine` -Implementierung deaktiviert das watchface Anti-Aliasing an, wenn es im ambient-Modus befindet.

Weitere Informationen zu dieser Methode finden Sie im Android [OnAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) -API-Dokumentation.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Diese Methode wird aufgerufen, wenn die Überwachung wird sichtbar oder ausgeblendet. In `MyWatchFaceEngine`, diese Methode registriert/hebt die Registrierung des Zeitzone Empfängers (siehe unten) gemäß den Sichtbarkeitszustand.

Weitere Informationen zu dieser Methode finden Sie im Android [OnVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) -API-Dokumentation.


### <a name="time-zone-feature"></a>Zeitzone-Funktion

Die neue **MyWatchFaceService.cs** enthält auch Funktionen, um die aktuelle Uhrzeit, wenn die Zeit zone Änderungen (z. B. unterwegs zwischen Zeitzonen) zu aktualisieren. Am Ende **MyWatchFaceService.cs**, eine Zeit, die Zeitzone ändern `BroadcastReceiver` wird definiert, die Zeitzone geänderten Intent-Objekte verarbeitet:

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

Die `RegisterTimezoneReceiver` und `UnregisterTimezoneReceiver` Methoden werden aufgerufen, indem die `OnVisibilityChanged` Methode.
`UnregisterTimezoneReceiver` wird aufgerufen, wenn in der Sichtbarkeitsstatus des dem Zifferblatt Ihrer Apple Watch geändert wird ausgeblendet. Wenn das watchface wieder sichtbar ist `RegisterTimezoneReceiver` aufgerufen wird (finden Sie unter den `OnVisibilityChanged` Methode).

Die Engine `RegisterTimezoneReceiver` Methode deklariert einen Handler für diese Zeitzone des Empfängers `Receive` Aufgabe, diesen Handler, aktualisiert der `time` Objekt mit der Zeit immer eine Zeitzone überschritten wird:

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Ein Zielfilter erstellt und registriert für den Empfänger Zeitzone:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

Die `UnregisterTimezoneReceiver` Methode hebt die Registrierung des Zeitzone Empfängers:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Führen Sie das verbesserte watchface

Erstellen und die app erneut mit dem Wear-Gerät bereitstellen. Wählen Sie aus der Auswahl Fläche sehen Sie sich als vor dem Zifferblatt Ihrer Apple Watch. Die Vorschau sehen Sie sich das Element in der auf der linken Seite angezeigt wird, und das neue watchface wird auf der rechten Seite angezeigt:

[![Analoge watchface](creating-a-watchface-images/13-analog-watchface.png "verbessert analoge Face in-Auswahl und auf Gerät")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

In diesem Screenshot wird der zweite Zeiger einmal pro Sekunde verschoben. Wenn Sie diesen Code auf einem Wear-Gerät ausführen, wird des zweiten Zeigers ausgeblendet, wenn es sich bei die Apple Watch ambient-Modus wechselt.


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde eine benutzerdefinierte 1.0 für Android Wear Watchface implementiert und getestet. Die `CanvasWatchFaceService` und `CanvasWatchFaceService.Engine` Klassen wurden eingeführt, und die wichtigen Methoden der Engine-Klasse zum Erstellen einer einfachen digitalen watchface implementiert wurden. Diese Implementierung mit mehr Funktionalität zum Erstellen einer analogen watchface aktualisiert wurde, und zusätzliche Methoden wurden implementiert, um die Änderungen im Sichtbarkeit, ambient-Modus und Unterschiede in den Eigenschaften des Geräts zu bewältigen. Schließlich wurde ein übertragungsempfänger Zeitzone implementiert, sodass die Überwachung die Zeit automatisch aktualisiert, wann eine Zeitzone überschritten wird.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Watchfaces](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace-Beispiel](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
