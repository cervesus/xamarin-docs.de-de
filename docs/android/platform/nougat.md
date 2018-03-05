---
title: Nougat-Funktionen
description: "Wie die ersten Schritte mit Xamarin.Android zum Entwickeln von apps für Android Nougat."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 60879273b5a736d4834bd6ba1685d5685fd05e67
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="nougat-features"></a>Nougat-Funktionen

_Wie die ersten Schritte mit Xamarin.Android zum Entwickeln von apps für Android Nougat._

Dieser Artikel bietet ein Überblick über die Funktionen in Android Nougat wird erläutert, wie Android Nougat Entwicklung Xamarin.Android Vorbereitung und enthält Links zu Beispielanwendungen, die veranschaulichen, wie Android Nougat-Features verwenden Xamarin.Android-apps.

<a name="overview" />

## <a name="overview"></a>Übersicht

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) Google nachverfolgung kommentieren, Android 6.0 Marshmallow ist. Xamarin.Android bietet Unterstützung für **Android 7.x Bindungen** in Xamarin Android 7.0 und höher. Android Nougat fügt viele neue APIs für die unten beschriebenen Nougat Features hinzu. Diese APIs sind für Xamarin.Android-apps verfügbar, wenn Sie Xamarin.Android 7.0 verwenden.

