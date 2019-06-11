---
title: KitKat-Funktionen
description: Android 4.4 (KitKat) enthält eine humorvolle Features für Benutzer und Entwickler. Dieses Handbuch werden einige dieser Funktionen hervorgehoben, und bietet Codebeispiele und Implementierungsdetails können Sie die KitKat optimal.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f957bd5b361d7287353542186916c7f934ee0490
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827782"
---
# <a name="kitkat-features"></a>KitKat-Funktionen

_Android 4.4 (KitKat) enthält eine humorvolle Features für Benutzer und Entwickler. Dieses Handbuch werden einige dieser Funktionen hervorgehoben, und bietet Codebeispiele und Implementierungsdetails können Sie die KitKat optimal._

## <a name="overview"></a>Übersicht

Android 4.4 (API-Ebene 19), auch bekannt als "KitKat," wurde im Ende 2013 veröffentlicht. KitKat bietet es sich um eine Vielzahl von neuen Features und Verbesserungen, darunter:

-  [Benutzerfreundlichkeit](#user_experience) &ndash; einfache Animationen mit Übergang Framework lichtdurchlässiger Balken für Status und die Navigation und immersive Vollbildmodus unterstützen eine bessere Erfahrung für den Benutzer zu erstellen.

-  [Benutzerinhalte](#user_content) &ndash; benutzerverwaltung-Datei, die durch Storage Access-Framework vereinfacht; Drucken von Bildern, Websites und andere Inhalte wird durch eine verbesserte-Druck-APIs erleichtert.

-  [Hardware](#hardware) &ndash; verwandeln Sie jede app in einem NFC-Karte mit NFC-Host-basierte Karte Emulation; ausgeführt werden. mit geringem Sensoren mit der `SensorManager` .

-  [Entwicklertools](#developer_tools) &ndash; Screencast-Anwendungen in Aktion, mit dem Android Debug Bridge-Client, verfügbar als Teil des Android SDK.


Dieses Handbuch enthält Anweisungen für die Migration einer vorhandenen Xamarin.Android-Anwendung für KitKat, als auch einen allgemeinen Überblick über die KitKat für Xamarin.Android-Entwickler.

## <a name="requirements"></a>Anforderungen

Zum Entwickeln von Xamarin.Android-Anwendungen mithilfe von KitKat, müssen Sie *Xamarin.Android 4.11.0* oder höher sowie Android 4.4 (API-Ebene 19) über den Android SDK Manager installiert, wie im folgenden Screenshot dargestellt:

[![Auswählen von Android 4.4 im Android SDK Manager](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Migrieren Sie Ihre App zu KitKat

Dieser Abschnitt enthält einige Elemente der ersten Antwort Übergang von vorhandenen Anwendungen auf Android 4.4 können.

### <a name="check-system-version"></a>Überprüfen Sie die Systemversion

Wenn eine Anwendung mit älteren Versionen von Android kompatibel sein muss, achten Sie darauf, dass Sie alle KitKat-spezifischen Code in eine Überprüfung der System-Version zu umschließen, wie das folgende Codebeispiel veranschaulicht:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Alarm Batchverarbeitung

Android verwendet Wecker-Dienste, um eine app im Hintergrund zu einem bestimmten Zeitpunkt zu aktivieren. KitKat geht noch einen Schritt weiter durch das Zusammenfassen von Alarmen Power beizubehalten. Dies bedeutet, dass anstelle von jeder app zu einem genauen Zeitpunkt wird aktiviert, KitKat bevorzugt, um mehrere Anwendungen gruppieren, die registriert werden, um während des gleichen Zeitraums aktiviert, und aktivieren diese zur gleichen Zeit.
Aufrufen, um Android, aktivieren eine app während eines angegebenen Zeitintervalls zu sagen, `SetWindow` auf die [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/), und übergeben Sie die minimale und maximale Zeit in Millisekunden, die verstreichen kann, bevor die app aktiviert ist, und den auszuführenden Vorgang Bei der Reaktivierung.
Der folgende Code bietet ein Beispiel für eine Anwendung, die zwischen einer halben Stunde und einer Stunde ab dem Zeitpunkt, der das Fenster eingerichtet reaktiviert werden muss:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Verwenden Sie zum Fortsetzen des Vorgangs reaktivieren eine app zu einem genauen Zeitpunkt `SetExact`, und übergeben Sie die genaue Zeit, die die app aktiviert werden soll, und den auszuführenden Vorgang:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat kann nicht mehr einen genauen wiederholten Alarm einzurichten. Anwendungen, die verwenden [`SetRepeating`](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/)
und genau Alarme werden jetzt muss jede Alarm ausgelöst wird, manuell zu arbeiten.

### <a name="external-storage"></a>Externer Speicher

Externer Speicher ist jetzt in zwei Arten - Speicher, die nur in der Anwendung und die von mehreren Anwendungen gemeinsam verwendet werden, unterteilt. Lesen und Schreiben in Ihrer app bestimmten Speicherort auf externen speichern, benötigt keine besonderen Berechtigungen. Interagieren nun mit den Daten auf freigegebenen Speicher erfordert die `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` Berechtigung. Die beiden Typen können als standardänderungen klassifiziert werden:

-  Wenn Sie einen Datei- oder Verzeichnispfad optimal nutzen, durch Aufrufen einer Methode auf `Context` – z. B. [`GetExternalFilesDir`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/)
   Oder [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - Ihre app erfordert keine zusätzlichen Berechtigungen.

-  Wenn Sie einen Datei- oder Verzeichnispfad optimal nutzen, durch den Zugriff auf eine Eigenschaft oder Aufrufen einer Methode für `Environment` , z. B. [`GetExternalStorageDirectory`](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/)
   Oder [`GetExternalStoragePublicDirectory`](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/)
   , Ihre app benötigt die `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` Berechtigung.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` impliziert die `READ_EXTERNAL_STORAGE` -Berechtigung, daher Sie immer nur eine Berechtigung festlegen müssen sollten.

### <a name="sms-consolidation"></a>SMS-Konsolidierung

KitKat vereinfacht messaging für den Benutzer durch aggregieren alle SMS-Inhalte in einer standardanwendung, die vom Benutzer ausgewählt wurde. Der Entwickler ist verantwortlich für die Bereitstellung der app als der Standard-messaging-Anwendung ausgewählt werden und verhält sich entsprechend in Code und im Leben bei die Anwendung nicht aktiviert ist. Weitere Informationen zum Übergang Ihrer SMS-app für KitKat, finden Sie in der [der SMS-Apps bereit für KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Handbuch von Google.

### <a name="webview-apps"></a>WebView-Apps

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) eine Überarbeitung in KitKat haben. Die größte Änderung wurde die Sicherheit zum Laden von Inhalt in einem `WebView`. Während die meisten Anwendungen, die für ältere API-Versionen wie erwartet funktionieren sollte, Testen von Anwendungen, mit denen die `WebView` Klasse wird dringend empfohlen. Weitere Informationen zu den betroffenen WebView-APIs finden Sie in der Android [Migrieren zu WebView in Android 4.4](https://developer.android.com/guide/webapps/migrating.html) Dokumentation.

<a name="user_experience" />

## <a name="user-experience"></a>Benutzerfreundlichkeit

KitKat enthält mehrere neue APIs zur Verbesserung der benutzerfreundlichkeit, einschließlich das neuen Übergang-Framework für die Behandlung Eigenschaftenanimationen und einem durchscheinenden Benutzeroberflächenoption für Designs. Diese Änderungen sind unten beschrieben.

### <a name="transition-framework"></a>Transition-Framework

Der Übergang Framework erleichtert die Animationen zu implementieren. KitKat können Sie eine einfache Eigenschaft Animation mit nur einer Codezeile ausführen, oder passen Sie mithilfe von Übergängen *Szenen*.

#### <a name="simple-property-animation"></a>Einfache Eigenschaft Animation

Die neue Bibliothek für Android Übergänge vereinfacht den Code hinter der Eigenschaftenanimationen. Das Framework ermöglicht Ihnen, einfache Animationen mit minimalem Code auszuführen. Verwendet beispielsweise das folgende Codebeispiel herunter [`TransitionManager.BeginDelayedTransition`](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/)
zum Animieren von ein- und Ausblenden von einem `TextView`:

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

Das obige Beispiel verwendet das Framework Übergang erstellen Sie eine automatische, Standardübergang zwischen der Änderung der Eigenschaftswerte an. Da die Animation durch eine einzige Zeile Code behandelt werden, können ganz einfach, können Sie dies mit älteren Versionen von Android kompatibel wrapping der `BeginDelayedTransition` rufen Sie in einem der Betriebssystemversion. Finden Sie unter den [Migrieren Ihrer App, KitKat](#Migrating_Your_App_to_KitKat) Abschnitt Weitere.

Der folgende Screenshot zeigt die app vor der Animation an:

[![Screenshot der App vor dem Starten der animation](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

Der folgende Screenshot zeigt die app nach der Animation an:

[![App-Screenshot nach Abschluss der animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Sie erhalten mehr Kontrolle über den Übergang mit Szenen, die im nächsten Abschnitt behandelt werden.

#### <a name="android-scenes"></a>Android-Szenen

[Im Hintergrund](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) wurden als Teil des Übergangs-Frameworks, die dem Entwickler mehr Kontrolle über Animationen haben eingeführt. Im Hintergrund erstellen Sie einen dynamischen Bereich in der Benutzeroberfläche: Sie geben Sie einen Container und mehrere Versionen oder "Szenen", für den XML-Inhalt im Container und Android übernimmt den Rest der Arbeit die Übergänge zwischen den Szenen zu animieren. Android Szenen können Sie auf der Seite für die Entwicklung komplexe Animationen mit minimalem Aufwand zu erstellen.

Das statische Element der Benutzeroberfläche des dynamischen Inhalts Speichern einer mit dem Namen wird ein *Container* oder *Szene Basis*. Im folgenden Beispiel wird die Android-Designer zum Erstellen einer `RelativeLayout` namens `container`:

[![Verwenden von Android Designer zum Erstellen eines Containers RelativeLayout](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

Das Beispiel definiert auch einer Schaltfläche namens `sceneButton` unterhalb der `container`. Diese Schaltfläche wird den Übergang ausgelöst.

Der dynamische Inhalt innerhalb des Containers ist erforderlich, zwei neue Android-Layouts. Diese Layouts geben nur den Code *in* des Containers.
Die unten stehenden Code definiert ein Layout mit dem Namen *Scene1* mit zwei Textfeldern lesen "Kit" und "Kat" bzw. sowie eine zweite Layout bezeichnet *Scene2* , enthält die gleichen Textfelder, die rückgängig gemacht. Der XML-Code lautet wie folgt aus:

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

Im Beispiel oben wird `merge` kürzeren Code in der Ansicht und die Hierarchie von Inhaltsansichten zu vereinfachen. Erfahren Sie mehr über `merge` Layouts [hier](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Durch Aufrufen eine Szene erstellt [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/), und übergeben Sie die Container-Objekt, das Ressourcen-ID der Layoutdatei der Szene, und das aktuelle `Context`, wie im folgenden Codebeispiel dargestellt:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Durch Klicken auf die Schaltfläche mit den wechselt zwischen den beiden Szenen, die Android mit den Standardwerten für den Übergang animiert wird:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Der folgende Screenshot zeigt die Szene, bevor die Animation:

[![Screenshot der app vor dem Start der animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Der folgende Screenshot zeigt die Szene, nachdem die Animation:

[![Screenshot der app, die nach Ende der Animation](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> Gibt es eine [bekannter Fehler](https://code.google.com/p/android/issues/detail?id=62450) in der Android-Übergänge-Bibliothek, die bewirkt, im Hintergrund dass erstellt mit `GetSceneForLayout` zu unterbrechen, wenn ein Benutzer über eine Aktivität beim zweiten Mal navigiert. Eine Java-problemumgehung beschrieben [hier](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Benutzerdefinierte Übergänge in Szenen

Ein benutzerdefinierter Übergang kann definiert werden, in einer XML-Ressourcendatei in die `transition` Verzeichnis unter `Resources`, wie im folgenden Screenshot dargestellt:

[![Speicherort der Datei transition.xml Ressourcen/Übergang-Verzeichnis](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

Das folgende Codebeispiel definiert einen Übergang, der fünf Sekunden lang animiert und verwendet die [auch Interpolators](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Der Übergang wird erstellt, in der Aktivität mit der [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/), wie im folgenden Code dargestellt:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Der neue Zustandsübergang wird dann hinzugefügt, um die `Go` Aufruf, der die Animation beginnt:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Lichtdurchlässiger UI

KitKat steuern Sie mehr über das Design Ihrer app mit optionalen lichtdurchlässiger Balken für Status und die Navigation. Sie können die Transparenz des System-UI-Elemente in der gleichen XML-Datei ändern, die Sie verwenden, um Ihr Android-Design zu definieren. KitKat führt die folgenden Eigenschaften:

-  `windowTranslucentStatus` – Wenn festgelegt auf "true" werden die obersten Statusleiste lichtdurchlässiger.

-  `windowTranslucentNavigation` – Wenn festgelegt auf "true" werden die untere Navigationsleiste lichtdurchlässiger.

-  `fitsSystemWindows` – Festlegen der Leiste oben oder unten, um Transcluent verschiebt Inhalt unter die Elemente der Benutzeroberfläche für das transparente standardmäßig. Wenn diese Eigenschaft auf `true` ist eine einfache Möglichkeit, um zu verhindern, dass Inhalt überschneidet sich mit die Elemente der Benutzeroberfläche lichtdurchlässiger System.


Der folgende Code definiert ein Design mit lichtdurchlässiger Status und die Navigation Balken:

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

Der folgende Screenshot zeigt das Design über lichtdurchlässiger Status und Navigationsleisten an:

[![Beispielhafter Screenshot der app mit lichtdurchlässiger Status und die Navigation Balken](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>Inhalte von Benutzern

### <a name="storage-access-framework"></a>Storage Datenzugriffs-Framework

Der Speicher-Access-Framework (SAF) ist eine neue Möglichkeit für Benutzer, für die Interaktion mit gespeicherten Inhalte wie Bilder, Videos und Dokumente. Statt einzelner Benutzer ein Dialogfeld zum Auswählen einer Anwendung zur Handhabung von Inhalt, öffnet KitKat eine neue Benutzeroberfläche, die Benutzern den Zugriff auf ihre Daten in einem Aggregat Ort ermöglicht. Sobald Inhalt ausgewählt wurde, der Benutzer wird zurückgegeben, um die Anwendung, die der Inhalt angefordert, und die app-Funktionalität wird ganz normal fortgesetzt.

Diese Änderung erfordert zwei Aktionen auf der Seite für Entwickler: apps, die Inhalte von Anbietern erfordern zunächst eine neue Möglichkeit des anfordernden Inhalts aktualisiert werden. Zweite, Anwendungen, die Daten zum Schreiben einer `ContentProvider` müssen geändert werden, um das neue Framework zu verwenden. Beide Szenarien hängen von der neuen [`DocumentsProvider`](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/)
API.

#### <a name="documentsprovider"></a>DocumentsProvider

In KitKat, Interaktionen mit `ContentProviders` abstrahiert werden, mit der `DocumentsProvider` Klasse. Dies bedeutet, dass SAF sich nicht darum, in denen die Daten physisch sind kümmert solange es über die `DocumentsProvider` API. Lokale Anbieter, cloud Services und externe Speichergeräte jegliche Nutzung, die die gleiche Schnittstelle und die gleiche Weise behandelt den Benutzer und Entwickler mit einem zentralen Ort für die Interaktion mit der Benutzer die Inhalte bereitstellen.

In diesem Abschnitt wird beschrieben, wie Laden und Speichern von Inhalt mit dem Storage-Access-Framework wird.

#### <a name="request-content-from-a-provider"></a>Fordern Sie Inhalte von einem Anbieter

Wir können KitKat sagen, wir möchten Sie Inhalt mithilfe der SAF UI mit Auswählen der `ActionOpenDocument` Absicht, was bedeutet, dass wir alle auf dem Gerät verfügbar Inhaltsanbieter herstellen möchten. Sie können einige Filter auf diese Absicht durch Angabe hinzufügen `CategoryOpenable`, also nur Inhalte, die (d. h. zugegriffen werden kann, verwendet werden-Inhalt) geöffnet werden können zurückgegeben werden. KitKat auch ermöglicht das Filtern von Inhalten mit der `MimeType`. Z. B. den folgenden Filter für Image-Ergebnisse, indem Sie das Image angeben Code `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Aufrufen von `StartActivityForResult` startet die SAF UI, die der Benutzer dann navigieren kann, um ein Bild auszuwählen:

[![Beispielscreenshot, der eine app mit dem Storage-Zugriff-Framework für das Durchsuchen von zu einem Bild](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

Nachdem der Benutzer ein Bild ausgewählt hat `OnActivityResult` gibt die `Android.Net.Uri` der ausgewählten Datei. Das folgende Codebeispiel zeigt die Bildauswahl des Benutzers:

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

Zusätzlich zum Laden von Inhalt aus der SAF UI, KitKat können Sie auch Inhalte in alle speichern `ContentProvider` , implementiert die `DocumentProvider` API. Speichern von Inhalt verwendet eine `Intent` mit `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Im obigen Beispiel lädt die SAF UI, sodass des Benutzers, die den Dateinamen ändern, und wählen ein Verzeichnis an, die neue Datei gespeichert:

[![Screenshot des Benutzers, ändern den Dateinamen zu NewDoc im Downloads-Verzeichnis](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Wenn der Benutzer drückt **speichern**, `OnActivityResult` übergeben die `Android.Net.Uri` der neu erstellte Datei, die mit zugegriffen werden kann `data.Data`. Der Uri kann zum Streamen von Daten in die neue Datei verwendet werden:

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

Beachten Sie, dass [`ContentResolver.OpenOutputStream(Android.Net.Uri)`](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri))
Gibt eine `System.IO.Stream`, sodass der gesamte streaming-Prozess in .NET geschrieben werden kann.

Weitere Informationen zu laden, erstellen und Bearbeiten von Inhalten mit dem Storage-Access-Framework finden Sie unter den [Android-Dokumentation für das Storage-Access-Framework](https://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Drucken

Drucken-Inhalt wird mit der Einführung von in KitKat vereinfacht die [Druckdienste](https://developer.xamarin.com/api/namespace/Android.PrintServices/) und `PrintManager`. KitKat ist auch die erste API-Version vollständig nutzen die [Drucken von Cloud-Dienst-APIs von Google](https://developers.google.com/cloud-print/) mithilfe der [Google Cloud-druckanwendung](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
Drucken von Google Cloud-app für die meisten Geräte aus dem Lieferumfang von KitKat automatisch herunterladen und die [HP drucken-Service-Plug-Ins](https://play.google.com/store/apps/details?id=com.hp.android.printservice)Wenn sie zunächst eine Verbindung mit WiFi. Ein Benutzer kann seine des drucken geräteeinstellungen überprüfen durch Navigieren zu **Einstellungen > System > Drucken**:

[![Beispielscreenshot, der den Druck-Einstellungen](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> Obwohl die Druck-APIs so eingerichtet sind, arbeiten mit Google für Clouddruck standardmäßig, kann Android weiterhin Entwickler mit den neuen APIs Druckinhalte vorbereiten und senden Sie sie an andere Anwendungen drucken zu behandeln.



#### <a name="printing-html-content"></a>Drucken von HTML-Inhalt

KitKat erstellt automatisch eine [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) für eine Webansicht mit `WebView.CreatePrintDocumentAdapter`. Drucken von Webinhalten ist koordiniert zwischen einem [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) , wartet der HTML-Inhalt geladen und können die Aktivität, die wissen, dass die Option "Drucken" in das Menü "Optionen" verfügbar machen und die Aktivität, die wartet, bis der Benutzer Wählen Sie die Option "Drucken" und ruft `Print`auf die `PrintManager`. Dieser Abschnitt enthält die grundlegende Einrichtung erforderlich, um auf dem Bildschirm Drucken von HTML-Inhalt.

Beachten Sie, dass die Internet-Berechtigung erforderlich, laden und Drucken von Web-Inhalte ist:

[![Festlegen von Internet-Berechtigung in den app-Optionen](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Menüelements Druck

Die print-Option wird in der Regel angezeigt, in der Aktivitäts [Menü "Optionen"](https://developer.android.com/guide/topics/ui/menus.html#options-menu).
Das Menü "Optionen" ermöglicht Benutzern das Ausführen von Aktionen für eine Aktivität. Dabei handelt es sich in der oberen rechten Ecke des Bildschirms, sieht wie folgt aus:

[![Beispielhafter Screenshot der Menüelement ' Drucken ' in der oberen rechten Ecke des Bildschirms angezeigt](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


Zusätzliche Menüelemente können definiert werden, der *Menü*Verzeichnis unter *Ressourcen*. Der folgende Code definiert ein Menüelement mit dem Namen Sample [Drucken](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Interaktion mit dem Menü "Optionen" in der Aktivität erfolgt über die `OnCreateOptionsMenu` und `OnOptionsItemSelected` Methoden.
`OnCreateOptionsMenu` ist der Ort, an die neuen Menüelemente an, wie das Print-Option von Hinzufügen der *Menü* Verzeichnis "Resources".
`OnOptionsItemSelected` überwacht, die für den Benutzer, die Sie im Menü die Option Drucken auswählen und beginnt die Ausgabe:

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

Der obige Code definiert außerdem eine Variable namens `dataLoaded` zu verfolgen den Status der HTML-Inhalt. Die `WebViewClient` wird setzen diese Variable auf "true" alle Inhalte geladen wurde, damit die Aktivität weiß, dass das Menüelement für den Druck auf das Menü "Optionen" hinzufügen.

##### <a name="webviewclient"></a>WebViewClient

Die Aufgabe der `WebViewClient` besteht darin, sicherzustellen, dass Daten in die `WebView` vollständig geladen ist, bevor die Option "Drucken" klicken Sie im Menü angezeigt, was geschieht wird mit den `OnPageFinished` Methode. `OnPageFinished` Lauscht auf von Webinhalten auf geladen wurden, und weist die Aktivität, um das Optionsmenü, mit neu zu erstellen `InvalidateOptionsMenu`:

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

`OnPageFinished` legt auch fest, die `dataLoaded` Wert `true`, sodass `OnCreateOptionsMenu` können neu erstellen, klicken Sie im Menü mit der Option "Drucken" vorhanden.

##### <a name="printmanager"></a>PrintManager

Das folgende Codebeispiel gibt den Inhalt einer `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` als Argumente akzeptiert: einen Namen für den Auftrag ("MyWebPage" in diesem Beispiel), ein [`PrintDocumentAdapter`](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/)
aus dem Inhalt, der zu druckenden Dokuments generiert und [`PrintAttributes`](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/)
(`null` im obigen Beispiel). Sie können angeben, `PrintAttributes` können setzen von Inhaltselementen auf der Seite gedruckt, obwohl die meisten Szenarien die Standardattribute behandelt werden sollen.

Aufrufen von `Print` lädt die print-Benutzeroberfläche, die Optionen für den Druckauftrag aufgelistet sind. Die Benutzeroberfläche ermöglicht Benutzern die Möglichkeit, Drucken und speichern den HTML-Inhalt einer PDF-Format, wie die folgenden Screenshots veranschaulicht:

[![Screenshot der PrintHtmlActivity das Menü "Drucken" anzeigen](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Screenshot der PrintHtmlActivity speichern als PDF-Menü anzeigen](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>Hardware

KitKat fügt mehrere APIs, um die neuen Gerätefeatures zu berücksichtigen. Die wichtigsten davon sind hostbasierte Karte Emulation und die neue `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Host-basierte Karte-Emulation in der NFC

Host-basierte Karte Emulation (HCE) können Anwendungen wie NFC-Karten oder NFC-Lesegeräte zu Verhalten, ohne Rückgriff auf des Netzbetreibers proprietäre Secure-Element. Bevor HCE eingerichtet haben, stellen Sie sicher HCE finden Sie auf dem Gerät mit `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE erfordert, dass sowohl die HCE-Funktion und die `Nfc` Berechtigung in der Anwendung registriert werden `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Festlegen der NFC-Berechtigung in den app-Optionen](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Um zu arbeiten, HCE im Hintergrund ausgeführt werden muss, und es muss zu starten, wenn der Benutzer über eine NFC-Transaktion trifft, selbst wenn die Anwendung mithilfe von HCE nicht ausgeführt wird. Wir können dies erreichen, indem Sie das Schreiben des Codes HCE als eine `Service`. Ein Dienst HCE implementiert die `HostApduService` -Schnittstelle, die die folgenden Methoden implementiert:

-  *ProcessCommandApdu* : An Application Protocol Data Unit (APDU) ist, was zwischen den NFC-Reader und dem HCE-Dienst gesendet wird. Diese Methode nutzt eine ADPU aus dem Reader und gibt eine Dateneinheit als Antwort zurück.

-  *OnDeactivated* : die `HostAdpuService` wird deaktiviert, wenn der Dienst HCE nicht mehr mit der NFC-Reader kommuniziert.


Ein Dienst HCE muss auch das Manifest der Anwendung registriert werden, und mit den erforderlichen Berechtigungen, Zielfilter und Metadaten versehen. Der folgende Code ist ein Beispiel für eine `HostApduService` registriert, mit dem Android-Manifest unter Verwendung der `Service` Attribut (Weitere Informationen zu Attributen finden Sie in der Xamarin [arbeiten mit Android-Manifest](~/android/platform/android-manifest.md) Guide):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("android.nfc.cardemulation.host.apdu_service",
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

Der oben aufgeführten Dienst bietet eine Möglichkeit der NFC-Reader, der mit der Anwendung interagieren, aber der NFC-Reader wurde noch keine Möglichkeit, zu wissen, ob dieser Dienst die NFC-Karte emuliert wird, die er suchen muss. Damit können den NFC-Reader, der den Dienst zu identifizieren, wir können weist dem Dienst eine eindeutige *Anwendung-ID (AID)* . Wir geben Sie die Hilfe, zusammen mit anderen Metadaten über den HCE-Dienst, in einer XML-Ressourcendatei registriert mit der `MetaData` Attribut (Siehe obigen Codebeispiel). Diese Ressourcendatei gibt einen oder mehrere AID - Bezeichner-Zeichenfolgen im Hexadezimalformat, die die Unterstützung von Lesegeräten, eine oder mehrere NFC entsprechen:

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

Zusätzlich zu Hilfe filtern, die XML-Ressourcendatei enthält zudem eine Benutzer-Beschreibung des Diensts HCE gibt eine Hilfe-Gruppe (Payment-Anwendung im Vergleich zu "other") und im Falle einer Zahlung-Anwendung ein 260 x 96-dp-Banner, um den Benutzer anzuzeigen.

Das oben beschriebene Setup bietet die grundlegenden Bausteine für eine Anwendung, die auf eine Karte NFC emulieren. NFC selbst erfordert einige weitere Schritte und weitere Tests, um zu konfigurieren. Weitere Informationen zu hostbasierten Karte Emulation, finden Sie in der [Android dokumentationsportal](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Weitere Informationen zur Verwendung von NFC mit Xamarin, sehen Sie sich die [NFC für Xamarin-Beispiele](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Sensoren

KitKat ermöglicht den Zugriff auf die gerätesensoren über eine [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/).
Die `SensorManager` erlaubt es dem Betriebssystem so planen Sie die Bereitstellung von Sensorinformationen zu einer Anwendung in Batches beibehalten Akkuverbrauch gering zu halten.

KitKat umfasst auch mit zwei neuen Sensor zum Nachverfolgen des Benutzers Schritte. Diese basieren auf den Beschleunigungsmesser und umfassen:

-  *StepDetector* -App wird benachrichtigt/aktiviert werden, wenn der Benutzer geht noch einen Schritt sowie des Detektors einen Zeitwert für des Auftretens des Schritts.

-  *StepCounter* -verfolgt des die Anzahl der Schritte, die der Benutzer übernommen hat, seit der Registrierung des Sensors *bis zum nächsten Neustart des Geräts*.

Der folgende Screenshot zeigt den Schritt Leistungsindikator in Aktion:

[![Screenshot der app SensorsActivity einen Schritt Indikator anzeigen](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Sie erstellen eine `SensorManager` durch Aufrufen von `GetSystemService(SensorService)` und wandelt das Ergebnis als eine `SensorManager`. Rufen Sie zum Verwenden des Zähler Schritt `GetDefaultSensor` auf die `SensorManager`. Sie registrieren den Sensor und Lauschen auf Änderungen in der Anzahl der Schritte mithilfe der [`ISensorEventListener`](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/)
Schnittstelle, wie das folgende Codebeispiel veranschaulicht:

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

`OnSensorChanged` wird aufgerufen, wenn die Anzahl der Schritte aktualisiert, während die Anwendung im Vordergrund ist. Wenn die Anwendung in den Hintergrund wechselt oder das Gerät sich im Standbymodus befindet, `OnSensorChanged` wird nicht aufgerufen werden; jedoch weiterhin die Schritte, die bis gezählt werden `UnregisterListener` aufgerufen wird.

Beachten Sie, dass *der Step-Count-Wert ist für alle Anwendungen, die den Sensor registrieren kumulativ*. Dies bedeutet, dass, selbst wenn Sie deinstallieren und installieren Sie die Anwendung neu, und initialisieren Sie die `count` Variablen bei 0 beim Starten der Anwendung, der vom Sensor gemeldete Wert bleibt die Gesamtzahl der Schritte, die während der Sensor registriert wurde, von Ihrem Anwendung oder einen anderen. Sie können verhindern, dass Ihre Anwendung durch Aufrufen der Schritt Indikator hinzugefügt `UnregisterListener` auf die `SensorManager`, wie im folgenden Code dargestellt:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Starten Sie es neu, wird die Anzahl der Schritte auf 0 zurückgesetzt. Ihre app ist erforderlich, zusätzlichen Code, um sicherzustellen, dass es eine genaue Anzahl für die Anwendung, unabhängig von anderen Anwendungen, die mit dem Sensor oder der Status des Geräts angibt.


> [!NOTE]
> Während der API für die Schritt-Erkennung und gezählt wird im Lieferumfang von KitKat sind nicht alle Telefone mit dem Sensor ausgestattet. Sie können überprüfen, ob der Sensor verfügbar ist, mit `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, oder Überprüfen Sie, ob den zurückgegebenen Wert der `GetDefaultSensor` ist nicht `null`.


<a name="developer_tools" />

## <a name="developer-tools"></a>Entwicklertools

### <a name="screen-recording"></a>Bildschirmaufzeichnung

KitKat enthält neue bildschirmaufzeichnung Funktionen, damit Entwickler Anwendungen in Aktion aufgezeichnet werden können. Bildschirmaufzeichnung kann über die [Android Debug Bridge (ADB)](https://developer.android.com/tools/help/adb.html) -Client, der als Teil des Android SDK heruntergeladen werden kann.

Verbinden Sie Ihr Gerät, um den Bildschirm zu zeichnen, Klicken Sie dann die Android SDK-Installation suchen, navigieren Sie zu der **plattformtools** Verzeichnis, und führen die **Adb** Client:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Der obige Befehl wird ein Standardwert 3-minütigen Video, in der standardauflösung des 4 Mbit/s aufgezeichnet. Um die Länge zu bearbeiten, Hinzufügen der *--Frist* Flag.
Um die Auflösung zu ändern, fügen die *--Bitrate* Flag. Der folgende Befehl wird eine Minute lang Videoinhalte 8Mbps aufgezeichnet:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Sie finden Ihr Video auf Ihrem Gerät – es wird in Ihrem Katalog angezeigt, wenn die Aufzeichnung abgeschlossen ist.


## <a name="other-kitkat-additions"></a>Weitere Ergänzungen KitKat

Zusätzlich zu den oben beschriebenen Änderungen ermöglicht KitKat:

-  *Verwenden Sie den gesamten Bildschirm* -KitKat wird ein neuer [immersiven Modus](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) zum Durchsuchen des Inhalts, spielen und Ausführen von anderen Anwendungen, die aus einer Vollbild-Erfahrung profitieren können.

-  *Anpassen der Benachrichtigungen* -erhalten Sie weitere Informationen über die Benachrichtigungen des Systems mit dem [`NotificationListenerService`](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/)
   sein. Dadurch können Sie die Informationen in eine andere Weise in Ihrer app vorhanden.

-  *Spiegeln zeichenbare Ressourcen* -zeichenbare Ressourcen haben einen neuen [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
   Attribut, das dem System mitgeteilt erstellen Sie eine gespiegelte Version für Bilder, die für Links-nach-rechts-Layouts erfordern ein umlegen.

-  *Anhalten von Animationen* -anhalten und fortsetzen Animationen erstellt, mit der [`Animator`](https://developer.xamarin.com/api/type/Android.Animation.Animator/)
   -Klasse.

-  *Dynamisch ändern von Text zu lesen* -Teile der Benutzeroberfläche, die dynamisch durch neuen Text als "live Regionen" mit dem neuen update zu kennzeichnen [ `accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
   Attribut, damit der neue Text automatisch im Zugriffsmodus gelesen werden.

-  *Audio-Erfahrung verbessern* -stellen lauter verfolgt, mit der [`LoudnessEnhancer`](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/)
   , suchen Sie die maximale und die RMS des Audiostreams mit der [`Visualizer`](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/)
   Klasse, und Abrufen von Informationen aus einer [audio Zeitstempel](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) um Audio-Video-Synchronisierung zu unterstützen.

-  *Synchronisieren ContentResolver in benutzerdefinierten Intervallen* -KitKat fügt einige Variabilität, mit der Zeit, die eine synchronisierungsanforderung ausgeführt wird. Synchronisierung eine `ContentResolver` benutzerdefinierte Zeit oder Intervall durch den Aufruf `ContentResolver.RequestSync` und übergeben Sie einen `SyncRequest`.

-  *Formatvorlagenquelle kennzeichnen zwischen Controllern* -im KitKat zugewiesen Domänencontroller sind eindeutige ganzzahlige Bezeichner, die über des Geräts zugegriffen werden können `ControllerNumber` Eigenschaft. Dies erleichtert es auseinander Spieler in einem Spiel zu informieren.

-  *Remotesteuerung* -KitKat mit einigen Änderungen auf die Hardware und Software-Seite, können Sie ein Gerät mit einem IR-Sender in eine Remotesteuerung mit ausgestattet aktivieren die `ConsumerIrService`, und interagieren mit Peripheriegeräten, bei denen mit dem neuen [`RemoteController`](https://developer.xamarin.com/api/type/Android.Media.RemoteController/)
   APIs.

Weitere Informationen zu den oben genannten API-Änderungen, finden Sie auf der Google [Android 4.4-APIs](https://developer.android.com/about/versions/android-4.4.html) Übersicht.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel einige der neuen APIs zur Verfügung, in Android 4.4 (API-Ebene 19) eingeführt und behandelt bewährte Methoden beim Übergang von einer Anwendung, KitKat. IT beschriebene Änderungen an der APIs, die Auswirkungen auf Benutzer auftreten, einschließlich der *Übergang Framework* und neue Optionen zum *Design*. Sie als Nächstes eingeführt, die *Speicherzugriffs Framework* und `DocumentsProvider` -Klasse als auch die neue *Druck-APIs*. Er untersucht *NFC-Host-basierte Karte-Emulation* und Arbeiten mit *energiesparenden Sensoren*, einschließlich der zwei neue Sensoren für die nachverfolgung des Benutzers Schritte. Schließlich erfassen in Echtzeit Demos von Anwendungen mit nachgewiesen *bildschirmaufzeichnung*, und eine detaillierte Liste der KitKat, API-Änderungen und Ergänzungen bereitgestellt.


## <a name="related-links"></a>Verwandte Links

- [KitKat-Beispiel](https://developer.xamarin.com/samples/monodroid/KitKat/)
- [Android 4.4 APIs](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
