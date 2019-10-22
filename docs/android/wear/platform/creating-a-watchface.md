---
title: Erstellen eines Watch-Gesichts für Android Wear 1,0
description: In diesem Handbuch wird erläutert, wie ein benutzerdefinierter Watch-Face Service für Android Wear 1,0 implementiert wird. Eine Schritt-für-Schritt-Anleitung wird zum Erstellen eines abzurufenden Digital Watch-Gesichts dienstanweises bereitgestellt, gefolgt von mehr Code zum Erstellen einer Überwachung mit einem Analog-Stil.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/23/2018
ms.openlocfilehash: a6dfab949eb19708f69d838a7c792f2e7bbd76b3
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70758515"
---
# <a name="creating-a-watch-face"></a>Erstellen eines Zifferblatts

_In diesem Handbuch wird erläutert, wie ein benutzerdefinierter Watch-Face Service für Android Wear 1,0 implementiert wird. Eine Schritt-für-Schritt-Anleitung wird zum Erstellen eines abzurufenden Digital Watch-Gesichts dienstanweises bereitgestellt, gefolgt von mehr Code zum Erstellen einer Überwachung mit einem Analog-Stil._

## <a name="overview"></a>Übersicht

In dieser exemplarischen Vorgehensweise wird ein grundlegender Überwachungsdienst erstellt, um die Grundlagen der Erstellung eines benutzerdefinierten Android Wear 1,0-Überwachungs Gesichts zu veranschaulichen.
Der Dienst für die anfängliche Überwachung zeigt eine einfache digitale Überwachung an, auf der die aktuelle Zeit in Stunden und Minuten angezeigt wird:

