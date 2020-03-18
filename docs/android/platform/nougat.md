---
title: Nougat-Features
description: Dieser Artikel bietet Unterstützung beim Einstieg in die Verwendung von Xamarin.Android zur Entwicklung von Apps für Android Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 6274c75abf229268070d495ced662724f5c16627
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027093"
---
# <a name="nougat-features"></a>Nougat-Features

_Dieser Artikel bietet Unterstützung beim Einstieg in die Verwendung von Xamarin.Android zur Entwicklung von Apps für Android Nougat._

Dieser Artikel bietet einen Überblick über die in Android Nougat eingeführten Features, erläutert, wie Sie Xamarin.Android für die Entwicklung für Android Nougat vorbereiten, und stellt Links zu Beispielanwendungen bereit, die die Verwendung der neuen Android Nougat-Features in Xamarin.Android-Apps veranschaulichen.

## <a name="overview"></a>Übersicht

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) ist der Nachfolger von Google für Android 6.0 Marshmallow. Xamarin.Android bietet Unterstützung für **Android 7.x-Bindungen** in Xamarin Android 7.0 und höher. Android Nougat fügt eine Vielzahl neuer APIs für die unten beschriebenen Nougat-Features hinzu. Diese APIs sind für Xamarin.Android-Apps verfügbar, wenn Sie Xamarin.Android 7.0 verwenden.

