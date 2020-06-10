---
title: Lokale Benachrichtigungen unter Android
description: In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in xamarin. Android implementiert werden. Darin werden die verschiedenen Elemente der Benutzeroberfläche einer Android-Benachrichtigung erläutert, und es wird erläutert, welche API zum Erstellen und Anzeigen einer Benachrichtigung beteiligt ist.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/16/2018
ms.openlocfilehash: 87a6790e89f7f7ff0f84b4b59e7b4de9b1383bc3
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84566964"
---
# <a name="local-notifications-on-android"></a>Lokale Benachrichtigungen unter Android

_In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in xamarin. Android implementiert werden. Darin werden die verschiedenen Elemente der Benutzeroberfläche einer Android-Benachrichtigung erläutert, und es wird erläutert, welche API zum Erstellen und Anzeigen einer Benachrichtigung beteiligt ist._

## <a name="local-notifications-overview"></a>Übersicht über lokale Benachrichtigungen

Android bietet zwei System gesteuerte Bereiche zum Anzeigen von Benachrichtigungs Symbolen und Benachrichtigungs Informationen für den Benutzer. Wenn eine Benachrichtigung zum ersten Mal veröffentlicht wird, wird das zugehörige Symbol im *Benachrichtigungsbereich*angezeigt, wie im folgenden Screenshot zu sehen:

![Beispiel für einen Benachrichtigungsbereich auf einem Gerät](local-notifications-images/01-notification-shade.png)

Um Details zu der Benachrichtigung zu erhalten, kann der Benutzer die Benachrichtigungsleiste öffnen (wodurch das Benachrichtigungssymbol erweitert wird, um Benachrichtigungs Inhalt anzuzeigen) und alle mit den Benachrichtigungen verknüpften Aktionen ausführen. Der folgende Screenshot zeigt eine *Benachrichtigungs* Leiste, die dem oben angezeigten Benachrichtigungsbereich entspricht:

