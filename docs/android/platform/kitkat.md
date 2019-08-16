---
title: Kitkat-Features
description: Android 4,4 (KitKat) ist mit einer erlernender der Features für Benutzer und Entwickler geladen. In diesem Leitfaden werden einige dieser Features hervorgehoben, und es werden Codebeispiele und Implementierungsdetails vorgestellt, die Ihnen helfen, KitKat optimal zu nutzen.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 3e68ac0a39d3268ce7c84f583c64b247e9f82362
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524183"
---
# <a name="kitkat-features"></a>Kitkat-Features

_Android 4,4 (KitKat) ist mit einer erlernender der Features für Benutzer und Entwickler geladen. In diesem Leitfaden werden einige dieser Features hervorgehoben, und es werden Codebeispiele und Implementierungsdetails vorgestellt, die Ihnen helfen, KitKat optimal zu nutzen._

## <a name="overview"></a>Übersicht

Android 4,4 (API-Ebene 19), auch bekannt als "Kitkat", wurde in spätem 2013 veröffentlicht. Kitkat bietet eine Reihe von neuen Features und Verbesserungen, einschließlich:

- [Benutzer](#user_experience) Darstellung &ndash; Einfache Animationen mit Übergangs Framework, durchlässigem Status und Navigationsleisten sowie der Vollbildmodus für den Vollbildmodus sorgen für einen besseren Benutzerkomfort.

- [Benutzer Inhalt](#user_content) &ndash; Die Benutzerdatei Verwaltung wurde mit dem Speicherzugriffs Framework vereinfacht; das Drucken von Bildern, Websites und anderen Inhalten ist dank verbesserter Druck-APIs einfacher.

- [Hardware](#hardware) Schalten Sie jede app in eine NFC-Karte mit NFC-Host basierter Karten Emulation ein, `SensorManager` und führen Sie Hochleistungs Sensoren mit aus. &ndash;

- [Entwicklertools](#developer_tools) &ndash; Screencast Anwendungen in Aktion mit dem Android Debug Bridge Client, verfügbar als Teil der Android SDK.


Dieser Leitfaden enthält Anleitungen zum Migrieren einer vorhandenen xamarin. Android-Anwendung zu KitKat sowie eine allgemeine Übersicht über KitKat für xamarin. Android-Entwickler.

## <a name="requirements"></a>Anforderungen

Zum Entwickeln von xamarin. Android-Anwendungen mit KitKat benötigen Sie *xamarin. Android 4.11.0* oder höher und Android 4,4 (API-Ebene 19 Android SDK), wie im folgenden Screenshot veranschaulicht:

[![Auswählen von Android 4,4 im Android SDK-Manager](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Migrieren Ihrer APP zu KitKat

Dieser Abschnitt enthält einige Elemente der ersten Reaktion, mit denen Sie vorhandene Anwendungen auf Android 4,4 umstellen können.

### <a name="check-system-version"></a>System Version überprüfen

Wenn eine Anwendung mit älteren Versionen von Android kompatibel sein muss, stellen Sie sicher, dass Sie jeden KitKat-spezifischen Code in eine System Versions Überprüfung einschließen, wie im folgenden Codebeispiel veranschaulicht:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Alarm Batch Verarbeitung

Android verwendet Alarmdienste zum Aktivieren einer APP im Hintergrund zu einem bestimmten Zeitpunkt. Kitkat führt dies schrittweise durch die Batch Verarbeitung von Alarmen, um die Leistung aufrechtzuerhalten. Dies bedeutet, dass KitKat, anstatt jede APP zu einem exakten Zeitpunkt zu reaktivieren, es bevorzugt, mehrere Anwendungen zu gruppieren, die während des gleichen Zeitintervalls für die Aktivierung registriert sind, und Sie gleichzeitig zu reaktivieren.
Um Android zu informieren, dass eine APP innerhalb eines bestimmten Zeitraums aktiviert werden `SetWindow` soll, [`AlarmManager`](xref:Android.App.AlarmManager)müssen Sie für den Aufruf von durchführen, um die minimale und maximale Zeit in Millisekunden zu übergeben, die vergehen kann, bevor die APP reaktiviert wird, und den Vorgang, der bei der Aktivierung durchgeführt werden kann.
Der folgende Code enthält ein Beispiel für eine Anwendung, die zwischen einer halben Stunde und einer Stunde ab dem Zeitpunkt der Festlegung des Fensters aktiviert werden muss:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Verwenden `SetExact`Sie zum Fortsetzen der Reaktivierung einer APP zu einem exakten Zeitpunkt, und geben Sie dabei die genaue Zeit an, zu der die APP reaktiviert werden soll, und den auszuführenden Vorgang:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

Kitkat ermöglicht nicht mehr das Festlegen eines exakten Wiederholungs Alarms. Anwendungen, die verwenden[`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*)
und erfordern genaue Alarme, damit Sie funktionieren, müssen Sie nun jeden Alarm manuell auslösen.

### <a name="external-storage"></a>Externer Speicher

Externer Speicher ist nun in zwei Typen unterteilt: Speicher, der für Ihre Anwendung eindeutig ist, und Daten, die von mehreren Anwendungen gemeinsam genutzt werden. Für das Lesen und schreiben an den spezifischen Speicherort Ihrer APP im externen Speicher sind keine besonderen Berechtigungen erforderlich. Die Interaktion mit Daten im freigegebenen Speicher erfordert `READ_EXTERNAL_STORAGE` jetzt `WRITE_EXTERNAL_STORAGE` die Berechtigung oder. Die beiden Typen können als solche klassifiziert werden:

- Wenn Sie einen Datei `Context` -oder Verzeichnispfad abrufen, indem Sie eine Methode für aufrufen, z. b.[`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
   noch[`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs)
   - Ihre APP erfordert keine zusätzlichen Berechtigungen.

- Wenn Sie einen Datei-oder Verzeichnispfad abrufen, indem Sie auf eine Eigenschaft zugreifen oder eine `Environment` Methode für aufrufen, z. b.[`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
   noch[`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*)
   , Ihre APP erfordert die `READ_EXTERNAL_STORAGE` - `WRITE_EXTERNAL_STORAGE` Berechtigung oder die-Berechtigung.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE`impliziert die `READ_EXTERNAL_STORAGE` -Berechtigung, daher sollten Sie immer nur eine Berechtigung festlegen müssen.

### <a name="sms-consolidation"></a>SMS-Konsolidierung

Kitkat vereinfacht das Messaging für den Benutzer, indem alle SMS-Inhalte in einer vom Benutzer ausgewählten Standardanwendung aggregierte werden. Der Entwickler ist dafür verantwortlich, die APP als Standard-Messaging Anwendung auswählbar zu machen, und verhält sich entsprechend im Code und im Leben, wenn die Anwendung nicht ausgewählt ist. Weitere Informationen zum Übergang ihrer SMS-App zu KitKat finden Sie im Handbuch zum [Bereitstellung von SMS-Apps für KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) von Google.

### <a name="webview-apps"></a>WebView-apps

[WebView](xref:Android.Webkit.WebView) hat eine Überarbeitung in KitKat erhalten. Die größte Änderung ist die hinzugefügte Sicherheit beim Laden `WebView`von Inhalten in eine. Die meisten Anwendungen, die auf ältere API-Versionen abzielen, sollten erwartungsgemäß funktionieren. `WebView` das Testen von Anwendungen, die diese Klasse verwenden, wird dringend empfohlen Weitere Informationen zu den betroffenen WebView-APIs finden Sie in der Dokumentation Android [Migration to WebView in Android 4,4](https://developer.android.com/guide/webapps/migrating.html) .

<a name="user_experience" />

## <a name="user-experience"></a>Benutzerfreundlichkeit

Kitkat verfügt über mehrere neue APIs, um die Benutzeroberfläche zu verbessern, einschließlich des neuen Übergangs Frameworks für die Handhabung von Eigenschafts Animationen und einer durchlässigen Benutzeroberflächen Option für das Design. Diese Änderungen werden im folgenden beschrieben.

### <a name="transition-framework"></a>Übergangs Framework

Das Übergangs Framework vereinfacht die Implementierung von Animationen. Kitkat ermöglicht das Ausführen einer einfachen Eigenschafts Animation mit nur einer Codezeile oder das Anpassen von Übergängen mithilfe von *Szenen*.

#### <a name="simple-property-animation"></a>Einfache Eigenschafts Animation

Die neue Android-Übergangs Bibliothek vereinfacht den Code hinter Eigenschafts Animationen. Das Framework ermöglicht es Ihnen, einfache Animationen mit minimalem Code auszuführen. Im folgenden Codebeispiel wird z. b. verwendet.[`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*)
So animieren Sie das Einblenden `TextView`und Ausblenden eines:

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

Im obigen Beispiel wird das Übergangs Framework verwendet, um einen automatischen Standard Übergang zwischen den veränderlichen Eigenschafts Werten zu erstellen. Da die Animation durch eine einzelne Codezeile behandelt wird, können Sie diese mit älteren Versionen von Android problemlos kompatibel machen, indem Sie den `BeginDelayedTransition` -Befehl in eine System Versions Überprüfung umschließen. Weitere Informationen finden Sie im Abschnitt [Migrieren Ihrer APP zu KitKat](#Migrating_Your_App_to_KitKat) .

Der folgende Screenshot zeigt die APP vor der Animation:

[![Screenshot der APP vor dem Starten der Animation](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

Der folgende Screenshot zeigt die APP nach der Animation:

[![Screenshot der APP nach Abschluss der Animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Sie erhalten mehr Kontrolle über den Übergang mit Szenen, die im nächsten Abschnitt behandelt werden.

#### <a name="android-scenes"></a>Android-Szenen

[Szenen](xref:Android.Transitions.Scene) wurden als Teil des Übergangs Frameworks eingeführt, um dem Entwickler mehr Kontrolle über Animationen zu verschaffen. Szenen erstellen einen dynamischen Bereich in der Benutzeroberfläche: Sie geben einen Container und mehrere Versionen ("Szenen") für den XML-Inhalt innerhalb des Containers an, und Android erledigt den Rest der Arbeit, um die Übergänge zwischen den Kulissen zu animieren. Mit Android-Szenen können Sie komplexe Animationen mit minimaler Arbeit auf der Entwicklungsseite erstellen.

Das statische UI-Element, in dem sich der dynamische Inhalt befindet, wird als *Container* oder *Szenen Basis*bezeichnet. Im folgenden Beispiel wird der Android Designer verwendet, um `RelativeLayout` eine `container`mit dem Namen zu erstellen:

[![Verwenden des Android Designer zum Erstellen eines relativelayout-Containers](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

Das Beispiel Layout definiert auch eine Schaltfläche `sceneButton` , die `container`unterhalb von aufgerufen wird. Mit dieser Schaltfläche wird der Übergang auslöst.

Der dynamische Inhalt innerhalb des Containers erfordert zwei neue Android-Layouts. Diese Layouts geben nur den Code *im* Container an.
Der folgende Beispielcode definiert ein Layout namens *Scene1* , das zwei Textfelder enthält, die "Kit" und "Kat" enthalten, und ein zweites Layout mit dem Namen " *Scene2* ", das die gleichen Textfelder umgekehrt enthält. Der XML-Code lautet wie folgt:

 **Scene1. axml**:

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

 **Scene2. axml**:

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

Im obigen Beispiel wird `merge` verwendet, um den Ansichts Code zu verkürzen und die Ansichts Hierarchie zu vereinfachen. Weitere Informationen zu `merge` Layouts finden Sie [hier](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Eine Szene wird durch Aufrufen [`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*)von erstellt, indem das Container Objekt, die Ressourcen-ID der Layoutdatei der Szene und die aktuelle `Context`übergeben werden, wie im folgenden Codebeispiel veranschaulicht:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Wenn Sie auf die Schaltfläche klicken, wird zwischen den beiden Kulissen ein kippt erstellt, in dem Android mit den Standard Übergangs Werten animiert wird:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Der folgende Screenshot veranschaulicht die Szene vor der Animation:

[![Screenshot der APP vor dem Start der Animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Der folgende Screenshot veranschaulicht die Szene hinter der Animation:

[![Screenshot der APP nach Abschluss der Animation](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> In der Android-Übergangs Bibliothek gibt es einen [bekannten Fehler](https://code.google.com/p/android/issues/detail?id=62450) , der bewirkt, `GetSceneForLayout` dass Szenen, die mithilfe von erstellt wurden, unterbrechen, wenn ein Benutzer das zweite Mal durch eine Aktivität navigiert. Eine Java-Problem Umgehung wird [hier](http://www.doubleencore.com/2013/11/new-transitions-framework/)beschrieben.


##### <a name="custom-transitions-in-scenes"></a>Benutzerdefinierte Übergänge in Kulissen

Ein benutzerdefinierter Übergang kann in einer XML-Ressourcen Datei im `transition` Verzeichnis unter `Resources`definiert werden, wie im folgenden Screenshot veranschaulicht:

[![Speicherort der Datei "Transition. xml" unter Ressourcen/Übergangs Verzeichnis](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

Im folgenden Codebeispiel wird ein Übergang definiert, der fünf Sekunden lang animiert und den [überschritten-interpolators](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html)verwendet:

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Der Übergang wird in der-Aktivität mithilfe von [transitioninflater](xref:Android.Transitions.TransitionInflater)erstellt, wie im folgenden Code veranschaulicht:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Der neue Übergang wird dann dem-Befehl `Go` hinzugefügt, der die Animation startet:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Durchlässiges UI

Kitkat bietet Ihnen mehr Kontrolle über das Design Ihrer APP mit optionalen Status-und Navigationsleisten. Sie können die unter Lässigkeit von Systembenutzer Oberflächenelementen in derselben XML-Datei ändern, die Sie verwenden, um Ihr Android-Design zu definieren. Kitkat führt die folgenden Eigenschaften ein:

- `windowTranslucentStatus`: Wenn der Wert auf true festgelegt ist, wird die obere Statusleiste durchsichtig.

- `windowTranslucentNavigation`: Wenn diese Einstellung auf true festgelegt ist, wird die untere Navigationsleiste durchsichtig.

- `fitsSystemWindows`-Durch das Festlegen der oberen oder unteren Leiste auf "transcluent" wird der Inhalt unter den transparenten Benutzeroberflächen Elementen standardmäßig verschoben. Das Festlegen dieser Eigenschaft `true` auf ist eine einfache Methode, um zu verhindern, dass Inhalte mit den Benutzeroberflächen Elementen der überlappenden System überlappend


Der folgende Code definiert ein Design mit dem durchlässigem Status und Navigationsleisten:

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

Der folgende Screenshot zeigt das obige Design mit dem durchlässigem Status und den Navigationsleisten:

[![Screenshot der APP mit dem durchlässigem Status und Navigationsleisten](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>Benutzer Inhalt

### <a name="storage-access-framework"></a>Speicherzugriffs-Framework

Das Storage Access Framework (SAF) ist eine neue Möglichkeit für Benutzer, mit gespeicherten Inhalten wie Bildern, Videos und Dokumenten zu interagieren. Anstatt Benutzern ein Dialogfeld zum Auswählen einer Anwendung zur Handhabung von Inhalten zu präsentieren, öffnet KitKat eine neue Benutzeroberfläche, die Benutzern den Zugriff auf Ihre Daten an einem einzigen Aggregat Speicherort ermöglicht. Nachdem der Inhalt ausgewählt wurde, kehrt der Benutzer zur Anwendung zurück, die den Inhalt angefordert hat, und die APP wird normal fortgesetzt.

Diese Änderung erfordert zwei Aktionen auf der Entwicklerseite: Erstens müssen apps, die Inhalte von Anbietern benötigen, auf eine neue Art und Weise aktualisiert werden, um Inhalte anzufordern. Zweitens müssen Anwendungen, die Daten in einen `ContentProvider` schreiben, geändert werden, damit das neue Framework verwendet werden kann. Beide Szenarien sind abhängig von der neuen[`DocumentsProvider`](xref:Android.Provider.DocumentsProvider)
API.

#### <a name="documentsprovider"></a>Documentsprovider

In KitKat werden Interaktionen mit `ContentProviders` der `DocumentsProvider` -Klasse abstrahiert. Dies bedeutet, dass die Daten in der Datenbank nicht physisch verarbeitet werden, solange Sie über die `DocumentsProvider` API zugänglich ist. Lokale Anbieter, Clouddienste und externe Speichergeräte verwenden die gleiche Schnittstelle und werden auf die gleiche Weise behandelt, sodass der Benutzer und der Entwickler einen Ort für die Interaktion mit dem Inhalt des Benutzers bereitstellen können.

In diesem Abschnitt wird erläutert, wie Inhalt mit dem Speicherzugriffs Framework geladen und gespeichert wird.

#### <a name="request-content-from-a-provider"></a>Anforderungs Inhalt von einem Anbieter

Wir können KitKat darüber informieren, dass wir Inhalt mithilfe der SAF-Benutzeroberfläche mit `ActionOpenDocument` der Absicht auswählen möchten, was bedeutet, dass wir eine Verbindung mit allen Inhaltsanbietern herstellen möchten, die für das Gerät verfügbar sind. Sie können dieser Absicht einige Filter hinzufügen, indem `CategoryOpenable`Sie angeben. Dies bedeutet, dass nur Inhalte, die geöffnet werden können (d. h. barrierefreie, verwendbarer Inhalt), zurückgegeben werden. Kitkat ermöglicht außerdem das Filtern von Inhalten mit `MimeType`dem. Der folgende Code filtert z. b. nach Bild Ergebnissen, indem `MimeType`er das Image angibt:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Durch `StartActivityForResult` Aufrufen von wird die Benutzeroberfläche von SAF gestartet, mit der der Benutzer ein Bild auswählen kann:

[![Screenshot einer APP mit dem Speicherzugriffs Framework zum Durchsuchen eines Bilds](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

Nachdem der Benutzer ein Bild ausgewählt hat, `OnActivityResult` gibt den `Android.Net.Uri` der ausgewählten Datei zurück. Im folgenden Codebeispiel wird die Bildauswahl des Benutzers angezeigt:

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

#### <a name="write-content-to-a-provider"></a>Schreiben von Inhalt in einen Anbieter

Zusätzlich zum Laden von Inhalt aus der SAF-Benutzeroberfläche ermöglicht KitKat auch das Speichern von Inhalten `ContentProvider` in allen, `DocumentProvider` die die API implementieren. Beim Speichern von Inhalt `Intent` wird `ActionCreateDocument`ein mit verwendet:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Im obigen Codebeispiel wird die Benutzeroberfläche von SAF geladen, sodass der Benutzer den Dateinamen ändern und ein Verzeichnis auswählen kann, in dem die neue Datei gespeichert wird:

[![Screenshot des Benutzers, der den Dateinamen im Verzeichnis "Downloads" in "newdoc" ändert](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Wenn der Benutzer auf **Speichern**drückt `OnActivityResult` , wird der `Android.Net.Uri` der neu erstellten Datei, auf die mit `data.Data`zugegriffen werden kann, übermittelt. Der URI kann zum Streamen von Daten in die neue Datei verwendet werden:

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

Beachten Sie, dass[`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)
Gibt einen `System.IO.Stream`zurück, sodass der gesamte streamingprozess in .NET geschrieben werden kann.

Weitere Informationen zum Laden, erstellen und Bearbeiten von Inhalten mit dem Speicherzugriffs Framework finden Sie in der [Android-Dokumentation für das Storage Access Framework](https://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Drucken

Das Drucken von Inhalten wird in KitKat durch die Einführung der [Druckdienste](xref:Android.PrintServices) und `PrintManager`vereinfacht. Kitkat ist auch die erste API-Version, mit der die [Google Cloud Print Service-APIs](https://developers.google.com/cloud-print/) mithilfe der [Google Cloud Print-Anwendung](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)vollständig genutzt werden können.
Die meisten Geräte, die mit KitKat ausgeliefert werden, Laden automatisch Google Cloud Print APP und das [HP Print Service-Plug](https://play.google.com/store/apps/details?id=com.hp.android.printservice)-in herunter, wenn Sie zum ersten Mal eine Verbindung Benutzer können die Druckeinstellungen des Geräts überprüfen, indem Sie zu **Einstellungen > System > Drucken**navigieren:

[![Screenshot des Bildschirms "Druckeinstellungen"](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> Obwohl die Druck-APIs standardmäßig für die Verwendung mit Google Cloud Print eingerichtet sind, können Entwickler mit Android weiterhin Druck Inhalte mithilfe der neuen APIs vorbereiten und Sie an andere Anwendungen senden, um den Druck zu verarbeiten.



#### <a name="printing-html-content"></a>Drucken von HTML-Inhalten

Kitkat erstellt automatisch einen [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter) für eine Webansicht `WebView.CreatePrintDocumentAdapter`mit. Das Drucken von Webinhalten ist ein koordinierter [`WebViewClient`](xref:Android.Webkit.WebViewClient) Aufwand zwischen einem-Wert, der auf das Laden des HTML-Inhalts wartet und der Aktivität weiß, dass die Print-Option im Menü Optionen verfügbar ist, und die-Aktivität, die darauf wartet, dass der Benutzer die Print-Option auswählt und aufruft. `Print`auf dem `PrintManager`. In diesem Abschnitt wird das grundlegende Setup behandelt, das zum Drucken von HTML-Inhalten im Bildschirm erforderlich ist

Beachten Sie, dass für das Laden und Drucken von Webinhalten die Internet Berechtigung erforderlich ist:

[![Festlegen der Internet Berechtigung in den App-Optionen](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Menü Element "Drucken"

Die Option Drucken wird in der Regel im [Menü Optionen](https://developer.android.com/guide/topics/ui/menus.html#options-menu)der Aktivität angezeigt.
Das Menü Optionen ermöglicht Benutzern das Ausführen von Aktionen für eine Aktivität. Sie befindet sich in der rechten oberen Ecke des Bildschirms und sieht wie folgt aus:

[![Beispiel-Screenshot des Menü Elements "Drucken", das in der oberen rechten Ecke des Bildschirms angezeigt wird](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


Weitere Menü Elemente können im *Menü*Verzeichnis unter " *Ressourcen*" definiert werden. Der folgende Code definiert ein Beispiel Menü Element mit dem Namen " [Print](xref:Android.Print.PrintManager)":

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Die Interaktion mit dem Menü Optionen in der Aktivität erfolgt durch `OnCreateOptionsMenu` die `OnOptionsItemSelected` -Methode und die-Methode.
`OnCreateOptionsMenu`ist der Ort, an dem neue Menü Elemente, wie z. b. die Option Drucken, aus dem *Menü* Ressourcenverzeichnis hinzugefügt werden.
`OnOptionsItemSelected`lauscht auf den Benutzer, indem er im Menü die Option Drucken auswählt und mit dem Drucken beginnt:

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

Der obige Code definiert auch eine Variable namens `dataLoaded` , um den Status des HTML-Inhalts nachzuverfolgen. Der `WebViewClient` legt diese Variable auf true fest, wenn der gesamte Inhalt geladen wurde, sodass die-Aktivität weiß, dass das Menü Element Drucken dem Menü Optionen hinzugefügt werden kann.

##### <a name="webviewclient"></a>WebViewClient

Der Auftrag `WebViewClient` der besteht darin sicherzustellen, dass die `WebView` Daten in vollständig geladen werden, bevor die Option Drucken im Menü angezeigt wird. dies `OnPageFinished` geschieht mit der-Methode. `OnPageFinished`überwacht, dass Webinhalt abgeschlossen ist, und weist die Aktivität an, das Menü "Optionen `InvalidateOptionsMenu`" mit folgenden Optionen neu zu erstellen:

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

`OnPageFinished`Außerdem wird der `dataLoaded` Wert auf `true`festgelegt `OnCreateOptionsMenu` , sodass das Menü mit der Option Drucken neu erstellt werden kann.

##### <a name="printmanager"></a>PrintManager

Im folgenden Codebeispiel wird der Inhalt eines `WebView`ausgegeben:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print`annimmt als Argumente: ein Name für den Druckauftrag ("mywebseite" in diesem Beispiel), a[`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter)
, der das Druck Dokument aus dem Inhalt generiert, und[`PrintAttributes`](xref:Android.Print.PrintAttributes)
(`null` im obigen Beispiel). Sie können angeben `PrintAttributes` , um das Layout von Inhalten auf der gedruckten Seite zu erleichtern, obwohl die Standard Attribute die meisten Szenarien verarbeiten sollten.

Durch `Print` Aufrufen von wird die Druck Benutzeroberfläche geladen, die Optionen für den Druckauftrag auflistet. Die Benutzeroberfläche bietet Benutzern die Möglichkeit, den HTML-Inhalt in einer PDF-Datei zu drucken oder zu speichern, wie in den folgenden Screenshots veranschaulicht:

[![Bildschirm Abbildung von printhtmlactivity mit Anzeige des Menüs "Drucken"](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Screenshot von printhtmlactivity, das das Menü "als PDF speichern" anzeigt](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>Hardware

Kitkat fügt verschiedene APIs hinzu, um neue Geräte Features zu bieten. Die bemerkenswertesten sind die Host basierte Karten Emulation und die neue `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Host basierte Karten Emulation in NFC

Mit der Host basierten Karten Emulation (HCE) können sich Anwendungen wie NFC-Karten oder NFC-Kartenleser Verhalten, ohne sich auf das proprietäre sichere Element des Netzbetreibers verlassen zu müssen. Vergewissern Sie sich vor dem Einrichten von HCE, dass HCE auf dem `PackageManager.HasSystemFeature`Gerät mit folgenden Aktionen verfügbar ist:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE erfordert, dass die HCE-Funktion und `Nfc` die- `AndroidManifest.xml`Berechtigung für die Anwendung registriert werden:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Festlegen der NFC-Berechtigung in App-Optionen](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

HCE muss im Hintergrund ausgeführt werden können, und Sie muss gestartet werden, wenn der Benutzer eine NFC-Transaktion durchführt, auch wenn die Anwendung, die HCE verwendet, nicht ausgeführt wird. Dies können Sie erreichen, indem Sie den HCE-Code `Service`als schreiben. Ein HCE-Dienst implementiert `HostApduService` die-Schnittstelle, die die folgenden Methoden implementiert:

- *Processcommandapdu* -eine Anwendungsprotokoll-Dateneinheit (APDU), die zwischen dem NFC-Reader und dem HCE-Dienst gesendet wird. Diese Methode verwendet eine adpu vom Reader und gibt eine Dateneinheit als Antwort zurück.

- *Ondeaktiviert* : `HostAdpuService` wird deaktiviert, wenn der HCE-Dienst nicht mehr mit dem NFC-Reader kommuniziert.


Ein HCE-Dienst muss auch mit dem Manifest der Anwendung registriert und mit den entsprechenden Berechtigungen, dem beabsichtigten Filter und den entsprechenden Metadaten versehen werden. Der folgende Code ist ein Beispiel für eine `HostApduService` , die mit dem- `Service` Attribut für das Android-Manifest registriert ist (Weitere Informationen zu Attributen finden Sie im Handbuch xamarin [Working with Android Manifest](~/android/platform/android-manifest.md) ):

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

Der obige Dienst bietet eine Möglichkeit für den NFC-Reader, mit der Anwendung zu interagieren, aber der NFC-Reader hat immer noch keine Möglichkeit, zu wissen, ob der Dienst die zu überprüfende NFC-Karte emuttet. Damit der NFC-Reader den Dienst identifizieren kann, können wir dem Dienst eine eindeutige *Anwendungs-ID zuweisen (Help)* . Wir geben eine Hilfe zusammen mit anderen Metadaten zum HCE-Dienst in einer XML-Ressourcen Datei an, die mit `MetaData` dem-Attribut registriert ist (siehe Codebeispiel oben). Diese Ressourcen Datei gibt einen oder mehrere hilfsfilter an: eindeutige Bezeichnerzeichenfolgen im Hexadezimal Format, die den Hilfen von mindestens einem NFC-Lesegerät entsprechen:

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

Zusätzlich zu den Hilfe Filtern bietet die XML-Ressourcen Datei auch eine Benutzer seitige Beschreibung des HCE-Dienstanbieter, gibt eine Hilfe Gruppe (Zahlungs Anwendung im Vergleich zu "Sonstige") und im Fall einer Zahlungs Anwendung ein DP-Banner mit 260x96 an, das dem Benutzer angezeigt wird.

Das oben beschriebene Setup stellt die grundlegenden Bausteine für eine Anwendung bereit, die eine NFC-Karte emumt. NFC selbst erfordert mehrere weitere Schritte und weitere Tests, um zu konfigurieren. Weitere Informationen zur Host basierten Karten Emulation finden Sie im [Android-Dokumentations Portal](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Weitere Informationen zur Verwendung von NFC mit xamarin finden Sie in den [xamarin NFC-Beispielen](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Sensoren

Kitkat bietet über einen [`SensorManager`](xref:Android.Hardware.SensorManager)Zugriff auf die Sensoren des Geräts.
`SensorManager` Ermöglicht dem Betriebssystem das Planen der Übermittlung von Sensorinformationen an eine Anwendung in Batches, wobei die Akku Lebensdauer beibehalten wird.

Kitkat ist auch mit zwei neuen Sensortypen für die Nachverfolgung der Benutzer Schritte ausgeliefert. Diese basieren auf dem Beschleunigungsmesser und umfassen Folgendes:

- *Stepdetector* : die APP wird benachrichtigt/reaktiviert, wenn der Benutzer einen Schritt annimmt, und der Detektor gibt einen Zeitwert für den Zeitpunkt an, zu dem der Schritt eingetreten ist.

- *Step counter* : verfolgt die Anzahl der Schritte, die der Benutzer seit der Registrierung des Sensors *bis zum nächsten Geräte Neustart*durchgeführt hat.

Der folgende Screenshot zeigt den Schritt Counter in Aktion:

[![Screenshot der sensorsactivity-APP, die einen Schritt Counter anzeigt](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Sie können einen `SensorManager` erstellen, indem `GetSystemService(SensorService)` Sie aufrufen und das Ergebnis als `SensorManager`umwandeln. Um den Schritt Counter zu verwenden, `GetDefaultSensor` wenden Sie `SensorManager`auf an. Sie können den Sensor registrieren und Änderungen in der Anzahl der Schritte mit der Hilfe des[`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener)
Schnittstelle, wie im folgenden Codebeispiel veranschaulicht:

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

`OnSensorChanged`wird aufgerufen, wenn die Anzahl der Schritte aktualisiert wird, während sich die Anwendung im Vordergrund befindet. Wenn die Anwendung in den Hintergrund wechselt oder das Gerät inaktiv ist `OnSensorChanged` , wird nicht aufgerufen. die Schritte werden jedoch weiterhin gezählt, bis `UnregisterListener` aufgerufen wird.

Beachten Sie, dass *der Wert für die Schritt Anzahl in allen Anwendungen kumulativ ist, die den Sensor registrieren*. Dies bedeutet, dass selbst dann, wenn Sie die Anwendung deinstallieren und neu installieren und `count` die Variable beim Anwendungsstart bei 0 initialisiert wird, der vom Sensor gemeldete Wert die Gesamtzahl der Schritte, die während der Registrierung des Sensors durchgeführt wurden, beibehalten wird. Anwendung oder eine andere Anwendung. Sie können verhindern, dass Ihre Anwendung dem Schritt-Counter hinzufügt, `UnregisterListener` indem Sie `SensorManager`für den aufrufen, wie im folgenden Code veranschaulicht:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Durch einen Neustart des Geräts wird die Anzahl der Schritte auf 0 zurückgesetzt. Ihre APP erfordert zusätzlichen Code, um sicherzustellen, dass Sie eine genaue Anzahl für die Anwendung meldet, unabhängig von anderen Anwendungen, die den Sensor oder den Status des Geräts verwenden.


> [!NOTE]
> Während die API für die Schritt Erkennung und-Zählung mit KitKat ausgeliefert wird, sind nicht alle Telefone mit dem Sensor ausgerüstet. Sie können überprüfen, ob der Sensor verfügbar ist `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, indem Sie ausführen, oder überprüfen, `GetDefaultSensor` ob `null`der zurückgegebene Wert von ist.


<a name="developer_tools" />

## <a name="developer-tools"></a>Entwicklertools

### <a name="screen-recording"></a>Bildschirmaufzeichnung

Kitkat bietet neue Funktionen für die Bildschirmaufzeichnung, damit Entwickler Anwendungen in Aktion aufzeichnen können. Die Bildschirmaufzeichnung steht über den [Android Debug Bridge (ADB)](https://developer.android.com/tools/help/adb.html) -Client zur Verfügung, der als Teil des Android SDK heruntergeladen werden kann.

Um Ihren Bildschirm aufzuzeichnen, verbinden Sie Ihr Gerät. Suchen Sie anschließend die Android SDK-Installation, navigieren Sie zum Verzeichnis " **Platform-Tools** ", und führen Sie den **ADB** -Client aus:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Mit dem obigen Befehl wird ein Standardmäßiges 3-minütiges Video mit der Standardauflösung von 4 Mbit/s aufgezeichnet. Fügen Sie zum Bearbeiten der Länge das *--Zeit Limit-* Flag hinzu.
Um die Auflösung zu ändern, fügen Sie das *-Bit-Rate-* Flag hinzu. Mit dem folgenden Befehl wird ein Videos mit einer Länge von 8 Mbit/s aufgezeichnet:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Sie können Ihr Video auf Ihrem Gerät finden, und es wird im Katalog angezeigt, wenn die Aufzeichnung fertiggestellt ist.


## <a name="other-kitkat-additions"></a>Weitere KitKat-Ergänzungen

Zusätzlich zu den oben beschriebenen Änderungen ermöglicht KitKat Folgendes:

- *Verwenden Sie den voll Bild* Modus. KitKat führt einen neuen [immersiven Modus](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) zum Durchsuchen von Inhalten, spielen von spielen und Ausführen anderer Anwendungen ein, die von einem voll Bildschirm profitieren können.

- *Anpassen von Benachrichtigungen* : Weitere Informationen zu System Benachrichtigungen erhalten Sie mit der[`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService)
   . Auf diese Weise können Sie die Informationen in Ihrer APP auf andere Weise präsentieren.

- *Spiegel drawable-Ressourcen* : für drawable-Ressourcen gibt es eine neue[`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
   Attribut, das dem System mitteilt, dass eine gespiegelte Version für Bilder erstellt wird, die das Kippen von links nach rechts Layouts erfordern.

- Anhalten von *Animationen* : anhalten und Fortsetzen von Animationen, die mit dem[`Animator`](xref:Android.Animation.Animator)
   -Klasse.

- *Lesen dynamisch ändernder Text* : Teile der Benutzeroberfläche, die dynamisch aktualisiert werden, mit neuem Text als "Live Bereiche" mit dem neuen[`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
   -Attribut, sodass der neue Text automatisch im Barrierefreiheits Modus gelesen wird.

- *Verbessern* Sie die Audiodarstellung: machen Sie Ihre Spuren mit der[`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer)
   , ermitteln Sie die Spitzen und RMS eines Audiodatenstroms mit dem[`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms)
   und erhalten Informationen aus einem [Audiozeit Stempel](xref:Android.Media.AudioTimestamp) , um die audiovideosynchronisierung zu unterstützen.

- *Synchronisierungs-contentresolver in benutzerdefiniertem Intervall* : KitKat erhöht die Zeitspanne, in der eine Synchronisierungs Anforderung ausgeführt wird. Synchronisieren Sie `ContentResolver` eine zu einer benutzerdefinierten Zeit oder `ContentResolver.RequestSync` einem bestimmten Intervall, `SyncRequest`indem Sie aufrufen und einen übergeben.

- Unter *scheiden zwischen Controllern* : in KitKat werden Controllern eindeutige ganzzahlige Bezeichner zugewiesen, auf die über `ControllerNumber` die-Eigenschaft des Geräts zugegriffen werden kann. Dadurch ist es einfacher, die Spieler in einem Spiel zu teilen.

- *Remote Steuerung* : mit einigen Änderungen auf der Hardware-und Software Seite können Sie mithilfe `ConsumerIrService`von KitKat ein Gerät, das mit einem IR-Sender ausgerüstet ist, in eine Remote Steuerung umwandeln und mit den neuen[`RemoteController`](xref:Android.Media.RemoteController)
   APIs.

Weitere Informationen zu den oben genannten API-Änderungen finden Sie in der Übersicht über Google [Android 4,4-APIs](https://developer.android.com/about/versions/android-4.4.html) .


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige der neuen APIs vorgestellt, die in Android 4,4 (API-Ebene 19) verfügbar sind, sowie bewährte Methoden beim Übergang einer Anwendung in KitKat. Es wurden Änderungen an den APIs erläutert, die sich auf die Benutzer Darstellung auswirken, einschließlich des *Übergangs-Frameworks* und neuer Optionen für das Design. Im nächsten Schritt wurden das *Speicherzugriffs Framework* und `DocumentsProvider` die Klasse sowie die neuen *Druck-APIs*eingeführt. Sie untersuchte *NFC-Host basierte Karten Emulation* und die Arbeit mit Hochleistungs *Sensoren*, einschließlich zwei neuer Sensoren für die Nachverfolgung der Schritte des Benutzers. Schließlich haben wir demonstriert, wie Sie Echt Zeit Demos von Anwendungen mit *Bildschirmaufzeichnung*erfassen und eine ausführliche Liste der Änderungen und Ergänzungen der KitKat-API bereitstellen.


## <a name="related-links"></a>Verwandte Links

- [Kitkat-Beispiel](https://docs.microsoft.com/samples/xamarin/monodroid-samples/kitkat)
- [Android 4,4-APIs](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
