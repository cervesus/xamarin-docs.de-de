---
title: KitKat Features
description: "Android 4.4 (KitKat) wird mit einem Cornucopia Funktionen für Benutzer und Entwickler. Dieses Handbuch beschreibt einige dieser Funktionen und bietet Codebeispiele und Details zur Implementierung, um das beste aus KitKat treffen zu können."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: ae6b89e48005ca028db5d13f1a55f237888ae08b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="kitkat-features"></a>KitKat Features

_Android 4.4 (KitKat) wird mit einem Cornucopia Funktionen für Benutzer und Entwickler. Dieses Handbuch beschreibt einige dieser Funktionen und bietet Codebeispiele und Details zur Implementierung, um das beste aus KitKat treffen zu können._

## <a name="overview"></a>Übersicht

Android 4.4 (API-Ebene 19), auch bekannt als "KitKat", wurde in spät 2013 veröffentlicht. KitKat bietet eine Vielzahl von neuen Features und Verbesserungen, darunter:

-  [Benutzererfahrung](#user_experience) &ndash; einfache Animationen mit Übergang Framework, transparent Status sowie die Navigation Balken und beeindruckende Vollbildmodus unterstützen Benutzerfreundlicher für den Benutzer zu erstellen.

-  [Benutzerinhalt](#user_content) &ndash; dateiverwaltung Benutzer vereinfacht wird, mit dem Speicher Zugriff Framework; Drucken von Bildern, Websites und andere Inhalte wird durch eine verbesserte drucken APIs erleichtert.

-  [Hardware](#hardware) &ndash; wandeln Sie jede app in einem NFC-Karte mit NFC hostbasierte Karte Emulation; führen Sie LP-Sensoren mit der `SensorManager` .

-  [Entwicklertools](#developer_tools) &ndash; Screencast Anwendungen in Aktion mit dem Android Debug Bridge-Client verfügbar als Teil des Android SDKS.


Dieses Handbuch enthält Anweisungen zum Migrieren einer vorhandenen Anwendung von Xamarin.Android KitKat als auch einen allgemeinen Überblick über die KitKat für Xamarin.Android-Entwickler.

## <a name="requirements"></a>Anforderungen

Um mit KitKat Xamarin.Android-Anwendungen entwickeln, müssen Sie *Xamarin.Android 4.11.0* oder höher sowie Android 4.4 (API-Ebene 19) über den Android SDK Manager installiert, wie der folgende Screenshot veranschaulicht:

[![Android 4.4 auswählen im Android SDK-Manager](kitkat-images/api19.png)](kitkat-images/api19.png)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Migrieren Ihrer App auf KitKat

Dieser Abschnitt enthält einige Elemente der ersten "Anforderungsantwort" Übergang von vorhandenen Anwendungen auf Android 4.4 helfen.

### <a name="check-system-version"></a>Überprüfen Sie die Betriebssystemversion

Wenn eine Anwendung für die Kompatibilität mit älteren Versionen von Android muss, achten Sie darauf, dass Sie alle KitKat-spezifischen Code in eine versionsüberprüfung System zu umschließen, wie im folgenden Codebeispiel veranschaulicht:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Alarm Batchverarbeitung

Android verwendet Alarm-Dienste, um eine app im Hintergrund zu einem bestimmten Zeitpunkt zu aktivieren. KitKat wird dies noch einen Schritt weiter durch die Batchverarbeitung Alarme, um Power beizubehalten. Dies bedeutet, dass anstelle von reaktivieren jede app einem genauen Zeitpunkt an, KitKat bevorzugt, um mehrere Anwendungen zu gruppieren, die registriert sind, während das gleiche Zeitintervall Wake-on, und reaktivieren sie zur gleichen Zeit.
Aufrufen, um Android eine app während eines bestimmten Zeitintervalls aktiviert aufzufordern, `SetWindow` auf die [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/), und übergeben Sie die minimale und maximale Zeit in Millisekunden, die verstreichen kann, bevor die app reaktiviert ist, und den auszuführenden Vorgang Bei der Reaktivierung.
Der folgende Code bietet ein Beispiel für eine Anwendung, die reaktiviert werden, zwischen eine halbe Stunde und einer Stunde ab dem Zeitpunkt des Fensters festgelegt ist:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Verwenden Sie zum Fortsetzen des Vorgangs reaktivieren eine app zu einem genauen Zeitpunkt `SetExact`, und übergeben Sie die genaue Zeit, die die app reaktiviert werden sollen, und den auszuführenden Vorgang:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat können nicht mehr einen genaue wiederholten Alarm festgelegt. Anwendungen, die [ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/) und erfordern genaue Alarme werden jetzt müssen jedes Alarm ausgelöst wird, manuell zu arbeiten.

### <a name="external-storage"></a>Externer Speicher

Externe Speicherung ist jetzt in zwei Arten - Speicher nur für die Anwendung und Daten, die von mehreren Anwendungen gemeinsam genutzt unterteilt. Lesen und Schreiben in bestimmten Speicherort Ihrer app auf externen speichern, benötigt keine besonderen Berechtigungen. Interagieren mit Daten in einem freigegebenen Speicher jetzt erfordert die `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` Berechtigung. Die beiden Typen können als solche klassifiziert werden:

-  Wenn Sie eine Datei- oder Verzeichnispfad durch Aufrufen einer Methode auf der Information erhalten `Context` – z. B. [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/) oder [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - Ihre app benötigt keine zusätzlichen Berechtigungen.

-  Wenn Sie eine Datei- oder Verzeichnispfad Information erhalten, durch den Zugriff auf eine Eigenschaft oder das Aufrufen einer Methode für `Environment` , wie z. B. [ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/) oder [ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) , Ihre app benötigt die `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` Berechtigung.

> [!NOTE]
> **Hinweis:** `WRITE_EXTERNAL_STORAGE` impliziert die `READ_EXTERNAL_STORAGE` Berechtigung, damit Sie immer nur eine Berechtigung festlegen müssen.

### <a name="sms-consolidation"></a>SMS-Konsolidierung

KitKat vereinfacht messaging für den Benutzer durch alle SMS-Inhalt in ein Standard-Anwendung, die vom Benutzer ausgewählten aggregieren. Der Entwickler ist dafür verantwortlich, machen die app als der Standard-messaging-Anwendung ausgewählt werden und verhält sich entsprechend in Code und Lebensdauer bei die Anwendung nicht aktiviert ist. Weitere Informationen zum Wechsel Ihrer app SMS KitKat finden Sie in der [des SMS-Apps bereit für KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Handbuch von Google.

### <a name="webview-apps"></a>WebView-Apps

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) eine Webseminar in KitKat erhalten. Die größte Änderung ist, soll zusätzliche Sicherheit für das Laden von Inhalt in einem `WebView`. Während die meisten Anwendungen, die für ältere Versionen des API wie erwartet funktioniert, Testen von Anwendungen, mit denen die `WebView` Klasse wird dringend empfohlen. Weitere Informationen zu den betroffenen WebView-APIs finden Sie in der Android [Migrieren zu in Android 4.4 WebView](http://developer.android.com/guide/webapps/migrating.html) Dokumentation.

<a name="user_experience" />

## <a name="user-experience"></a>Benutzerfreundlichkeit

Im Lieferumfang von KitKat sind mehrere neue APIs zur Verbesserung der benutzerfreundlichkeit, einschließlich des neuen Übergang-Frameworks für die Behandlung Eigenschaftenanimationen und eine transparente UI-Option für Designs. Diese Änderungen werden im folgenden erläutert.

### <a name="transition-framework"></a>Transition-Framework

Der Übergang Framework erleichtert die Animationen, implementieren. Führen Sie eine einfache Eigenschaft Animation mit nur einer Zeile des Codes, oder passen Sie mithilfe von Übergängen KitKat ermöglicht *Szenen*.

#### <a name="simple-property-animation"></a>Einfache Eigenschaft Animation

Die neue Android Übergänge Bibliothek vereinfacht den Code hinter der Eigenschaftenanimationen. Das Framework können Sie einfache Animationen mit minimalem Codeeinsatz durchzuführen. Im folgenden Codebeispiel beispielsweise Beispiel verwendet [ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/) zu animierende anzeigen und Ausblenden einer `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

Im obigen Beispiel verwendet das Framework der Übergang zum Erstellen einer automatisch standardmäßige Übergang zwischen der Änderung der Eigenschaftswerte an. Da die Animation auf eine einzige Codezeile behandelt wird, können Sie mühelos vornehmen dieser kompatibel mit früheren Versionen von Android durch wrapping der `BeginDelayedTransition` in eine versionsüberprüfung System aufrufen. Finden Sie unter der [Migrieren Ihrer App zu KitKat](#Migrating_Your_App_to_KitKat) Abschnitt Weitere.

Der folgende Screenshot zeigt die app vor der Animation an:

[![Bevor die Animation startet die App-screenshot](kitkat-images/trans-before.png)](kitkat-images/trans-before.png)

Der folgende Screenshot zeigt die app nach der Animation an:

[![App-Screenshot nach Animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png)

Sie erhalten mehr Kontrolle über den Übergang mit Szenen, die im nächsten Abschnitt behandelt werden.

#### <a name="android-scenes"></a>Android Scenes

[Szenen](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) wurden als Teil des Übergangs-Framework so erteilen Sie dem Entwickler mehr Kontrolle über die Animationen eingeführt. Szenen erstellen einen dynamischen Bereich in der Benutzeroberfläche: Sie geben Sie einen Container und mehrere Versionen oder "Hintergrund" für den XML-Inhalt innerhalb des Containers und Android ist für den Rest der Arbeit die Übergänge zwischen den Kulissen animiert. Android Szenen ermöglichen Ihnen das Erstellen von komplexer Animationen mit minimalem Aufwand auf der Seite für die Entwicklung.

Das statische Element der Benutzeroberfläche Gehäuse dynamischen Inhalt bezeichnet wird eine *Container* oder *Szene Basis*. Im folgenden Beispiel wird die Android-Designer zum Erstellen einer `RelativeLayout` aufgerufen `container`:

[![Mithilfe der Android-Designer zum Erstellen eines Containers RelativeLayout](kitkat-images/container.png)](kitkat-images/container.png)

Das Beispiel-Layout definiert auch eine Schaltfläche namens `sceneButton` unterhalb der `container`. Diese Schaltfläche wird den Übergang auslösen.

Die dynamische Inhalte innerhalb des Containers erfordert zwei neue Android Layouts. Geben Sie nur den Code für diese Layouts *in* den Container.
Der nachfolgende Code definiert ein Layout mit der Bezeichnung *Scene1* , zwei Textfelder lesen "Kit" und "Kat" bzw. enthält, und eine zweite Layout bezeichnet *Scene2* , enthält die gleichen Textfelder rückgängig gemacht. Der XML-Code lautet wie folgt:

 **Scene1.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

Im Beispiel oben verwendet `merge` stellen die kürzere Code anzeigen und vereinfachen die Hierarchie anzeigen. Sie können erfahren Sie mehr über `merge` Layouts [hier](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Eine Szene wird erstellt, durch den Aufruf [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/), und übergeben Sie die Container-Objekt, die Ressourcen-ID der Szene Layoutdatei und das aktuelle `Context`, wie im folgenden Codebeispiel veranschaulicht:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Klicken Sie auf die Schaltfläche spiegelt zwischen den zwei Szenen, die mit den Standardwerten für den Übergang Android animiert wird:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Der folgende Screenshot zeigt der Szene haben, bevor die Animation:

[![Screenshot der app, bevor die Animation wird gestartet.](kitkat-images/trans-after.png)](kitkat-images/trans-after.png)

Der folgende Screenshot zeigt der Szene haben nach der Animation an:

[![Screenshot der app, die nach Ende der Animation](kitkat-images/scene.png)](kitkat-images/scene.png)


> [!NOTE]
> **Hinweis:** besteht eine [bekannter Fehler](https://code.google.com/p/android/issues/detail?id=62450) in Android Übergänge Bibliothek, die im Hintergrund führt dazu, dass mit erstellt `GetSceneForLayout` zu unterbrechen, wenn ein Benutzer über eine Aktivität ein zweites Mal navigiert. Wird eine problemumgehung Java beschrieben [hier](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Benutzerdefinierte Übergänge im Hintergrund

Kann ein benutzerdefinierter Übergang definiert werden, in einer XML-Ressourcendatei in die `transition` Verzeichnis unter `Resources`, wie der folgende Screenshot veranschaulicht:

[![Speicherort der Datei transition.xml Ressourcen/Übergang-Verzeichnis](kitkat-images/resources.png)](kitkat-images/resources.png)

Im folgenden Codebeispiel definiert einen Übergang, die eine Animation 5 Sekunden lang und verwendet die [überschritten haben Interpolators](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Der Übergang wird erstellt, in der Aktivität mithilfe der [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/), wie in den folgenden Code veranschaulicht:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Der neue Zustandsübergang wird dann hinzugefügt, um die `Go` aufrufen, die die Animation beginnt:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Translucent UI

KitKat bietet mehr Designumgebung Ihrer app mit optionalen Transclucent Balken für Status und die Navigation zu steuern. Sie können die Lichtdurchlässigkeit eines System-UI-Elemente in der gleichen XML-Datei ändern, die Sie verwenden, um Ihr Android-Designs definieren. KitKat führt die folgenden Eigenschaften:

-  `windowTranslucentStatus` – Wenn festlegen auf "true" werden oben auf der Statusleiste transparent.

-  `windowTranslucentNavigation` – Wenn festlegen auf "true" werden die untere Navigationsleiste transparent.

-  `fitsSystemWindows` -Die Top- oder Bottom-Leiste auf Transcluent festlegen verschiebt Inhalt unter der transparente Benutzeroberflächenelemente standardmäßig. Wenn diese Eigenschaft auf `true` ist eine einfache Möglichkeit, um zu verhindern, dass Inhalt Überlappung mit den durchsichtigen System UI-Elementen.


Der folgende Code definiert ein Design mit durchsichtigen Status sowie die Navigation Balken:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

Der folgende Screenshot zeigt das Design oben durchsichtigen Status und Navigationsleisten:

[![Beispiel-Screenshot der app durchsichtigen Balken für Status und navigation](kitkat-images/theme.png)](kitkat-images/theme.png)

<a name="user_content" />

## <a name="user-content"></a>Benutzerinhalt

### <a name="storage-access-framework"></a>Speicherzugriffs Framework

Der Speicher Zugriff Framework (SAF) ist eine neue Möglichkeit für Benutzer für die Interaktion mit gespeicherten Inhalten wie Bilder, Videos und Dokumente. Statt Benutzer ein Dialogfeld zum Auswählen einer Anwendung zur Handhabung von Inhalt, öffnet KitKat eine neue Benutzeroberfläche, die Benutzern ermöglicht, ihre Daten in einem aggregieren Speicherort zugreifen. Sobald Inhalt ausgewählt wurde, der Benutzer zurück an die Anwendung, die der Inhalt angefordert, und die app-Erfahrung weiterhin wie gewohnt.

Diese Änderung erfordert zwei Aktionen auf der Seite für Entwickler: apps, die Inhalte von Anbietern erfordern zunächst auf eine neue Art der Reqesting Inhalt aktualisiert werden. Zweitens Anwendungen, die Daten geschrieben werden sollen eine `ContentProvider` muss geändert werden, um das neue Framework zu verwenden. Beide Szenarien hängen von der neuen [ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) API.

#### <a name="documentsprovider"></a>DocumentsProvider

In KitKat Interaktionen mit `ContentProviders` abstrahiert werden, mit der `DocumentsProvider` Klasse. Dies bedeutet, dass SAF care nicht, in dem die Daten physisch, werden, solange Sie darauf zugreifen können, über die `DocumentsProvider` API. Lokale Anbieter cloud-Dienste und externe Speichergeräte alle verwenden die gleiche Schnittstelle und die gleiche Weise behandelt der Benutzer und Entwickler mit einem Ort für die Interaktion mit dem Inhalt des Benutzers bereitstellen.

In diesem Abschnitt wird beschrieben, wie zum Laden und Speichern von Inhalt mit dem Speicher-Access-Framework.

#### <a name="request-content-from-a-provider"></a>Inhalt von einem Anbieter anfordern

Wir erkennen KitKat, dass wir mithilfe der SAF UI mit Inhalt auswählen möchten die `ActionOpenDocument` beabsichtigte Lesevorgänge, was bedeutet, dass wir alle Inhaltsanbietern verfügbar, auf dem Gerät eine Verbindung herstellen möchten. Sie können einige Filtern zu diesem Zweck durch Angabe hinzufügen `CategoryOpenable`, also nur Inhalt, der (d. h. zugegriffen werden kann, verwendbaren Inhalt) geöffnet werden kann zurückgegeben werden. KitKat auch ermöglicht das Filtern von Inhalten mit der `MimeType`. Z. B. der Code unten Filter für Image-Ergebnisse, indem Sie das Bild angeben `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Aufrufen von `StartActivityForResult` startet die SAF UI, die der Benutzer dann navigieren kann, um ein Bild auszuwählen:

[![Beispiel-Screenshot, der eine app mithilfe des Speicher-Access-Frameworks für die Navigation zu einem Bild](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png)

Nachdem der Benutzer ein Bild ausgewählt hat `OnActivityResult` gibt die `Android.Net.Uri` der ausgewählten Datei. Das folgende Codebeispiel zeigt Bildauswahl des Benutzers:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>Schreiben von Inhalt an einen Anbieter

Zusätzlich zum Laden des Inhalts aus der SAF UI, KitKat können Sie auch Inhalte in alle speichern `ContentProvider` , implementiert die `DocumentProvider` API. Speichern von Inhalt verwendet ein `Intent` mit `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Im obigen Beispiel lädt der SAF UI, sodass der Benutzers den Dateinamen ändern, und wählen Sie ein Verzeichnis aus, um die neue Datei gespeichert:

[![Screenshot des Benutzers ändern den Dateinamen in NewDoc im Downloads-Verzeichnis](kitkat-images/saf-save.png)](kitkat-images/saf-save.png)

Wenn der Benutzer drückt **speichern**, `OnActivityResult` weitergegeben wird die `Android.Net.Uri` der neu erstellten Datei, die mit zugegriffen werden kann `data.Data`. Der Uri kann zum Streamen von Daten in die neue Datei verwendet werden:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Beachten Sie, dass [ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri)) gibt eine `System.IO.Stream`, sodass der gesamte streaming Prozess in .NET geschrieben werden kann.

Weitere Informationen zu laden, erstellen und Bearbeiten von Inhalten mit dem Speicher-Access-Framework finden Sie auf der [Android Dokumentation zu Storage Access Framework](http://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Drucken

Drucken-Inhalt wird durch die Einführung des in KitKat vereinfacht die [Druckdienste](https://developer.xamarin.com/api/namespace/Android.PrintServices/) und `PrintManager`. KitKat ist auch die erste API-Version vollständig nutzen die [Google Cloud Druckdienst APIs](https://developers.google.com/cloud-print/) mithilfe der [Google Cloud drucken Anwendung](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
Die meisten Geräte, die automatisch mit KitKat ausgelieferten Drucken von Google Cloud-app herunterladen und die [HP Print-Dienst-Plug-Ins](https://play.google.com/store/apps/details?id=com.hp.android.printservice)Wenn sie zunächst eine Verbindung mit WiFi. Ein Benutzer kann sein eigenes Gerät drucken Einstellungen überprüfen, indem Sie navigieren zu **Einstellungen > System > Drucken**:

[![Beispiel-Screenshot des Bildschirms Einstellungen drucken](kitkat-images/printing.png)](kitkat-images/printing.png)


> [!NOTE]
> **Hinweis:** zwar die Drucken-APIs so eingerichtet sind, arbeiten mit Google Cloud Drucken standardmäßig, Android weiterhin ermöglicht Entwicklern das Druckinhalte mithilfe der neuen APIs vorbereiten und senden Sie es für andere Anwendungen, Drucken zu behandeln.



#### <a name="printing-html-content"></a>Drucken von HTML-Inhalt

KitKat erstellt automatisch eine [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) für eine Webansicht mit `WebView.CreatePrintDocumentAdapter`. Drucken von Webinhalten ist koordiniert zwischen einer [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) , wartet, bis der HTML-Inhalt zu laden und können die Aktivität, die wissen, um die Option "Drucken" im Menü "Optionen" verfügbar machen und die Actvity, der wartet, für den Benutzer Wählen Sie die Option "Drucken" und die Aufrufe `Print`auf die `PrintManager`. Dieser Abschnitt behandelt die grundlegenden Setup erforderlich, um auf dem Bildschirm drucken HTML-Inhalt.

Beachten Sie, dass die Internet-Berechtigung erforderlich, laden und Drucken von Webinhalten ist:

[![Internet-Berechtigung festgelegt in den app-Optionen.](kitkat-images/internet.png)](kitkat-images/internet.png)

##### <a name="print-menu-item"></a>Menüelements Druck

Die Option "Drucken" erscheint in der Regel in der Aktivitätssymbols [Menü "Optionen"](http://developer.android.com/guide/topics/ui/menus.html#options-menu).
Menü "Optionen" ermöglicht Benutzern das Ausführen von Aktionen für eine Aktivität. Es befindet sich in der oberen rechten Ecke des Bildschirms, und sieht wie folgt aus:

[![Beispiel-Screenshot des drucken Menü Element angezeigt, in der oberen rechten Ecke des Bildschirms](kitkat-images/menu.png)](kitkat-images/menu.png)


Zusätzliche Menüelemente können definiert werden, der *Menü*Verzeichnis unter *Ressourcen*. Der folgende Code definiert ein Datenelement namens Beispiel-Menü [Drucken](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Interaktion mit dem Menü "Optionen" in der Aktivität erfolgt über die `OnCreateOptionsMenu` und `OnOptionsItemSelected` Methoden.
`OnCreateOptionsMenu` bietet die Möglichkeit zum Hinzufügen neue Menüelemente, wie die Option Drucken aus der *Menü* Ressourcenverzeichnis.
`OnOptionsItemSelected` überwacht, die für den Benutzer, die im Kontextmenü die Option Drucken auswählen, und Drucken beginnt:

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

Der obige Code definiert außerdem eine Variable namens `dataLoaded` zum Nachverfolgen den Status des HTML-Inhalts. Die `WebViewClient` wird setzen diese Variable auf "true", wenn alle Inhalte geladen, damit die Aktivität weiß, dass er das Drucken-Menüelement im Menü "Optionen" hinzufügen.

##### <a name="webviewclient"></a>WebViewClient

Die Aufgabe von der `WebViewClient` besteht darin sicherzustellen, dass Daten in der `WebView` vollständig geladen ist, bevor die Option "Drucken" klicken Sie im Menü angezeigt, muss wird mit der `OnPageFinished` Methode. `OnPageFinished` überwacht von Webinhalten auf geladen wurden, und weist die Aktivität die Menü "Optionen", mit neu erstellen `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` Zudem wird die `dataLoaded` Wert `true`, sodass `OnCreateOptionsMenu` können erneut erstellen, klicken Sie im Menü mit der Option "Drucken" vorhanden.

##### <a name="printmanager"></a>PrintManager

Im folgenden Codebeispiel wird der Inhalt des gedruckt eine `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` als Argumente akzeptiert: einen Namen für den Auftrag ("MyWebPage" in diesem Beispiel), eine [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) , die zu druckenden Dokument aus dem Inhalt generiert und [ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null` in der Beispiel oben). Sie können angeben, `PrintAttributes` helfen gedruckte Seite Inhaltslayout, obwohl die meisten Scenarions die Standardattribute behandelt werden sollen.

Aufrufen von `Print` lädt die print-Benutzeroberfläche, der Optionen für den Druckauftrag aufgelistet sind. Die Benutzeroberfläche ermöglicht Benutzern die Möglichkeit, drucken oder speichern die HTML-Inhalt in PDF-Datei, wie in den nachstehenden Screenshots veranschaulicht:

[![Bildschirmabbildung von PrintHtmlActivity anzeigen im drucken](kitkat-images/print1.png)](kitkat-images/print1.png)

[![Screenshot der PrintHtmlActivity speichern als PDF-Menü anzeigen](kitkat-images/print2.png)](kitkat-images/print2.png)

<a name="hardware" />

## <a name="hardware"></a>Hardware

KitKat fügt mehrere APIs verwendet, um neue Gerätefunktionen aufzunehmen. Der wichtigste davon sind hostbasierte Karte Emulation und der neuen `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Host-basierte Karte Emulation in NFC

Host-basierte Karte Emulation (HCE) können Anwendungen verhält sich wie NFC-Karten oder NFC-Lesegeräte ohne Rückgriff auf der Netzbetreiber proprietären Secure-Element. Stellen Sie vor dem Einrichten der HCE, sicher HCE steht auf dem Gerät mit `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE erfordert, dass die HCE-Funktion und die `Nfc` Berechtigung mit der Anwendungsverzeichnis registriert werden `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Die NFC-Berechtigung festgelegt in den app-Optionen.](kitkat-images/nfc.png)](kitkat-images/nfc.png)

Um zu arbeiten, HCE im Hintergrund ausgeführt werden muss, und er muss zu starten, wenn der Benutzer eine Transaktion NFC macht, selbst wenn die Anwendung mit HCE nicht ausgeführt wird. Hierfür können wir das Schreiben des Codes HCE als eine `Service`. Ein Dienst HCE implementiert die `HostApduService` -Schnittstelle, die die folgenden Methoden implementiert:

-  *ProcessCommandApdu* -ein Application Protocol Data Unit (APDU) wird zwischen der NFC-Reader und dem HCE-Dienst gesendet. Diese Methode nutzt ein ADPU aus dem Reader und gibt eine Dateneinheit als Antwort zurück.

-  *OnDeactivated* : die `HostAdpuService` wird deaktiviert, wenn der Dienst HCE nicht mehr mit der NFC-Reader kommuniziert.


Ein Dienst HCE muss auch Manifest der Anwendung registriert werden, und mit den richtigen Berechtigungen, die beabsichtigte Filter und die Metadaten ergänzt. Der folgende Code ist ein Beispiel für eine `HostApduService` registriert, mit der Android-Manifest mit der `Service` Attribut (Weitere Informationen zu Attributen finden Sie in der Xamarin [arbeiten mit Android-Manifest](~/android/platform/android-manifest.md) Guide):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Der oben genannten Dienst bietet eine Möglichkeit der NFC-Reader, der mit der Anwendung interagieren, aber der NFC-Reader hat noch keine Möglichkeit zu wissen, ob dieser Dienst die NFC-Karte emuliert Scannen benötigten. Zum unterstützen des NFC-Readers, den Dienst zu identifizieren, wir können zuweisen dem Dienst eine eindeutige *Anwendung-ID (AID)*. Wir geben URSACHENANALYSE, zusammen mit anderen Metadaten über den Dienst HCE in einer XML-Ressourcendatei registriert die `MetaData` Attribut (Siehe obigen Codebeispiel). Diese Ressourcendatei gibt ein oder mehrere AID Filter - Bezeichnerzeichenfolgen im Hexadezimalformat angegeben, die die Unterstützung von Lesegeräten für eine oder mehrere NFC entsprechen:

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

Zusätzlich zur Hilfe Filter, die XML-Ressourcendatei enthält auch eine benutzerseitige-Beschreibung des Diensts HCE gibt eine URSACHENANALYSE-Gruppe (Payment-Anwendung im Vergleich zu "andere") und sich für eine Zahlung-Anwendung ein 260 x 96-dp-Banner, die dem Benutzer angezeigt.

Die oben beschriebene Setup bietet die grundlegenden Bausteine für eine Anwendung eine NFC-Karte emulieren. NFC selbst erfordert einige zusätzliche Schritte und weitere Tests, um zu konfigurieren. Weitere Informationen zu Host-basierte Karte Emulation, finden Sie in der [Android dokumentationsportal](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Weitere Informationen zur Verwendung von NFC mit Xamarin sehen Sie sich die [Xamarin NFC Beispiele](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Sensoren

KitKat ermöglicht den Zugriff auf das Gerät Sensoren über eine [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/).
Die `SensorManager` ermöglicht das Betriebssystem der Übermittlung der Sensorinformationen zu einer Anwendung in Batches Akkulaufzeit beibehalten.

KitKat umfasst auch zwei neue Sensor-Typen für das Nachverfolgen des Benutzers Schritte. Diese basieren auf den Beschleunigungsmesser und umfassen:

-  *StepDetector* -App wird benachrichtigt/reaktiviert, wenn der Benutzer einen Schritt nimmt und die Erkennung von einen Zeitwert bietet für der Schritt des Auftretens.

-  *StepCounter* -behält den Überblick über der Anzahl der Schritte, die der Benutzer übernommen hat, seit der Registrierung des Sensors *erst beim nächsten Neustart des Geräts*.

Der folgende Screenshot zeigt die Schritt-Zähler in Aktion:

[![Screenshot der SensorsActivity app einen Schritt Indikator anzeigen](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png)

Sie erstellen eine `SensorManager` durch Aufrufen `GetSystemService(SensorService)` und das Ergebnis als Umwandlung eine `SensorManager`. Rufen Sie zum Verwenden des Zähler Schritt `GetDeafultSensor` auf die `SensorManager`. Sie registrieren den Sensor und Lauschen auf Änderungen in Schritt Count mit der Hilfe der [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/) -Schnittstelle ein, wie im folgenden Codebeispiel veranschaulicht:

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` wird aufgerufen, wenn die Schrittanzahl aktualisiert, während die Anwendung im Vordergrund befindet. Wenn die Anwendung wechselt in den Hintergrund oder das Gerät inaktiv ist, `OnSensorChanged` wird nicht aufgerufen werden, jedoch weiterhin die Schritte zu zählenden bis `UnregisterListener` aufgerufen wird.

Beachten Sie, dass *Step Count-Wert ist ein kumulatives Update für alle Anwendungen, die den Sensor registrieren*. Dies bedeutet, dass, auch wenn deinstallieren und installieren Sie die Anwendung neu, und Initialisieren der `count` Variable mit 0 beim Start der Anwendung, der vom Sensor gemeldete Wert bleibt die Gesamtanzahl der Schritte, die während der Sensor registriert wurde, von Ihrer Anwendung oder eine andere. Sie können verhindern, dass Ihre Anwendung mit dem Leistungsindikator Schritt hinzufügen, durch Aufrufen von `UnregisterListener` auf die `SensorManager`, wie in den folgenden Code dargestellt:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Das Gerät neu zu starten, wird die Schrittanzahl auf 0 zurückgesetzt. Ihre app erfordern zusätzlichen Code, um sicherzustellen, dass es eine genaue Anzahl für die Anwendung, unabhängig von anderen Anwendungen, die mit der Sensor oder den Status des Geräts angibt.


> [!NOTE]
> **Hinweis:** während der API für die Erkennung Schritt und Lieferumfang KitKat zählen nicht alle Telefone mit der Sensor ausgestattet sind. Sie können überprüfen, wenn der Sensor verfügbar, durch Ausführen von ist `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, oder stellen Sie sicher, den zurückgegebenen Wert des `GetDefaultSensor` ist nicht `null`.


 <a name="developer_tools" />


## <a name="developer-tools"></a>Entwicklertools

### <a name="screen-recording"></a>Bildschirmaufzeichnung

KitKat enthält Bildschirm für neuen Funktionen aufzeichnen, sodass Entwickler Anwendungen in Aktion aufgezeichnet werden können. Bildschirmaufzeichnung kann über die [Android Debug Bridge (ADB)](http://developer.android.com/tools/help/adb.html) Client, der als Bestandteil des Android SDK heruntergeladen werden kann.

Verbinden Sie Ihr Gerät, um den Bildschirm zu erfassen; Klicken Sie dann die Android-SDK-Installation suchen, navigieren Sie zu der **plattformtools** Verzeichnis, und führen die **Adb** Client:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Der obige Befehl wird ein Standardwert 3 Minuten-Video, in der standardauflösung des 4Mbps aufgezeichnet. Um die Länge zu bearbeiten, Hinzufügen der *--Frist* Flag.
Um die Auflösung zu ändern, Hinzufügen der *--Bitrate* Flag. Der folgende Befehl wird eine Minute lang-Video mit 8Mbps aufgezeichnet:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Sie finden das Video auf Ihrem Gerät: Es wird im Katalog angezeigt, wenn die Aufzeichnung abgeschlossen ist.

<a name="other_kitkat_additions" />

## <a name="other-kitkat-additions"></a>Andere KitKat Ergänzungen

Zusätzlich zu den Änderungen, die oben beschriebenen ermöglicht KitKat:

-  *Verwenden Sie den gesamten Bildschirm* -KitKat bietet den neuen [faszinierend Modus](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) zum Durchsuchen von Inhalt, spielen und Ausführen anderer Anwendungen, die von einer Vollbild-Erfahrung profitieren könnten.

-  *Anpassen der Benachrichtigungen* -erhalten Sie weitere Informationen zum systembenachrichtigungen mit den [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) . Dadurch können Sie die Informationen auf andere Weise in Ihre app darstellen.

-  *Spiegeln zeichenbaren Ressourcen* -zeichenbare Ressourcen haben ein neues [ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored) Attribut, das System mitteilt, erstellen Sie eine gespiegelte Version für Bilder, die für Links-nach-rechts-Layouts erfordern ein umlegen.

-  *Anhalten von Animationen* -anhalten und Fortsetzen von Fenstern mit erstellt die [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/) Klasse.

-  *Dynamisch ändern von Text zu lesen* -Teile der Benutzeroberfläche, die dynamisch durch neuen Text als "live Regionen" mit dem neuen update zu kennzeichnen [ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) Attribut, damit der neue Text automatisch im Zugriffsmodus gelesen werden.

-  *Audio erleichtern* -stellen verfolgt lauter mit der [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) , suchen Sie die Haupt- und der einen Audiostream mit RMS die [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/) Klasse, und Abrufen von Informationen aus einer [audio Zeitstempel](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) , Audio/Video-Synchronisierung verwenden können.

-  *Synchronisieren von ContentResolver in benutzerdefinierten Intervallen* -KitKat fügt einige Variabilität in die Zeit, die eine synchronisierungsanforderung ausgeführt wird. Synchronisierung eine `ContentResolver` auf benutzerdefinierte oder Intervall durch den Aufruf `ContentResolver.RequestSync` und übergibt eine `SyncRequest`.

-  *Formatvorlagenquelle kennzeichnen zwischen Domänencontrollern* -In KitKat Domänencontroller sind eindeutige ganzzahlige Bezeichner, die über des Geräts zugegriffen werden können zugewiesen `ControllerNumber` Eigenschaft. Dies erleichtert es in ein Spiel auseinander Player mitteilen.

-  *Remotesteuerung* – mit einigen Änderungen auf Hardware und Software Seite KitKat können Sie ein Gerät mit einer IR-Sender in eine Remotesteuerung mit ausgestattet aktivieren die `ConsumerIrService`, und damit interagieren Peripheriegeräten, bei denen mit den neuen [ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) APIs.

Weitere Informationen zu den oben beschriebenen API-Änderungen, finden Sie auf der Google [Android 4.4-APIs](http://developer.android.com/about/versions/android-4.4.html) (Übersicht).


## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, einige der neuen APIs in Android 4.4 (API-Ebene 19) verfügbar und behandelt bewährte Methoden beim Übergang von einer Anwendung KitKat. IT beschriebene Änderungen an den APIs, die sich auf Benutzer auswirken auftreten, einschließlich der *Übergang Framework* und neue Optionen für *Designumgebung*. Als Nächstes Einführung der *Speicherzugriffs Framework* und `DocumentsProvider` Klasse als auch die neue *drucken APIs*. Sie untersucht *NFC hostbasierte Karte Emulation* und zum Arbeiten mit *LP-Sensoren*, einschließlich der zwei neue Sensoren zum Nachverfolgen des Benutzers Schritte. Schließlich nachgewiesen erfassen Echtzeit Demos von Anwendungen mit *bildschirmaufzeichnung*, und eine detaillierte Liste der KitKat-API-Änderungen und Ergänzungen bereitgestellt.


## <a name="related-links"></a>Verwandte Links

- [KitKat-Beispiel](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 APIs](http://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](http://developer.android.com/about/versions/kitkat.html)