[![Beispiel einer Benachrichtigungsleiste, die drei Benachrichtigungen anzeigt](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Für Android-Benachrichtigungen werden zwei Arten von Layouts verwendet:

- ***Basis Layout*** &ndash; ein kompaktes, festes Präsentationsformat.

- ***Erweitertes Layout*** &ndash; ein Darstellungsformat, das auf eine größere Größe erweitert werden kann, um weitere Informationen anzuzeigen.

Die einzelnen Layouttypen (und deren Erstellung) werden in den folgenden Abschnitten erläutert.

> [!NOTE]
> Dieser Leitfaden konzentriert sich auf die [notificationcompat-APIs](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) aus der [Android-Unterstützungs Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Diese APIs stellen eine maximale Abwärtskompatibilität mit Android 4,0 (API-Ebene 14) sicher.

### <a name="base-layout"></a>Basis Layout

Alle Android-Benachrichtigungen basieren auf dem basislayoutformat, das mindestens die folgenden Elemente enthält:

1. Ein *Benachrichtigungssymbol*, das die ursprüngliche APP darstellt, oder der Benachrichtigungstyp, wenn die APP verschiedene Benachrichtigungs Typen unterstützt.

2. Der Benachrichtigungs *Titel*oder der Name des Absenders, wenn es sich bei der Benachrichtigung um eine persönliche Nachricht handelt.

3. Die Benachrichtigungs Meldung.

4. Ein *Zeitstempel*.

Diese Elemente werden wie in der folgenden Abbildung dargestellt angezeigt:

[![Speicherort von Benachrichtigungs Elementen](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Basis Layouts sind auf 64 Dichte unabhängige Pixel (DP) beschränkt. Android erstellt standardmäßig diesen Standard Benachrichtigungs Stil.

Optional kann für Benachrichtigungen ein großes Symbol angezeigt werden, das die Anwendung oder das Foto des Absenders darstellt. Wenn ein großes Symbol in einer Benachrichtigung in Android 5,0 und höher verwendet wird, wird das kleine Benachrichtigungssymbol als Badge über dem großen Symbol angezeigt:

![Einfaches Benachrichtigungs Foto](local-notifications-images/04-simple-notification-photo.png)

Ab Android 5,0 können Benachrichtigungen auch auf dem Sperrbildschirm angezeigt werden:

[![Beispiel für eine Sperrbildschirm Benachrichtigung](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Der Benutzer kann auf die Sperrbildschirm Benachrichtigung Doppel tippen, um das Gerät zu entsperren und zu der APP zu springen, die diese Benachrichtigung ausgelöst hat, oder die Benachrichtigung zu verwerfen. Apps können die Sichtbarkeits Ebene einer Benachrichtigung festlegen, um zu steuern, was auf dem Sperrbildschirm angezeigt wird, und Benutzer können auswählen, ob sensible Inhalte in Sperrbildschirm Benachrichtigungen angezeigt werden sollen.

Android 5,0 führte ein Benachrichtigungs Präsentationsformat mit hoher Priorität als *Heads-up*ein. Heads-up-Benachrichtigungen werden für einige Sekunden vom oberen Bildschirmrand nach unten und dann wieder in den Benachrichtigungsbereich zurückgezogen:

[![Beispiel für eine Heads-up-Benachrichtigung](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Heads-up-Benachrichtigungen ermöglichen es der Benutzeroberfläche des Systems, wichtige Informationen vor dem Benutzer zu platzieren, ohne den Zustand der aktuell laufenden Aktivität zu unterbrechen.

Android bietet Unterstützung für Benachrichtigungs Metadaten, sodass Benachrichtigungen Intelligent sortiert und angezeigt werden können. Benachrichtigungs Metadaten Steuern auch, wie Benachrichtigungen auf dem Sperrbildschirm und im Heads-up-Format angezeigt werden. Anwendungen können die folgenden Typen von Benachrichtigungs Metadaten festlegen:

- **Priorität** &ndash; Die Prioritätsstufe bestimmt, wie und wann Benachrichtigungen angezeigt werden. Beispielsweise werden in Android 5,0 Benachrichtigungen mit hoher Priorität als Heads-up-Benachrichtigungen angezeigt.

- **Sichtbarkeit** &ndash; Gibt an, wie viel Benachrichtigungs Inhalt angezeigt werden soll, wenn die Benachrichtigung auf dem Sperrbildschirm angezeigt wird.

- **Kategorie** &ndash; Informiert das System darüber, wie die Benachrichtigung in verschiedenen Fällen behandelt werden soll, z. b. wenn sich das Gerät im Modus " *nicht stören* " befindet.

> [!NOTE]
> **Sichtbarkeit** und **Kategorie** wurden in Android 5,0 eingeführt und sind in früheren Versionen von Android nicht verfügbar. Ab Android 8,0 werden [Benachrichtigungs Kanäle](#notif-chan) verwendet, um zu steuern, wie Benachrichtigungen für den Benutzer angezeigt werden.

### <a name="expanded-layouts"></a>Erweiterte Layouts

Ab Android 4,1 können Benachrichtigungen mit erweiterten Layoutstilen konfiguriert werden, die es dem Benutzer ermöglichen, die Höhe der Benachrichtigung zu erweitern, um weitere Inhalte anzuzeigen. Im folgenden Beispiel wird beispielsweise eine erweiterte layoutbenachrichtigung im Modus "vertraglich" veranschaulicht:

![Benachrichtigung über den Vertrag](local-notifications-images/07-contracted-notification.png)

Wenn diese Benachrichtigung erweitert wird, wird die gesamte Meldung angezeigt:

![Erweiterte Benachrichtigung](local-notifications-images/08-expanded-notification.png)

Android unterstützt drei erweiterte Layoutstile für Benachrichtigungen mit nur einem Ereignis:

- ***Big Text*** &ndash; Im Modus "vertraglich" wird ein Auszug der ersten Zeile der Nachricht angezeigt, gefolgt von zwei Zeiträumen. Im erweiterten Modus zeigt die gesamte Meldung an (wie im obigen Beispiel gezeigt).

- ***Posteingang*** &ndash; Im Modus "vertraglich" wird die Anzahl der neuen Nachrichten angezeigt. Im erweiterten Modus wird die erste e-Mail-Nachricht oder eine Liste der Nachrichten im Posteingang angezeigt.

- ***Bild*** &ndash; Im Modus "vertraglich" wird nur der Meldungs Text angezeigt. Im erweiterten Modus zeigt den Text und ein Bild an.

[Über die grundlegende Benachrichtigung hinaus](#beyond-the-basic-notification) (später in diesem Artikel *) wird erläutert, wie*Sie *Big Text*-, Eingangsbox-und *Bild* Benachrichtigungen erstellen.

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>Benachrichtigungs Kanäle

Ab Android 8,0 (Oreo) können Sie die Funktion " *Benachrichtigungs Kanäle* " verwenden, um einen vom Benutzer anpassbaren Kanal für jede Art von Benachrichtigung zu erstellen, die Sie anzeigen möchten. Benachrichtigungs Kanäle ermöglichen es Ihnen, Benachrichtigungen zu gruppieren, sodass alle an einen Kanal geposteten Benachrichtigungen das gleiche Verhalten aufweisen. Beispielsweise können Sie einen Benachrichtigungs Kanal haben, der für Benachrichtigungen gedacht ist, die sofortige Aufmerksamkeit erfordern, und einen separaten "ruhigeren" Kanal, der für Informationsmeldungen verwendet wird.

Die mit Android Oreo installierte **YouTube** -App listet zwei Benachrichtigungs Kategorien auf: **Download Benachrichtigungen** und **Allgemeine Benachrichtigungen**:

[![Benachrichtigungs Bildschirme für YouTube in Android Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Jede dieser Kategorien entspricht einem Benachrichtigungs Kanal. Die YouTube-App implementiert einen **Download Benachrichtigungs** Kanal und einen **allgemeinen Benachrichtigungs** Kanal. Der Benutzer kann auf **Benachrichtigungen herunterladen**tippen, wodurch der Bildschirm "Einstellungen" für den Download Benachrichtigungs Kanal der App angezeigt wird:

[![Bildschirm zum Herunterladen von Benachrichtigungen für die YouTube-App](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

In diesem Bildschirm kann der Benutzer das Verhalten des Kanals zum **herunterladen** von Benachrichtigungen wie folgt ändern:

- Legen Sie die Wichtigkeits Stufe auf " **dringend**", " **hoch**", " **Mittel**" oder " **niedrig**" fest, um die Ebene der Sound-und visuelle Unterbrechung

- Aktivieren oder deaktivieren Sie den Benachrichtigungs Punkt.

- Aktivieren oder deaktivieren Sie die blinkende Beleuchtung.

- Anzeigen oder Ausblenden von Benachrichtigungen auf dem Sperrbildschirm

- Überschreiben **Sie** die Einstellung nicht stören.

Der Kanal für **Allgemeine Benachrichtigungen** weist ähnliche Einstellungen auf:

[![Bildschirm "allgemeine Benachrichtigungen" für die YouTube-App](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Beachten Sie, dass Sie nicht die absolute Kontrolle darüber haben, wie Ihre Benachrichtigungs Kanäle mit dem Benutzer interagieren &ndash; . der Benutzer kann die Einstellungen für einen beliebigen Benachrichtigungs Kanal auf dem Gerät ändern, wie in den obigen Screenshots gezeigt. Sie können jedoch Standardwerte konfigurieren (wie unten beschrieben). Wie diese Beispiele veranschaulichen, können Sie mit der neuen Benachrichtigungs Kanäle eine präzisere Kontrolle über verschiedene Arten von Benachrichtigungen erhalten.

## <a name="notification-creation"></a>Benachrichtigungs Erstellung

Um eine Benachrichtigung in Android zu erstellen, verwenden Sie die [notificationcompat. Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder) -Klasse aus dem nuget-Paket [xamarin. Android. Support. v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) . Diese Klasse ermöglicht das Erstellen und Veröffentlichen von Benachrichtigungen für ältere Versionen von Android.
`NotificationCompat.Builder`Außerdem wird erläutert.

`NotificationCompat.Builder`bietet Methoden zum Festlegen der verschiedenen Optionen in einer Benachrichtigung, z. b.:

- Der Inhalt, einschließlich Titel, Nachrichtentext und Benachrichtigungssymbol.

- Der Stil der Benachrichtigung, z. b. *Big Text*, Eingangsbox oder *Bildstil* . *Inbox*

- Die Priorität der Benachrichtigung: minimal, niedrig, Standard, hoch oder Maximum. Unter Android 8,0 und höher wird die Priorität über einen [_Benachrichtigungs Kanal_](#notification-channels)festgelegt.

- Die Sichtbarkeit der Benachrichtigung auf dem Sperrbildschirm: öffentlich, privat oder Geheimnis.

- Kategorieinformationen, die Android beim klassifizieren und Filtern der Benachrichtigung unterstützen.

- Eine optionale Absicht, die eine Aktivität angibt, die beim Tippen der Benachrichtigung gestartet werden soll.

- Die ID des Benachrichtigungs Kanals, auf dem die Benachrichtigung veröffentlicht wird (Android 8,0 und höher).

Nachdem Sie diese Optionen im Generator festgelegt haben, generieren Sie ein Benachrichtigungs Objekt, das die Einstellungen enthält. Zum Veröffentlichen der Benachrichtigung übergeben Sie dieses Benachrichtigungs Objekt an den *Benachrichtigungs-Manager*. Android stellt die [notificationmanager](xref:Android.App.NotificationManager) -Klasse zur Verfügung, die für das Veröffentlichen von Benachrichtigungen und das Anzeigen der Benachrichtigungen für den Benutzer zuständig ist. Ein Verweis auf diese Klasse kann aus jedem Kontext abgerufen werden, z. b. einer Aktivität oder einem Dienst.

### <a name="creating-a-notification-channel"></a>Erstellen eines Benachrichtigungs Kanals

Apps, die unter Android 8,0 ausgeführt werden, müssen für Ihre Benachrichtigungen einen Benachrichtigungs Kanal erstellen. Ein Benachrichtigungs Kanal erfordert die folgenden drei Informationen:

- Eine ID-Zeichenfolge, die für das Paket, das den Kanal identifiziert, eindeutig ist.
- Der Name des Kanals, der dem Benutzer angezeigt wird.  Der Name muss zwischen 1 und 40 Zeichen lang sein.
- Die Wichtigkeit des Kanals.

Apps müssen die Version von Android überprüfen, auf der Sie ausgeführt werden.
Geräte, auf denen ältere Versionen als Android 8,0 ausgeführt werden, sollten keinen Benachrichtigungs Kanal erstellen. Die folgende Methode ist ein Beispiel für das Erstellen eines Benachrichtigungs Kanals in einer Aktivität:

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channelName = Resources.GetString(Resource.String.channel_name);
    var channelDescription = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
                  {
                      Description = channelDescription
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Der Benachrichtigungs Kanal sollte jedes Mal erstellt werden, wenn die Aktivität erstellt wird. Für die- `CreateNotificationChannel` Methode sollte Sie in der-Methode einer-Aktivität aufgerufen werden `OnCreate` .

### <a name="creating-and-publishing-a-notification"></a>Erstellen und Veröffentlichen einer Benachrichtigung

Führen Sie die folgenden Schritte aus, um eine Benachrichtigung in Android zu generieren:

1. Instanziieren Sie ein `NotificationCompat.Builder`-Objekt.

2. Ruft verschiedene Methoden für das- `NotificationCompat.Builder` Objekt auf, um Benachrichtigungs Optionen festzulegen.

3. Ruft die [Build](xref:Android.App.Notification.Builder.Build) -Methode des- `NotificationCompat.Builder` Objekts auf, um ein Benachrichtigungs Objekt zu instanziieren.

4. Ruft die [Benachrichtigungs Methode des](xref:Android.App.NotificationManager.Notify*) Benachrichtigungs-Managers auf, um die Benachrichtigung zu veröffentlichen.

Sie müssen mindestens die folgenden Informationen für jede Benachrichtigung angeben:

- Ein kleines Symbol (24x24 DP in Größe)

- Ein Kurztitel

- Der Text der Benachrichtigung.

Im folgenden Codebeispiel wird veranschaulicht, wie verwendet wird, `NotificationCompat.Builder` um eine grundlegende Benachrichtigung zu generieren. Beachten Sie, dass `NotificationCompat.Builder` Methoden das [Verketten](https://en.wikipedia.org/wiki/Method_chaining)von Methoden unterstützen, d. h. jede Methode gibt das Generator Objekt zurück, sodass Sie das Ergebnis des letzten Methoden Aufrufs verwenden können, um den nächsten Methodenaufruf aufzurufen:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

In diesem Beispiel wird ein neues- `NotificationCompat.Builder` Objekt `builder` mit der Bezeichnung instanziiert, zusammen mit der ID des zu verwendenden Benachrichtigungs Kanals. Der Titel und der Text der Benachrichtigung werden festgelegt, und das Benachrichtigungssymbol wird aus " **Resources/drawable/ic_notification. png**" geladen. Der-aufruder der-Methode des Benachrichtigungs-Generators `Build` erstellt ein Benachrichtigungs Objekt mit diesen Einstellungen. Der nächste Schritt besteht darin, die- `Notify` Methode des Benachrichtigungs-Managers aufzurufen. Um den Benachrichtigungs-Manager zu suchen, nennen Sie `GetSystemService` , wie oben gezeigt.

Die `Notify` -Methode akzeptiert zwei Parameter: den Benachrichtigungs Bezeichner und das Benachrichtigungs Objekt. Der Benachrichtigungs Bezeichner ist eine eindeutige ganze Zahl, die die Benachrichtigung an Ihre Anwendung identifiziert. In diesem Beispiel ist der Benachrichtigungs Bezeichner auf NULL (0) festgelegt. in einer Produktionsanwendung empfiehlt es sich jedoch, jeder Benachrichtigung einen eindeutigen Bezeichner zu übergeben. Wenn Sie den vorherigen Bezeichnerwert in einem-Befehl verwenden, `Notify` wird die letzte Benachrichtigung überschrieben.

Wenn dieser Code auf einem Android 5,0-Gerät ausgeführt wird, generiert er eine Benachrichtigung, die wie im folgenden Beispiel aussieht:

![Benachrichtigungs Ergebnis für den Beispielcode](local-notifications-images/09-hello-world.png)

Das Benachrichtigungssymbol wird auf der linken Seite der Benachrichtigung angezeigt &ndash; . dieses Bild eines kreisförmigen &ldquo; i &rdquo; hat einen Alphakanal, sodass Android einen grauen Zirkel Hintergrund dahinter zeichnen kann. Sie können auch ein Symbol ohne Alphakanal angeben. Informationen zum Anzeigen eines Foto Bilds als Symbol finden Sie unter [großes Symbol Format](#large-icon-format) weiter unten in diesem Thema.

Der Zeitstempel wird automatisch festgelegt, aber Sie können diese Einstellung überschreiben, indem Sie die [setwhen](xref:Android.App.Notification.Builder.SetWhen*) -Methode des Benachrichtigungs-Generators aufrufen. Im folgenden Codebeispiel wird z. b. der Zeitstempel auf die aktuelle Zeit festgelegt:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Aktivieren von Sound und Vibrationen

Wenn Sie möchten, dass die Benachrichtigung ebenfalls einen Sound wieder gibt, können Sie die [SetDefaults](xref:Android.App.Notification.Builder.SetDefaults*) -Methode des Benachrichtigungs-Generators aufrufen und das- `NotificationDefaults.Sound` Flag übergeben:

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Dieser-Befehl `SetDefaults` bewirkt, dass das Gerät beim Veröffentlichen der Benachrichtigung einen Sound wieder gibt. Wenn Sie möchten, dass das Gerät vibrieren kann, anstatt einen Sound wiederzugeben, können Sie an übergeben, `NotificationDefaults.Vibrate` `SetDefaults.` Wenn Sie möchten, dass das Gerät einen Sound wieder gibt und das Gerät vibrieren kann. Sie können beide Flags an Folgendes übergeben `SetDefaults` :

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Wenn Sie Sound ohne Angabe eines Sounds aktivieren, verwendet Android das standardmäßige System Benachrichtigungs Sound. Sie können jedoch den Sound ändern, der wiedergegeben wird, indem Sie die [setsound](xref:Android.App.Notification.Builder.SetSound*) -Methode des Benachrichtigungs-Generators aufrufen. Wenn Sie z. b. den Alarm Sound mit Ihrer Benachrichtigung (anstelle des standardbenachrichtigungs Sounds) wiedergeben möchten, können Sie den URI für den Alarm Sound von " [ringtonemanager](xref:Android.Media.RingtoneManager) " erhalten und an Folgendes übergeben `SetSound` :

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternativ können Sie den Standardwert für den System Standardton für Ihre Benachrichtigung verwenden:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Nachdem Sie ein Benachrichtigungs Objekt erstellt haben, ist es möglich, Benachrichtigungs Eigenschaften für das Benachrichtigungs Objekt festzulegen (anstatt Sie im Voraus durch Methoden zu konfigurieren `NotificationCompat.Builder` ). Anstatt z. b. die `SetDefaults` -Methode zum Aktivieren von Vibrationen für eine Benachrichtigung aufzurufenden, können Sie das Bitflag der [Standard](xref:Android.App.Notification.Defaults) Eigenschaft der Benachrichtigung direkt ändern:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Dieses Beispiel bewirkt, dass das Gerät beim Veröffentlichen der Benachrichtigung vibrieren kann.

### <a name="updating-a-notification"></a>Aktualisieren einer Benachrichtigung

Wenn Sie den Inhalt einer Benachrichtigung nach der Veröffentlichung aktualisieren möchten, können Sie das vorhandene Objekt wieder verwenden, `NotificationCompat.Builder` um ein neues Benachrichtigungs Objekt zu erstellen und diese Benachrichtigung mit dem Bezeichner der letzten Benachrichtigung zu veröffentlichen. Beispiel:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

In diesem Beispiel wird das vorhandene- `NotificationCompat.Builder` Objekt zum Erstellen eines neuen Benachrichtigungs Objekts mit einem anderen Titel und einer anderen Nachricht verwendet.
Das neue Benachrichtigungs Objekt wird mit dem Bezeichner der vorherigen Benachrichtigung veröffentlicht. Dadurch wird der Inhalt der zuvor veröffentlichten Benachrichtigung aktualisiert:

![Aktualisierte Benachrichtigung](local-notifications-images/12-updated-notification.png)

Der Text der vorherigen Benachrichtigung wird nur dann wieder verwendet, &ndash; Wenn der Titel und der Text der Benachrichtigung geändert werden, während die Benachrichtigung in der Benachrichtigungs Anzeige angezeigt wird. Der Titeltext ändert sich von "Sample Notification" in "aktualisierte Benachrichtigung", und der Meldungs Text ändert sich von "Hallo Welt! Dies ist meine erste Benachrichtigung. " an diese Meldung "geändert".

Eine Benachrichtigung bleibt sichtbar, bis eines von drei Dingen geschieht:

- Der Benutzer lehnt die Benachrichtigung ab (oder tippt auf *Alle löschen*).

- Die Anwendung ruft an `NotificationManager.Cancel` und übergibt dabei die eindeutige Benachrichtigungs-ID, die beim Veröffentlichen der Benachrichtigung zugewiesen wurde.

- Die Anwendung ruft auf `NotificationManager.CancelAll` .

Weitere Informationen zum Aktualisieren von Android-Benachrichtigungen finden Sie unter [Ändern einer Benachrichtigung](https://developer.android.com/training/notify-user/managing.html#Updating).

### <a name="starting-an-activity-from-a-notification"></a>Starten einer Aktivität über eine Benachrichtigung

In Android ist es üblich, dass eine Benachrichtigung mit einer *Aktion* verknüpft wird, &ndash; eine Aktivität, die gestartet wird, wenn der Benutzer auf die Benachrichtigung tippt. Diese Aktivität kann sich in einer anderen Anwendung oder sogar in einer anderen Aufgabe befinden. Wenn Sie einer Benachrichtigung eine Aktion hinzufügen möchten, erstellen Sie ein [pdingintent](xref:Android.App.PendingIntent) -Objekt und ordnen das der `PendingIntent` Benachrichtigung zu. Ein `PendingIntent` ist eine besondere Art von Absicht, die es der Empfänger Anwendung ermöglicht, ein vordefiniertes Code Element mit den Berechtigungen der sendenden Anwendung auszuführen. Wenn der Benutzer auf die Benachrichtigung tippt, startet Android die Aktivität, die von angegeben wird `PendingIntent` .

Der folgende Code Ausschnitt veranschaulicht, wie eine Benachrichtigung mit einem erstellt wird, der `PendingIntent` die Aktivität der Ursprungs-App startet `MainActivity` :

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Dieser Code ähnelt dem Benachrichtigungs Code im vorherigen Abschnitt, mit dem Unterschied, dass `PendingIntent` dem Benachrichtigungs Objekt eine hinzugefügt wird. In diesem Beispiel ist der der `PendingIntent` Aktivität der Ursprungs-App zugeordnet, bevor Sie an die [setcontentintent](xref:Android.App.Notification.Builder.SetContentIntent*) -Methode des Benachrichtigungs-Generators übermittelt wird. Das- `PendingIntentFlags.OneShot` Flag wird an die-Methode übermittelt `PendingIntent.GetActivity` , sodass `PendingIntent` nur einmal verwendet wird. Wenn dieser Code ausgeführt wird, wird die folgende Benachrichtigung angezeigt:

![Erste Aktions Benachrichtigung](local-notifications-images/10-first-action-notification.png)

Wenn Sie auf diese Benachrichtigung tippen, wird der Benutzer zur ursprünglichen Aktivität zurückkehren.

In einer Produktions-app muss Ihre APP den *BackStack* verarbeiten, wenn der Benutzer die Schaltfläche " **zurück** " innerhalb der Benachrichtigungs Aktivität drückt (wenn Sie mit Android-Aufgaben und dem BackStack nicht vertraut sind, finden Sie weitere Informationen unter [Tasks und BackStack](https://developer.android.com/guide/components/tasks-and-back-stack.html)).
In den meisten Fällen sollte die Navigation aus der Benachrichtigungs Aktivität den Benutzer aus der APP und zurück zum Startbildschirm zurückgeben. Um den BackStack zu verwalten, verwendet Ihre APP die [taskstackbuilder](xref:Android.App.TaskStackBuilder) -Klasse, um eine `PendingIntent` mit einem BackStack zu erstellen.

Ein weiterer realer Aspekt ist, dass die Ursprungs Aktivität möglicherweise Daten an die Benachrichtigungs Aktivität sendet. Die Benachrichtigung kann z. b. angeben, dass eine Textnachricht eingetroffen ist, und die Benachrichtigungs Aktivität (ein Bildschirm für die Nachrichten Anzeige) erfordert, dass die ID der Nachricht dem Benutzer angezeigt wird. Die Aktivität, die den erstellt, `PendingIntent` kann die [Intent. putextra](xref:Android.Content.Intent.PutExtra*) -Methode verwenden, um Daten (z. b. eine Zeichenfolge) der Absicht hinzuzufügen, sodass diese Daten an die Benachrichtigungs Aktivität weitergegeben werden.

Im folgenden Codebeispiel wird veranschaulicht, wie verwendet `TaskStackBuilder` wird, um den BackStack zu verwalten. er enthält ein Beispiel für das Senden einer einzelnen Meldungs Zeichenfolge an eine Benachrichtigungs Aktivität namens `SecondActivity` :

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

In diesem Codebeispiel besteht die APP aus zwei Aktivitäten: `MainActivity` (enthält den obigen Benachrichtigungs Code) und `SecondActivity` , dem Bildschirm, der dem Benutzer nach dem Tippen auf die Benachrichtigung angezeigt wird. Wenn dieser Code ausgeführt wird, wird eine einfache Benachrichtigung (ähnlich wie im vorherigen Beispiel) präsentiert. Wenn Sie auf die Benachrichtigung tippen, wird der Benutzer auf dem `SecondActivity` Bildschirm angezeigt:

![Screenshot der zweiten Aktivität](local-notifications-images/11-second-activity.png)

Die Zeichen folgen Nachricht (die an die-Methode der Absicht geleitet `PutExtra` wird) wird in `SecondActivity` über diese Codezeile abgerufen:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Diese abgerufene Meldung "Greetings from mainactivity!" wird auf dem `SecondActivity` Bildschirm angezeigt, wie im obigen Screenshot dargestellt. Wenn der Benutzer die Schaltfläche " **zurück** " drückt `SecondActivity` , führt die Navigation aus der APP und zurück zum Bildschirm, der vor dem Start der APP liegt.

Weitere Informationen zum Erstellen von ausstehenden Intents finden Sie unter " [tzdingintent](xref:Android.App.PendingIntent)".

<a name="beyond-the-basic-notification"></a>

## <a name="beyond-the-basic-notification"></a>Über die grundlegende Benachrichtigung hinaus

Benachrichtigungen werden standardmäßig in Android als basislayoutformat angezeigt, aber Sie können dieses grundlegende Format verbessern, indem Sie zusätzliche `NotificationCompat.Builder` Methodenaufrufe durchführen. In diesem Abschnitt erfahren Sie, wie Sie Ihrer Benachrichtigung ein großes Foto Symbol hinzufügen, und Sie sehen Beispiele zum Erstellen erweiterter layoutbenachrichtigungen.

<a name="large-icon-format"></a>

### <a name="large-icon-format"></a>Großes Symbol Format

Android-Benachrichtigungen zeigen in der Regel das Symbol der Ursprungs-APP an (auf der linken Seite der Benachrichtigung). Benachrichtigungen können jedoch ein Bild oder ein Foto (ein *großes Symbol*) anstelle des standardmäßigen kleinen Symbols anzeigen. Beispielsweise könnte eine Messaging-App ein Foto des Absenders anstelle des App-Symbols anzeigen.

Im folgenden finden Sie ein Beispiel für eine einfache Android 5,0-Benachrichtigung, dass &ndash; nur das Symbol für die kleine APP angezeigt wird:

![Beispiel für normale Benachrichtigung](local-notifications-images/13-sample-notification.png)

Und hier ist ein Screenshot der Benachrichtigung, nachdem Sie geändert wurde, dass ein großes Symbol angezeigt wird. &ndash; Sie verwendet ein Symbol, das aus einem Bild eines xamarin-Code-affals erstellt wurde:

![Beispiel Benachrichtigung über großes Symbol](local-notifications-images/14-large-icon-sample.png)

Beachten Sie Folgendes: Wenn eine Benachrichtigung im großen Symbol Format angezeigt wird, wird das Symbol für die kleine APP als Badge in der unteren rechten Ecke des großen Symbols angezeigt.

Wenn Sie ein Bild als großes Symbol in einer Benachrichtigung verwenden möchten, können Sie die [setlargeicon](xref:Android.App.Notification.Builder.SetLargeIcon*) -Methode des Benachrichtigungs-Generators aufrufen und eine Bitmap des Bilds übergeben. Im Gegensatz zu `SetSmallIcon` `SetLargeIcon` akzeptiert nur eine Bitmap. Um eine Bilddatei in eine Bitmap zu konvertieren, verwenden Sie die [bitmapfactory](xref:Android.Graphics.BitmapFactory) -Klasse. Beispiel:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Dieser Beispielcode öffnet die Bilddatei unter **Resources/drawable/monkey_icon. png**, konvertiert sie in eine Bitmap und übergibt die resultierende Bitmap an `NotificationCompat.Builder` . Normalerweise ist die Quell Abbild Auflösung größer als das kleine Symbol, &ndash; aber nicht viel größer. Ein zu großes Bild kann zu unnötigen Änderungs Vorgängen führen, die das Senden der Benachrichtigung verzögern können.

### <a name="big-text-style"></a>Big Text-Stil

Der *Big Text* -Stil ist eine erweiterte Layoutvorlage, mit der Sie lange Nachrichten in Benachrichtigungen anzeigen können. Wie bei allen erweiterten layoutbenachrichtigungen wird die Big Text-Benachrichtigung anfänglich in einem Compact Presentation-Format angezeigt:

![Beispiel für eine Big Text-Benachrichtigung](local-notifications-images/15-big-text-notification.png)

In diesem Format wird nur ein Auszug der Nachricht angezeigt, der durch zwei Punkte beendet wird. Wenn der Benutzer die Benachrichtigung abzieht, wird er erweitert, um die gesamte Benachrichtigungs Meldung anzuzeigen:

![Erweiterte Big Text-Benachrichtigung](local-notifications-images/16-big-text-expanded.png)

Dieses erweiterte Layoutformat enthält auch den Zusammenfassungs Text am unteren Rand der Benachrichtigung. Die maximale Höhe der *Big Text* -Benachrichtigung beträgt 256 DP.

Um eine *Big Text* -Benachrichtigung zu erstellen, instanziieren Sie ein `NotificationCompat.Builder` -Objekt wie zuvor, und instanziieren Sie anschließend ein [bigtextstyle](xref:Android.App.Notification.BigTextStyle) -Objekt, und fügen Sie es dem- `NotificationCompat.Builder` Objekt hinzu. Beispiel:

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

In diesem Beispiel werden der Meldungs Text und der Zusammenfassungs Text im- `BigTextStyle` Objekt ( `textStyle` ) vor der Übergabe an gespeichert.`NotificationCompat.Builder.`

### <a name="image-style"></a>Bildstil

Der *Bildstil* (auch als *großer Bildstil* bezeichnet) ist ein erweitertes Benachrichtigungs Format, das Sie verwenden können, um ein Bild im Text einer Benachrichtigung anzuzeigen. Beispielsweise kann eine Bildschirm-APP oder eine Foto-APP den *Bild* Benachrichtigungs Stil verwenden, um dem Benutzer eine Benachrichtigung über das zuletzt erfasste Bild bereitzustellen. Beachten Sie, dass die maximale Höhe der *Abbild* Benachrichtigung 256 ist, dass DP &ndash; Android die Größe des Bilds in die Größenbeschränkung der maximalen Höhe innerhalb der Grenzen des verfügbaren Arbeitsspeichers einfügt.

Wie alle erweiterten layoutbenachrichtigungen werden *Bild* Benachrichtigungen zuerst in einem kompakten Format angezeigt, das einen Auszug aus dem zugehörigen Meldungs Text anzeigt:

![Benachrichtigung über Compact-Image zeigt kein Bild an](local-notifications-images/17-image-compact.png)

Wenn der Benutzer die *Bild* Benachrichtigung abzieht, wird er erweitert, um ein Bild anzuzeigen. Hier ist beispielsweise die erweiterte Version der vorherigen Benachrichtigung:

![Erweiterte Bild Benachrichtigung zeigt Bild an](local-notifications-images/18-image-expanded.png)

Beachten Sie, dass bei Anzeige der Benachrichtigung im Compact-Format ein Benachrichtigungs Text angezeigt wird (der Text, der an die-Methode des Benachrichtigungs Generators weitergeleitet wird `SetContentText` , wie zuvor gezeigt). Wenn die Benachrichtigung jedoch erweitert wird, um das Bild anzuzeigen, wird ein Übersichts Text oberhalb des Bilds angezeigt.

Um eine *Bild* Benachrichtigung zu erstellen, instanziieren Sie ein `NotificationCompat.Builder` -Objekt wie zuvor, und erstellen Sie dann ein [bigpicturestyle](xref:Android.App.Notification.BigPictureStyle) -Objekt, und fügen Sie es in das-Objekt ein `NotificationCompat.Builder` . Beispiel:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

Wie die- `SetLargeIcon` Methode von `NotificationCompat.Builder` erfordert die [BigPicture](xref:Android.App.Notification.BigPictureStyle.BigPicture*) -Methode von `BigPictureStyle` eine Bitmap des Bilds, das im Text der Benachrichtigung angezeigt werden soll. In diesem Beispiel liest die [decoderesource](xref:Android.Graphics.BitmapFactory.DecodeResource*) -Methode von `BitmapFactory` die Bilddatei unter **Resources/drawable/x_bldg. png** und konvertiert sie in eine Bitmap.

Sie können auch Bilder anzeigen, die nicht als Ressource verpackt sind. Der folgende Beispielcode lädt z. b. ein Bild von der lokalen SD-Karte und zeigt es in einer *Bild* Benachrichtigung an:

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

In diesem Beispiel wird die Bilddatei unter **/sdcard/Pictures/My-Tshirt.jpg** geladen, in der Größe auf die Hälfte der ursprünglichen Größe verkleinert und dann in eine Bitmap konvertiert, um Sie in der Benachrichtigung zu verwenden:

![Beispiel eines T-Shirt-Bilds in der Benachrichtigung](local-notifications-images/19-tshirt-notification.png)

Wenn Sie die Größe der Bilddatei im Voraus nicht kennen, empfiehlt es sich, den Aufrufe von [bitmapfactory. DecodeFile](xref:Android.Graphics.BitmapFactory.DecodeFile*) in einem Ausnahmehandler zu umschließen. &ndash; eine `OutOfMemoryError` Ausnahme kann ausgelöst werden, wenn das Image zu groß für die Größenänderung von Android ist.

Weitere Informationen zum Laden und Decodieren von großen Bitmap-Bildern finden Sie unter [effizientes Laden von großen Bitmaps](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently).

### <a name="inbox-style"></a>Posteingangs Stil

Das Eingangsbox Format ist eine erweiterte Layoutvorlage *, die zum* Anzeigen von separaten Textzeilen (z. b. eine Zusammenfassung der e-Mail-Eingangsbox) im Text Die Format Benachrichtigung für den *Posteingang* wird zuerst in einem kompakten Format angezeigt:

![Beispiel Benachrichtigung über Compact Inbox](local-notifications-images/20-inbox-compact.png)

Wenn der Benutzer die Benachrichtigung abzieht, wird er erweitert, um eine e-Mail-Zusammenfassung anzuzeigen, wie im folgenden Screenshot zu sehen:

![Beispiel für erweiterte Posteingangs Benachrichtigung](local-notifications-images/21-inbox-expanded.png)

Um eine *Posteingangs* Benachrichtigung zu erstellen, instanziieren Sie ein `NotificationCompat.Builder` -Objekt wie zuvor, und fügen Sie dem ein [inboxstyle](xref:Android.App.Notification.InboxStyle) -Objekt hinzu `NotificationCompat.Builder` . Beispiel:

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

Um dem Benachrichtigungs Text neue Textzeilen hinzuzufügen, müssen Sie die [AddLine](xref:Android.App.Notification.InboxStyle.AddLine*) -Methode des- `InboxStyle` Objekts (die maximale Höhe der *Posteingang* Benachrichtigung ist 256 DP) aufzurufen. Beachten Sie, dass der Eingangsbox Stil im Gegensatz zum *groß Text* *Stil einzelne* Textzeilen im Benachrichtigungs Text unterstützt.

Sie können auch den *Posteingangs* Stil für jede Benachrichtigung verwenden, die einzelne Textzeilen in einem erweiterten Format anzeigen muss. Beispielsweise kann der Benachrichtigungs Stil für die Eingangsbox verwendet werden, um mehrere ausstehende Benachrichtigungen in einer Zusammenfassungs *Benachrichtigung zu* kombinieren &ndash; . Sie können eine einzelne Eingangsbox-Benachrichtigung mit neuen Benachrichtigungs Inhalt aktualisieren (siehe [Aktualisieren einer obigen Benachrichtigung](#updating-a-notification) ), anstatt einen kontinuierlichen Stream neuer, größtenteils ähnlicher Benachrichtigungen zu generieren. *Inbox*

## <a name="configuring-metadata"></a>Konfigurieren von Metadaten

`NotificationCompat.Builder`schließt Methoden ein, die aufgerufen werden können, um Metadaten zu Ihrer Benachrichtigung festzulegen, z. b. Priorität, Sichtbarkeit und Kategorie. Android verwendet diese Informationen &mdash; zusammen mit den Einstellungen der Benutzereinstellungen &mdash; , um zu bestimmen, wie und wann Benachrichtigungen angezeigt werden sollen.

### <a name="priority-settings"></a>Prioritäts Einstellungen

Apps, die unter Android 7,1 und niedriger ausgeführt werden, müssen die Priorität direkt auf die Benachrichtigung festlegen. Die Prioritäts Einstellung einer Benachrichtigung bestimmt zwei Ergebnisse, wenn die Benachrichtigung veröffentlicht wird:

- Gibt an, wo die Benachrichtigung in Bezug auf andere Benachrichtigungen angezeigt wird.
    Beispielsweise werden Benachrichtigungen mit hoher Priorität in der Benachrichtigungsleiste oberhalb der Benachrichtigungen mit niedrigerer Priorität angezeigt, unabhängig davon, wann die einzelnen Benachrichtigungen veröffentlicht wurden.

- Gibt an, ob die Benachrichtigung im Heads-up-Benachrichtigungs Format (Android 5,0 und höher) angezeigt wird. Nur Warnungen für *hohe* und *Maximale* Priorität werden als Heads-up-Benachrichtigungen angezeigt.

Xamarin. Android definiert die folgenden Enumerationen zum Festlegen der Benachrichtigungs Priorität:

- `NotificationPriority.Max`&ndash;Benachrichtigt den Benutzer auf eine dringende oder eine kritische Bedingung (z. b. einen eingehenden oder ein-und ausschalten oder eine Notfall Warnung). Auf Geräten mit Android 5,0 und höher werden Benachrichtigungen mit maximaler Priorität im Heads-up-Format angezeigt.

- `NotificationPriority.High`&ndash;Informiert den Benutzer über wichtige Ereignisse (z. b. wichtige e-Mails oder die Ankunft von Echt Zeit Chat Nachrichten). Auf Geräten mit Android 5,0 und höher werden Benachrichtigungen mit hoher Priorität im Heads-up-Format angezeigt.

- `NotificationPriority.Default`&ndash;Benachrichtigt den Benutzer über Bedingungen mit einer mittleren Wichtigkeit.

- `NotificationPriority.Low`&ndash;Für nicht dringende Informationen, über die der Benutzer informiert werden muss (z. b. Software Update Erinnerungen oder Updates von sozialen Netzwerken).

- `NotificationPriority.Min`&ndash;Hintergrundinformationen, die der Benutzer nur beim Anzeigen von Benachrichtigungen bemerkt (z. b. Speicherort-oder Wetterinformationen).

Um die Priorität einer Benachrichtigung festzulegen, müssen Sie die [SetPriority](xref:Android.App.Notification.Builder.SetPriority*) -Methode des- `NotificationCompat.Builder` Objekts aufrufen, indem Sie die Prioritätsstufe übergeben. Beispiel:

```csharp
builder.SetPriority (NotificationPriority.High);
```

Im folgenden Beispiel wird die Benachrichtigung mit hoher Priorität "eine wichtige Meldung!" wird am oberen Rand der Benachrichtigungsleiste angezeigt:

![Beispiel Benachrichtigung mit hoher Priorität](local-notifications-images/22-hi-priority-drawer.png)

Da es sich hierbei um eine Benachrichtigung mit hoher Priorität handelt, wird Sie auch als Heads-up-Benachrichtigung über dem Bildschirm aktuelle Aktivität des Benutzers in Android 5,0 angezeigt:

![Beispiel für eine Heads-up-Benachrichtigung](local-notifications-images/23-heads-up-example.png)

Im nächsten Beispiel wird die Benachrichtigung "dachte für den Tag" mit niedriger Priorität unter einer Benachrichtigung über den Akku Pegel mit höherer Priorität angezeigt:

![Beispiel Benachrichtigung mit niedriger Priorität](local-notifications-images/24-lo-priority-drawer.png)

Da es sich bei der Benachrichtigung für den Tag um eine Benachrichtigung mit niedriger Priorität handelt, wird Sie von Android nicht im Heads-up-Format angezeigt.

> [!NOTE]
> Unter Android 8,0 und höher wird die Priorität der Benachrichtigung durch die Priorität des Benachrichtigungs Kanals und der Benutzereinstellungen festgelegt.

### <a name="visibility-settings"></a>Sichtbarkeitseinstellungen

Ab Android 5,0 ist die *Sichtbarkeits* Einstellung verfügbar, um zu steuern, wie viel Benachrichtigungs Inhalt auf dem sicheren Sperrbildschirm angezeigt wird.
Xamarin. Android definiert die folgenden Enumerationen zum Festlegen der Sichtbarkeit von Benachrichtigungen:

- `NotificationVisibility.Public`&ndash;Der vollständige Inhalt der Benachrichtigung wird auf dem Bildschirm sichere Sperre angezeigt.

- `NotificationVisibility.Private`&ndash;Auf dem sicheren Sperrbildschirm werden nur wichtige Informationen angezeigt (z. b. das Benachrichtigungssymbol und der Name der APP, von der das Symbol gepostet wurde), die restlichen Details der Benachrichtigung werden jedoch ausgeblendet. Alle Benachrichtigungen werden standardmäßig auf eingestellt `NotificationVisibility.Private` .

- `NotificationVisibility.Secret`&ndash;Auf dem sicheren Sperrbildschirm wird nichts angezeigt, nicht sogar das Benachrichtigungssymbol. Der Benachrichtigungs Inhalt ist erst verfügbar, nachdem der Benutzer das Gerät entsperrt hat.

Um die Sichtbarkeit einer Benachrichtigung festzulegen, wird die- `SetVisibility` Methode des-Objekts von apps aufgerufen `NotificationCompat.Builder` , und die Sichtbarkeits Einstellung wird übergeben. Beispielsweise wird mit dem folgenden Befehl `SetVisibility` die Benachrichtigung erstellt `Private` :

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Wenn eine `Private` Benachrichtigung gesendet wird, werden nur der Name und das Symbol der APP auf dem sicheren Sperrbildschirm angezeigt. Der Benutzer sieht anstelle der Benachrichtigungs Meldung "Entsperren Ihres Geräts, um diese Benachrichtigung anzuzeigen":

![Entsperren der Geräte Benachrichtigungs Meldung](local-notifications-images/25-lockscreen-private.png)

In diesem Beispiel ist **notificationslab** der Name der ursprünglichen app. Diese redktive Version der Benachrichtigung wird nur angezeigt, wenn der Sperrbildschirm sicher ist (z. b. durch PIN, Muster oder Kennwort geschützt) &ndash; , wenn der Sperrbildschirm nicht sicher ist. der gesamte Inhalt der Benachrichtigung ist auf dem Sperrbildschirm verfügbar.

### <a name="category-settings"></a>Kategorieeinstellungen

Ab Android 5,0 sind vordefinierte Kategorien für das Anordnen und Filtern von Benachrichtigungen verfügbar. Xamarin. Android stellt die folgenden Enumerationen für diese Kategorien bereit:

- `Notification.CategoryCall`&ndash;Eingehender Telefonanruf.

- `Notification.CategoryMessage`&ndash;Eingehende Textnachricht.

- `Notification.CategoryAlarm`&ndash;Eine Alarmbedingung oder ein Timer-Ablauf.

- `Notification.CategoryEmail`&ndash;Eingehende e-Mail-Nachricht

- `Notification.CategoryEvent`&ndash;Ein Kalenderereignis.

- `Notification.CategoryPromo`&ndash;Eine Werbe Meldung oder eine Werbung.

- `Notification.CategoryProgress`&ndash;Der Fortschritt eines Hintergrund Vorgangs.

- `Notification.CategorySocial`&ndash;Aktualisierung des sozialen Netzwerks.

- `Notification.CategoryError`&ndash;Fehler bei einem Hintergrund Vorgang oder Authentifizierungs Vorgang.

- `Notification.CategoryTransport`&ndash;Update der Medienwiedergabe.

- `Notification.CategorySystem`&ndash;Reserviert für die Verwendung durch das System (System-oder Gerätestatus).

- `Notification.CategoryService`&ndash;Gibt an, dass ein Hintergrunddienst ausgeführt wird.

- `Notification.CategoryRecommendation`&ndash;Eine Empfehlungs Nachricht, die sich auf die aktuell laufende App bezieht.

- `Notification.CategoryStatus`&ndash;Informationen zum Gerät.

Wenn Benachrichtigungen sortiert werden, hat die Priorität der Benachrichtigung Vorrang vor der Einstellung ihrer Kategorie. Beispielsweise wird eine Benachrichtigung mit hoher Priorität als Heads-up angezeigt, auch wenn Sie zur `Promo` Kategorie gehört. Um die Kategorie einer Benachrichtigung festzulegen, wird die- `SetCategory` Methode des- `NotificationCompat.Builder` Objekts aufgerufen, und die-Kategorieeinstellung wird übergeben. Beispiel:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

Die Funktion " *nicht stören* " (neu in Android 5,0) filtert Benachrichtigungen auf der Grundlage von Kategorien. Beispielsweise ermöglicht der Bildschirm " *nicht stören* " in " **Einstellungen** " dem Benutzer das Ausschließen von Benachrichtigungen für Telefonanrufe und Nachrichten:

![Bildschirm Switches nicht stören](local-notifications-images/26-do-not-disturb.png)

Wenn der Benutzer konfiguriert ist, dass *nicht* alle Interrupts mit Ausnahme von Telefon anrufen blockiert werden (wie im obigen Screenshot dargestellt), ermöglicht Android, dass Benachrichtigungen mit der Kategorieeinstellung angezeigt `Notification.CategoryCall` werden, während sich das Gerät im Modus " *nicht stören* " befindet. Beachten Sie, dass `Notification.CategoryAlarm` Benachrichtigungen im Modus " *nicht stören* " niemals blockiert werden.

Das [localnotification](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications) -Beispiel veranschaulicht, wie verwendet wird, `NotificationCompat.Builder` um eine zweite Aktivität aus einer Benachrichtigung zu starten. Dieser Beispielcode wird in der exemplarischen Vorgehensweise [Verwenden von lokalen Benachrichtigungen in xamarin. Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) erläutert.

### <a name="notification-styles"></a>Benachrichtigungs Stile

Zum Erstellen von Benachrichtigungen über *Big Text*-, *Image*-oder *Inbox* -Stil mit `NotificationCompat.Builder` muss Ihre APP die Kompatibilitäts Versionen dieser Stile verwenden. Um z. b. den *großen Textstil* zu verwenden, instanziieren Sie Folgendes `NotificationCompat.BigTextstyle` :

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Ebenso kann Ihre APP und für Eingangsbox `NotificationCompat.InboxStyle` `NotificationCompat.BigPictureStyle` -und *Bild* Stile verwenden. *Inbox*

### <a name="notification-priority-and-category"></a>Benachrichtigungs Priorität und-Kategorie

`NotificationCompat.Builder`unterstützt die- `SetPriority` Methode (verfügbar ab Android 4,1). Die- `SetCategory` Methode wird jedoch *nicht* von unterstützt, `NotificationCompat.Builder` da Kategorien Teil des neuen Benachrichtigungs Metadatensystems sind, das in Android 5,0 eingeführt wurde.

Um ältere Versionen von Android zu unterstützen, wobei `SetCategory` nicht verfügbar ist, kann Ihr Code die API-Ebene zur Laufzeit überprüfen, um bedingt einen aufzurufen, `SetCategory` Wenn die API-Ebene gleich oder größer als Android 5,0 (API-Ebene 21) ist:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

In diesem Beispiel ist das **Ziel Framework** der APP auf Android 5,0 festgelegt, und die **Android-Mindestversion** ist auf **Android 4,1 (API-Ebene 16)** festgelegt. Da `SetCategory` auf API-Ebene 21 und höher verfügbar ist, wird in diesem Beispielcode nur aufgerufen, `SetCategory` Wenn er verfügbar ist &ndash; . er wird nicht aufgerufen, `SetCategory` Wenn die API-Ebene kleiner als 21 ist.

### <a name="lock-screen-visibility"></a>Sichtbarkeit für Sperrbildschirm

Da Android Sperrbildschirm Benachrichtigungen vor Android 5,0 (API-Ebene 21) nicht unterstützte, wird `NotificationCompat.Builder` die-Methode von nicht unterstützt `SetVisibility` . Wie bereits erläutert `SetCategory` , kann Ihr Code die API-Ebene zur Laufzeit überprüfen und nur dann anrufen, `SetVisiblity` Wenn Sie verfügbar ist:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetVisibility (Notification.Public);
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie lokale Benachrichtigungen in Android erstellt werden. Es wurde die Anatomie einer Benachrichtigung beschrieben. es wurde erläutert, wie Sie `NotificationCompat.Builder` zum Erstellen von Benachrichtigungen, zum Formatieren von Benachrichtigungen in großen Symbolen, in *Big Text*-, *Bild* -und *Posteingang* -Formaten, zum Festlegen von Benachrichtigungs metadateneinstellungen wie Priorität, Sichtbarkeit und Kategorie und zum Starten einer Aktivität aus einer Benachrichtigung verwenden können. Außerdem wurde in diesem Artikel beschrieben, wie diese Benachrichtigungseinstellungen mit den neuen Köpfen, dem Sperrbildschirm und den in Android 5,0 eingeführten Features *nicht gestört* werden. Schließlich haben Sie erfahren, wie Sie verwenden, `NotificationCompat.Builder` um die Benachrichtigungs Kompatibilität mit früheren Versionen von Android aufrechtzuerhalten.

Richtlinien zum Entwerfen von Benachrichtigungen für Android finden Sie unter [Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html).

## <a name="related-links"></a>Verwandte Links

- [Notificationslab (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-notificationslab)
- [Localbenachrichtigungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Lokale Benachrichtigungen in Android Exemplarische Vorgehensweise](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Benachrichtigen des Benutzers](https://developer.android.com/training/notify-user/index.html)
- [Benachrichtigung](xref:Android.App.Notification)
- [Notificationmanager](xref:Android.App.NotificationManager)
- [Notificationcompat. Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)