[![Digitales Überwachungs Gesicht](creating-a-watchface-images/01-initial-face.png "Screenshot des ersten digitalen Überwachungs Gesichts")](creating-a-watchface-images/01-initial-face.png#lightbox)

Nachdem dieses digitale Überwachungs Gesicht entwickelt und getestet wurde, wird zusätzlicher Code hinzugefügt, um ihn auf ein ausgereifteres, analoges Überwachungs Gesicht mit drei Händen zu aktualisieren:

[![Analog ansehen](creating-a-watchface-images/02-example-watchface.png "Beispiel Bildschirm Abbildung der abschließenden Seite mit der analogen Uhr")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Überwachen der Gesichts Dienste werden gebündelt und als Teil einer Wear 1,0-App installiert. In den folgenden Beispielen enthält `MainActivity` nichts mehr als den Code aus der APP-Vorlage "Wear 1,0", sodass der Watch-Dienst verpackt und als Teil der APP auf der Smartwatch bereitgestellt werden kann. Tatsächlich dient diese APP ausschließlich als Vehikel, um den Überwachungsdienst in das Wear 1,0-Gerät (oder den Emulator) zu laden und zu Debuggen und zu testen.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um einen Watch-Gesichts Dienst zu implementieren:

- Android 5,0 (API-Ebene 21) oder höher auf dem Wear-Gerät oder-Emulator.

- Die [xamarin Android Wear-Unterstützungs Bibliotheken](https://www.nuget.org/packages/Xamarin.Android.Wear) müssen dem xamarin. Android-Projekt hinzugefügt werden.

Android 5,0 ist zwar die minimale API-Ebene für die Implementierung eines Watch-Face-Dienstanbieter, aber Android 5,1 oder höher wird empfohlen. Android Wear-Geräte, auf denen Android 5,1 (API 22) oder höher ausgeführt wird, ermöglichen es Ihnen, auf dem Bildschirm zu steuern, was auf dem Bildschirm angezeigt wird, während sich das Gerät *im Energiespar* Modus Wenn das Gerät den *Umgebungs* Modus mit geringem Energiesparmodus verlässt, befindet es sich im *interaktiven* Modus. Weitere Informationen zu diesen Modi finden [Sie unter Beibehalten der App sichtbar](https://developer.android.com/training/wearables/apps/always-on.html).

## <a name="start-an-app-project"></a>Starten eines App-Projekts

Erstellen Sie ein neues Android Wear 1,0-Projekt namens **watchface** (Weitere Informationen zum Erstellen neuer xamarin. Android-Projekte finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Dialogfeld für neues Projekt](creating-a-watchface-images/03-wear-project-vs-sml.png "Wählen Sie im Dialogfeld "Neues Projekt" die Option App")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Dialogfeld für neues Projekt](creating-a-watchface-images/03-wear-project-xs-sml.png "Wählen Sie im Dialogfeld "Neues Projekt" die Option App")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----

Legen Sie den Paketnamen auf `com.xamarin.watchface` fest:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Einstellung für Paketname](creating-a-watchface-images/04-package-name-vs.png "Legen Sie den Paketnamen auf com. xamarin. watchface fest.")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Einstellung für Paketname](creating-a-watchface-images/04-package-name-xs.png "Legen Sie den Paketnamen auf com. xamarin. watchface fest.")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Führen Sie außerdem einen Bildlauf nach unten durch, und aktivieren Sie die Berechtigungen **Internet** und **WAKE_LOCK** :

[![Erforderliche Berechtigungen](creating-a-watchface-images/05-required-permissions-vs.png "Internet-und WAKE_LOCK-Berechtigungen aktivieren")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Legen Sie die Android-Mindestversion auf **Android 5,1 (API-Ebene 22)** fest.
Aktivieren Sie außerdem die Berechtigungen **Internet** und **wakelock** :

[![Erforderliche Berechtigungen](creating-a-watchface-images/05-required-permissions-xs.png "Internet-und wakelock-Berechtigungen aktivieren")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Laden Sie als nächstes die Datei [Preview. png](creating-a-watchface-images/preview.png) herunter &ndash; die später in dieser exemplarischen Vorgehensweise dem Ordner **drawables** hinzugefügt wird.

## <a name="add-the-xamarinandroid-wear-package"></a>Hinzufügen des xamarin. Android-Wear-Pakets

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Starten Sie den nuget-Paket-Manager (Klicken Sie in Visual Studio mit der rechten Maustaste auf **Verweise** im **Projektmappen-Explorer** , und wählen Sie **nuget-Pakete verwalten...** ). Aktualisieren Sie das Projekt auf die neueste stabile Version von **xamarin. Android. Wear**:

[![Nuget-Paket-Manager hinzufügen](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Hinzufügen des xamarin. Android. Wear-Pakets")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Wenn **xamarin. Android. Support. V13** installiert ist, deinstallieren Sie es als nächstes:

[![Nuget-Paket-Manager entfernen](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin. Support. V13 entfernen")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Starten Sie den nuget-Paket-Manager (Klicken Sie in Visual Studio für Mac im Bereich Projekt Mappe mit der rechten Maustaste auf **Pakete** **, und wählen** Sie **Pakete hinzufügen**aus. Aktualisieren Sie das Projekt auf die neueste stabile Version von **xamarin. Android. Wear**:

[![Nuget-Paket-Manager hinzufügen](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Hinzufügen des xamarin. Android. Wear-Pakets")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----

Erstellen und Ausführen der APP auf einem Wear-Gerät oder-Emulator (Weitere Informationen hierzu finden Sie im Leitfaden [zu](~/android/wear/get-started/index.md) den ersten Schritten.) Auf dem Wear-Gerät sollte der folgende APP-Bildschirm angezeigt werden:

[![Screenshot der APP](creating-a-watchface-images/08-app-screen.png "App-Bildschirm auf Wear-Gerät")](creating-a-watchface-images/08-app-screen.png#lightbox)

An diesem Punkt verfügt die grundlegende Wear-APP nicht über die Funktion zum Überwachen von Gesichtspunkten, da Sie noch keine Überwachungsdienst Implementierung bereitstellt. Dieser Dienst wird als nächstes hinzugefügt.

## <a name="canvaswatchfaceservice"></a>Canvaswatchfakeservice

Android Wear implementiert Watch-Gesichter über die `CanvasWatchFaceService`-Klasse. `CanvasWatchFaceService` wird von `WatchFaceService` abgeleitet, die selbst von `WallpaperService` abgeleitet ist, wie in der folgenden Abbildung dargestellt:

[![Vererbungs Diagramm](creating-a-watchface-images/09-inheritance-diagram-sml.png "Canvaswatchfakeservice-Vererbungs Diagramm")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` enthält eine `CanvasWatchFaceService.Engine`, Sie instanziiert ein `CanvasWatchFaceService.Engine` Objekt, das die eigentliche Arbeit des Zeichnens der Überwachungs Fläche übernimmt. `CanvasWatchFaceService.Engine` wird von `WallpaperService.Engine` abgeleitet, wie in der obigen Abbildung dargestellt.

Das in diesem Diagramm nicht gezeigte `Canvas` ist eine, die `CanvasWatchFaceService` zum Zeichnen der Überwachungs Fläche verwendet, &ndash; dieser `Canvas` wie unten beschrieben über die `OnDraw`-Methode weitergegeben wird.

In den folgenden Abschnitten wird ein benutzerdefinierter Überwachungs Gesichts Dienst erstellt, indem die folgenden Schritte ausgeführt werden:

1. Definieren Sie eine Klasse mit dem Namen `MyWatchFaceService`, die von `CanvasWatchFaceService` abgeleitet ist.

2. Erstellen Sie innerhalb `MyWatchFaceService` eine geschachtelte Klasse mit dem Namen `MyWatchFaceEngine`, die von `CanvasWatchFaceService.Engine` abgeleitet ist.

3. Implementieren Sie in `MyWatchFaceService` eine `CreateEngine` Methode, die `MyWatchFaceEngine` instanziiert und zurückgibt.

4. Implementieren Sie in `MyWatchFaceEngine` die `OnCreate`-Methode, um den Stil der überwachenden Seite zu erstellen und alle anderen Initialisierungs Aufgaben auszuführen.

5. Implementieren Sie die `OnDraw`-Methode von `MyWatchFaceEngine`. Diese Methode wird immer dann aufgerufen, wenn die Überwachungs Fläche neu gezeichnet werden muss (d. h. *ungültig*). `OnDraw` ist die Methode zum Zeichnen (und Neuzeichnen) von Überwachungs Elementen, wie z. b. Stunde, Minute und Sekunde.

6. Implementieren Sie die `OnTimeTick`-Methode von `MyWatchFaceEngine`.
    `OnTimeTick` wird mindestens einmal pro Minute (sowohl im Ambient-als auch im interaktiven Modus) oder nach dem Ändern des Datums bzw. der Uhrzeit aufgerufen.

Weitere Informationen zu `CanvasWatchFaceService` finden Sie in der Android [canvaswatchfakeservice](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) -API-Dokumentation.
Ebenso erläutert [canvaswatchfakeservice. Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) die tatsächliche Implementierung der Watch-Seite.

### <a name="add-the-canvaswatchfaceservice"></a>Hinzufügen von canvaswatchfakeservice

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Fügen Sie eine neue Datei mit dem Namen **MyWatchFaceService.cs** (in Visual Studio, klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf **watchface** , klicken Sie auf **> Neues Element hinzufügen...** , und wählen Sie **Klasse**aus).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Fügen Sie eine neue Datei namens **MyWatchFaceService.cs** hinzu (Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf das **watchface** -Projekt, klicken Sie auf **> neue Datei hinzufügen...** , und wählen Sie **leere Klasse**aus).

-----

Ersetzen Sie den Inhalt dieser Datei durch den folgenden Code:

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

`MyWatchFaceService` (abgeleitet von `CanvasWatchFaceService`) ist das Hauptprogramm der Uhr. `MyWatchFaceService` implementiert nur eine Methode, `OnCreateEngine`, die ein `MyWatchFaceEngine` Objekt instanziiert und zurückgibt (`MyWatchFaceEngine` wird von `CanvasWatchFaceService.Engine` abgeleitet). Das instanziierte `MyWatchFaceEngine` Objekt muss als `WallpaperService.Engine` zurückgegeben werden. Das kapselnde `MyWatchFaceService` Objekt wird an den-Konstruktor übergeben.

`MyWatchFaceEngine` ist die tatsächliche Implementierung der Überwachungs Ansicht &ndash; Sie den Code enthält, der das Überwachungs Gesicht zeichnet. Außerdem werden Systemereignisse wie Bildschirm Änderungen (Ambient/Interactive-Modi, Bildschirm ausschalten usw.) behandelt.

### <a name="implement-the-engine-oncreate-method"></a>Implementieren der OnCreate-Methode der Engine

Die `OnCreate`-Methode initialisiert das Watch-Gesicht. Fügen Sie `MyWatchFaceEngine` das folgende Feld hinzu:

```csharp
Paint hoursPaint;
```

Dieses `Paint` Objekt wird verwendet, um die aktuelle Uhrzeit auf dem Überwachungs Gesicht zu zeichnen. Fügen Sie als nächstes `MyWatchFaceEngine` die folgende Methode hinzu:

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

`OnCreate` wird kurz nach dem Start `MyWatchFaceEngine` aufgerufen. Er richtet die `WatchFaceStyle` ein (die steuert, wie das Wear-Gerät mit dem Benutzer interagiert) und instanziiert das `Paint` Objekt, das zum Anzeigen der Uhrzeit verwendet wird.

Der `SetWatchFaceStyle`-Aufrufe führt folgende Aktionen aus:

1. Legt den *PEEK-Modus* auf `PeekModeShort` fest, wodurch Benachrichtigungen in der Anzeige als kleine "Peek"-Karten angezeigt werden.

2. Legt die Sichtbarkeit des Hintergrunds auf `Interruptive` fest, was bewirkt, dass der Hintergrund einer Peek-Karte nur kurz angezeigt wird, wenn Sie eine destruktiv Benachrichtigung darstellt.

3. Deaktiviert das Zeichnen der Standard-Benutzeroberflächen Zeit auf dem ansichtgesicht, damit das benutzerdefinierte Überwachungs Gesicht stattdessen die Zeit anzeigen kann.

Weitere Informationen zu diesen und anderen Optionen für das Überwachen von Optionen finden Sie in der API-Dokumentation zu Android [watchfakestyle. Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) .

Nachdem `SetWatchFaceStyle` abgeschlossen ist, instanziiert `OnCreate` das `Paint` Objekt (`hoursPaint`) und legt seine Farbe auf weiß und seine Textgröße auf 48 Pixel fest ([TEXTSIZE](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) muss in Pixel angegeben werden).

### <a name="implement-the-engine-ondraw-method"></a>Implementieren der OnDraw-Methode der Engine

Die `OnDraw`-Methode ist vielleicht die wichtigste `CanvasWatchFaceService.Engine` Methode &ndash; Sie ist die Methode, die tatsächlich Überwachungs Elemente wie Ziffern und Takt Gesicht zeichnet.
Im folgenden Beispiel wird eine Zeit Zeichenfolge auf dem Überwachungs Gesicht gezeichnet.
Fügen Sie `MyWatchFaceEngine` zur folgenden Methode hinzu:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

Wenn Android `OnDraw` aufruft, übergibt es eine `Canvas` Instanz und die Begrenzungen, in denen das Gesicht gezeichnet werden kann. Im obigen Codebeispiel wird `DateTime` verwendet, um die aktuelle Zeit in Stunden und Minuten (im 12-Stunden-Format) zu berechnen. Die resultierende Zeit Zeichenfolge wird mithilfe der `Canvas.DrawText`-Methode auf der Canvas gezeichnet. Die Zeichenfolge wird 70 Pixel vom linken Rand und 80 Pixel vom oberen Rand aus angezeigt.

Weitere Informationen zur `OnDraw`-Methode finden Sie in der Dokumentation zur Android [OnDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) -API.

### <a name="implement-the-engine-ontimetick-method"></a>Implementieren der ontimetick-Methode der Engine

Android ruft in regelmäßigen Abständen die `OnTimeTick` Methode auf, um die Zeit zu aktualisieren, die von der Uhr angezeigt wird Sie wird mindestens einmal pro Minute (sowohl im Ambient-als auch im interaktiven Modus) oder bei einer Änderung von Datum/Uhrzeit oder Zeitzone aufgerufen. Fügen Sie `MyWatchFaceEngine` zur folgenden Methode hinzu:

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Diese Implementierung von `OnTimeTick` einfach `Invalidate` aufruft. Die `Invalidate`-Methode plant `OnDraw`, das Überwachungs Gesicht neu zu zeichnen.

Weitere Informationen zur `OnTimeTick`-Methode finden Sie in der Dokumentation zur Android [ontimetick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) -API.

## <a name="register-the-canvaswatchfaceservice"></a>Registrieren von canvaswatchfakeservice

`MyWatchFaceService` muss in der Datei " **androidmanifest. XML** " der zugeordneten Wear-App registriert werden. Fügen Sie zu diesem Zweck dem `<application>` Abschnitt den folgenden XML-Code hinzu:

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

Dieser XML-Code führt folgende Schritte aus:

1. Legt die `android.permission.BIND_WALLPAPER` Berechtigung fest. Mit dieser Berechtigung erhält der Überwachungsdienst die Berechtigung, das System Hintergrundbild auf dem Gerät zu ändern. Beachten Sie, dass diese Berechtigung im `<service>` Abschnitt anstatt im äußeren `<application>` Abschnitt festgelegt werden muss.

2. Definiert eine `watch_face` Ressource. Diese Ressource ist eine kurze XML-Datei, die eine `wallpaper` Ressource deklariert (diese Datei wird im nächsten Abschnitt erstellt).

3. Deklariert ein drawable-Image mit dem Namen `preview`, das auf dem Auswahlbildschirm der Überwachungs Auswahl angezeigt wird.

4. Enthält eine `intent-filter`, um Android zu informieren, dass `MyWatchFaceService` ein Watch-Gesicht anzeigt.

Damit wird der Code für das grundlegende `WatchFace` Beispiel vervollständigt. Der nächste Schritt besteht darin, die erforderlichen Ressourcen hinzuzufügen.

## <a name="add-resource-files"></a>Ressourcen Dateien hinzufügen

Bevor Sie den Überwachungsdienst ausführen können, müssen Sie die Ressource **watch_face** und das Vorschaubild hinzufügen. Erstellen Sie zunächst eine neue XML-Datei unter **Resources/XML/watch_face. XML** , und ersetzen Sie deren Inhalt durch den folgenden XML-Code:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Legen Sie die Buildaktion dieser Datei auf " **androidresource**" fest:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Buildaktion](creating-a-watchface-images/10-android-resource-vs.png "Buildaktion auf "androidresource" festlegen")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Buildaktion](creating-a-watchface-images/10-android-resource-xs.png "Buildaktion auf "androidresource" festlegen")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Diese Ressourcen Datei definiert ein einfaches `wallpaper` Element, das für die Überwachung verwendet wird.

Wenn Sie dies noch nicht getan haben, laden Sie [Preview. png](creating-a-watchface-images/preview.png)herunter.
Installieren Sie Sie unter " **Resources/drawable/Preview. png**". Stellen Sie sicher, dass Sie diese Datei dem `WatchFace`-Projekt hinzufügen. Dieses Vorschaubild wird dem Benutzer in der überwachenden Gesichts Auswahl auf dem Wear-Gerät angezeigt. Zum Erstellen eines Vorschau Bilds für Ihr eigenes Watch-Gesicht können Sie einen Screenshot der Uhr sehen, während er ausgeführt wird. (Weitere Informationen zum erhalten von Screenshots von Wear-Geräten finden [Sie unter Screenshot](~/android/wear/deploy-test/debug-on-device.md#screenshots)).

## <a name="try-it"></a>Versuch es!

Erstellen Sie die APP und stellen Sie Sie auf dem Wear-Gerät bereit. Der Bildschirm zum Laden der APP sollte wie zuvor angezeigt werden. Gehen Sie folgendermaßen vor, um das neue Watch-Gesicht zu aktivieren:

1. Wischen Sie nach rechts, bis der Hintergrund des Bildschirms Bildschirms angezeigt wird.

2. Halten Sie sich für zwei Sekunden an einer beliebigen Stelle im Hintergrund des Bildschirms.

3. Wischen Sie von links nach rechts, um die verschiedenen Überwachungs Gesichter zu durchsuchen.

4. Wählen Sie die Ansicht **xamarin Sample** Watch (auf der rechten Seite angezeigt):

    [![Watchface-Auswahl](creating-a-watchface-images/11-watchface-picker.png "Zum Suchen nach xamarin Sample Watch Gesicht schwenken")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5. Tippen Sie auf die Ansicht **xamarin Sample** Watch, um Sie auszuwählen.

Dadurch wird die Watch-Oberfläche des Wear-Geräts so geändert, dass der bisher implementierte benutzerdefinierte Überwachungsdienst verwendet wird:

[![Digitales Überwachungs Gesicht](creating-a-watchface-images/12-digital-watchface.png "Benutzerdefinierte digitale Überwachung auf dem Wear-Gerät")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Dabei handelt es sich um eine relativ grobe Überwachung, da die APP-Implementierung minimal ist (z. b. enthält Sie keinen Hintergrund, und Sie ruft keine `Paint` Anti-Alias-Methoden auf, um die Darstellung zu verbessern).
Es implementiert jedoch die Bare-Fisch-Funktionalität, die zum Erstellen eines benutzerdefinierten Überwachungs Gesichts benötigt wird.

Im nächsten Abschnitt wird dieses Überwachungs Gesicht auf eine anspruchsvollere Implementierung aktualisiert.

## <a name="upgrading-the-watch-face"></a>Aktualisieren der Überwachungsseite

Im restlichen Teil dieser exemplarischen Vorgehensweise wird `MyWatchFaceService` aktualisiert, um eine Überwachungs Fläche im Analog Format anzuzeigen, und Sie wird erweitert, um weitere Funktionen zu unterstützen. Die folgenden Funktionen werden hinzugefügt, um das aktualisierte Überwachungs Gesicht zu erstellen:

1. Gibt die Uhrzeit mit der analogen Stunde, Minute und zweiten Handzeichen an.

2. Reagiert auf Änderungen der Sichtbarkeit.

3. Reagiert auf Änderungen zwischen Umgebungs Modus und interaktivem Modus.

4. Liest die Eigenschaften des zugrunde liegenden Wear-Geräts.

5. Die Zeit, zu der eine Zeit Zonen Änderung stattfindet, wird von automatisch aktualisiert.

Bevor Sie die unten aufgeführten Codeänderungen implementieren, laden Sie [drawable. zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true)herunter, entpacken Sie Sie, und verschieben Sie die entzippten PNG-Dateien in **Ressourcen/drawable** (überschreiben Sie die vorherige **Vorschau. png**). Fügen Sie die neuen PNG-Dateien dem `WatchFace`-Projekt hinzu.

### <a name="update-engine-features"></a>Features der Update-Engine

Der nächste Schritt ist das Upgrade von **MyWatchFaceService.cs** auf eine-Implementierung, die ein analoges Überwachungs Gesicht zeichnet und neue Features unterstützt. Ersetzen Sie den Inhalt von **MyWatchFaceService.cs** durch die analoge Version des Watch-Gesichts Codes in [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (Sie können diese Quelle Ausschneiden und in die vorhandene **MyWatchFaceService.cs**einfügen).

Diese Version von **MyWatchFaceService.cs** fügt den vorhandenen Methoden weiteren Code hinzu und umfasst zusätzliche überschriebene Methoden, um weitere Funktionen hinzuzufügen. Die folgenden Abschnitte bieten eine Einführung in den Quellcode.

#### <a name="oncreate"></a>OnCreate

Mit der aktualisierten **OnCreate** -Methode wird der Stil der Überwachungsseite wie zuvor konfiguriert, es sind jedoch einige zusätzliche Schritte erforderlich:

1. Legt das Hintergrundbild auf die **xamarin_background** -Ressource fest, die sich in " **Resources/drawable-hdpi/xamarin_background. png**" befindet.

2. Initialisiert `Paint`-Objekte zum Zeichnen der Stunde, der Minute und der zweiten Seite.

3. Initialisiert ein `Paint` Objekt zum Zeichnen der Stunden Ticks um den Rand der Uhr.

4. Erstellt einen Timer, der die `Invalidate` (Redraw)-Methode aufruft, damit die zweite Seite jede Sekunde neu gezeichnet wird. Beachten Sie, dass dieser Timer notwendig ist, da `OnTimeTick` Anrufe nur einmal pro Minute `Invalidate`.

Dieses Beispiel enthält nur ein **xamarin_background. png** -Bild. Möglicherweise möchten Sie jedoch ein anderes Hintergrundbild für jede Bildschirm Dichte erstellen, die Ihr benutzerdefiniertes Überwachungs Gesicht unterstützt.

#### <a name="ondraw"></a>OnDraw

Die aktualisierte **OnDraw** -Methode zeichnet mit den folgenden Schritten eine Überwachung mit dem Analog-Stil:

1. Ruft die aktuelle Zeit ab, die nun in einem `time` Objekt verwaltet wird.

2. Bestimmt die Begrenzungen der Zeichen Oberfläche und deren Mitte.

3. Zeichnet den Hintergrund, der auf das Gerät skaliert wird, wenn der Hintergrund gezeichnet wird.

4. Zeichnet zwölf *Ticks* um das Gesicht der Uhr (entsprechend den Stunden der Takt Fläche).

5. Berechnet den Winkel, die Drehung und die Länge für jede Überwachungs Hand.

6. Zeichnet jede Hand auf der überwachenden Oberfläche. Beachten Sie, dass die zweite Hand nicht gezeichnet wird, wenn sich die Überwachung im Umgebungs Modus befindet.

#### <a name="onpropertieschanged"></a>Onpropertieschge

Diese Methode wird aufgerufen, um `MyWatchFaceEngine` über die Eigenschaften des Wear-Geräts (z. b. den Low-Bit-Umgebungs Modus und den Burn-in-Schutz) zu informieren. In `MyWatchFaceEngine` prüft diese Methode nur den Modus mit geringem Bit-Ambient (im unteren Umgebungs Modus unterstützt der Bildschirm weniger Bits für jede Farbe).

Weitere Informationen zu dieser Methode finden Sie in der Dokumentation zur Android [onpropertieschge](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) -API.

#### <a name="onambientmodechanged"></a>Onambientmodechanged

Diese Methode wird aufgerufen, wenn das Wear-Gerät in den Umgebungs Modus wechselt oder Sie verlässt. In der `MyWatchFaceEngine`-Implementierung deaktiviert das Überwachungs Gesicht das Antialiasing, wenn es sich im Umgebungs Modus befindet.

Weitere Informationen zu dieser Methode finden Sie in der API-Dokumentation zu Android [onambientmodechanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) .

#### <a name="onvisibilitychanged"></a>Onvisibilitychanged

Diese Methode wird immer dann aufgerufen, wenn die Überwachung sichtbar oder ausgeblendet wird. In `MyWatchFaceEngine` registriert diese Methode den Zeit Zonen Empfänger (unten beschrieben) gemäß dem Sichtbarkeits Zustand und hebt die Registrierung auf.

Weitere Informationen zu dieser Methode finden Sie in der Android [onvisibilitychanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) -API-Dokumentation.

### <a name="time-zone-feature"></a>Zeit Zonen Feature

Die neue **MyWatchFaceService.cs** enthält auch Funktionen zum Aktualisieren der aktuellen Zeit, wenn sich die Zeitzone ändert (z. b. während der Übertragung zwischen Zeitzonen). Am Ende der **MyWatchFaceService.cs**wird ein Zeit Zonen Änderungs `BroadcastReceiver` definiert, der die für die Zeit Zonen Änderung vorgesehenen Intent-Objekte behandelt:

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

Die Methoden `RegisterTimezoneReceiver` und `UnregisterTimezoneReceiver` werden von der `OnVisibilityChanged`-Methode aufgerufen.
`UnregisterTimezoneReceiver` wird aufgerufen, wenn der Sichtbarkeits Zustand der Überwachungs Fläche in ausgeblendet geändert wird. Wenn das Überwachungs Gesicht wieder sichtbar ist, wird `RegisterTimezoneReceiver` aufgerufen (siehe `OnVisibilityChanged`-Methode).

Die Engine `RegisterTimezoneReceiver`-Methode deklariert einen Handler für das `Receive` Ereignis dieses Zeit Zonen Empfängers. Dieser Handler aktualisiert das `time` Objekt mit dem neuen Zeitpunkt, wenn eine Zeitzone überschritten wird:

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Ein Intent-Filter wird erstellt und für den Zeit Zonen Empfänger registriert:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

Durch die `UnregisterTimezoneReceiver`-Methode wird die Registrierung des Zeit Zonen Empfängers aufgehoben:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Ausführen der verbesserten Überwachungs Fläche

Erstellen Sie die APP erneut auf dem Wear-Gerät Wählen Sie das Überwachungs Gesicht aus der Ansicht Gesichts Auswahl wie zuvor aus. Die Vorschau in der Überwachungs Auswahl wird auf der linken Seite angezeigt, und die neue Seite "überwachen" wird auf der rechten Seite angezeigt:

[![Analog ansehen](creating-a-watchface-images/13-analog-watchface.png "Verbessertes analoges Gesicht in Auswahl und auf Gerät")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

In diesem Screenshot wird die zweite Seite einmal pro Sekunde verschoben. Wenn Sie diesen Code auf einem Wear-Gerät ausführen, verschwindet die zweite Hand, wenn die Überwachung in den Umgebungs Modus wechselt.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde ein benutzerdefiniertes Android Wear 1,0 watchface implementiert und getestet. Die Klassen `CanvasWatchFaceService` und `CanvasWatchFaceService.Engine` wurden eingeführt, und die wesentlichen Methoden der Engine-Klasse wurden implementiert, um ein einfaches digitales Überwachungs Gesicht zu erstellen. Diese Implementierung wurde mit mehr Funktionalität aktualisiert, um eine analoge Überwachungs Fläche zu erstellen, und es wurden zusätzliche Methoden implementiert, um Änderungen an der Sichtbarkeit, dem Umgebungs Modus und den Unterschieden in den Geräteeigenschaften zu verarbeiten. Schließlich wurde ein Zeit Zonen Broadcast-Empfänger implementiert, sodass die Überwachung automatisch den Zeitpunkt aktualisiert, zu dem eine Zeitzone überschritten wird.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Überwachungs Flächen](https://developer.android.com/training/wearables/watch-faces/index.html)
- [Watchface-Beispiel](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)
- [Watchfakeservice. Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
