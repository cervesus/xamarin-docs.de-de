---
title: Nougat-Features
description: Beginnen Sie mit der Verwendung von xamarin. Android zum Entwickeln von Apps für Android-Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: 128982abdee7a0fea8df79f7b7b9ecd6a290775a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761259"
---
# <a name="nougat-features"></a>Nougat-Features

_Beginnen Sie mit der Verwendung von xamarin. Android zum Entwickeln von Apps für Android-Nougat._

Dieser Artikel enthält eine Übersicht über die in Android-Nougat eingeführten Features, erläutert, wie xamarin. Android für die Entwicklung von Android-Nougat vorbereitet wird, und bietet Links zu Beispielanwendungen, die veranschaulichen, wie Sie die Funktionen von Android-Nougat in Xamarin. Android-Apps.

## <a name="overview"></a>Übersicht

[Android-Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) ist Google-Nachverfolgung zu Android 6,0 Marshmallow. Xamarin. Android bietet Unterstützung für **Android 7. x-Bindungen** in xamarin Android 7,0 und höher. Android-Nougat fügt viele neue APIs für die unten beschriebenen Nougat-Features hinzu. Diese APIs sind für xamarin. Android-Apps verfügbar, wenn Sie xamarin. Android 7,0 verwenden.

[![Hero-Images von Android-Tablets und Smartphones mit Android-Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Weitere Informationen zu Android 7. x-APIs finden Sie unter [Android 7,1 für Entwickler](https://developer.android.com/preview/api-overview.html).
Eine Liste bekannter Probleme mit xamarin. Android 7,0 finden Sie in den Anmerkungen zu dieser [Version](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md).

Android-Nougat bietet viele neue Features, die für xamarin. Android-Entwickler von Interesse sind. Zu diesen Features zählen:

- **Unterstützung für mehrere Fenster** &ndash; Diese Erweiterung ermöglicht es Benutzern, zwei apps gleichzeitig auf dem Bildschirm zu öffnen.

- **Benachrichtigungs Erweiterungen** Das neu gestaltete Benachrichtigungssystem in Android Nougat enthält eine Funktion für *direkte Antworten* , die es Benutzern ermöglicht, schnell auf Textnachrichten direkt von der Benachrichtigungs-UI zu reagieren. &ndash; Wenn Ihre APP außerdem Benachrichtigungen für empfangene Nachrichten erstellt, kann die neue *gebündelte Benachrichtigungs* Funktion Benachrichtigungen zusammen als eine einzelne Gruppe bündeln, wenn mehr als eine Nachricht empfangen wird.

- **Daten Ersparnis** &ndash; Bei diesem Feature handelt es sich um einen neuen Systemdienst, der die Nutzung der Mobilfunkdaten durch Apps verringert und Benutzern die Steuerung der Verwendung von Mobil Funkdaten durch Apps ermöglicht.

Außerdem bietet Android Nougat viele weitere Verbesserungen, die für App-Entwickler von Interesse sind, wie z. b. ein neues Feature für die Netzwerk Sicherheitskonfiguration, Doze unterwegs, Key Attestation, neue schnell Einstellungs-APIs, Unterstützung mehrerer Gebiets Schemas, ICU4J-APIs, WebView-Erweiterungen, Zugriff auf Java 8-sprach Features, Bereichs bezogenen Verzeichnis Zugriff, eine benutzerdefinierte Zeiger-API, Unterstützung für Platform VR, virtuelle Dateien und Optimierungen der Hintergrundverarbeitung.

In diesem Artikel wird erläutert, wie Sie mit der Entwicklung von apps mit Android-Nougat beginnen, um die neuen Features zu testen und die Migration oder Funktions Arbeit für die neue Android-Nougat-Plattform zu planen.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Android-Funktionen von Nougat in xamarin-basierten apps zu verwenden:

- **Visual Studio oder Visual Studio für Mac** &ndash; Wenn Sie Visual Studio verwenden, ist die Version 4.2.0.628 oder höher von Visual Studio-Tools für xamarin erforderlich. Wenn Sie Visual Studio für Mac verwenden, ist Version 6.1.0 oder höher von Visual Studio für Mac erforderlich.

- **Xamarin. Android** &ndash; xamarin. Android 7,0 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden.

- **Android SDK** -Android SDK 7,0 (API 24) oder höher muss über den Android SDK Manager installiert werden.

- **Java Developer Kit** Die Entwicklung von xamarin Android 7,0 erfordert [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch API-Ebenen vor 24). &ndash; Die 64-Bit-Version von JDK 8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder den Formular-Previewer verwenden.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

Beachten Sie, dass apps mit xamarin C6SR4 oder höher neu erstellt werden müssen, damit Sie zuverlässig mit Android-Nougat funktionieren. Da Android-Nougat nur mit [NDK-bereitgestellten nativen Bibliotheken](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)verknüpfen kann, können vorhandene apps, die Bibliotheken wie **Mono. Data. sqlite. dll** verwenden, bei Ausführung unter Android-Nougat abstürzen, wenn Sie nicht ordnungsgemäß neu erstellt werden.

## <a name="getting-started"></a>Erste Schritte

Um mit der Verwendung von Android Nougat mit xamarin. Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie ein Android-Nougat-Projekt erstellen können:

1. Installieren Sie die neuesten xamarin. Android-Updates aus xamarin.

2. Installieren Sie die Pakete und Tools von **Android 7,0 (API 24)** oder höher.

3. Erstellen Sie ein neues xamarin. Android-Projekt, das Android-Nougat als Ziel hat.

4. Konfigurieren Sie einen Emulator oder ein Gerät für Android-Nougat.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="install-xamarin-updates"></a>Installieren von xamarin-Updates

Um xamarin-Unterstützung für Android-Nougat hinzuzufügen, ändern Sie den Update Kanal in Visual Studio, oder Visual Studio für Mac in den stabilen Channel, und wenden Sie die neuesten Updates an. Wenn Sie auch Funktionen benötigen, die derzeit nur im Alpha-oder Beta-Kanal verfügbar sind, können Sie zum Alpha-oder Beta-Kanal wechseln (die Alpha-und Beta-Kanäle bieten auch Unterstützung für Android 7. x). Informationen zum Ändern des Updates (Releases) finden Sie unter [Ändern des Updates-Kanals](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Zum Erstellen eines Projekts mit xamarin Android 7,0 müssen Sie zunächst den Android SDK-Manager zum Installieren der **SDK-Plattform Android N (API 24)** oder höher verwenden. Außerdem müssen Sie die neuesten **Android SDK Tools**installieren:

1. Starten Sie den Android SDK-Manager (> Verwenden Sie in Visual Studio für Mac Extras, um **Android SDK&hellip;Manager zu öffnen**. verwenden Sie in Visual Studio Extras **> Android > Android SDK-Manager**).

2. Installieren Sie **Android 7,0 (API 24)** oder höher:

    [![Auswählen von Android 7,0-Paketen im Android SDK-Manager](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3. Installieren Sie die neuesten Android SDK Tools:

    [![Auswählen der neuesten Android SDK Tools im Android SDK Manager](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Sie müssen Android SDK Tools Revision 25.2.2 oder höher, Android SDK Platt Form Tools 24.0.3 oder höher und Android SDK Build Tools 24.0.2 oder höher installieren.

4. Vergewissern Sie sich, dass der **Speicherort für das Java Development Kit** für JDK 1,8 konfiguriert ist:

    [![Konfigurieren des JDK 8-Pfads unter "Tools"-Optionen](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Um diese Einstellung in Visual Studio anzuzeigen, klicken Sie auf Extras **> Optionen > xamarin > Android-Einstellungen**. Klicken Sie in Visual Studio für Mac auf **Einstellungen > Projekte > SDK-Speicherorte > Android**.

### <a name="start-a-xamarinandroid-project"></a>Starten eines xamarin. Android-Projekts

Erstellen Sie ein neues xamarin. Android-Projekt. Wenn Sie mit der Android-Entwicklung mit xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Weitere Informationen zum Erstellen von xamarin. Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versions Einstellungen so konfigurieren, dass Sie Android 7,0 oder höher als Ziel haben. Wenn Sie z. b. Ihr Projekt für Android 7,0 als Ziel festlegen möchten, müssen Sie die Android-API-Ziel Ebene Ihres Projekts auf **Android 7,0 (API 24-Nougat)** konfigurieren. Es wird empfohlen, dass Sie die zielframeworkebene auf API 24 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

> [!NOTE]
> Derzeit müssen Sie die **Android-Mindestversion** auf **Android 7,0 (API 24-Nougat)** festlegen, um Ihre APP auf Android-Nougat-Geräten oder-Emulatoren bereitzustellen.

### <a name="configure-an-emulator-or-device"></a>Konfigurieren eines Emulators oder Geräts

Wenn Sie einen Emulator verwenden, starten Sie den Android AVD-Manager, und erstellen Sie ein neues Gerät mit den folgenden Einstellungen:

- Gerät: Nexus 5x, Nexus 6, Nexus 6p, Nexus Player, Nexus 9 oder Pixel C.
- Ziel: Android 7,0-API-Ebene 24
- ABI: x86 oder x86\_64

Beispielsweise ist dieses virtuelle Gerät so konfiguriert, dass es einen Nexus 6 emuliert:

[![Konfigurieren von AVD mit Nexus 6-Gerät, Android 7,0-Ziel und Intel Atom x86-CPU/Abi](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Wenn Sie ein physisches Gerät (z. b. ein Nexus 5x, 6 oder 9) verwenden, können Sie Ihr Gerät entweder durch automatische über-Air-Updates (OTA) aktualisieren oder ein System Abbild herunterladen und Ihr Gerät direkt blinken. Weitere Informationen zum manuellen Aktualisieren Ihres Geräts auf Android-Nougat finden Sie unter [OTA-Images für Nexus-Geräte](https://developers.google.com/android/nexus/ota).

Beachten Sie, dass Nexus 5-Geräte nicht von Android-Nougat unterstützt werden.

## <a name="new-features"></a>Neue Funktionen

Android-Nougat führt eine Reihe von neuen Features und Funktionen ein, z. b. Unterstützung für mehrere Fenster, Benachrichtigungs Erweiterungen und Daten Ersparnis. In den folgenden Abschnitten werden diese Features hervorgehoben, und es werden Links bereitgestellt, die Ihnen bei der Verwendung in Ihrer APP helfen.

### <a name="multi-window-mode"></a>Modus für mehrere Fenster

Der Modus für mehrere Fenster ermöglicht Benutzern das gleichzeitige Öffnen von zwei apps mit vollständiger Multitasking-Unterstützung. Diese Apps können parallel (Querformat) oder ein-oben-(Hochformat) im Split-Screen-Modus ausgeführt werden.
Benutzer können einen unter Teiler zwischen den apps ziehen, um deren Größe zu ändern, und Sie können den Inhalt zwischen apps Ausschneiden und einfügen. Wenn zwei apps im Modus für mehrere Fenster angezeigt werden, wird die ausgewählte Aktivität weiterhin ausgeführt, während die nicht ausgewählte Aktivität angehalten, aber immer noch sichtbar ist. Der Modus für mehrere Fenster ändert den Android-Aktivitäts Lebenszyklus nicht.

[![Beispiel-apps, die im Modus für mehrere Fenster im hoch-und Querformat ausgeführt werden](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Sie können konfigurieren, wie die Aktivitäten ihrer xamarin. Android-App den Modus für mehrere Fenster unterstützen. Beispielsweise können Sie Attribute konfigurieren, die die minimale Größe und die Standardhöhe und-Breite Ihrer APP im Modus für mehrere Fenster festlegen. Sie können die neue `Activity.IsInMultiWindowMode` -Eigenschaft verwenden, um zu bestimmen, ob sich die Aktivität im Modus für mehrere Fenster befindet. Beispiel:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Die Beispiel-app " [multiwindowplayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) " enthält C# Code, der veranschaulicht, wie Sie die Vorteile mehrerer Fensterbenutzer Oberflächen mit ihrer App nutzen können.

Weitere Informationen zum Modus für mehrere Fenster finden Sie [unter Unterstützung mehrerer Fenster](https://developer.android.com/guide/topics/ui/multi-window.html).

### <a name="enhanced-notifications"></a>Erweiterte Benachrichtigungen

Android-Nougat führt ein überarbeitete Benachrichtigungssystem ein. Es verfügt über ein neues Feature für direkte Antworten, das es Benutzern ermöglicht, schnell auf Benachrichtigungen für eingehende Textnachrichten in der Benachrichtigungs-UI zu antworten. Ab Android 7,0 können Benachrichtigungs Meldungen als einzelne Gruppe gebündelt werden, wenn mehr als eine Nachricht empfangen wird. Entwickler können auch Benachrichtigungs Ansichten anpassen, System Dekorationen in Benachrichtigungen nutzen und neue Benachrichtigungs Vorlagen nutzen, wenn Sie Benachrichtigungen erstellen.

#### <a name="direct-reply"></a>Direkte Antwort

Wenn ein Benutzer eine Benachrichtigung für die eingehende Nachricht erhält, ermöglicht Android-Nougat das Antworten auf die Nachricht innerhalb der Benachrichtigung (anstatt die Messaging-APP zu öffnen, um eine Antwort zu senden).
Diese Funktion für Inline Antworten ermöglicht es Benutzern, schnell auf eine SMS-oder Textnachricht direkt innerhalb der Benachrichtigungs Schnittstelle zu reagieren:

[![Screenshot einer Benachrichtigung mit einem Inline-direkt Antwortfeld](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Um dieses Feature in Ihrer APP zu unterstützen, müssen Sie der APP *Inline Antwort Aktionen* über ein [remoteinput](xref:Android.App.RemoteInput) -Objekt hinzufügen, damit Benutzer über die Benachrichtigungs Benutzeroberfläche direkt über Text Antworten können.
Der folgende Code erstellt z. b. `RemoteInput` einen zum Empfangen von Texteingaben, erstellt eine ausstehende Absicht für die Antwort Aktion und erstellt eine aktivierte Remote Eingabe Aktion:

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Diese Aktion wird der Benachrichtigung hinzugefügt:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

Die Beispiel-App für den C# [Messaging Dienst](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) enthält Code, der veranschaulicht, wie `RemoteInput` Benachrichtigungen mit einem-Objekt erweitert werden. Weitere Informationen zum Hinzufügen von Inline Antwort Aktionen zu Ihrer APP für Android 7,0 oder höher finden Sie im Thema Android Response [to Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) .

#### <a name="bundled-notifications"></a>Gebündelte Benachrichtigungen

Android-Nougat kann Benachrichtigungs Meldungen gruppieren (z. b. nach Nachrichtenthema) und die Gruppe anstelle jeder separaten Nachricht anzeigen.
Diese *gebündelte Benachrichtigungs* Funktion ermöglicht es Benutzern, eine Gruppe von Benachrichtigungen in einer Aktion zu verwerfen oder zu archivieren. Der Benutzer kann nach unten verschieben, um das Benachrichtigungs Bündel zu erweitern und die einzelnen Benachrichtigungen ausführlich anzuzeigen:

[![Screenshot: Beispiel für gebündelte Benachrichtigungen](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Um gebündelte Benachrichtigungen zu unterstützen, kann Ihre APP die [Builder. SetGroup](xref:Android.App.Notification.Builder.SetGroup*) -Methode verwenden, um ähnliche Benachrichtigungen zu bündeln. Weitere Informationen zu gebündelten Benachrichtigungs Gruppen in Android N finden Sie im Thema Android- [Bundle-Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) .

#### <a name="custom-views"></a>Benutzerdefinierte Ansichten

Mit Android-Nougat können Sie benutzerdefinierte Benachrichtigungs Ansichten mit System Benachrichtigungs Headern, Aktionen und erweiterbaren Layouts erstellen. Weitere Informationen zu benutzerdefinierten Benachrichtigungs Ansichten in Android-Nougat finden Sie im Thema Android- [Benachrichtigungs Erweiterungen](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) .

### <a name="data-saver"></a>Daten Ersparnis

Ab Android-Nougat können Benutzer eine neue *Daten Schoner* Einstellung aktivieren, mit der die Verwendung von Hintergrunddaten blockiert wird. Diese Einstellung signalisiert Ihrer APP außerdem, möglichst weniger Daten im Vordergrund zu verwenden. Der [connectivitymanager](xref:Android.Net.ConnectivityManager) wurde in Android-Nougat erweitert, sodass Ihre APP überprüfen kann, ob der Benutzer den Daten Schoner aktiviert hat, damit Ihre APP die Datennutzung einschränken kann, wenn der Daten Schoner aktiviert ist.

Weitere Informationen zum neuen Datenspeicher Feature in Android Nougat finden Sie im Thema Android- [Optimierungs Netzwerk-Datenverwendung](https://developer.android.com/training/basics/network-ops/data-saver.html) .

### <a name="app-shortcuts"></a>App-Verknüpfungen

Android 7,1 hat eine Funktion für *App* -Verknüpfungen eingeführt, die es Benutzern ermöglicht, häufig gängige oder empfohlene Aufgaben mit Ihrer APP zu starten.
Um das Menü mit Verknüpfungen zu aktivieren, drückt der Benutzer das App-Symbol für ein zweites oder &ndash; mehr, wenn das Menü mit einer schnellen Vibration angezeigt wird.
Wenn Sie die Presse freigeben, wird das Menü beibehalten:

[![Beispiel Bildschirm eines App-Kontextmenüs für eine Messaging-App](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Diese Funktion ist nur auf API-Ebene 25 oder höher verfügbar.
Weitere Informationen zur Funktion "neue APP-Verknüpfungen" in Android 7,1 finden Sie im Thema Android- [App](https://developer.android.com/guide/topics/ui/shortcuts.html) -Verknüpfungen.

### <a name="sample-code"></a>Beispielcode

Es stehen mehrere xamarin. Android-Beispiele zur Verfügung, die Ihnen zeigen, wie Sie die Funktionen von Android-Nougat nutzen können:

- [Multiwindowplayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) veranschaulicht die Verwendung der Multi-Window-API, die in Android-Nougat verfügbar ist. Sie können die Beispiel-app in den multiwindows-Modus wechseln, um zu sehen, wie sich diese auf den Lebenszyklus und das Verhalten der APP auswirkt.

- Der [Messaging Dienst](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) ist ein einfacher Dienst, der `NotificationCompatManager`mithilfe von Benachrichtigungen sendet. Außerdem wird die Benachrichtigung mit einem `RemoteInput` -Objekt erweitert, damit Android-Nougat-Geräte per Text direkt aus der Benachrichtigung antworten können, ohne eine APP öffnen zu müssen.

- [Aktive Benachrichtigungen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications) veranschaulichen, wie die `NotificationManager` API verwendet wird, um Ihnen mitzuteilen, wie viele Benachrichtigungen derzeit von Ihrer Anwendung angezeigt werden.

- Bereichs bezogener [Verzeichnis Zugriff](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess) Veranschaulicht die Verwendung der Bereichs bezogenen Verzeichniszugriffs-API für den einfachen Zugriff auf bestimmte Verzeichnisse. Dies dient als Alternative zum Definieren `READ_EXTERNAL_STORAGE` von-oder `WRITE_EXTERNAL_STORAGE` -Berechtigungen in ihrem Manifest.

- [Direkter Start](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot) Veranschaulicht das Speichern von Daten in einem Geräte verschlüsselten Speicher, der immer verfügbar ist, wenn das Gerät vor und nach der Eingabe von Benutzer Anmelde Informationen (PIN/Muster/Kennwort) gestartet wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android-Nougat vorgestellt und erläutert, wie die neuesten Tools und Pakete für die xamarin. Android-Entwicklung auf Android-Nougat installiert und konfiguriert werden. Außerdem bietet es eine Übersicht über die wichtigsten Features, die in Android-Nougat verfügbar sind, und enthält Links zu Beispielen, die Ihnen den Einstieg in das Erstellen von Apps für Android-Nougat erleichtern.

## <a name="related-links"></a>Verwandte Links

- [Android 7,1 für Entwickler](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Anmerkungen zu dieser Version von xamarin Android 7,0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