[![Hero Bilder von Android-Tablets und Telefone mit Android Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png)

Weitere Informationen zur Android 7.x APIs finden Sie unter [Android 7.1 für Entwickler](http://developer.android.com/preview/api-overview.html).
Eine Liste der bekannten Probleme bei der Xamarin.Android 7.0, finden Sie unter der [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android Nougat bietet viele neue Funktionen von Interesse für Xamarin.Android-Entwickler. Zu diesen Funktionen gehören:

-   **Unterstützung für mehrere Fenster** &ndash; diese Erweiterung ermöglicht es Benutzern, zwei Web-apps auf dem Bildschirm auf einmal zu öffnen.

-   **Benachrichtigung Verbesserungen** &ndash; das Benachrichtigungssystem neu gestaltete in Android Nougat enthält eine *direkte Antwort* Feature, mit denen Benutzer schnell auf SMS-Nachrichten direkt aus die Benachrichtigung reagieren können. UI. Auch wenn die app Benachrichtigungen für erstellt empfangen Nachrichten, wird die neue *gebündelt Benachrichtigungen* Feature kann bündeln Benachrichtigungen als einzelne Gruppe Wenn mehr als eine Nachricht empfangen wird.

-   **Daten Bildschirmschoner** &ndash; diese Funktion ist ein neuer Systemdienst, die der Identifikation von apps mobilfunkdaten reduzieren; es kann Benutzer steuern, wie apps mobilfunkdaten verwenden.

Darüber hinaus bietet Android Nougat viele weitere Verbesserungen für app-Entwickler, z. B. eine neue Sicherheitsfunktion Konfiguration Netzwerk relevant, Doze auf dem laufenden, Schlüssel Nachweis, neue schnelle Einstellungen APIs mit mehreren Gebietsschemas unterstützen, ICU4J-APIs WebView Erweiterungen Zugriff auf die Funktionen der Programmiersprache Java 8, Bereichsbezogene Verzeichniszugriff, eine benutzerdefinierte Zeiger-API, VR-plattformunterstützung, virtuellen Dateien und Optimierungen Verarbeitung im Hintergrund.

In diesem Artikel wird erläutert, wie für den Einstieg in das Erstellen von apps mit Android Nougat Probieren Sie die neuen Funktionen und Planen Sie die Migration oder eine Funktion arbeiten, um die neue Android Nougat Zielplattform wird.


<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Funktionen für Android Nougat in Xamarin-basierten apps zu verwenden:

-   **Visual Studio oder Visual Studio für Mac** &ndash; verwenden Sie Visual Studio, Version 4.2.0.628 oder höher von Xamarin für Visual Studio ist erforderlich. Wenn Sie Visual Studio für Mac, Version 6.1.0 oder später von Visual Studio arbeiten für Mac erforderlich ist.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-   **Android SDK** -Android SDK 7.0 (24-API) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin Android 7.0 development requires [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) or later if you are developing for API level 24 or greater (JDK 8 also supports API levels earlier than 24). Die 64-Bit-Version des JDK 8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder das Vorschauprogramm Forms verwenden.

> [!IMPORTANT]
> **Hinweis:** Xamarin.Android JDK 9 nicht unterstützt.

Beachten Sie, dass apps mit Xamarin C6SR4 oder höher neu erstellt, müssen damit zuverlässig mit Android Nougat arbeiten. Da nur für Android Nougat verknüpfen können [NDK bereitgestellte systemeigene Bibliotheken](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), vorhandene apps mit Bibliotheken wie **Mono.Data.Sqlite.dll** stürzt möglicherweise ab, wenn auf Android Nougat ausgeführt werden soll, wenn sie nicht richtig sind neu erstellt.


<a name="gettingstarted" />

## <a name="getting-started"></a>Erste Schritte

Zum Einstieg Xamarin.Android Android Nougat mit müssen Sie herunterladen und installieren die neuesten Tools und SDK-Paketen, vor dem Erstellen eines Projekts für Android Nougat:

1.  Installieren Sie die neuesten Updates von Xamarin.Android, von der Xamarin.

2.  Installieren der **Android 7.0 (24-API)** -Pakete und-Tools oder höher.

3.  Erstellen Sie ein neues Xamarin.Android-Projekt, dessen Ziel Android Nougat.

4.  Konfigurieren Sie einen Emulator oder Gerät für Android Nougat an.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

<a name="updates" />

### <a name="install-xamarin-updates"></a>Installieren Sie Xamarin-Updates

Xamarin-Unterstützung für Android Nougat hinzufügen, ändern Sie den Kanal "Updates" in Visual Studio oder Visual Studio für Mac stabil Kanal und wenden Sie die neuesten Updates an. Wenn Sie auch Funktionen, die zurzeit nur in den Alpha- oder Beta-Kanal verfügbar sind, können Sie auf den Alpha- oder Beta-Kanal wechseln (die Kanäle Alpha und Beta bieten auch Unterstützung für Android 7.x). Weitere Informationen zu den Updates (Versionen)-Kanal zu ändern, finden Sie unter [ändern den Kanal Updates](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).


<a name="sdk" />

### <a name="install-the-android-sdk"></a>Android SDK installieren

Zum Erstellen eines Projekts mit Xamarin Android 7.0 müssen Sie zuerst den Android SDK-Manager verwenden, installieren **SDK Android-Plattform-N (24-API)** oder höher. Sie müssen zudem installieren Sie das neueste **Android SDK-Tools**:

1.  Starten Sie den Android SDK-Manager (in Visual Studio für Mac verwenden **Tools > Öffnen Sie den Android SDK Manager&hellip;**; in Visual Studio verwenden **Tools > Android > Android SDK Manager**).

2.  Installieren Sie **Android 7.0 (24-API)** oder höher:

    [![Auswählen von Android 7.0-Pakete im Android SDK Manager](nougat-images/preview-packages.png)](nougat-images/preview-packages.png)

3.  Installieren Sie die neueste Android SDK-Tools:

    [![Die neueste Android SDK-Tools auswählen im Android SDK Manager](nougat-images/preview-tools.png)](nougat-images/preview-tools.png)

    Installieren Sie Android SDK-Tools Revision 25.2.2 oder höher, Android-Plattform SDK-Tools 24.0.3 oder höher sowie Android SDK-Buildtools 24.0.2 oder höher.

4.  Überprüfen Sie, ob die **Java Development Kit Speicherort** für JDK 1.8 konfiguriert ist:

    [![Konfigurieren des Pfads JDK 8 unter Extras – Optionen](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png)

    Um diese Einstellung in Visual Studio anzuzeigen, klicken Sie auf **Extras > Optionen > Xamarin > Android-Einstellungen**. Klicken Sie in Visual Studio für Mac auf **Voreinstellungen > Projekte > SDK-Verzeichnissen > Android**.


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Starten eines Projekts Xamarin.Android

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Projekten Xamarin.Android.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen Ziel Android 7.0 oder höher konfigurieren. Z. B. um das Projekt für Android 7.0 abzielen möchten, müssen, konfigurieren Sie die Zielebene Android-API des Projekts in **Android 7.0 (24-API - Nougat)**. Es wird empfohlen, dass Sie die Ziel-Frameworkebene und API-24 oder höher festgelegt ist. Weitere Informationen zum Konfigurieren von Android-API-Ebene Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> **Hinweis:** müssen Sie zurzeit Festlegen der **mindestens Android-Version** auf **Android 7.0 (24-API - Nougat)** zum Bereitstellen Ihrer app für Android Nougat Geräte oder Emulatoren.


<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>Konfigurieren Sie einen Emulator oder Gerät

Wenn Sie einen Emulator verwenden, starten Sie den Android AVD-Manager, und erstellen Sie ein neues Gerät mit den folgenden Einstellungen:

-   Gerät: Nexus 5 X Nexus Nexus 6P Nexus-Player Nexus 9 oder Pixel C. 6
-   Ziel: Android 7.0 - API-Ebene 24
-   ABI: X86 oder X86\_64

Beispielsweise wird dieses virtuelle Gerät zum Emulieren einer Nexus 6 konfiguriert:

[![Konfigurieren ein AVD mit Nexus-6-Geräte, Android 7.0-Ziel und Intel Atom X86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png)

Wenn Sie ein physisches Gerät, z. B. eine Nexus 5 X, 6 oder 9 verwenden, können entweder Aktualisieren Ihres Geräts über automatisch über die Updates per Funk (OTA) oder Herunterladen ein Abbilds und Ihr Gerät direkt flash. Weitere Informationen zum manuellen Aktualisieren Ihres Geräts auf Android Nougat finden Sie unter [OTA Abbildern für Nexus-Geräte](https://developers.google.com/android/nexus/ota).

Beachten Sie, dass Nexus-5-Geräte nicht von Android Nougat unterstützt werden.


<a name="newfeatures" />

## <a name="new-features"></a>Neue Funktionen

Android Nougat führt zu einer Vielzahl von neuen Features und Funktionen, z. B. Unterstützung für Multi-Fenster, Benachrichtigungen Verbesserungen und Daten Bildschirmschoner. In den folgenden Abschnitten veranschaulichen die diese Funktionen und Links helfen Ihnen beim Einstieg diese in Ihrer app bieten.


<a name="multiwindow" />

### <a name="multi-window-mode"></a>Multi-Fenster-Modus

Mit mehreren Fenstermodus ermöglicht es Benutzern, zwei Web-apps mit Unterstützung für die vollständige Multitasking gleichzeitig zu öffnen. Diese Anwendungen können im Modus mit geteiltem Bildschirm Seite-an-Seite (Querformat) oder 1-oben-des-andere (Hochformat) ausgeführt werden.
Benutzer können einen Unterteiler zwischen apps Größe ziehen und sie Ausschneiden und Einfügen von Inhalt können die zwischen apps. Zwei Web-apps im Modus mit mehreren Fenster gegeben werden, wird die ausgewählte Aktivität weiterhin ausgeführt, während die nicht ausgewählte Aktivität angehalten, aber immer noch sichtbar ist. Mit mehreren Fenstermodus ändert nicht den Aktivitätenlebenszyklus von Android.

[![Beispiel-apps, die im Modus mit mehreren Fenster Hochformat- und querformatausrichtung ausgeführt werden.](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png)

Sie können konfigurieren, wie die Aktivitäten Ihrer App Xamarin.Android mit mehreren Fenstermodus unterstützen. Beispielsweise können Sie Attribute konfigurieren, die die minimale Größe und die Standardhöhe und-Breite der app im Multi-Fenster-Modus festgelegt. Sie können die neue `Activity.IsInMultiWindowMode` Eigenschaft, um zu bestimmen, ob die Aktivität im Fenster Multi-Modus befindet. Zum Beispiel:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Die [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Beispiel-app enthält C#-Code, der veranschaulicht, wie Sie mehrere Fenster von Benutzeroberflächen für Ihre app nutzen.

Weitere Informationen zum Fenster Multi-Modus finden Sie unter der [Unterstützung für mehrere Fenster](https://developer.android.com/guide/topics/ui/multi-window.html).


<a name="enhanced_notifications" />

### <a name="enhanced-notifications"></a>Verbesserte Benachrichtigungen

Android Nougat führt ein neu entworfenes Benachrichtigungssystem. Es bietet eine neue direkte Antwort-Funktion, die es Benutzern, schnell eine Antwort von Benachrichtigungen für eingehende SMS direkt in der Benachrichtigung Benutzeroberfläche ermöglicht. Beginnend mit Android 7.0, Benachrichtigung, dass Nachrichten zusammen als eine einzelne Gruppe zusammengefasst werden können, wenn mehr als eine Nachricht empfangen wird. Darüber hinaus können Entwickler Benachrichtigung Ansichten nutzen System Ergänzungen, die in Benachrichtigungen und nutzen Sie die Vorteile der neuen Benachrichtigungsvorlagen beim Generieren von Benachrichtigungen anpassen.

<a name="direct_reply" />

#### <a name="direct-reply"></a>Direkte Antwort

Wenn ein Benutzer eine Benachrichtigung für eingehende Nachricht empfängt, vereinfacht Android Nougat Antworten auf die Nachricht innerhalb der Benachrichtigung (statt der messaging-app zum Senden einer Antwort zu öffnen).
Diese Inline-Antwort-Funktion ermöglicht es Benutzern, schnell auf eine Nachricht per SMS oder Text direkt in der Benutzeroberfläche für die Benachrichtigung reagieren:

[![Bildschirmabbildung von einer Benachrichtigung mit einem Inline-Direct-Antwort-Feld](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png)

Sie müssen zur Unterstützung dieser Funktion in Ihrer app hinzufügen *Inline Antwortaktionen* auf Ihre app über eine [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) Objekt, sodass Benutzer über Text direkt in der UI-Benachrichtigung Antworten können.
Im folgenden Codebeispiel beispielsweise Builds eine `RemoteInput` für den Empfang von Texteingabe, erstellt eine ausstehende beabsichtigte für die Antwortaktion und remote aktiviert Eingabeaktion erstellt:

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

Die [Messagingdienst](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) Beispiel-app enthält C#-Code, der veranschaulicht, wie Benachrichtigungen mit erweitert eine `RemoteInput` Objekt. Weitere Informationen zum Hinzufügen von Inline-Antwort Aktionen an, die Ihre app für Android 7.0 oder höher, finden Sie unter den Android [Antworten auf Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) Thema.

<a name="bundled_notifications" />

#### <a name="bundled-notifications"></a>Gebündelte Benachrichtigungen

Android Nougat kann benachrichtigungsmeldungen (z. B. durch Message Thema) gruppieren und werden jede separate Nachricht, sondern der Gruppe angezeigt.
Dies *gebündelt Benachrichtigungen* Feature ermöglicht es Benutzern, verwerfen oder eine Gruppe von Benachrichtigungen in einer Aktion zu archivieren. Der Benutzer kann nach unten ziehen, um das Paket von Benachrichtigungen an jede Benachrichtigung im Detail zu erweitern:

[![Bildschirmabbildung von Beispiel gebündelte Benachrichtigungen](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png)

Um gebündelte Benachrichtigungen zu unterstützen, können Sie Ihre app die [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) Methode, um ähnliche Benachrichtigungen bündeln. Weitere Informationen zu gebündelte Benachrichtigungsgruppen in Android N, finden Sie unter den Android [Bundling Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) Thema.

<a name="custom_views" />

#### <a name="custom-views"></a>Benutzerdefinierte Ansichten

Android Nougat ermöglicht Ihnen die Erstellung von benutzerdefinierten Benachrichtigung-Ansichten mit Notification Systemheadern, Aktionen und erweiterbaren Layouts. Weitere Informationen zu benutzerdefinierten Benachrichtigung-Ansichten in Android Nougat, finden Sie unter den Android [Benachrichtigung Verbesserungen](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) Thema.


<a name="datasaver" />

### <a name="data-saver"></a>Daten Bildschirmschoner

Beginnen mit dem Android Nougat, Benutzer können ein neues *Daten Bildschirmschoner* festlegen, dass Blöcke Datennutzung im Hintergrund. Diese Einstellung signalisiert auch Ihre app nach Möglichkeit weniger Daten in den Vordergrund zu verwenden. Die [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) erweitert wurde in Android Nougat, damit Ihre app überprüfen kann, ob der Benutzer Daten Bildschirmschoner aktiviert hat, damit die Ihrer app kann Bestreben, ihre Datennutzung eingeschränkt werden, wenn Daten Bildschirmschoner aktiviert ist.

Weitere Informationen zu den neuen Daten Bildschirmschoner-Funktion in Android Nougat finden Sie unter den Android [Optimieren der Netzwerkverwendung](https://developer.android.com/training/basics/network-ops/data-saver.html) Thema.


<a name="app_shortcuts" />

### <a name="app-shortcuts"></a>App-Tastenkombinationen

Android-7.1 eingeführt ein *App Verknüpfungen* -Funktion, die für Benutzer schnell Start allgemeine oder empfohlene Tasks mit der app ermöglicht.
Um das Menü mit Tastenkombinationen zu aktivieren, der Benutzer lange-drückt das app-Symbol für einen zweiten oder mehrere &ndash; im Menü wird mit einer schnellen Vibrationen angezeigt.
Drücken Sie die Freigabe bewirkt, dass das Menü bleibt:

[![Seite "Beispiel" des ein app-Kontextmenü für eine messaging-app](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png)

Diese Funktion ist verfügbar nur API-Ebene 25 oder höher.
Weitere Informationen zu der neuen Funktion von App-Verknüpfungen Android 7.1, finden Sie unter den Android [App Verknüpfungen](https://developer.android.com/guide/topics/ui/shortcuts.html) Thema.

<a name="sample_code" />

### <a name="sample-code"></a>Beispielcode

Mehrere Beispiele für die Xamarin.Android stehen zur Veranschaulichung der Vorgehensweise Android Nougat-Funktionen nutzen:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) veranschaulicht die Verwendung der API mit mehreren Fenster in Android Nougat verfügbar. Sie können die Beispiel-app wechseln, in mehreren Windows-Modus zu sehen, wie sie die app-Lebenszyklus und Verhalten auswirkt.

-   [Messaging-Dienst](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) verwendet ein einfacher Dienst, das Benachrichtigungen sendet die `NotificationCompatManager`. Erweitert die Benachrichtigung mit einem `RemoteInput` Objekt zulassen für Android Nougat Geräte über Text direkt in der Benachrichtigung Antworten, ohne eine app zu öffnen.

-   [Aktive Benachrichtigungen](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) veranschaulicht, wie die `NotificationManager` API Ihnen mitteilen, wie viele Benachrichtigungen Ihre Anwendung derzeit anzeigt.

-   [Im Bereich einer Verzeichniszugriff](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) veranschaulicht, wie die Bereichsbezogene Verzeichniszugriff API auf bestimmte Verzeichnisse zugreifen. Dies dient als Alternative bis hin zu definieren, `READ_EXTERNAL_STORAGE` oder `WRITE_EXTERNAL_STORAGE` Berechtigungen im Manifest.

-   [Direkte Boot](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) wird veranschaulicht, wie zum Speichern von Daten in einem Speicher Gerät verschlüsselt, die immer verfügbar ist, während das Gerät sowohl gestartet wird, bevor und nachdem alle Benutzer credentials(PIN/Pattern/Password) eingegeben werden.

<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel Android Nougat eingeführt, und es wird erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für die Entwicklung von Xamarin.Android auf Android Nougat. Außerdem wird eine Übersicht über die wichtigsten Funktionen in Android Nougat verfügbar bereitgestellt, mit Links zu Beispielcode über die Quelle, die Ihnen beim Einstieg in das Erstellen von apps für Android Nougat helfen.


## <a name="related-links"></a>Verwandte Links

- [Android 7.1 für Entwickler](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0-Versionshinweise](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