[![Hero-Bilder von Android-Tablets und -Smartphones mit Android Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Weitere Informationen zu Android 7.x-APIs finden Sie auf der [Entwicklerseite für Android 7.1](https://developer.android.com/preview/api-overview.html).
Eine Liste bekannter Probleme mit Xamarin.Android 7.0 finden Sie in den [Versionshinweisen](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md).

Android Nougat bietet zahlreiche neue Features, die für Xamarin.Android-Entwickler interessant sind. Zu diesen Features zählen:

- **Unterstützung für mehrere Fenster** &ndash; diese Erweiterung ermöglicht Benutzern, zwei Apps gleichzeitig auf dem Bildschirm zu öffnen.

- **Benachrichtigungserweiterungen** &ndash; das neu gestaltete Benachrichtigungssystem in Android Nougat enthält ein *Direktantwort*-Feature, mit dem Benutzer schnell von der Benachrichtigungsbenutzeroberfläche aus direkt auf SMS antworten können. Auch wenn Ihre App Benachrichtigungen für empfangene Nachrichten erstellt, kann das neue Feature der *gebündelten Benachrichtigungen* Benachrichtigungen zu einer einzelnen Gruppe bündeln, wenn mehrere Nachrichten empfangen werden.

- **Data Saver** &ndash; dieses Feature ist ein neuer Systemdienst, der die Nutzung von Mobilfunkdaten durch Apps reduziert. Es ermöglicht Benutzern die Kontrolle über die Verwendung von Mobilfunkdaten durch Apps.

Außerdem bietet Android Nougat viele weitere Verbesserungen, die für App-Entwickler von Interesse sind, wie z. B. ein neues Feature zur Konfiguration der Netzwerksicherheit, Doze on the Go, Schlüsselnachweis, neue Schnelleinstellungen-APIs, Unterstützung mehrerer Gebietsschemas, ICU4J-APIs, WebView-Erweiterungen, Zugriff auf Java 8-Sprachfeatures, bereichsbezogenen Verzeichniszugriff, eine API für benutzerdefinierte Zeiger, Plattform-VR-Unterstützung, virtuelle Dateien und Optimierungen der Hintergrundverarbeitung.

In diesem Artikel wird erläutert, wie Sie mit der Entwicklung von Apps mit Android Nougat beginnen, um die neuen Features zu testen und die Migrations- oder Funktionsarbeit für die neue Android Nougat-Plattform zu planen.

## <a name="requirements"></a>Anforderungen

Für die Verwendung von Android Nougat-Features in Xamarin-basierten Apps gelten die folgenden Voraussetzungen:

- **Visual Studio oder Visual Studio für Mac** &ndash; wenn Sie Visual Studio verwenden, ist Version 4.2.0.628 oder höher der Visual Studio-Tools für Xamarin erforderlich. Wenn Sie Visual Studio für Mac verwenden, ist Version 6.1.0 oder höher erforderlich.

- **Xamarin.Android** &ndash; Xamarin.Android 7.0 oder höher muss entweder mit Visual Studio oder Visual Studio für Mac installiert und konfiguriert sein.

- **Android SDK** – Android SDK 7.0 (API 24) oder höher muss über den Android-SDK-Manager installiert werden.

- **Java Developer Kit** &ndash; die Xamarin Android 7.0-Entwicklung erfordert mindestens [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch niedrigere API-Ebenen als 24). Die 64-Bit-Version von JDK 8 wird benötigt, wenn Sie benutzerdefinierte Steuerelemente oder die Forms-Vorschau verwenden.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

Beachten Sie, dass Apps mit Xamarin C6SR4 oder höher neu erstellt werden müssen, damit sie zuverlässig mit Android Nougat funktionieren. Da Android Nougat nur mit [NDK-bereitgestellten nativen Bibliotheken](https://developer.android.com/about/versions/nougat/android-7.0-changes.html) Verknüpfungen herstellen kann, können vorhandene Apps wie **Mono.Data.Sqlite.dll**, die Bibliotheken verwenden, bei Ausführung auf Android Nougat abstürzen, wenn sie nicht ordnungsgemäß neu erstellt werden.

## <a name="getting-started"></a>Erste Schritte

Um mit der Nutzung von Android Nougat mit Xamarin.Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie ein Android Nougat-Projekt erstellen können:

1. Installieren Sie die neuesten Xamarin.Android-Updates von Xamarin.

2. Installieren Sie die Pakete und Tools für **Android 7.0 (API 24)** oder höher.

3. Erstellen Sie ein neues Xamarin.Android-Projekt für die Zielplattform Android Nougat.

4. Konfigurieren Sie einen Emulator oder ein Gerät für Android Nougat.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="install-xamarin-updates"></a>Installieren von Xamarin-Updates

Um Xamarin-Unterstützung für Android Nougat hinzuzufügen, ändern Sie den Updateskanal in Visual Studio oder Visual Studio für Mac in den stabilen Kanal, und wenden Sie die neuesten Updates an. Wenn Sie auch Features benötigen, die derzeit nur im Alpha- oder Beta-Kanal verfügbar sind, können Sie zum Alpha- oder Beta-Kanal wechseln (Alpha- und Beta-Kanal bieten auch Unterstützung für Android 7.x). Informationen zum Ändern des Kanals für Updates (Releases) finden Sie unter [Changing the Updates Channel](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel) (Ändern des Kanals für Updates).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Um ein Projekt mit Xamarin.Android 7.0 zu erstellen, müssen Sie zunächst den Android-SDK-Manager verwenden, um die **SDK-Plattform für Android N (API 24)** oder höher zu installieren. Sie müssen außerdem die neuesten **Android SDK Tools** installieren:

1. Starten Sie den Android-SDK-Manager (in Visual Studio für Mac über **Extras > Android-SDK-Manager öffnen&hellip;** ; in Visual Studio über **Extras > Android > Android-SDK-Manager**).

2. Installieren Sie **Android 7.0 (API 24)** oder höher:

    [![Auswählen der Android 7.0-Pakete im Android-SDK-Manager](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3. Installieren Sie die neuesten Android SDK Tools:

    [![Auswählen der neuesten Android SDK Tools im Android-SDK-Manager](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Sie müssen Android SDK Tools Revision 25.2.2 oder höher, Android SDK Platform-Tools 24.0.3 oder höher und Android SDK Build-Tools 24.0.2 oder höher installieren.

4. Vergewissern Sie sich, dass der **Java Development Kit-Speicherort** für JDK 1.8 konfiguriert ist:

    [![Konfigurieren des JDK 8-Pfads unter „Extras > Optionen“](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Um diese Einstellung anzuzeigen, klicken Sie in Visual Studio auf **Extras > Optionen > Xamarin > Android-Einstellungen**. Klicken Sie in Visual Studio für Mac auf **Einstellungen > Projekte > SDK-Speicherorte > Android**.

### <a name="start-a-xamarinandroid-project"></a>Starten eines Xamarin.Android-Projekts

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie mit der Android-Entwicklung mit Xamarin noch nicht vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Xamarin.Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versionseinstellungen für Android 7.0 oder höher konfigurieren. Wenn Sie Ihr Projekt beispielsweise auf Android 7.0 ausrichten, müssen Sie die Android-API-Zielebene Ihres Projekts auf **Android 7.0 (API 24 – Nougat)** festlegen. Sie sollten die Ebene für das Zielframework auf API 24 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

> [!NOTE]
> Derzeit müssen Sie die **Android-Mindestversion** auf **Android 7.0 (API 24 – Nougat)** festlegen, um Ihre App auf Android Nougat-Geräten oder -Emulatoren bereitzustellen.

### <a name="configure-an-emulator-or-device"></a>Konfigurieren eines Emulators oder Geräts

Wenn Sie einen Emulator verwenden, starten Sie den Android Virtual Device Manager (AVD Manager), und erstellen Sie unter Verwendung der folgenden Einstellungen ein neues Gerät:

- Gerät: Nexus 5X, Nexus 6, Nexus 6P, Nexus Player, Nexus 9 oder Pixel C.
- Ziel: Android 7.0 / API-Level 24
- ABI: x86 oder x86\_64

Dieses virtuelle Gerät ist beispielsweise für die Emulation eines Nexus 6 konfiguriert:

[![Konfigurieren eines AVD für ein Nexus 6-Gerät, Android 7.0 als Ziel und Intel Atom x86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Wenn Sie ein physisches Gerät wie Nexus 5X, 6 oder 9 verwenden, können Sie Ihr Gerät entweder automatisch mithilfe von OTA-Updates (Over the Air) aktualisieren oder ein Systemimage herunterladen und Ihr Gerät direkt flashen. Weitere Informationen zum manuellen Aktualisieren Ihres Geräts auf Android Nougat finden Sie unter [Full OTA Images for Nexus and Pixel Devices](https://developers.google.com/android/nexus/ota) (Vollständige OTA-Images für Nexus- und Pixel-Geräte).

Beachten Sie, dass Nexus 5-Geräte nicht von Android Nougat unterstützt werden.

## <a name="new-features"></a>Neue Funktionen

Android Nougat führt eine Reihe von neuen Features und Funktionen ein, z. B. Unterstützung für mehrere Fenster, Benachrichtigungserweiterungen und Data Saver. Diese Features werden in den folgenden Abschnitten näher beleuchtet, und es werden Links mit weiteren Informationen zur Nutzung dieser Features in Ihrer App bereitgestellt.

### <a name="multi-window-mode"></a>Modus für mehrere Fenster

Der Modus für mehrere Fenster ermöglicht Benutzern das gleichzeitige Öffnen von zwei Apps mit vollständiger Multitasking-Unterstützung. Diese Apps können nebeneinander (Querformat) oder übereinander (Hochformat) im Split-Screen-Modus ausgeführt werden.
Benutzer können die Größe der Apps ändern, indem sie eine Trennlinie ziehen, und sie können mittels Ausschneiden und Einfügen den Inhalt zwischen den Apps austauschen. Wenn zwei Apps im Modus für mehrere Fenster angezeigt werden, wird die ausgewählte Aktivität weiterhin ausgeführt, während die nicht ausgewählte Aktivität angehalten wird, aber immer noch sichtbar ist. Der Modus für mehrere Fenster ändert den Lebenszyklus der Android-Aktivität nicht.

[![Beispiel-Apps, die im Modus für mehrere Fenster im Hoch- und Querformat ausgeführt werden](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Sie können konfigurieren, wie die Aktivitäten Ihrer Xamarin.Android-App den Modus für mehrere Fenster unterstützen. Beispielsweise können Sie Attribute konfigurieren, die die minimale Größe und die Standardhöhe und -breite Ihrer App im Modus für mehrere Fenster festlegen. Sie können die neue `Activity.IsInMultiWindowMode`-Eigenschaft verwenden, um zu bestimmen, ob sich die Aktivität im Modus für mehrere Fenster befindet. Zum Beispiel:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Die [MultiWindowPlayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground)-Beispiel-App enthält C#-Code, der zeigt, wie Sie die Vorteile einer Benutzeroberfläche mit mehreren Fenstern mit Ihrer App nutzen können.

Weitere Informationen zum Modus für mehrere Fenster finden Sie unter [Multi-Window Support](https://developer.android.com/guide/topics/ui/multi-window.html) (Unterstützung für mehrere Fenster).

### <a name="enhanced-notifications"></a>Verbesserte Benachrichtigungen

Android Nougat führt ein überarbeitetes Benachrichtigungssystem ein. Es verfügt über ein neues Direktantwortfeature, das Benutzern ermöglicht, schnell auf Benachrichtigungen für eingehende SMS in der Benachrichtigungs-Benutzeroberfläche zu antworten. Ab Android 7.0 können Benachrichtigungsmeldungen als einzelne Gruppe gebündelt werden, wenn mehrere Nachrichten empfangen werden. Entwickler können auch Benachrichtigungsansichten anpassen, Systemdekorationen in Benachrichtigungen und neue Benachrichtigungsvorlagen nutzen, wenn sie Benachrichtigungen erstellen.

#### <a name="direct-reply"></a>Direkte Antwort

Wenn ein Benutzer eine Benachrichtigung für die eingehende Nachricht erhält, ermöglicht Android Nougat, innerhalb der Benachrichtigung auf die Nachricht zu antworten (anstatt die Messaging-App zu öffnen, um eine Antwort zu senden).
Dieses Inlineantwortfeature ermöglicht Benutzern, direkt innerhalb der Benachrichtigungs-Benutzeroberfläche schnell auf eine SMS zu reagieren:

[![Screenshot einer Benachrichtigung mit einem Inline-Direktantwortfeld](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Um dieses Feature in Ihrer App zu unterstützen, müssen Sie der App über ein [RemoteInput](xref:Android.App.RemoteInput)-Objekt *Inlineantwortaktionen* hinzufügen, damit Benutzer direkt über die Benutzeroberfläche der Benachrichtigung per Text antworten können.
Der folgende Code erstellt z. B. ein `RemoteInput`-Objekt zum Empfangen von Texteingaben, eine ausstehende Absicht für die Antwortaktion und eine aktivierte Remoteeingabeaktion:

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

Die Beispiel-App für den [Messaging-Dienst](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) enthält C#-Code, der das Erweitern von Benachrichtigungen mit einem `RemoteInput`-Objekt veranschaulicht. Weitere Informationen zum Hinzufügen von Inlineantwortaktionen zu Ihrer App für Android 7.0 oder höher finden Sie im Android-Thema [Replying to Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) (Antworten auf Benachrichtigungen).

#### <a name="bundled-notifications"></a>Gebündelte Benachrichtigungen

Android Nougat kann Benachrichtigungsmeldungen gruppieren (z. B. nach Nachrichtenthema) und die Gruppe anstelle jeder separaten Nachricht anzeigen.
Dieses Feature der *gebündelten Benachrichtigungen* ermöglicht Benutzern, eine Gruppe von Benachrichtigungen in einer Aktion zu verwerfen oder zu archivieren. Der Benutzer kann nach unten streichen, um das Benachrichtigungsbündel zur ausführlichen Anzeige der einzelnen Benachrichtigungen zu erweitern:

[![Screenshot: Beispiel für gebündelte Benachrichtigungen](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Um gebündelte Benachrichtigungen zu unterstützen, kann Ihre App ähnliche Benachrichtigungen mit der [Builder.SetGroup](xref:Android.App.Notification.Builder.SetGroup*)-Methode bündeln. Weitere Informationen zu Gruppen gebündelter Benachrichtigungen in Android N finden Sie im Android-Thema [Bundling Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) (Bündeln von Benachrichtigungen).

#### <a name="custom-views"></a>Benutzerdefinierte Ansichten

Mit Android Nougat können Sie benutzerdefinierte Benachrichtigungsansichten mit Systembenachrichtigungsheadern, Aktionen und erweiterbaren Layouts erstellen. Weitere Informationen zu benutzerdefinierten Benachrichtigungsansichten in Android Nougat finden Sie im Android-Thema [Notification Enhancements](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) (Benachrichtigungserweiterungen).

### <a name="data-saver"></a>Data Saver

Ab Android-Nougat können Benutzer eine neue *Data Saver*-Einstellung aktivieren, die die Verwendung von Hintergrunddaten blockiert. Diese Einstellung signalisiert Ihrer App außerdem, möglichst weniger Daten im Vordergrund zu verwenden. Der [ConnectivityManager](xref:Android.Net.ConnectivityManager) wurde in Android Nougat erweitert, sodass Ihre App überprüfen kann, ob der Benutzer den Data Saver aktiviert hat, damit Ihre App versuchen kann, die Datennutzung einzuschränken, wenn der Data Saver aktiviert ist.

Weitere Informationen zum neuen Data Saver-Feature in Android Nougat finden Sie im Android-Thema [Optimizing Network Data Usage](https://developer.android.com/training/basics/network-ops/data-saver.html) (Optimieren der Netzwerkdatenverwendung).

### <a name="app-shortcuts"></a>App-Verknüpfungen

Mit Android 7.1 wurde das Feature *App-Verknüpfungen* eingeführt, das Benutzern ermöglicht, gängige oder empfohlene Aufgaben mit Ihrer App schnell zu starten.
Um das Menü der Verknüpfungen zu aktivieren, berührt der Benutzer das App-Symbol mindestens eine Sekunde lang &ndash; das Menü wird mit einer schnellen Vibration angezeigt.
Wenn der Benutzer das Symbol nicht mehr berührt, wird das Menü weiterhin angezeigt:

[![Beispielbildschirm eines Menüs mit App-Verknüpfungen für eine Messaging-App](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Dieses Feature ist nur unter API-Ebene 25 oder höher verfügbar.
Weitere Informationen zum neuen App-Verknüpfungen-Feature in Android 7.1 finden Sie im Android-Thema [App-Verknüpfungen](https://developer.android.com/guide/topics/ui/shortcuts.html).

### <a name="sample-code"></a>Beispielcode

Es stehen verschiedene Xamarin.Android-Beispiele zur Verfügung, die veranschaulichen, wie Sie die Vorteile der Android Nougat-Features nutzen:

- [MultiWindowPlayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) veranschaulicht die Verwendung der Mehrfenster-API, die in Android Nougat verfügbar ist. Sie können die Beispiel-App in den Modus für mehrere Fenster umschalten, um zu sehen, wie sich dies auf den Lebenszyklus und das Verhalten der App auswirkt.

- Beim [Messaging-Dienst](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) handelt es sich um einen einfachen Dienst, der Benachrichtigungen mithilfe des `NotificationCompatManager` sendet. Außerdem wird die Benachrichtigung mit einem `RemoteInput`-Objekt erweitert, damit Android Nougat-Geräte direkt per Text von der Benachrichtigung aus antworten können, ohne eine App öffnen zu müssen.

- [Aktive Benachrichtigungen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications) veranschaulicht, wie die `NotificationManager`-API verwendet wird, um Ihnen mitzuteilen, wie viele Benachrichtigungen derzeit von Ihrer Anwendung angezeigt werden.

- [Bereichsbezogener Verzeichniszugriff](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess) veranschaulicht, wie die API für bereichsbezogenen Verzeichniszugriff für den einfachen Zugriff auf bestimmte Verzeichnisse verwendet wird. Dies dient als Alternative zum Definieren von `READ_EXTERNAL_STORAGE`- oder `WRITE_EXTERNAL_STORAGE`-Berechtigungen in ihrem Manifest.

- [Direkter Start](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot) veranschaulicht, wie Daten in einem geräteverschlüsselten Speicher gespeichert werden, der sowohl vor als auch nach der Eingabe von Benutzeranmeldeinformationen (PIN/Muster/Kennwort) immer verfügbar ist, während das Gerät gestartet wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android Nougat vorgestellt und erläutert, wie die neuesten Tools und Pakete zur Xamarin.Android-Entwicklung für Android Nougat installiert und konfiguriert werden. Außerdem bietet er eine Übersicht über die wichtigsten in Android Nougat verfügbaren Features und enthält Links zu Beispielquellcode, die Ihnen den Einstieg in das Erstellen von Apps für Android Nougat erleichtern.

## <a name="related-links"></a>Verwandte Links

- [Android 7.1 für Entwickler](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Versionshinweise zu Xamarin.Android 7.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
