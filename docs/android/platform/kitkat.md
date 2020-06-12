---
title: Features von KitKat
description: Android 4.4 (KitKat) bietet ein Füllhorn an Features für Benutzer und Entwickler. Dieser Leitfaden stellt verschiedene dieser Features vor und bietet Codebeispiele sowie Implementierungsdetails, um Sie dabei zu unterstützen, KitKat optimal zu nutzen.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 7a3fd9e22bcf037ec669c77ac919035b0d04b942
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567930"
---
# <a name="kitkat-features"></a>Features von KitKat

_Android 4.4 (KitKat) bietet ein Füllhorn an Features für Benutzer und Entwickler. Dieser Leitfaden stellt verschiedene dieser Features vor und bietet Codebeispiele sowie Implementierungsdetails, um Sie dabei zu unterstützen, KitKat optimal zu nutzen._

## <a name="overview"></a>Übersicht

Android 4.4 (API-Ebene 19), auch als „KitKat“ bezeichnet, wurde Ende 2013 veröffentlicht. KitKat bietet eine Vielzahl neuer Features und Verbesserungen, einschließlich der folgenden:

- [Benutzerfreundlichkeit](#user_experience) &ndash; einfache Animationen mit Übergangsframework, durchsichtigen Status- und Navigationsleisten und einem plastischen Vollbildmodus, der die Benutzerfreundlichkeit verbessert.

- [Benutzerinhalte](#user_content) &ndash; vereinfachte Benutzerdateiverwaltung mit dem Storage Access Framework (SAF); das Drucken von Bildern, Websites und anderen Inhalten wird dank verbesserter Druck-APIs ebenfalls vereinfacht.

- [Hardware](#hardware) &ndash; mit der hostbasierten Kartenemulation (Host-Based Card Emulation, HCE) für NFC lässt sich jede App in eine NFC-Karte verwandeln; der `SensorManager` ermöglicht die Nutzung von Sensoren mit geringerem Stromverbrauch.

- [Entwicklertools](#developer_tools) &ndash; zeichnen Sie mit dem Android Debug Bridge-Client, der mit dem Android SDK verfügbar ist, Anwendungen in Aktion auf.

Der vorliegende Leitfaden bietet Anleitungen zum Migrieren einer vorhandenen Xamarin.Android-Anwendung zu KitKat sowie einen allgemeinen Überblick über KitKat für Xamarin.Android-Entwickler.

## <a name="requirements"></a>Anforderungen

Um Xamarin.Android-Anwendungen für KitKat zu entwickeln, müssen Sie über *Xamarin.Android 4.11.0* oder höher verfügen und Android 4.4 (API-Ebene 19) über den Android-SDK-Manager installiert haben, wie im folgenden Screenshot gezeigt:

[![Auswählen von Android 4.4 im Android-SDK-Manager](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat"></a>

## <a name="migrating-your-app-to-kitkat"></a>Migrieren Ihrer App zu KitKat

In diesem Abschnitt werden einige wichtige Aspekte beleuchtet, die Sie beim Migrieren vorhandener Apps zu Android 4.4 unterstützen.

### <a name="check-system-version"></a>Überprüfen der Systemversion

Wenn eine Anwendung mit älteren Android-Versionen kompatibel sein muss, umschließen Sie jeglichen KitKat-spezifischen Code mit einer Überprüfung der Systemversion, wie im folgenden Code veranschaulicht:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Batchverarbeitung von Alarmen

Android verwendet Alarmdienste, um eine App im Hintergrund zu einem bestimmten Zeitpunkt zu aktivieren. KitKat erweitert diese Funktionalität, indem Alarme in Batches verarbeitet werden, um Strom zu sparen. Das bedeutet Folgendes: Anstatt jede App zu einem exakten Zeitpunkt zu aktivieren, gruppiert KitKat verschiedene Anwendungen, die für eine Aktivierung im selben Zeitintervall registriert sind, und aktiviert diese alle gleichzeitig.
Um Android anzuweisen, eine App während eines bestimmten Zeitintervalls zu aktivieren, rufen Sie `SetWindow` im [`AlarmManager`](xref:Android.App.AlarmManager) auf, und übergeben Sie den minimalen und maximalen Zeitraum in Millisekunden, der vor der Aktivierung einer App vergehen darf, sowie den Vorgang, der bei der Aktivierung ausgeführt werden soll.
Der folgende Code bietet ein Beispiel für eine Anwendung, die eine halbe bis eine Stunde nach dem festgelegten Zeitpunkt des Fensters aktiviert werden soll:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Um eine App weiterhin zu einem exakten Zeitpunkt zu aktivieren, verwenden Sie `SetExact` und übergeben den exakten Zeitpunkt der Aktivierung der App sowie den auszuführenden Vorgang:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

Unter KitKat können Sie keinen exakten Wiederholungsalarm mehr festlegen. Bei Anwendungen, die [`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*) verwenden
und exakte Alarme benötigen, muss jetzt jeder Alarm manuell ausgelöst werden.

### <a name="external-storage"></a>Externer Speicher

Externer Speicher ist jetzt in zwei Typen unterteilt: eindeutiger Speicher für eine Anwendung und von mehreren Anwendungen gemeinsam genutzte Daten. Für das Lesen und Schreiben im App-spezifischen Speicherort eines externen Speichers sind keine besonderen Berechtigungen erforderlich. Die Interaktion mit Daten in gemeinsam genutztem Speicher ist jetzt die Berechtigung `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` erforderlich. Die beiden Typen können folgendermaßen klassifiziert werden:

- Beim Abrufen einer Datei oder eines Verzeichnispfads durch Aufrufen einer Methode in `Context` – z. B. [`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
  oder [`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs) –
  - benötigt Ihre App keine zusätzlichen Berechtigungen.

- Beim Abrufen einer Datei oder eines Verzeichnispfads durch Zugreifen auf eine Eigenschaft oder durch Aufrufen einer Methode in `Environment` – z. B. [`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
  oder [`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*) –
  benötigt Ihre App die Berechtigung `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE`.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` impliziert die Berechtigung `READ_EXTERNAL_STORAGE`, sodass Sie immer nur eine Berechtigung festlegen müssen.

### <a name="sms-consolidation"></a>SMS-Konsolidierung

KitKat vereinfacht das Messaging für die Benutzer durch Aggregieren sämtlicher SMS-Inhalte in eine vom Benutzer ausgewählte Standard-App. Der Entwickler ist dafür zuständig, dass die App als Standard-App für das Messaging ausgewählt werden kann und ein geeignetes Verhalten im Code und in der Praxis zeigt, wenn die App nicht ausgewählt ist. Weitere Informationen zum Migrieren Ihrer SMS-App zu KitKat finden Sie im Leitfaden [Getting Your SMS Apps Ready for KitKat](https://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) (Vorbereiten Ihrer SMS-App auf KitKat) von Google.

### <a name="webview-apps"></a>WebView-Apps

[WebView](xref:Android.Webkit.WebView) wurde in KitKat gründlich überarbeitet. Die größte Änderung betrifft die zusätzliche Sicherheit beim Laden von Inhalten in einen `WebView`. Die meisten Anwendungen, die für ältere API-Versionen entwickelt wurden, sollten erwartungsgemäß funktionieren. Allerdings empfiehlt es sich dringend, Anwendungen zu testen, die die `WebView`-Klasse verwenden. Weitere Informationen zu den betroffenen WebView-APIs finden Sie in der Android-Dokumentation [Migrating to WebView in Android 4.4](https://developer.android.com/guide/webapps/migrating.html) (Migrieren zu WebView in Android 4.4).

<a name="user_experience"></a>

## <a name="user-experience"></a>Benutzerfreundlichkeit

KitKat enthält verschiedene neue APIs zur Verbesserung der Benutzerfreundlichkeit, einschließlich eines neuen Übergangsframeworks zum Verarbeiten von Eigenschaftsanimationen und eine Option für durchsichtige Benutzeroberflächenelemente für Themes. Diese Änderungen werden im Folgenden erläutert.

### <a name="transition-framework"></a>Übergangsframework

Mit dem Übergangsframework lassen sich Animationen einfacher implementieren. In KitKat können Sie eine einfache Eigenschaftsanimation mit einer einzigen Codezeile ausführen oder Übergänge mithilfe von *Szenen* anpassen.

#### <a name="simple-property-animation"></a>Einfache Eigenschaftsanimationen

Die neue Android-Bibliothek für Übergänge vereinfacht den Code für Eigenschaftsanimationen. Das Framework ermöglicht die Ausführung einfacher Animationen mit minimalem Codeaufwand. Das folgende Codebeispiel verwendet [`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*),
um das Anzeigen und Ausblenden eines `TextView` zu animieren:

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

Dieses Beispiel verwendet das Übergangsframework, um einen automatischen Standardübergang zwischen den wechselnden Eigenschaftswerten zu erstellen. Da die Animation mit einer einzigen Codezeile verarbeitet wird, können Sie ganz einfach Kompatibilität mit älteren Android-Versionen herstellen, indem Sie den `BeginDelayedTransition`-Aufruf mit einer Überprüfung der Systemversion umschließen. Weitere Informationen finden Sie im Abschnitt [Migrieren Ihrer App zu KitKat](#Migrating_Your_App_to_KitKat).

Der folgende Screenshot zeigt die App vor der Animation:

[![App-Screenshot vor dem Start der Animation](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

Der folgende Screenshot zeigt die App nach der Animation:

[![App-Screenshot nach dem Abschluss der Animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Mit Szenen – diese werden im nächsten Abschnitt behandelt – erzielen Sie eine bessere Steuerung der Übergänge.

#### <a name="android-scenes"></a>Szenen in Android

[Szenen](xref:Android.Transitions.Scene) wurde als Teil des Übergangsframeworks eingeführt, um Entwicklern eine bessere Steuerung der Animationen zu ermöglichen. Szenen erstellen einen dynamischen Bereich auf der Benutzeroberfläche: Entwickler geben einen Container und verschiedene Versionen bzw. „Szenen“ für die XML-Inhalte im Container an, und Android führt alle notwendigen Vorgänge aus, um die Übergänge zwischen den Szenen zu animieren. Mit Android-Szenen können Sie mit sehr wenig Entwicklungsaufwand komplexe Animationen erstellen.

Das statische Benutzeroberflächenelement, das die dynamischen Inhalte enthält, wird als *Container* oder *Szenenbasis* bezeichnet. Das folgende Beispiel verwendet Android Designer, um ein `RelativeLayout` namens `container` zu erstellen:

[![Verwenden von Android Designer zum Erstellen eines RelativeLayout-Containers](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

Das Beispiellayout definiert auch eine Schaltfläche namens `sceneButton` unterhalb von `container`. Mit dieser Schaltfläche wird der Übergang ausgelöst.

Die dynamischen Inhalte im Container erfordern zwei neue Android-Layouts. Diese Layouts geben nur den Code *innerhalb* des Containers an.
Der folgende Beispielcode definiert ein Layout namens *Scene1*, das zwei Textfelder mit dem Inhalt „Kit“ bzw. „Kat“ enthält, sowie ein zweites Layout namens *Scene2*, das dieselben Textfelder in umgekehrter Reihenfolge enthält. Der XML-Code lautet wie folgt:

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

Dieses Beispiel verwendet `merge`, um den Ansichtscode zu verkürzen und die Ansichtshierarchie zu vereinfachen. Weitere Informationen zu `merge`-Layouts finden Sie [hier](https://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Zum Erstellen einer Szene wird [`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*) aufgerufen, und dabei werden das Containerobjekt, die Ressourcen-ID der Layoutdatei der Szene sowie der aktuelle `Context` übergeben, wie im folgenden Codebeispiel veranschaulicht:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Durch Klicken auf die Schaltfläche erfolgt ein Wechsel zwischen den beiden Szenen, den Android mit den Werten für den Standardübergang animiert:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Der folgende Screenshot zeigt die Szene vor der Animation:

[![Screenshot der App vor dem Start der Animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Der folgende Screenshot zeigt die Szene nach der Animation:

[![Screenshot der App nach dem Start der Animation](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)

> [!NOTE]
> Es gibt einen [bekannten Fehler](https://code.google.com/p/android/issues/detail?id=62450) in der Übergangsbibliothek von Android, der dazu führt, dass mithilfe von `GetSceneForLayout` erstellte Szenen unterbrochen werden, wenn ein Benutzer ein zweites Mal durch eine Aktivität navigiert. [Hier](http://www.doubleencore.com/2013/11/new-transitions-framework/) finden Sie eine Java-Problemumgehung.

##### <a name="custom-transitions-in-scenes"></a>Benutzerdefinierte Übergänge in Szenen

Ein benutzerdefinierter Übergang kann in einer XML-Ressourcendatei im Verzeichnis `transition` unter `Resources` definiert werden, wie im folgenden Screenshot gezeigt:

[![Speicherort der Datei „transition.xml	“ im Verzeichnis „Resources/transition“](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

Das folgende Codebeispiel definiert einen Übergang, der 5 Sekunden lang animiert wird und den [OvershootInterpolator](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html) verwendet:

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Der Übergang wird mit dem [TransitionInflater](xref:Android.Transitions.TransitionInflater) in der Aktivität erstellt, wie im folgenden Code veranschaulicht:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Der neue Übergang wird dann dem `Go`-Aufruf hinzugefügt, der die Animation startet:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Durchsichtige Benutzeroberflächenelemente

KitKat ermöglicht Ihnen mit optionalen durchsichtigen Status- und Navigationsleisten eine bessere Steuerung der Designs in Ihrer App. Sie können die Durchsichtigkeit von Systemelementen auf der Benutzeroberfläche in derselben XML-Datei ändern, die Sie auch verwenden, um Ihr Android-Theme zu definieren. KitKat führt die folgenden Eigenschaften ein:

- `windowTranslucentStatus` – bei Festlegung auf „true“ wird die obere Statusleiste durchsichtig angezeigt.

- `windowTranslucentNavigation` – bei Festlegung auf „true“ wird untere obere Statusleiste durchsichtig angezeigt.

- `fitsSystemWindows` – wenn die obere oder untere Leiste als durchsichtig festgelegt wird, werden Inhalte unter den transparenten Benutzeroberflächenelementen standardmäßig verschoben. Durch Festlegen dieser Eigenschaft auf `true` lässt sich ganz einfach verhindern, dass Inhalte mit den durchsichtigen Systemelementen auf der Benutzeroberfläche überlappen.

Der folgende Code definiert ein Theme mit durchsichtigen Status- und Navigationsleisten:

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

Der folgende Screenshot zeigt das obige Theme mit durchsichtigen Status- und Navigationsleisten:

[![Beispielscreenshot der App durchsichtigen Status- und Navigationsleisten](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content"></a>

## <a name="user-content"></a>Benutzerinhalte

### <a name="storage-access-framework"></a>Storage Access Framework

Das Storage Access Framework (SAF) ist eine neue Möglichkeit für Benutzer zur Interaktion mit gespeicherten Inhalten wie Bildern, Videos und Dokumenten. Statt Benutzern ein Dialogfeld zur Auswahl einer Anwendung für die Verarbeitung von Inhalten anzuzeigen, öffnet KitKat eine neue Benutzeroberfläche, über die Benutzer an einem einzigen aggregierten Speicherort auf ihre Daten zugreifen können. Wenn der entsprechende Inhalt ausgewählt wurde, kehrt der Benutzer zu der Anwendung zurück, die den Inhalt angefordert hat, und die App-Ausführung wird normal fortgesetzt.

Diese Änderung erfordert zwei Aktionen seitens des Entwicklers: Erstens müssen Apps, die Inhalte von Anbietern benötigen, auf diese neue Art der Inhaltsanforderung aktualisiert werden. Zweitens müssen Anwendungen, die Daten in einen `ContentProvider` schreiben, so geändert werden, dass sie das neue Framework verwenden. Beide Szenarien benötigen die neue [`DocumentsProvider`](xref:Android.Provider.DocumentsProvider)-
API.

#### <a name="documentsprovider"></a>DocumentsProvider

In KitKat werden Interaktionen mit `ContentProviders` über die `DocumentsProvider`-Klasse abstrahiert. Das bedeutet, dass es für das SAF keine Rolle spielt, wo die Daten physisch gespeichert sind, solange über die `DocumentsProvider`-API darauf zugegriffen werden kann. Lokale Anbieter, Clouddienste und externe Speichergeräte verwenden alle dieselbe Schnittstelle und werden auf dieselbe Weise behandelt – so profitieren sowohl Benutzer als auch Entwickler von einer zentralen Stelle für die Interaktion mit Benutzerinhalten.

In diesem Abschnitt wird erläutert, wie Sie Inhalte über das Storage Access Framework laden und speichern.

#### <a name="request-content-from-a-provider"></a>Anfordern von Inhalten von einem Anbieter

Sie können KitKat über die SAF-Benutzeroberfläche mit der Absicht `ActionOpenDocument` mitteilen, dass Inhalte abgerufen werden sollen. Das bedeutet, dass eine Verbindung mit allen Inhaltsanbietern hergestellt werden soll, die für das Gerät verfügbar sind. Sie können dieser Absicht durch Angabe von `CategoryOpenable` einen Filter hinzufügen, der dafür sorgt, dass nur Inhalte, die geöffnet werden können (also zugängliche und verwendbare Inhalte), zurückgegeben werden. KitKat ermöglicht auch die Filterung mit dem `MimeType`. Der folgende Code beispielsweise filtert durch Angabe des `MimeType` des Bilds nach Bildergebnissen:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Durch Aufrufen von `StartActivityForResult` wird die SAF-Benutzeroberfläche gestartet, die der Benutzer dann durchsuchen kann, um ein Bild auszuwählen:

[![Beispielscreenshot einer App, die das Storage Access Framework zum Suchen nach einem Bild verwendet](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

Nachdem der Benutzer ein Bild ausgewählt hat, gibt `OnActivityResult` den `Android.Net.Uri` der ausgewählten Datei zurück. Das folgende Codebeispiel zeigt die Bildauswahl des Benutzers:

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

#### <a name="write-content-to-a-provider"></a>Schreiben von Inhalten in einem Anbieter

Zusätzlich zum Laden von Inhalten aus der SAF-Benutzeroberfläche ermöglicht KitKat auch das Speichern von Inhalten in jeden `ContentProvider`, der die `DocumentProvider`-API implementiert. Zum Speichern von Inhalten wird ein `Intent`-Element mit `ActionCreateDocument` verwendet:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Das oben stehende Codebeispiel lädt die SAF-Benutzeroberfläche, ermöglicht dem Benutzer die Änderung des Dateinamens und die Auswahl eines Verzeichnisses zum Speichern der neuen Datei:

[![Screenshot der Änderung des Dateinamens in „NewDoc“ im Verzeichnis „Downloads“ durch einen Benutzer](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Wenn der Benutzer auf **Speichern** tippt, wird der `Android.Net.Uri` der neu erstellten Datei an `OnActivityResult` übergeben. Der Zugriff erfolgt mit `data.Data`. Der URI kann zum Streamen von Daten in die neue Datei verwendet werden:

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

Beachten Sie, dass [`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)
einen `System.IO.Stream` zurückgibt, sodass der gesamte Streamingprozess in .NET geschrieben werden kann.

Weitere Informationen zum Laden, Erstellen und Bearbeiten von Inhalten mit dem Storage Access Framework finden Sie in der [Android-Dokumentation zum Storage Access Framework](https://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Drucken

KitKat vereinfacht das Drucken mit der Einführung von [Druckdiensten](xref:Android.PrintServices) und `PrintManager`. KitKat ist auch die erste API-Version, die die [Google Cloud Print-APIs](https://developers.google.com/cloud-print/) über die [Google Cloud Print-App](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint) vollständig nutzen kann.
Die meisten Geräte, die mit KitKat ausgeliefert werden, laden die Google Cloud Print-App und das [HP Druckdienst-Plug-In](https://play.google.com/store/apps/details?id=com.hp.android.printservice) automatisch herunter, wenn sie zum ersten Mal eine Verbindung mit einem WLAN herstellen. Benutzer können die Druckeinstellungen ihres Geräts unter **Einstellungen > System > Drucken** überprüfen:

[![Beispielscreenshot des Bildschirms mit Druckeinstellungen](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)

> [!NOTE]
> Die Druck-APIs sind zwar standardmäßig für die Verwendung mit Google Cloud Print eingerichtet, aber Android ermöglicht es Entwicklern weiterhin, Druckinhalte über die neuen APIs vorzubereiten und sie dann an andere Anwendungen zu senden, die den Druck verarbeiten.

#### <a name="printing-html-content"></a>Drucken von HTML-Inhalten

KitKat erstellt automatisch einen [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter) für eine Webansicht mit `WebView.CreatePrintDocumentAdapter`. Das Drucken von Webinhalten ist ein koordinierter Vorgang mit zwei Elementen: Zum einen ein [`WebViewClient`](xref:Android.Webkit.WebViewClient), der darauf wartet, dass HTML-Inhalte geladen werden, und die Aktivität darüber informiert, dass die Druckoption im Optionsmenü zur Verfügung gestellt werden soll; zum anderen die Aktivität, die darauf wartet, dass der Benutzer die Druckoption auswählt, und `Print` im `PrintManager` aufruft. Im folgenden Abschnitt wird das grundlegende Setup erläutert, das zum Drucken von HTML-Inhalten auf dem Bildschirm erforderlich ist.

Beachten Sie, dass zum Laden und Drucken von Webinhalten die Berechtigung „Internet“ benötigt wird:

[![Festlegen der Berechtigung „Internet“ in den App-Optionen](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Menüelement „Drucken“

Die Druckoption wird in der Regel im [Optionsmenü](https://developer.android.com/guide/topics/ui/menus.html#options-menu) der Aktivität angezeigt.
Über das Optionsmenü können Benutzer Aktionen in einer Aktivität ausführen. Es befindet sich in der oberen rechten Ecke und sieht so aus:

[![Beispielscreenshot des Menüelements „Drucken“ in der oberen rechten Ecke des Bildschirms](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)

Zusätzliche Menüelemente können im Verzeichnis *menu* unter *Resources* definiert werden. Der folgende Code definiert ein Beispielmenüelement namens [Print](xref:Android.Print.PrintManager):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Die Interaktion mit dem Optionsmenü in der Aktivität erfolgt über die Methoden `OnCreateOptionsMenu` und `OnOptionsItemSelected`.
In `OnCreateOptionsMenu` werden neue Menüelemente, wie z. B. die Druckoption, aus dem Ressourcenverzeichnis *menu* hinzugefügt.
`OnOptionsItemSelected` lauscht auf die Benutzerauswahl der Druckoption aus dem Menü und startet den Druckvorgang:

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

Der oben stehende Code definiert auch eine Variable namens `dataLoaded`, um den Status der HTML-Inhalte nachzuverfolgen. Der `WebViewClient` legt diese Variable auf „true“ fest, wenn sämtliche Inhalte geladen wurden, sodass die Aktivität weiß, dass das Menüelement „Drucken“ zum Optionsmenü hinzugefügt werden muss.

##### <a name="webviewclient"></a>WebViewClient

Der `WebViewClient` soll sicherstellen, dass die Daten im `WebView` vollständig geladen sind, bevor die Druckoption im Menü angezeigt wird, und verwendet dafür die `OnPageFinished`-Methode. `OnPageFinished` lauscht auf den Abschluss des Ladevorgangs für die Webinhalte und informiert die Aktivität darüber, dass das Optionsmenü mit `InvalidateOptionsMenu` neu erstellt werden muss:

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

`OnPageFinished` legt auch den `dataLoaded`-Wert auf `true` fest, sodass `OnCreateOptionsMenu` das Menü mit der Druckoption neu erstellen kann.

##### <a name="printmanager"></a>PrintManager

Das folgende Codebeispiel druckt die Inhalte eines `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` akzeptiert Folgendes als Argument: einen Name für den Druckauftrag (in diesem Beispiel „MyWebPage“), einen [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter),
der das Druckdokument aus den Inhalten generiert, und [`PrintAttributes`](xref:Android.Print.PrintAttributes)
(`null` im oben aufgeführten Beispiel). Zwar können die Standardattribute die meisten Szenarien verarbeiten, aber Sie können `PrintAttributes` angeben, um das Layout der Inhalte auf der gedruckten Seite einzurichten.

Durch Aufrufen von `Print` wird die Benutzeroberfläche für den Druckvorgang aufgerufen, die Optionen für den Druckauftrag enthält. Auf der Benutzeroberfläche können Benutzer die HTML-Inhalte drucken oder in eine PDF-Datei speichern, wie in den folgenden Screenshots gezeigt:

[![Screenshot von PrintHtmlActivity mit dem Menü „Drucken“](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Screenshot von PrintHtmlActivity mit dem Menü „Als PDF speichern“](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware"></a>

## <a name="hardware"></a>Hardware

KitKat fügt verschiedene APIs für neue Gerätefunktionen hinzu. Die wichtigsten dieser Funktionen sind die hostbasierte Kartenemulation und der neue `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Hostbasierte Kartenemulation für NFC

Mit der hostbasierten Kartenemulation (Host-Based Card Emulation, HCE) können Anwendungen sich wie NFC-Karten oder NFC-Kartenleser verhalten, ohne den proprietären Secure Element-Chip des Anbieters zu benötigen. Bevor Sie HCE einrichten, stellen Sie mit `PackageManager.HasSystemFeature` sicher, dass das Feature auf dem Gerät verfügbar ist:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

Für HCE müssen sowohl das HCE-Feature als auch die Berechtigung `Nfc` beim `AndroidManifest.xml` der Anwendung registriert sein:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Festlegen der NFC-Berechtigung in den App-Optionen](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Damit das Feature funktioniert, muss HCE im Hintergrund ausgeführt werden können und muss starten, wenn ein Benutzer eine NFC-Transaktion durchführt, auch wenn die Anwendung, die HCE verwendet, nicht ausgeführt wird. Dies lässt sich erreichen, indem der HCE-Code als `Service` geschrieben wird. Ein HCE-Dienst implementiert die `HostApduService`-Schnittstelle, die wiederum die folgenden Methoden implementiert:

- *ProcessCommandApdu*: Zwischen dem NFC-Leser und dem HCE-Dienst wird eine Anwendungsprotokoll-Dateneinheit (Application Protocol Data Unit, APDU) gesendet. Diese Methode verwendet eine APDU des Lesers und gibt als Antwort eine Dateneinheit zurück.

- *OnDeactivated*: Der `HostAdpuService` wird deaktiviert, wenn der HCE-Dienst nicht mehr mit dem NFC-Leser kommuniziert.

Ein HCE-Dienst muss auch beim Manifest der Anwendung registriert und mit den richtigen Berechtigungen, Absichtsfiltern und Metadaten versehen werden. Der folgende Code ist ein Beispiel für einen `HostApduService`, der über das `Service`-Attribut beim Android-Manifest registriert ist (weitere Informationen zu Attributen finden Sie im Xamarin-Leitfaden [Arbeiten mit dem Android-Manifest](~/android/platform/android-manifest.md)):

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

Der oben genannte Dienst ermöglicht dem NFC-Leser die Interaktion mit der Anwendung, aber der NFC-Leser erhält weiterhin keine Informationen dazu, ob dieser Dienst die NFC-Karte emuliert, die gescannt werden soll. Damit der NFC-Leser den Dienst identifizieren kann, weisen Sie dem Dienst eine eindeutige *Anwendungs-ID (AID)* zu. Die AID wird zusammen mit weiteren Metadaten zum HCE-Dienst in einer XML-Ressourcendatei angegeben, die mit dem `MetaData`-Attribut registriert wird (siehe Codebeispiel oben). Diese Ressourcendatei gibt mindestens einen AID-Filter an – eindeutige Bezeichnerzeichenfolgen im Hexadezimalformat, die den AIDs mindestens eines NFC-Lesegeräts entsprechen:

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

Zusätzlich zu AID-Filtern stellt die XML-Ressourcendatei auch eine Beschreibung des HCE-Dienst für den Benutzer bereit, gibt eine AID-Gruppe an (Zahlungsanwendung oder „sonstige Anwendung“) und stellt im Fall einer Zahlungsanwendung ein 260x96 px großes Banner bereit, das dem Benutzer angezeigt wird.

Das oben beschriebene Setup stellt auch die grundlegenden Bausteine für eine Anwendung bereit, die eine NFC-Karte emuliert. Zur Konfiguration von NFC selbst sind zahlreiche weitere Schritte sowie weitere Tests erforderlich. Weitere Informationen zur hostbasierten Kartenemulation finden Sie im [Android-Dokumentationsportal](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Weitere Informationen zur Verwendung von NFC mit Xamarin finden Sie in den [Xamarin-Beispielen für NFC](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Sensoren

KitKat bietet über einen [`SensorManager`](xref:Android.Hardware.SensorManager) Zugriff auf die Sensoren eines Geräts.
Der `SensorManager` ermöglicht es dem Betriebssystem, Anwendungen Sensorinformationen in Batches bereitzustellen und so Akkuleistung zu sparen.

KitKat wird auch mit zwei neuen Sensortypen zum Zählen von Schritten des Benutzers ausgeliefert. Diese basieren auf einem Beschleunigungsmesser und umfassen Folgendes:

- *StepDetector*: Die App wird benachrichtigt bzw. aktiviert, wenn der Benutzer einen Schritt geht, und der Detektor stellt einen Uhrzeitwert für diesen Schritt bereit.

- *StepCounter*: Verfolgt die Anzahl von Schritten nach, die der Benutzer seit der Registrierung gegangen ist. Dies gilt bis zum *nächsten Neustart des Geräts*.

Der folgende Screenshot zeigt den Schrittzähler in Aktion:

[![Screenshot der SensorsActivity-App mit Anzeige eines Schrittzählers](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Sie können einen `SensorManager` erstellen, indem Sie `GetSystemService(SensorService)` aufrufen und das Ergebnis in einen `SensorManager` umwandeln. Um den Schrittzähler zu verwenden, rufen Sie `GetDefaultSensor` im `SensorManager` auf. Verwenden Sie zum Registrieren des Sensors und Lauschen auf Änderungen an der Schrittzahl die Schnittstelle [`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener),
wie im folgenden Codebeispiel veranschaulicht:

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

`OnSensorChanged` wird aufgerufen, wenn die Schrittzahl aktualisiert wird, während die App sich im Vordergrund befindet. Wenn die App in den Hintergrund verlagert wird oder das Gerät in den Standbymodus wechselt, wird `OnSensorChanged` nicht aufgerufen. Die Schritte werden jedoch weiterhin gezählt, bis `UnregisterListener` aufgerufen wird.

Denken Sie daran, dass *der Schrittzahlwert über alle Anwendungen hinweg kumulativ ist, die den Sensor registrieren*. Das bedeutet Folgendes: Selbst wenn ein Benutzer Ihre App deinstalliert und neu installiert und die `count`-Variable beim App-Start mit dem Wert 0 initialisiert, ist der vom Sensor gemeldete Wert weiterhin die Gesamtzahl der Schritte, die der Benutzer gegangen ist, während der Sensor registriert war (bei Ihrer App oder einer anderen). Sie können verhindern, dass Ihre App Schritte zum Schrittzähler hinzufügt, indem Sie `UnregisterListener` im `SensorManager` aufrufen, wie im folgenden Code veranschaulicht:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Durch Neustarten des Geräts wird die Schrittzahl auf 0 zurückgesetzt. Für Ihre App ist zusätzlicher Code erforderlich, um sicherzustellen, dass die richtige Zählung für die App gemeldet wird, unabhängig davon, ob andere Anwendungen den Sensor oder Status des Geräts verwenden.

> [!NOTE]
> Die API für die Erfassung und Zählung von Schritten wird zwar mit KitKat ausgeliefert, aber nicht alle Smartphones sind mit einem entsprechenden Sensor ausgestattet. Mit `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);` können Sie überprüfen, ob ein Sensor verfügbar ist, oder sich vergewissern, dass der Rückgabewert von `GetDefaultSensor` nicht `null` lautet.

<a name="developer_tools"></a>

## <a name="developer-tools"></a>Entwicklertools

### <a name="screen-recording"></a>Bildschirmaufzeichnung

KitKat enthält neue Funktionen für Bildschirmaufzeichnungen, sodass Entwickler Anwendung in Aktion aufzeichnen können. Bildschirmaufzeichnungen sind über den [Android Debug Bridge-Client (ADB)](https://developer.android.com/tools/help/adb.html) verfügbar, der als Teil des Android SDK heruntergeladen werden kann.

Um Vorgänge auf Ihrem Bildschirm aufzuzeichnen, stellen Sie eine Verbindung mit Ihrem Gerät her. Dann suchen Sie Ihre Android SDK-Installation, navigieren zum Verzeichnis **platform-tools** und führen den **adb**-Client aus:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Der Befehl oben zeichnet ein 3-minütiges Standardvideo mit der Standardauflösung von 4 Mbit/s auf. Um die Länge zu bearbeiten, fügen Sie das Flag *--time-limit* hinzu.
Um die Auflösung zu ändern, fügen Sie das Flag *--bit-rate* hinzu. Der folgende Befehl zeichnet ein Video mit 8 Mbit/s und einer Länge von einer Minute auf:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Das Video wird auf dem Gerät in der Galerie angezeigt, wenn die Aufzeichnung beendet ist.

## <a name="other-kitkat-additions"></a>Weitere Ergänzungen in KitKat

Zusätzlich zu den oben beschriebenen Änderungen ermöglicht KitKat Folgendes:

- *Verwenden des gesamten Bildschirms*: KitKat führt einen neuen [plastischen Modus](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) zum Durchsuchen von Inhalten, zum Spielen und zum Ausführen anderer Anwendungen ein, bei denen ein Vollbildmodus nützlich ist.

- *Anpassen von Benachrichtigungen*: Mit dem [`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService) können zusätzliche Informationen zu Systembenachrichtigungen abgerufen werden
  . Damit können Sie die Informationen in Ihrer App auf andere Weise darstellen.

- *Spiegeln von zeichenbare Ressourcen*: Für zeichenbare Ressourcen steht ein neues [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)-
  Attribut zur Verfügung, mit dem das System angewiesen wird, eine gespiegelte Version von Bildern für Links-nach-Rechts-Layouts zu erstellen.

- *Anhalten von Animationen*: Ermöglicht das Anhalten und Fortsetzen von Animationen, die hiermit erstellt wurden: [`Animator`](xref:Android.Animation.Animator)
  -Klasse.

- *Lesen von dynamisch geändertem Text*: Kennzeichnen Sie Teile der Benutzeroberfläche, die dynamisch mit neuem Text aktualisiert werden, als „Liveregionen“. Verwenden Sie hierfür das neue [`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)-
  Attribut, sodass im Barrierefreiheitsmodus automatisch der neue Text vorgelesen wird.

- *Erweitern von Audiofunktionen*: Erhöhen Sie die Lautstärke von Audiospuren mit dem [`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer)
  , suchen Sie den Peak- und den RMS-Wert eines Audiostreams mit der [`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms)-
  Klasse, und rufen Sie Informationen aus einem [Audiozeitstempel](xref:Android.Media.AudioTimestamp) ab, um die Synchronisierung von Bild und Ton zu unterstützen.

- *Synchronisieren des ContentResolver in benutzerdefinierten Intervallen*: KitKat erhöht die Variabilität in Bezug auf den Zeitpunkt, zu dem eine Synchronisierungsanforderung ausgeführt wird. Synchronisieren Sie einen `ContentResolver` zu einem benutzerdefinierten Zeitpunkt oder in einem benutzerdefinierten Intervall, indem Sie `ContentResolver.RequestSync` aufrufen und eine `SyncRequest` übergeben.

- *Unterscheiden zwischen Controllern*: In KitKat werden Controllern eindeutige ganzzahlige Bezeichner zugewiesen, auf die über die `ControllerNumber`-Eigenschaft eines Geräts zugegriffen werden kann. So lassen sich beispielsweise Spieler in einem Spiel leichter auseinanderhalten.

- *Fernbedienung*: Mit einigen wenigen Änderungen an der Hardware und Software ermöglicht KitKat es, ein Gerät, das über einen Infrarotsender verfügt, mithilfe des `ConsumerIrService` in eine Fernbedienung umzuwandeln. Die Interaktion mit Peripheriegeräten erfolgt über die neuen [`RemoteController`](xref:Android.Media.RemoteController)-
  APIs.

Weitere Informationen zu den oben genannten API-Änderungen finden Sie in der Übersicht zu [Android 4.4-APIs](https://developer.android.com/about/versions/android-4.4.html).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige der neuen APIs vorgestellt, die in Android 4.4 (API-Ebene 19) verfügbar sind, und einige bewährte Methoden beim Migrieren einer Anwendung zu KitKat erläutert. Es wurden Änderungen an den APIs beschrieben, die die Benutzerfreundlichkeit betreffen, wie beispielsweise das *Übergangsframework* und neue Optionen für *Themes*. Danach wurden das *Storage Access Framework* und die `DocumentsProvider`-Klasse sowie die neuen *Druck-APIs* vorgestellt. Die *hostbasierte Kartenemulation für NFC* wurde ebenso erläutert wie die Arbeit mit *Sensoren mit geringem Stromverbrauch*, einschließlich zweier neuer Sensoren für die Schrittzählung. Zum Schluss wurde das Erfassen von Echtzeitdemonstrationen von Anwendungen per *Bildschirmaufzeichnung* beschrieben, und es wurde eine detaillierte Liste weiterer Änderungen und Ergänzungen der KitKat-API bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [KitKat-Beispiel](https://docs.microsoft.com/samples/xamarin/monodroid-samples/kitkat)
- [Android 4.4-APIs](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
