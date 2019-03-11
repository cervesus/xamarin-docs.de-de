---
title: Nougat-Funktionen
description: Informationen zum Einstieg in Xamarin.Android zum Entwickeln von apps für Android Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: de2b92a4007f085a14c16f0c1e8ca9e568df1aff
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668366"
---
# <a name="nougat-features"></a>Nougat-Funktionen

_Informationen zum Einstieg in Xamarin.Android zum Entwickeln von apps für Android Nougat._

Dieser Artikel bietet ein Überblick über die Funktionen in Android Nougat wird erläutert, wie Xamarin.Android für die Entwicklung von Android Nougat vorbereiten, und enthält Links zu Beispielanwendungen, die veranschaulichen, wie Sie mit Android Nougat-Funktionen in Xamarin.Android-apps.


## <a name="overview"></a>Übersicht

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) wird Googles unterwegs auf Android 6.0 Marshmallow. Xamarin.Android bietet Unterstützung für **Android 7.x Bindungen** in Xamarin Android 7.0 und höher. Android Nougat fügt viele neue APIs für die unten beschriebenen Nougat-Features; Diese APIs sind für Xamarin.Android-apps verfügbar, bei der Verwendung von Xamarin.Android 7.0.

[![Hero-Abbilder von Android-Tablets und Telefone mit Android Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Weitere Informationen zu Android 7.x-APIs, finden Sie unter [Android 7.1 für Entwickler](https://developer.android.com/preview/api-overview.html).
Eine Liste bekannter Probleme in Xamarin.Android 7.0, finden Sie unter den [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android Nougat bietet viele neue Funktionen für Xamarin.Android-Entwickler. Zu diesen Features zählen:

-   **Unterstützung für mehrere Fenster** &ndash; diese Erweiterung ermöglicht es Benutzern, zwei apps auf dem Bildschirm auf einmal zu öffnen.

-   **Verbesserungen der Benachrichtigung** &ndash; das Benachrichtigungssystem neu gestaltete im Android Nougat verfügt über eine *direkte Antworten* Feature, mit denen Benutzer schnell auf SMS-Nachrichten direkt aus die Benachrichtigung reagieren zu können -BENUTZEROBERFLÄCHE. Wenn Ihre app-Benachrichtigungen für erstellt darüber hinaus erhalten Nachrichten, wird die neue *gebündelt Benachrichtigungen* Feature kann bündeln Benachrichtigungen als einzelne Gruppe Wenn mehr als eine Nachricht empfangen wird.

-   **Daten-Bildschirmschoner** &ndash; dieses Feature ist ein neuer Systemdienst, mit dem können datenverbindungen von apps zu reduzieren; sie können Benutzer steuern, wie apps mobilfunkdaten verwenden.

Darüber hinaus bietet Android Nougat viele weitere Verbesserungen von app-Entwicklern wie z. B. ein neues Netzwerk Konfiguration Sicherheitsfeature von, Doze unterwegs, wichtige Verbesserungen für Nachweis, neue APIs schnell Einstellungen, mit mehreren Gebietsschema-Unterstützung, API-ICU4J WebView, Zugriff auf Java 8-Sprachfeatures, Bereichsbezogene Verzeichniszugriff, eine benutzerdefinierte Zeiger-API, VR-plattformunterstützung, virtuellen Dateien und Optimierungen für die hintergrundverarbeitung.

In diesem Artikel wird erläutert, wie für den Einstieg in die Erstellung von apps mit Android Nougat testen die neuen Features und Planen der Migration oder ein Feature arbeiten, um die neue Android Nougat-Plattform als Ziel festzulegen.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Features für Android Nougat in Xamarin-basierte apps verwenden:

-   **Visual Studio oder Visual Studio für Mac** &ndash; verwenden Sie Visual Studio, Version 4.2.0.628 oder höher von Visual Studio-Tools für Xamarin ist erforderlich. Wenn Sie Visual Studio für Mac, Version 6.1.0 oder höher von Visual Studio verwenden für Mac erforderlich ist.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 oder höher muss installiert und konfiguriert mit Visual Studio oder Visual Studio für Mac.

-   **Android SDK** -Android SDK-7.0 (API 24) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin Android 7.0-Entwicklung erfordert [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für API-Ebene 24 entwickeln oder größer (JDK 8 unterstützt auch API-Ebenen älter als 24). Die 64-Bit-Version von JDK 8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder die Forms-Vorschau verwenden.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

Beachten Sie, dass es sich bei apps mit Xamarin C6SR4 oder höher neu erstellt werden müssen mit Android Nougat zuverlässig funktioniert. Da nur für Android Nougat verknüpfen können [NDK bereitgestellte systemeigene Bibliotheken](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), vorhandene apps mit Bibliotheken wie **Mono.Data.Sqlite.dll** stürzt möglicherweise ab, wenn auf Android Nougat ausgeführt werden soll, wenn sie nicht richtig sind neu erstellt.



## <a name="getting-started"></a>Erste Schritte

Zum Einstieg anhand von Android Nougat mit Xamarin.Android, müssen Sie herunterladen und die neuesten Tools und SDK-Pakete installieren, bevor Sie ein Android Nougat-Projekt erstellen können:

1.  Installieren Sie die neuesten Xamarin.Android-Updates von Xamarin an.

2.  Installieren Sie die **Android 7.0 (API 24)** -Pakete und Tools oder höher.

3.  Erstellen eines neuen Xamarin.Android-Projekts, das als Ziel Android Nougat.

4.  Konfigurieren Sie einen Emulator oder Gerät für Android Nougat an.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:


### <a name="install-xamarin-updates"></a>Installieren von Xamarin-Updates

Xamarin-Unterstützung für Android Nougat hinzufügen, ändern Sie den updatekanal "in Visual Studio oder Visual Studio für Mac, in der stabile Kanal und wenden Sie die neuesten Updates an. Wenn Sie auch Features, die derzeit nur in den Alpha- oder Betakanal Kanal verfügbar sind benötigen, können Sie auf den Kanal Alpha- oder Betakanal wechseln (die Alpha- und Beta-Kanäle bieten auch Unterstützung für Android 7.x). Weitere Informationen zur Verwendung der updatekanal (Releases) zu ändern, finden Sie unter [Ändern der Updatekanal](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).



### <a name="install-the-android-sdk"></a>Installieren Sie das Android SDK

Zum Erstellen eines Projekts mit Xamarin Android 7.0 müssen Sie zuerst den Android SDK-Manager zum Installieren verwenden **SDK Android-Plattform-N (API 24)** oder höher. Außerdem müssen Sie die neueste Version installieren **Android SDK Tools**:

1.  Starten Sie den Android SDK Manager (verwenden Sie in Visual Studio für Mac **Tools > Open Android SDK Manager&hellip;**; in Visual Studio verwenden **Tools > Android > Android SDK-Manager**).

2.  Installieren Sie **Android 7.0 (API 24)** oder höher:

    [![Auswählen von Android 7.0-Pakete im Android SDK-Manager](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Installieren Sie die neuesten Android SDK-Tools:

    [![Wählen die neuesten Android SDK-Tools im Android SDK-Manager](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Sie müssen die Android SDK Tools-Revision 25.2.2 oder höher, Android SDK Platform-Tools 24.0.3 oder höher sowie Android SDK-Buildtools 24.0.2 installieren oder höher.

4.  Überprüfen Sie, ob die **Java Development Kit-Speicherort** für JDK 1.8 konfiguriert ist:

    [![Konfigurieren des Pfads JDK 8 unter Extras/Optionen](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Um diese Einstellung in Visual Studio anzuzeigen, klicken Sie auf **Tools > Optionen > Xamarin > Android-Einstellungen**. Klicken Sie in Visual Studio für Mac auf **Voreinstellungen > Projekte > SDK-Speicherorte > Android**.



### <a name="start-a-xamarinandroid-project"></a>Ein Xamarin.Android-Projekt starten

Erstellen eines neuen Xamarin.Android-Projekts an. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/index.md) um weitere Informationen zum Erstellen von Xamarin.Android-Projekte.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen Ziel Android 7.0 oder höher konfigurieren. Z. B. um das Projekt für Android 7.0 Ziel festzulegen, müssen Sie konfigurieren die Android-API-Zielebene des Projekts in **Android 7.0 (API 24 – Nougat)**. Es wird empfohlen, dass Sie Ihr Framework Zielebene auf API 24 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebene Level, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Derzeit müssen Sie festlegen, die **Minimum Android Version** zu **Android 7.0 (API 24 – Nougat)** zum Bereitstellen Ihrer app für Android Nougat-Geräte oder Emulatoren.



### <a name="configure-an-emulator-or-device"></a>Konfigurieren Sie einen Emulator oder Gerät

Wenn Sie einen Emulator verwenden, starten Sie den Android AVD-Manager aus, und erstellen Sie ein neues Gerät mit den folgenden Einstellungen:

-   Gerät: Nexus 5 X, Nexus 6, Nexus 6P, Nexus-Player Nexus 9 oder Pixel C.
-   Ziel: Android 7.0 – API-Ebene 24
-   ABI: X86 oder X86\_64

Dieses virtuelle Gerät ist beispielsweise so konfiguriert, um eine Nexus 6 emulieren:

[![Konfigurieren ein AVD mit Nexus 6-Geräte, Android 7.0-Ziel und Intel Atom X86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Bei Verwendung ein physisches Geräts wie eine Nexus 5 X, 6 oder 9, können Sie entweder Aktualisieren Ihres Geräts über automatische über die Updates per Funk (OTA) oder Laden ein Systemabbild und Ihr Gerät direkt flash. Weitere Informationen zum manuellen Aktualisieren der Ihr Gerät auf Android Nougat finden Sie unter [OTA-Images für Nexus-Geräte](https://developers.google.com/android/nexus/ota).

Beachten Sie, dass Nexus 5-Geräte nicht von Android Nougat unterstützt werden.



## <a name="new-features"></a>Neue Funktionen

Android Nougat führt zu einer Vielzahl neuer Features und Funktionen, z. B. Unterstützung für mehrere Fenster sowie Verbesserungen der Benachrichtigungen und Daten Bildschirmschoner. In den folgenden Abschnitten veranschaulichen die folgenden Funktionen und Links können Sie beginnen, verwenden diese in Ihrer app bieten.



### <a name="multi-window-mode"></a>Multi-Window-Modus

Mit mehreren Fenstern Installationsmodus für Benutzer zum Öffnen von zwei apps gleichzeitig mit Unterstützung von vollständigen Multitasking möglich. Diese Anwendungen können im Modus mit geteiltem Bildschirm Seite-an-Seite (Querformat) oder 1-über-die-other (Hochformat) ausgeführt werden.
Benutzer können eine Trennlinie ziehen, zwischen den apps, die ihre Größe zu ändern, und sie Ausschneiden und Einfügen von Inhalt können die zwischen apps. Wenn zwei apps im Modus mit mehreren Fenstern angezeigt werden, weiterhin die ausgewählte Aktivität ausgeführt werden, während die nicht ausgewählte Aktivität angehalten, aber immer noch sichtbar ist. Multi-Window-Modus ändert nicht den Lebenszyklus der Android-Aktivität.

[![Beispiel-apps, die im Modus mit mehreren Fenstern von hoch- und Querformat ausgeführt werden.](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Sie können konfigurieren, wie die Aktivitäten Ihrer Xamarin.Android-App mit mehreren Fenstern-Modus unterstützt. Sie können z. B. Attribute konfigurieren, die die minimale Größe und die Standardhöhe und die Breite der app in mehreren Fenstern Modus festgelegt. Sie können die neue `Activity.IsInMultiWindowMode` Eigenschaft, um zu bestimmen, ob die Aktivität im Multi-Window-Modus befindet. Zum Beispiel:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Die [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Beispiel-app enthält C#-Code, der zeigt, wie Sie mehrere Fenster von Benutzeroberflächen für Ihre app nutzen.

Weitere Informationen zum Multi-Window-Modus finden Sie unter den [Unterstützung für mehrere Fenster](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Verbesserte Benachrichtigungen

Android Nougat führt eine überarbeitete Benachrichtigungssystem. Es bietet eine neue direkte Antwort-Funktion, die für Benutzer, schnell eine Antwort von Benachrichtigungen für eingehende SMS direkt in der Benachrichtigung Benutzeroberfläche ermöglicht. Beginnend mit Android 7.0, Benachrichtigung, dass Nachrichten zusammen als eine einzelne Gruppe gebündelt werden können, wenn mehr als eine Nachricht empfangen wird. Entwickler können außerdem Benachrichtigung-Ansichten, System Ergänzungen in Benachrichtigungen nutzen und profitieren Sie von neuen Benachrichtigungsvorlagen beim Generieren von Benachrichtigungen anpassen.


#### <a name="direct-reply"></a>Direkte Antworten

Wenn ein Benutzer eine Benachrichtigung für die eingehende Nachricht empfängt, ermöglicht Android Nougat Antworten auf die Nachricht in der Benachrichtigung (und nicht öffnen Sie die messaging-app eine Antwort senden).
Dieses Feature der Inline-Antwort ermöglicht es Benutzern, schnell auf eine SMS oder Text-Nachricht direkt in der Benachrichtigungsschnittstelle reagieren:

[![Bildschirmabbildung von einer Benachrichtigung mit einem Inline-Direct-Antwort-Feld](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Um dieses Feature in Ihrer app zu unterstützen, müssen Sie hinzufügen, *Inline Antwortaktionen* zu Ihrer app über eine [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) Objekts, sodass Benutzer direkt aus der Benachrichtigung UI Antwort per Textnachricht senden können.
Der folgende code z. B. Builds eine `RemoteInput` für den Empfang von Texteingabe, erstellt eine pendingintent für die Antwortaktion, und eine Remoteaktion Eingabe aktivierte:

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

Diese Aktion wird die Benachrichtigung hinzugefügt:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

Die [Messagingdienst](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) Beispiel-app enthält C#-Code, der zeigt, wie Benachrichtigungen mit erweitert eine `RemoteInput` Objekt. Weitere Informationen zum Hinzufügen von Inline-Antwort Aktionen zu Ihrer app für Android 7.0 oder höher, finden Sie im Android [Antworten auf Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) Thema.


#### <a name="bundled-notifications"></a>Gebündelte Benachrichtigungen

Android Nougat kann Benachrichtigungen (z. B. durch Message Thema) gruppieren und anzeigen, die Gruppe statt jede separate Nachricht.
Dies *gebündelt Benachrichtigungen* Feature ermöglicht es Benutzern, verwerfen oder eine Gruppe von Benachrichtigungen in einer Aktion zu archivieren. Der Benutzer kann nach unten schieben, erweitern Sie das Paket von Benachrichtigungen an jede Benachrichtigung im Detail anzuzeigen:

[![Beispielhafter Screenshot der gebündelten Benachrichtigungen](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Um gebündelte abfragebenachrichtigungen zu unterstützen, können Sie Ihre app die [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) Methode, um ähnliche Benachrichtigungen zu bündeln. Weitere Informationen zu gebündelte Benachrichtigungsgruppen in Android-N, finden Sie unter Android [Bündelung Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) Thema.


#### <a name="custom-views"></a>Benutzerdefinierte Ansichten

Android Nougat ermöglicht es Ihnen, benutzerdefinierte Benachrichtigung-Ansichten mit Benachrichtigung Systemheader, Aktionen und erweiterbare Layouts erstellen. Weitere Informationen über benutzerdefinierte Benachrichtigung-Ansichten in Android Nougat finden Sie im Android [Benachrichtigung Verbesserungen](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) Thema.



### <a name="data-saver"></a>Daten-Bildschirmschoner

Ab Android Nougat, Benutzer können ein neues *Daten Bildschirmschoner* festlegen, dass die Blöcke im Hintergrund der Datennutzung. Diese Einstellung signalisiert auch Ihre app weniger Daten in den Vordergrund zu verwenden, wo immer dies möglich. Die [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) wurde erweitert, in Android Nougat, damit Ihre app prüfen, ob der Benutzer Daten Bildschirmschoner aktiviert hat, damit Ihre app kann zu dessen Verwendung zu beschränken, wenn Daten Bildschirmschoner aktiviert wird.

Weitere Informationen über die neue Daten Bildschirmschoner-Funktion in Android Nougat finden Sie im Android [Optimieren der Netzwerknutzung](https://developer.android.com/training/basics/network-ops/data-saver.html) Thema.



### <a name="app-shortcuts"></a>App-Verknüpfungen

Android 7.1 eingeführt, eine *App-Verknüpfungen* -Funktion, die für Benutzer schnell allgemeine oder empfohlenen starttasks mit Ihrer app ermöglicht.
Um das Menü mit Tastenkombinationen zu aktivieren, der Benutzer lange-drückt das app-Symbol für einen zweiten oder mehrere &ndash; das Menü wird angezeigt, mit der eine schnelle Vibration.
Drücken Sie die Freigabe bewirkt, dass Sie im Menü bleiben:

[![Beispielbildschirm, der eine app-Kontextmenü für eine messaging-app](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Dieses Feature ist verfügbar nur API-Ebene 25 oder höher.
Weitere Informationen über das neue Feature der App-Verknüpfungen in Android 7.1-Version finden Sie im Android [App-Verknüpfungen](https://developer.android.com/guide/topics/ui/shortcuts.html) Thema.


### <a name="sample-code"></a>Beispielcode

Mehrere Xamarin.Android-Beispiele sind verfügbar, wie Sie Android Nougat-Funktionen nutzen können:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) veranschaulicht die Verwendung der API mit mehreren Fenstern in Android Nougat verfügbar. Sie können die Beispiel-app wechseln, in mehreren Windows-Modus zu sehen, wie es sich um Lebenszyklus und das Verhalten der app auswirkt.

-   [Messaging-Dienst](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) verwendet ein einfacher Dienst, das Benachrichtigungen sendet die `NotificationCompatManager`. Es erweitert auch die Benachrichtigung mit einem `RemoteInput` Objekt, das Android Nougat Geräten über Text direkt aus der Benachrichtigung Antworten, ohne dass eine app geöffnet werden darf.

-   [Aktive Benachrichtigungen](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) veranschaulicht, wie die `NotificationManager` API Ihre Anwendung derzeit zeigt Ihnen mitteilen, wie viele Benachrichtigungen.

-   [Im Bereich einer Verzeichniszugriff](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) veranschaulicht, wie die Bereichsbezogene Verzeichniszugriff API und komfortabel auf bestimmte Verzeichnisse zugreifen. Dies dient als Alternative zum definieren müssen `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` Berechtigungen im Manifest.

-   [Direkte Boot](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) wird veranschaulicht, wie Sie Daten in einem Gerät verschlüsselte Speicher zu speichern, die immer verfügbar ist, während das Gerät sowohl gestartet wird, bevor und nachdem alle Benutzer credentials(PIN/Pattern/Password) eingegeben werden.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel Android Nougat eingeführt und wurde erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für die Entwicklung von Xamarin.Android auf Android Nougat. Außerdem wird eine Übersicht über die wichtigsten Features in Android Nougat bereitgestellt, mit Links zu den Beispielquellcode können Sie die ersten Schritte bei der Erstellung von apps für Android Nougat.


## <a name="related-links"></a>Verwandte Links

- [Android 7.1 für Entwickler](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0-Versionshinweise](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
