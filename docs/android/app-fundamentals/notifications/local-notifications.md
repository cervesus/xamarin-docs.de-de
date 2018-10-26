---
title: Lokale Benachrichtigungen
description: In diesem Abschnitt zeigt, wie lokale Benachrichtigungen in Xamarin.Android implementiert wird. Es erläutert die verschiedenen UI-Elemente für ein Android-benachrichtigungs und behandelt die API des mit dem Erstellen und Anzeigen einer Benachrichtigung beteiligt.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: a4ffae0bde39450778b340b4a4c4da8fe90d0bec
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117680"
---
<a name="compatibility"></a>

# <a name="local-notifications"></a>Lokale Benachrichtigungen

_In diesem Abschnitt zeigt, wie lokale Benachrichtigungen in Xamarin.Android implementiert wird. Es erläutert die verschiedenen UI-Elemente für ein Android-benachrichtigungs und behandelt die API des mit dem Erstellen und Anzeigen einer Benachrichtigung beteiligt._

## <a name="local-notifications-overview"></a>Übersicht über die lokale Benachrichtigungen

Android bietet zwei System kontrolliert Bereiche zur Anzeige von Benachrichtigungssymbole und Benachrichtigungsinformationen, die dem Benutzer. Wenn zuerst eine Benachrichtigung veröffentlicht wird, wird das entsprechende Symbol angezeigt, der *Infobereich*, wie im folgenden Screenshot gezeigt:

![Beispiel für Infobereich der Taskleiste auf einem Gerät](local-notifications-images/01-notification-shade.png)

Um Details zu die Benachrichtigung zu erhalten, kann der Benutzer die Benachrichtigung (der einzelnen Benachrichtigungssymbol um Benachrichtigungsinhalt anzuzeigen erweitert wird) öffnen und keine Aktionen, die die Benachrichtigungen zugeordnet. Der folgende Screenshot zeigt ein *Benachrichtigung* , die im Bereich für die oben angezeigte entspricht:

[![Beispiel-Benachrichtigung, die drei Benachrichtigungen anzeigen](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android-Benachrichtigungen verwenden zwei Typen von Layouts:

-   ***Basislayout*** &ndash; ein kompaktes, feste Präsentationsformat.

-   ***Erweiterte Layout*** &ndash; ein Darstellungsformat aus, die auf eine größere Größe um weitere Informationen anzuzeigen, erweitern kann.

Jeder dieser Layouttypen (und zu deren Erstellung) werden in den folgenden Abschnitten erläutert.

> [!NOTE]
> Dieser Leitfaden konzentriert sich auf die [NotificationCompat APIs](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) aus der [Android Support-Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Diese APIs werden maximal Abwärtskompatibilität mit Android 4.0 (API-Ebene 14) sichergestellt.


### <a name="base-layout"></a>Basis-layout

Alle Android-Benachrichtigungen werden auf das Basislayout-Format erstellt, die mindestens die folgenden Elemente enthält:

1.  Ein *Benachrichtigungssymbol*, der die ursprüngliche Anwendung oder der Benachrichtigungstyp darstellt, wenn die app auf verschiedene Arten von Benachrichtigungen unterstützt.

2.  Die Benachrichtigung *Titel*, oder der Name des Absenders, wenn die Benachrichtigung über eine persönliche Nachricht ist.

3.  Die Benachrichtigung.

4.  Ein *Zeitstempel*.

Diese Elemente werden angezeigt, wie im folgenden Diagramm dargestellt:

[![Speicherort von Elementen der Benachrichtigung](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Basis-Layouts sind 64 Dichte unabhängigen Pixeln (dp) in die Höhe auf. Android wird diese Art von einfachen Benachrichtigung standardmäßig erstellt.

Benachrichtigungen können optional ein großes Symbol angezeigt, das die Anwendung oder des Absenders Foto darstellt. Wenn ein großes Symbol in einer Benachrichtigung in Android 5.0 und höher verwendet wird, wird das kleine Benachrichtigungssymbol als Signal über großes Symbol angezeigt:

![Foto von einfachen Benachrichtigung](local-notifications-images/04-simple-notification-photo.png)

Ab Android 5.0, können Benachrichtigungen auch auf dem Sperrbildschirm angezeigt werden:

[![Beispiel für Benachrichtigung bei gesperrtem Bildschirm](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Der Benutzer kann Doppeltippen Sie auf die Benachrichtigung bei gesperrtem Bildschirm zum Entsperren des Geräts, und fahren Sie mit der app, die diese Benachrichtigung stammt oder Wischen, um diese zu schließen. Apps können die Sichtbarkeitsebene für eine Benachrichtigung zu steuern, was auf dem Sperrbildschirm angezeigt wird, und Benutzer können wählen, ob vertraulichen Inhalt Benachrichtigungen bei gesperrtem Bildschirm angezeigt werden können.

Android 5.0 eingeführt, ein wichtiges Präsentation benachrichtigungsformat namens *Heads-Up*. Heads-Up-Benachrichtigungen schieben Sie ein paar Sekunden nach unten aus dem oberen Rand des Bildschirms, und klicken Sie dann auf die Infobereich sichern zurückzukehren:

[![Beispiel-heads-up-Benachrichtigung](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Heads-Up-Benachrichtigungen können für das System UI wichtige Informationen vor dem Benutzer ohne Unterbrechung des Status der derzeit ausgeführten Aktivität einfügen.

Android bietet Unterstützung für Metadaten von abfragebenachrichtigungen auf, sodass Benachrichtigungen sortiert und auf intelligente Weise angezeigt werden können. Metadaten von abfragebenachrichtigungen steuert auch, wie Benachrichtigungen auf dem Sperrbildschirm angezeigt und im Heads-Up-Format dargestellt werden. Anwendungen können die folgenden Arten von Metadaten von abfragebenachrichtigungen festlegen:

-   **Priorität** &ndash; die Prioritätsstufe bestimmt, wie und wann die Benachrichtigungen angezeigt werden. Benachrichtigungen mit hoher Priorität werden als Heads-Up-Benachrichtigungen angezeigt, z. B. In Android 5.0.

-   **Sichtbarkeit** &ndash; gibt an, wie viel Benachrichtigungsinhalt angezeigt werden, wenn die Benachrichtigung auf dem Sperrbildschirm angezeigt wird.

-   **Kategorie** &ndash; informiert das System wie verarbeitet die Benachrichtigung in verschiedenen Situationen, z. B. wenn das Gerät wird *nicht stören* Modus.

**Hinweis:** **Sichtbarkeit** und **Kategorie** wurden eingeführt, in Android 5.0 und in früheren Versionen von Android nicht verfügbar sind. Ab Android 8.0 [benachrichtigungskanäle](#notif-chan) werden verwendet, um zu steuern, wie Benachrichtigungen, die dem Benutzer angezeigt werden.


### <a name="expanded-layouts"></a>Erweiterte layouts

Ab Android 4.1, können mit erweiterten Layout Stilen Benachrichtigungen konfiguriert werden, die dem Benutzer ermöglichen, die die Höhe der Benachrichtigung an weitere Inhalte zu erweitern. Im folgende Beispiel veranschaulicht z. B. eine erweiterte Layout-Benachrichtigung im zusammengezogen-Modus:

![Zusammengezogen Benachrichtigung](local-notifications-images/07-contracted-notification.png)

Wenn diese Benachrichtigung erweitert wird, zeigt es die gesamte Nachricht:

![Erweiterte Benachrichtigungen](local-notifications-images/08-expanded-notification.png)

Android unterstützt drei erweiterten Layout-Stile für Single-ereignisbenachrichtigungen an:

-   ***Großer Text*** &ndash; im Modus "zusammengezogen" zeigt einen Auszug aus der ersten Zeile der Meldung, gefolgt von zwei Punkten. Im erweiterten Modus zeigt die gesamte Nachricht (wie im Beispiel oben dargestellt).

-   ***Posteingang*** &ndash; im Modus "zusammengezogen" zeigt die Anzahl neuer Nachrichten. Im erweiterten Modus zeigt die erste e-Mail-Nachricht oder eine Liste der Nachrichten im Posteingang.

-   ***Image*** &ndash; im Modus "zusammengezogen" nur zeigt den Meldungstext an. Im erweiterten Modus können Sie den Text und ein Bild anzeigt.

[Über die grundlegenden Benachrichtigung](#beyond-the-basic-notification) (weiter unten in diesem Artikel) wird erläutert, wie erstellen *Big Text*, *Posteingang*, und *Image* Benachrichtigungen.

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>Benachrichtigungskanäle

Ab Android 8.0 (Oreo), können Sie die *benachrichtigungskanäle* Funktion, um einen anpassbaren Kanal für jeden Typ der Benachrichtigung zu erstellen, die Sie anzeigen möchten. Benachrichtigungskanäle erleichtern möglich, dass Sie die Gruppe Benachrichtigungen, damit alle Benachrichtigungen zu einem Kanal Anhang gesendet, das gleiche Verhalten. Beispielsweise müssen Sie möglicherweise einen Kanal, der für Benachrichtigungen vorgesehen ist, die sofortige Aufmerksamkeit erfordern, und einen separaten "ruhigerer" Kanal, der für informationsmeldungen verwendet wird.

Die **YouTube** -app, die mit Android Oreo installiert wird, führt zwei Benachrichtigungskategorien: **herunterladen Benachrichtigungen** und **allgemeine Benachrichtigungen**:

[![Benachrichtigungs-Bildschirme für YouTube in Android Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Jeder dieser Kategorien entspricht ein Benachrichtigungskanal. Die YouTube-app implementiert ein **herunterladen Benachrichtigungen** Channel und einen **allgemeine Benachrichtigungen** Kanal. Der Benutzer tippen kann **herunterladen Benachrichtigungen**, woraufhin der Bildschirm "Einstellungen" der app Benachrichtigungen von Channel-Download:

[![Benachrichtigungen-Bildschirm für die YouTube-app herunterladen](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

In diesem Bildschirm der Benutzer kann das Verhalten des Ändern der **herunterladen** Benachrichtigungen channel folgendermaßen aus:

-   Stellen Sie die Wichtigkeit auf **dringend**, **hohe**, **Mittel**, oder **mit niedriger**, die die Ebene der sound und visuelle Unterbrechung konfiguriert.

-   Aktivieren Sie den Punkt für die Benachrichtigung aus, oder deaktivieren.

-   Aktivieren Sie die blinkende LED, oder deaktivieren.

-   Ein- oder Ausblenden von Benachrichtigungen auf dem Sperrbildschirm angezeigt.

-   Überschreiben der **nicht stören** festlegen.

Die **allgemeine Benachrichtigungen** Kanal hat ähnliche Einstellungen:

[![Allgemeine Benachrichtigungen-Bildschirm für die YouTube-app](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Beachten Sie, dass Sie keine absolute Kontrolle über Ihre benachrichtigungskanäle wie mit dem Benutzer interagieren &ndash; der Benutzer kann die Einstellungen für alle Notification Channels auf dem Gerät ändern, wie in den oben stehenden Screenshots zu sehen. Allerdings können Sie Standardwerte konfigurieren, (wie unten beschrieben). Wie diese Beispiele veranschaulichen, erleichtert das neue Kanäle Benachrichtigungsfeature möglich, dass Sie Benutzern eine präzisere Kontrolle über die verschiedenen Arten von Benachrichtigungen gewähren.


## <a name="notification-creation"></a>Benachrichtigung erstellen

Um eine Benachrichtigung in Android zu erstellen, verwenden Sie die [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder) -Klasse aus der [Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet-Paket. Diese Klasse ermöglicht es, erstellen und Veröffentlichen von Benachrichtigungen in älteren Versionen von Android. Weitere Informationen zur Verwendung von `NotificationCompat.Builder`, finden Sie unter [Kompatibilität](#compatibility) weiter unten in diesem Thema.

`NotificationCompat.Builder` bietet Methoden für die verschiedenen Optionen wie z. B. in einer Benachrichtigung festlegen:

-   Der Inhalt, einschließlich Titel, den Nachrichtentext und das Symbol "Benachrichtigung".

-   Das Format der Benachrichtigung gesendet wird, wie z. B. *Big Text*, *Posteingang*, oder *Image* Stil.

-   Die Priorität der Benachrichtigung: Minimum, Niedrig, Standard, hohe oder das maximum. Unter Android 8.0 und höher, wird die Priorität festlegen, über eine [ _Benachrichtigungskanal_](#notification-channels).

-   Die Sichtbarkeit der Benachrichtigung auf dem Sperrbildschirm: öffentliche, Private oder geheimen Schlüssel.

-   Kategorie-Metadaten, mit Android klassifizieren und die Benachrichtigung zu filtern.

-   Ein optionaler Intent, der eine Aktivität zum Starten, wenn die Benachrichtigung angetippt wird angibt.

-   Die ID des benachrichtigungskanals, dass die Benachrichtigung auf (Android 8.0 und höher) veröffentlicht wird.

Nachdem Sie diese Optionen im Generator festgelegt haben, generieren Sie eine Benachrichtigungsobjekt, das die Einstellungen enthält. Um die Benachrichtigung zu veröffentlichen, die Sie übergeben diese Benachrichtigungsobjekt, das *Warnungs-Manager*. Android bietet die [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) -Klasse, die verantwortlich für die Veröffentlichung von Benachrichtigungen, und sie dem Benutzer angezeigt wird. Ein Verweis auf diese Klasse kann aus einem beliebigen Kontext, z. B. eine Aktivität oder einem Dienst abgerufen werden.


### <a name="creating-a-notification-channel"></a>Erstellen eines benachrichtigungskanals

Apps, die auf Android 8.0 ausgeführt werden, müssen einen Benachrichtigungskanal für ihre Benachrichtigungen erstellen. Ein Benachrichtigungskanal erfordert die folgenden drei Arten von Informationen an:

* Ein ID-Zeichenfolge, die für das Paket eindeutig ist, der den Kanal identifiziert.
* Der Name des Kanals, der dem Benutzer angezeigt wird.  Der Name muss zwischen 1 und 40 Zeichen.
* Die Wichtigkeit des Kanals.

Apps müssen die Version von Android zu überprüfen, die sie ausgeführt werden.
Geräte mit älteren Versionen als Android 8.0 sollte kein Benachrichtigungskanal erstellt. Die folgende Methode ist ein Beispiel dafür, wie einen Benachrichtigungskanal in einer Aktivität zu erstellen:

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

Der Benachrichtigungskanal sollte jedes Mal erstellt werden, wenn die Aktivität erstellt wird. Für die `CreateNotificationChannel` -Methode sollte aufgerufen werden der `OnCreate` Methode einer Aktivität.

### <a name="creating-and-publishing-a-notification"></a>Erstellen und Veröffentlichen einer benachrichtigungs

Um eine Benachrichtigung in Android generieren zu können, gehen Sie folgendermaßen vor:

1.  Instanziieren einer `NotificationCompat.Builder` Objekt.

2.  Rufen Sie die verschiedenen Methoden für die `NotificationCompat.Builder` Objekt zum Festlegen von Optionen.

3.  Rufen Sie die [erstellen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) -Methode der der `NotificationCompat.Builder` Objekt um eine Benachrichtigungsobjekt zu instanziieren.

4.  Rufen Sie die [benachrichtigen](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) -Methode der benachrichtigungs-Manager, um die Benachrichtigung zu veröffentlichen.

Sie müssen mindestens die folgende Informationen für jede Benachrichtigung angeben:

-   Ein kleines Symbol (24 x 24 dp Größe)

-   Einen kurzen Titel

-   Der Text der Benachrichtigung

Im folgenden Codebeispiel wird veranschaulicht, wie Sie mit `NotificationCompat.Builder` eine einfache Benachrichtigung generiert. Beachten Sie, dass `NotificationCompat.Builder` Methoden unterstützen [methodenverkettung](http://en.wikipedia.org/wiki/Method_chaining); d. h. gibt jede Methode das Builder-Objekt zurück, damit Sie das Ergebnis der dem letzten Methodenaufruf zum Aufrufen des nächsten Aufrufs der Methode verwenden können:

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

In diesem Beispiel ist ein neues `NotificationCompat.Builder` Objekt mit dem Namen `builder` instanziiert wird, zusammen mit der ID des benachrichtigungskanals verwendet werden. Titel und Text der Benachrichtigung festgelegt sind, und das Benachrichtigungssymbol aus geladen **Resources/drawable/ic_notification.png**. Der Aufruf auf der Benachrichtigung des Entwicklers `Build` Methode erstellt ein Benachrichtigungsobjekt mit diesen Einstellungen. Der nächste Schritt besteht, zum Aufrufen der `Notify` -Methode der benachrichtigungs-Manager. Um den benachrichtigungs-Manager zu suchen, rufen Sie `GetSystemService`, wie oben gezeigt.

Die `Notify` -Methode akzeptiert zwei Parameter: den benachrichtigungsbezeichner und das Benachrichtigungsobjekt. Die benachrichtigungs-ID ist eine eindeutige ganze Zahl, die die Benachrichtigung, um Ihre Anwendung identifiziert. In diesem Beispiel wird die benachrichtigungs-ID auf Null (0); festgelegt. in einer produktionsanwendung möchten jedoch wird jede Benachrichtigung geben Sie einen eindeutigen Bezeichner. Den vorherige Bezeichnerwert in einem Aufruf wiederverwendet `Notify` bewirkt, dass die letzte Benachrichtigung überschrieben werden sollen.

Wenn dieser Code auf einem Android 5.0-Gerät ausgeführt wird, generiert er eine Benachrichtigung, die wie im folgenden Beispiel aussieht:

![Benachrichtigungsergebnis für den Beispielcode](local-notifications-images/09-hello-world.png)

Das Symbol "Benachrichtigungen" wird angezeigt, auf der linken Seite der Benachrichtigung &ndash; mit diesem Image von einem Eingekreiste &ldquo;ich&rdquo; verfügt über einen alpha-Kanal, sodass Android zirkulären grauen Hintergrund dahinter zeichnen kann. Sie können auch ein Symbol ohne einen alpha-Kanal angeben. Um ein Foto als Symbol anzuzeigen, finden Sie unter [große Symbole an](#large-icon-format) weiter unten in diesem Thema.

Der Zeitstempel wird automatisch festgelegt, aber Sie können diese Einstellung überschreiben, durch den Aufruf der [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) Methode im Generator für die Benachrichtigung. Im folgenden Codebeispiel wird z. B. den Zeitstempel auf die aktuelle Uhrzeit festgelegt:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Aktivieren der Sound und vibration

Wenn Sie die Benachrichtigung, die auch einen Sound wiedergeben möchten, können Sie der Benachrichtigung des Entwicklers Aufrufen [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) -Methode und übergeben Sie die `NotificationDefaults.Sound` Flag:

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Dieser Aufruf `SetDefaults` bewirkt, dass das Gerät aus, um einen Sound wiederzugeben, wenn die Benachrichtigung veröffentlicht wird. Wenn Sie möchten das Gerät Vibrieren, anstatt einen Sound wiedergeben, können Sie übergeben `NotificationDefaults.Vibrate` zu `SetDefaults.` Wenn Sie möchten das Gerät einen Sound wiedergeben und Vibrieren des Geräts, können Sie beide Flags übergeben `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Wenn Sie Sound aktivieren, ohne einen Sound wiedergeben, verwendet Android die standardmäßige Benachrichtigung Systemsounds. Sie können jedoch ändern, den Sound, der wiedergegeben wird, durch den Aufruf der Benachrichtigung des Entwicklers [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) Methode. Z. B. den Alarm spielen sound mit die Benachrichtigung an (statt die standardmäßige Benachrichtigung Sound), erhalten Sie den URI für den Alarm sound aus der [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) und übergeben es an `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternativ können Sie den System standardmäßig Klingelton sound für die Benachrichtigung verwenden:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Nach der Erstellung eines Benachrichtigungsobjekts ist es möglich, Benachrichtigungseigenschaften für das Benachrichtigungsobjekt festlegen (statt sie im Voraus über konfigurieren `NotificationCompat.Builder` Methoden). Z. B. statt der `SetDefaults` Methode zum Aktivieren von Vibration auf eine Benachrichtigung, Sie können das Bitflag in der die Benachrichtigung direkt ändern [standardmäßig](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) Eigenschaft:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

In diesem Beispiel bewirkt, dass das Gerät Vibrieren, wenn die Benachrichtigung veröffentlicht wird.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Aktualisieren eine Benachrichtigung

Wenn Sie möchten den Inhalt einer Benachrichtigung zu aktualisieren, nachdem es veröffentlicht wurde, können Sie wiederverwenden, die vorhandene `NotificationCompat.Builder` Objekt, das ein neues Benachrichtigungsobjekt erstellen und veröffentlichen diese Benachrichtigung mit der ID der letzten Benachrichtigung. Zum Beispiel:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

In diesem Beispiel die vorhandenen `NotificationCompat.Builder` Objekt wird verwendet, um ein neues Benachrichtigungsobjekt mit einem anderen Titel und die Nachricht zu erstellen.
Das neue Benachrichtigungsobjekt wird unter Verwendung des Bezeichners der vorherigen Benachrichtigung veröffentlicht und aktualisiert den Inhalt der zuvor veröffentlichte Benachrichtigung:

![Aktualisierte Benachrichtigung](local-notifications-images/12-updated-notification.png)

Der Text der vorherigen Benachrichtigung wird wiederverwendet, &ndash; nur den Titel und der Text, der die Benachrichtigung ändert sich während die Benachrichtigung in die Benachrichtigung angezeigt wird. Der Titeltext ändert sich von "-Beispiel-Benachrichtigung" in "Benachrichtigung aktualisiert" und der Meldungstext ändert sich von "Hello World! Dies ist Meine erste Benachrichtigung!" Klicken Sie auf "Geändert, sodass diese Nachricht."

Eine Benachrichtigung bleibt sichtbar, bis eines von drei Dingen erfolgt:

-   Der Benutzer schließt die Benachrichtigung (oder tippt *alle löschen*).

-   Die Anwendung führt einen Aufruf von `NotificationManager.Cancel`, und übergeben Sie die eindeutige benachrichtigungs-ID, die zugewiesen wurde, wenn die Benachrichtigung veröffentlicht wurde.

-   Ruft die Anwendung `NotificationManager.CancelAll`.

Weitere Informationen zum Aktualisieren von Android-Benachrichtigungen, finden Sie unter [ändern Sie eine Benachrichtigung](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Starten Sie eine Aktivität von einer Benachrichtigung

In Android, ist es üblich, dass eine Benachrichtigung zugeordnet werden ein *Aktion* &ndash; eine Aktivität, die gestartet wird, wenn der Benutzer die Benachrichtigung tippt. Diese Aktivität kann in einer anderen Anwendung oder sogar in einer anderen Aufgabe befinden. Um eine Benachrichtigung eine Aktion hinzuzufügen, erstellen Sie eine [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) Objekt aus, und ordnen Sie die `PendingIntent` mit der Benachrichtigung. Ein `PendingIntent` ist eine besondere Art der Absicht, die von der empfangenden Anwendung einen vordefinierten Teil des Codes mit den Berechtigungen der sendenden Anwendung ermöglicht. Wenn der Benutzer die Benachrichtigung tippt, Android startet die Aktivität, die gemäß der `PendingIntent`.

Der folgende Codeausschnitt veranschaulicht, wie erstellen Sie eine Benachrichtigung mit einem `PendingIntent` startet, die die Aktivität der ursprünglichen app `MainActivity`:

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

Dieser Code ähnelt stark der Benachrichtigungscode im vorherigen Abschnitt, außer dass ein `PendingIntent` an das Benachrichtigungsobjekt hinzugefügt wird. In diesem Beispiel die `PendingIntent` bezieht sich auf die Aktivität der ursprünglichen app vor der Übergabe an des benachrichtigungs-Generators [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) Methode. Die `PendingIntentFlags.OneShot` Flag zum Übergeben der `PendingIntent.GetActivity` Methode, damit die `PendingIntent` nur einmal verwendet. Wenn dieser Code ausgeführt wird, wird die folgende Benachrichtigung angezeigt:

![Benachrichtigung über erste](local-notifications-images/10-first-action-notification.png)

Tippen Sie auf diese Benachrichtigung wird der Benutzer zurück zur ursprünglichen Aktivität.

In einer Produktions-app muss Ihre app behandeln die *BackStack* Wenn der Benutzer drückt die **wieder** innerhalb der Benachrichtigungsaktivität (Wenn Sie nicht mit Android-Aufgaben und den Backstack vertraut sind, finden Sie unter [ Aufgaben und BackStack](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
In den meisten Fällen sollte die Rückwärts navigieren, aus der Benachrichtigungsaktivität den Benutzer der app abmelden und dann wieder auf den Home-Bildschirm zurückgeben. Zum Verwalten von des BackStack, Ihre app verwendet die [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) -Klasse zur Erstellung einer `PendingIntent` mit einen BackStack.

Ein weiterer Aspekt der realen Welt ist, dass die ursprüngliche Aktivität zum Senden von Daten an die Benachrichtigungsaktivität möglicherweise. Z. B. möglicherweise die Benachrichtigung, dass eine SMS-Nachricht angekommen ist, und die Benachrichtigungsaktivität (eine Nachricht Bildschirm anzeigen), erfordert die ID der Nachricht, die die Nachricht an den Benutzer anzuzeigen. Die Aktivität, die erstellt die `PendingIntent` können die [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) Methode, um Daten (z. B. eine Zeichenfolge) zum Intent hinzuzufügen, damit diese Daten an die Benachrichtigungsaktivität übergeben werden.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie mit `TaskStackBuilder` zum Verwalten von den Backstack, und es enthält ein Beispiel zum Senden von einer einzelnen Zeichenfolge an die Benachrichtigungsaktivität eine mit dem Namen `SecondActivity`:

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

In diesem Codebeispiel wird die app besteht aus zwei Aktivitäten: `MainActivity` (die vorstehenden Benachrichtigungscode enthält), und `SecondActivity`, den Bildschirm, der dem Benutzer, die nach dem Tippen auf die Benachrichtigung angezeigt wird. Wenn dieser Code ausgeführt wird, wird eine einfache Benachrichtigung (ähnlich wie im vorherigen Beispiel) angezeigt. Tippen Sie auf die Benachrichtigung gelangen die Benutzer auf die `SecondActivity` Bildschirm:

![Screenshot der zweiten Aktivität](local-notifications-images/11-second-activity.png)

Die Zeichenfolgennachricht (der Absicht des übergebenen `PutExtra` Methode) wird in abgerufen `SecondActivity` über diese Codezeile:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Diese Meldung abgerufenen "Greetings aus MainActivity!" wird angezeigt, der `SecondActivity` Bildschirm, wie im Screenshot oben gezeigt. Wenn der Benutzer drückt die **wieder** Schaltfläche beim `SecondActivity`, Navigation führt der app abmelden und wieder auf dem Bildschirm, der vor dem Start der app.

Weitere Informationen zum Erstellen von ausstehende Intents finden Sie unter [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Über die grundlegenden Benachrichtigung

Benachrichtigungen standardmäßig einen einfachen Basislayout-Format in Android, aber Sie können dieses grundlegende Format verbessern, dazu zusätzliche `NotificationCompat.Builder` Methodenaufrufe. In diesem Abschnitt erfahren Sie, wie die Benachrichtigung an ein großes Foto Symbol hinzugefügt, und sehen Sie Beispiele zum Erstellen von erweiterten Layout-Benachrichtigungen.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Große Symbole an

Android-Benachrichtigungen ein Symbol in der Regel der ursprünglichen app (auf der linken Seite der Benachrichtigung). Benachrichtigungen können jedoch anzeigen, ein Bild oder ein Foto (eine *"große Symbole"*) anstelle des standardmäßigen kleinen Symbols an. Beispielsweise konnte ein Foto des Absender statt auf das Symbol der app eine messaging-app angezeigt werden.

Hier ist ein Beispiel einer einfachen Android 5.0-Benachrichtigung &ndash; nur die kleine app-Symbol angezeigt:

![Beispiel für normale Benachrichtigung](local-notifications-images/13-sample-notification.png)

Und hier ist ein Screenshot, der die Benachrichtigung nach der Änderung ein großes Symbol anzuzeigende &ndash; verwendet ein Symbol aus einem Image von einem Xamarin-Code-Monkey-Objekt erstellt:

![Beispiel "große Symbole" Benachrichtigung](local-notifications-images/14-large-icon-sample.png)

Beachten Sie, dass das Symbol "kleine app" als einen Badge auf der unteren rechten Ecke des großes Symbol angezeigt wird, wenn große Symbole an eine Benachrichtigung angezeigt wird, ein.

Um ein Bild als ein großes Symbol in einer Benachrichtigung verwenden möchten, rufen Sie der Benachrichtigung des Entwicklers [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) -Methode und übergeben Sie eine Bitmap des Bildes. Im Gegensatz zu `SetSmallIcon`, `SetLargeIcon` akzeptiert nur eine Bitmap. Um eine Bilddatei in eine Bitmap zu konvertieren, verwenden Sie die [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) Klasse. Zum Beispiel:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Dieser Beispielcode öffnet die Image-Datei unter **Resources/drawable/monkey_icon.png**, konvertiert es in eine Bitmap und übergibt Sie die resultierende Bitmap zu `NotificationCompat.Builder`. Die Quelle bildauflösung ist in der Regel größer als das kleine Symbol &ndash; jedoch nicht viel größer. Ein Bild, das zu groß ist möglicherweise unnötige größenänderungsvorgängen verursachen, die das Senden der Benachrichtigung verzögert werden kann.


### <a name="big-text-style"></a>Großer Text-Format

Die *Big Text* Stil ist eine erweiterte Layoutvorlage, die Sie zum Anzeigen von lange Meldungen in Benachrichtigungen verwenden. Wie alle erweiterten Layout Benachrichtigungen wird die Benachrichtigung Big Text zuerst in einem kompakten Darstellung-Format angezeigt:

![Beispiel für große SMS-Benachrichtigung](local-notifications-images/15-big-text-notification.png)

In diesem Format wird lediglich ein Auszug der Nachricht angezeigt, durch zwei Punkte beendet. Wenn der Benutzer auf die Benachrichtigung nach unten zieht, erweitert es um die gesamte Nachricht anzuzeigen:

![Erweiterte Big SMS-Benachrichtigung](local-notifications-images/16-big-text-expanded.png)

Diese erweiterten Layoutformat hinaus Zusammenfassungstext am unteren Rand der Benachrichtigung. Die maximale Höhe der *Big Text* Benachrichtigung ist 256 dp.

Zum Erstellen einer *Big Text* Notification, instanziieren Sie eine `NotificationCompat.Builder` Objekt, wie zuvor, und klicken Sie dann zu instanziieren und das Hinzufügen einer [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) -Objekt der `NotificationCompat.Builder` Objekt. Im Folgenden ein Beispiel:

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

In diesem Beispiel dem Meldungstext und der Zusammenfassungstext werden gespeichert der `BigTextStyle` Objekt (`textStyle`) vor der Übergabe an `NotificationCompat.Builder.`


### <a name="image-style"></a>Image-Format

Die *Image* Stil (so genannte der *Gesamtbild* Stil) ist ein erweiterter Notification-Format, die Sie verwenden können, um ein Bild im Text der Benachrichtigung anzuzeigen. Beispielsweise einen Screenshot-app oder eine Foto-app können die *Image* Benachrichtigung Stil, sodass den Benutzer eine Benachrichtigung über die letzte Abbildung, die erfasst wurden. Beachten Sie, dass die maximale Höhe der *Image* Benachrichtigung ist 256 dp &ndash; Android wird das Bild, das in dieser Einschränkung maximale Höhe innerhalb der Grenzen des verfügbaren Arbeitsspeichers passt dessen Größe automatisch.

Wie alle erweiterten Layout Benachrichtigungen *Image* Benachrichtigungen werden zuerst angezeigt, in einem kompakten Format, in dem einen Auszug aus dem zugehörigen Meldungstext angezeigt:

![Compact-Image-Benachrichtigung zeigt kein Bild](local-notifications-images/17-image-compact.png)

Wenn der Benutzer zieht nach unten auf der *Image* Benachrichtigung wird erweitert, um ein Bild anzuzeigen. Hier ist z. B. die erweiterte Version der vorherigen Benachrichtigung:

![Erweiterte Bild messende benachrichtigungsbilds](local-notifications-images/18-image-expanded.png)

Beachten Sie, dass wenn die Benachrichtigung in einem komprimierten Format angezeigt wird, zeigt er die Benachrichtigungstext (der Text, der der Benachrichtigung des Entwicklers an `SetContentText` Methode, wie weiter oben gezeigt). Wenn die Benachrichtigung erweitert wird, um das Bild anzuzeigen, zeigt er jedoch Zusammenfassungstext oberhalb des Bilds an.

Zum Erstellen einer *Image* Notification, instanziieren Sie ein `NotificationCompat.Builder` wie zuvor, und klicken Sie dann zu erstellen und fügen eine [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) -Objekt in der `NotificationCompat.Builder` Objekt. Zum Beispiel:

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

Wie die `SetLargeIcon` -Methode der `NotificationCompat.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) -Methode der `BigPictureStyle` erfordert eine Bitmap des Bildes, das im Text der Benachrichtigung angezeigt werden soll. In diesem Beispiel die [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) -Methode der `BitmapFactory` Lesevorgänge, die die Image-Datei befindet sich am **Resources/drawable/x_bldg.png** und konvertiert ihn in eine Bitmap.

Sie können Bilder, die nicht gepackt werden auch als Ressource anzeigen. Beispielsweise der folgende Code lädt ein Bild aus der lokalen SD-Karte, und zeigt ihn in ein *Image* Benachrichtigung:

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

In diesem Beispiel befindet sich die Image-Datei am **/sdcard/Pictures/my-tshirt.jpg** geladen, auf die Hälfte der ursprünglichen Größe geändert und dann in eine Bitmap für die Verwendung in der Benachrichtigung konvertiert:

![Beispiel für T-shirt-Image in Benachrichtigung](local-notifications-images/19-tshirt-notification.png)

Wenn Sie nicht die Größe der Bilddatei im Voraus wissen, ist es eine gute Idee, die den Aufruf umschließen [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) in einem Ausnahmehandler &ndash; ein `OutOfMemoryError` Ausnahme ausgelöst werden kann, wenn das Bild zu groß für Android zum Ändern der Größe.

Weitere Informationen zum Laden und Decodieren von Bildern für große Bitmap finden Sie unter [Load Large Bitmaps effiziente](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Eingangsbox-Stil

Die *Posteingang* Format ist eine erweiterte Layoutvorlage für die Anzeige von separaten Zeilen Text (z. B. eine e-Mail-Posteingang Zusammenfassung) im Text der Benachrichtigung vorgesehen. Die *Posteingang* Format-Benachrichtigung wird zuerst in einem kompakten Format angezeigt:

![Beispiel für compact Posteingang-Benachrichtigung](local-notifications-images/20-inbox-compact.png)

Wenn der Benutzer auf die Benachrichtigung nach unten zieht, wird es erweitert und anzeigen eine Zusammenfassung per e-Mail senden, wie im folgenden Screenshot zu sehen:

![Beispiel-Posteingang-Benachrichtigung erweitert](local-notifications-images/21-inbox-expanded.png)

Zum Erstellen einer *Posteingang* Notification, instanziieren Sie ein `NotificationCompat.Builder` Objekt, wie zuvor, und fügen eine [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) -Objekt die `NotificationCompat.Builder`. Im Folgenden ein Beispiel:

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

Um neue Textzeilen in den Benachrichtigungstext hinzuzufügen, rufen die [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) Methode der `InboxStyle` Objekt (die maximale Höhe des der *Posteingang* Benachrichtigung ist 256 dp). Beachten Sie, dass im Gegensatz zu *Big Text* Stil, der *Posteingang* Stil unterstützt eine einzelne Textzeilen in den Textkörper der Benachrichtigung.

Sie können auch die *Posteingang* Stil für Benachrichtigungen, die einzelne Textzeilen in einem erweiterten Format anzeigen. Z. B. die *Posteingang* Notification-Stil kann verwendet werden, um mehrere ausstehende Benachrichtigungen in einer zusammenfassungsbenachrichtigung kombinieren &ndash; können Sie eine einzelne aktualisieren *Posteingang* Benachrichtigung über neu formatieren Zeilen mit Benachrichtigungsinhalt (finden Sie unter [aktualisieren eine Benachrichtigung](#updating-a-notification) oben), sondern als einen kontinuierlichen Strom von neuen, größtenteils ähnlich wie Benachrichtigungen zu generieren.


## <a name="configuring-metadata"></a>Konfigurieren von Metadaten

`NotificationCompat.Builder` enthält Methoden, die Sie aufrufen können, um Metadaten über die Benachrichtigung ein, z. B. Sichtbarkeit, Priorität und Kategorie festzulegen. Android verwendet diese Information &mdash; zusammen mit benutzereinstellungseinstellungen &mdash; um zu bestimmen, wie und wann Benachrichtigungen angezeigt.


### <a name="priority-settings"></a>Prioritätseinstellungen

Apps, die auf Android 7.1 und niedrigere müssen zum Festlegen der Priorität direkt auf die Benachrichtigung selbst. Die vorrangige Einstellung einer Benachrichtigung bestimmt zwei Ergebnisse, wenn die Benachrichtigung veröffentlicht wird:

-   Die Benachrichtigung angezeigt, in denen in Bezug auf andere Benachrichtigungen.
    Beispielsweise werden Benachrichtigungen mit hoher Priorität über niedrigere Priorität Benachrichtigungen in der Detailansicht für Benachrichtigungen, unabhängig davon angezeigt, wenn jede Benachrichtigung veröffentlicht wurde.

-   Gibt an, ob die Benachrichtigung in der Heads-up-benachrichtigungsformat (Android 5.0 und höher) angezeigt wird. Nur *hohe* und *maximale* Priorität Benachrichtigungen werden immer als Heads-Up-Benachrichtigungen angezeigt.

Xamarin.Android definiert die folgenden Enumerationen zum Festlegen der Benachrichtigungspriorität:

-   `NotificationPriority.Max` &ndash; Benachrichtigt den Benutzer auf eine dringende oder der kritische Bedingung (z. B. einen eingehenden Anruf, Turn-von-Turn-Anweisungen oder ein Notfall Warnung). Auf Android 5.0 und höher werden die maximale Priorität Benachrichtigungen Heads-Up-Format angezeigt.

-   `NotificationPriority.High` &ndash; Informiert den Benutzer von wichtigen Ereignissen (z. B. wichtige e-Mails oder den Empfang von Echtzeit-Chat-Nachrichten). Auf Android 5.0 und höher werden Benachrichtigungen mit hoher Priorität im Heads-Up-Format angezeigt.

-   `NotificationPriority.Default` &ndash; Benachrichtigt den Benutzer, der Bedingungen, die eine mittlere Ebene von Bedeutung.

-   `NotificationPriority.Low` &ndash; Für nicht dringende Informationen, die der Benutzer (z. B. Software Update-Erinnerungen oder soziales Netzwerk Updates) informiert werden muss.

-   `NotificationPriority.Min` &ndash; Weitere Hintergrundinformationen, dass der Benutzer nur, wenn Benachrichtigungen Anzeigen von Benachrichtigungen (z. B. Speicherort oder Wetter-Informationen).

Rufen Sie zum Festlegen der Priorität einer Benachrichtigung die [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) Methode der `NotificationCompat.Builder` Objekts und übergibt dabei die Prioritätsstufe. Zum Beispiel:

```csharp
builder.SetPriority (NotificationPriority.High);
```

Im folgenden Beispiel, die Benachrichtigung von hoher Priorität, "Eine wichtige Meldung!" wird am oberen Rand die Benachrichtigung wird angezeigt:

![Beispiel für wichtige Benachrichtigungen](local-notifications-images/22-hi-priority-drawer.png)

Da dies eine wichtige Benachrichtigung ist, wird er auch als eine Heads-Up-Benachrichtigung über den Bildschirm des Benutzers aktuelle Aktivität in Android 5.0 angezeigt:

![Beispiel-Heads-Up-Benachrichtigung](local-notifications-images/23-heads-up-example.png)

Im nächsten Beispiel wird die mit niedriger Priorität "Für den Tag dachte" Benachrichtigung unter einer Ebene höherer Priorität Akku-Benachrichtigung angezeigt:

![Beispiel mit niedriger Priorität Benachrichtigung](local-notifications-images/24-lo-priority-drawer.png)

Da die Benachrichtigung "Überlegungen für den Tag" eine Benachrichtigung mit niedriger Priorität ist, wird Sie von Android nicht im Heads-up-Format angezeigt.

> [!NOTE]
> Unter Android 8.0 und höher, bestimmt die Priorität des Kanals und der Benutzer die Benachrichtigungseinstellungen die Priorität der Benachrichtigung.

### <a name="visibility-settings"></a>Einstellungen zur Sichtbarkeit

Ab Android 5.0: der *Sichtbarkeit* Einstellung kann steuern, wie viel Inhalt der Benachrichtigung auf dem sicheren Sperrbildschirm angezeigt wird.
Xamarin.Android definiert die folgenden Enumerationen für das Einstellen der Sichtbarkeit von Benachrichtigungen:

-   `NotificationVisibility.Public` &ndash; Der vollständige Inhalt der Benachrichtigung wird auf dem sicheren Sperrbildschirm angezeigt.

-   `NotificationVisibility.Private` &ndash; Nur wichtige Informationen auf dem sicheren Sperrbildschirm (z. B. das Benachrichtigungssymbol und den Namen der app, die es bereitgestellt) angezeigt wird, aber die restlichen die benachrichtigungs Details werden ausgeblendet. Alle Benachrichtigungen standardmäßig `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Auf dem sicheren Sperrbildschirm, auch nicht auf das Benachrichtigungssymbol wird nichts angezeigt. Inhalt der Benachrichtigung ist verfügbar, nur, nachdem der Benutzer das Gerät entsperrt wird.

Festlegen die Sichtbarkeit einer Benachrichtigung in der apps-Aufruf die `SetVisibility` Methode der `NotificationCompat.Builder` Objekts und übergibt dabei die sichtbarkeitseinstellung. Angenommen, dieser Aufruf `SetVisibility` stellt die Benachrichtigung `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Wenn eine `Private` Benachrichtigung gesendet wird, wird nur der Name und das Symbol der app auf dem sicheren Sperrbildschirm angezeigt. Anstatt die benachrichtigungsmeldung erhält der Benutzer "Entsperren Ihres Geräts zu dieser Benachrichtigung finden Sie unter":

![Entsperren Sie Ihr Gerät-Benachrichtigung](local-notifications-images/25-lockscreen-private.png)

In diesem Beispiel **NotificationsLab** ist der Name der ursprünglichen app. Diese bearbeitete Version der Benachrichtigung angezeigt wird, nur wenn der Sperrbildschirm sicher ist (d. h. per PIN, Muster und das Kennwort geschützt) &ndash; der Sperrbildschirm nicht sicher ist, der vollständige Inhalt der Benachrichtigung zur Verfügung, auf dem Sperrbildschirm angezeigt wird.


### <a name="category-settings"></a>Kategorieeinstellungen

Ab Android 5.0 sind vordefinierte Kategorien für das Zuweisen von Rängen und Filtern von Benachrichtigungen zur Verfügung. Xamarin.Android bietet die folgenden Enumerationen für diese Kategorien:

-   `Notification.CategoryCall` &ndash; Eingehenden Telefonanruf.

-   `Notification.CategoryMessage` &ndash; Eingehende Textnachricht.

-   `Notification.CategoryAlarm` &ndash; Ein Alarm Bedingung oder einen Timer Ablauf.

-   `Notification.CategoryEmail` &ndash; Eingehende e-Mail-Nachricht.

-   `Notification.CategoryEvent` &ndash; Ein Kalenderereignis.

-   `Notification.CategoryPromo` &ndash; Eine Werbe-Nachricht oder die Ankündigung.

-   `Notification.CategoryProgress` &ndash; Der Status eines Hintergrundvorgangs.

-   `Notification.CategorySocial` &ndash; Update für soziale Netzwerke.

-   `Notification.CategoryError` &ndash; Fehler beim Vorgang oder die Authentifizierung eines Hintergrundprozesses.

-   `Notification.CategoryTransport` &ndash; Media Playback-Update.

-   `Notification.CategorySystem` &ndash; Reserviert für das System (System oder Gerät Status).

-   `Notification.CategoryService` &ndash; Gibt an, dass ein Hintergrunddienst ausgeführt wird.

-   `Notification.CategoryRecommendation` &ndash; Eine Empfehlung-Meldung, die im Zusammenhang mit der aktuell ausgeführten app.

-   `Notification.CategoryStatus` &ndash; Informationen zum Gerät.

Die benachrichtigungs Priorität hat Vorrang vor allen die kategorieeinstellung für die, wenn Benachrichtigungen sortiert sind, ist. Eine wichtige Benachrichtigung werden z. B. als Heads-Up angezeigt werden, auch wenn er gehört die `Promo` Kategorie. Wenn die Kategorie einer Benachrichtigung festlegen möchten, rufen Sie die `SetCategory` Methode der `NotificationCompat.Builder` Objekts und übergibt dabei die kategorieeinstellung. Zum Beispiel:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

Die *nicht stören* (neue Funktion in Android 5.0) filtert Benachrichtigungen basierend auf Kategorien. Z. B. die *nicht stören* -Bildschirms in **Einstellungen** ermöglicht dem Benutzer ausgenommen Benachrichtigungen für Anrufe und Nachrichten:

![Bildschirm-Switches nicht stören](local-notifications-images/26-do-not-disturb.png)

Wenn der Benutzer konfiguriert *nicht stören* um alle Interrupts mit Ausnahme von Telefonanrufen (wie im Screenshot oben dargestellt) zu blockieren, kann Android-Benachrichtigungen mit einem-kategorieneinstellung des `Notification.CategoryCall` währen das Gerät angezeigt werden befindet sich im *nicht stören* Modus. Beachten Sie, dass `Notification.CategoryAlarm` Benachrichtigungen werden nie blockiert *nicht stören* Modus.

Die [LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) Beispiel veranschaulicht, wie `NotificationCompat.Builder` um eine zweite Aktivität von einer Benachrichtigung zu starten. Dieser Beispielcode wird erläutert, der [mithilfe von lokalen Benachrichtigungen in Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) Exemplarische Vorgehensweise.


### <a name="notification-styles"></a>Benachrichtigungs-Stile

Erstellung *Big Text*, *Image*, oder *Posteingang* Formatieren von Benachrichtigungen mit `NotificationCompat.Builder`, Ihre app muss die Kompatibilität Versionen dieser Stile verwenden. Um beispielsweise verwenden die *Big Text* formatieren, instanziieren `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Auf ähnliche Weise können Ihre app `NotificationCompat.InboxStyle` und `NotificationCompat.BigPictureStyle` für *Posteingang* und *Image* Stile, bzw.


### <a name="notification-priority-and-category"></a>Benachrichtigungspriorität und Kategorie

`NotificationCompat.Builder` unterstützt die `SetPriority` Methode (verfügbar ab Android 4.1). Allerdings die `SetCategory` Methode ist *nicht* von unterstützten `NotificationCompat.Builder` da Kategorien die neuen Metadaten-Benachrichtigungssystem gehören, die in Android 5.0 eingeführt wurde.

Zur Unterstützung von älterer Versionen von Android, wo `SetCategory` ist nicht verfügbar ist, Ihren Code sehen die API-Ebene zur Laufzeit bedingt Aufrufen `SetCategory` bei API-Ebene ist, gleich oder größer als Android 5.0 (API Level 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

In diesem Beispiel ist die app die **Zielframework** auf Android 5.0 festgelegt ist und die **Android-Mindestversion** nastaven NA hodnotu **Android 4.1 (API Level 16)**. Da `SetCategory` ist in der API-Ebene 21 und höher verfügbar – dieser Beispielcode ruft `SetCategory` nur, wenn es verfügbar ist &ndash; nicht aufrufen `SetCategory` bei API-Ebene ist weniger als 21.


### <a name="lock-screen-visibility"></a>Sichtbarkeit der Sperre-Bildschirm

Da Android keine Benachrichtigungen bei gesperrtem Bildschirm vor Android 5.0 (API Level 21), unterstützen `NotificationCompat.Builder` unterstützt nicht die `SetVisibility` Methode. Wie bereits dargelegt für `SetCategory`, Ihren Code sehen die API-Ebene auf die Common Language Runtime, und rufen `SetVisiblity` nur, wenn es verfügbar ist:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie lokale Benachrichtigungen unter Android zu erstellen. Es wurde beschrieben, den Aufbau einer Benachrichtigung gesendet wird, es wurde erklärt, wie mit `NotificationCompat.Builder` zum Erstellen von Benachrichtigungen, wie Style-Benachrichtigungen in große Symbole *Big Text*, *Image* und *Posteingang*  Formate, Benachrichtigung über Metadateneinstellungen wie z. B. Sichtbarkeit, Priorität und Kategorie festlegen und wie Sie eine Aktivität aus einer Benachrichtigung zu starten. Außerdem wird in diesem Artikel beschrieben, wie diese Benachrichtigungseinstellungen mit der neuen Heads-Up, Sperrbildschirm, funktionieren und *nicht stören* Features, die in Android 5.0 eingeführt wurde. Schließlich haben Sie gelernt, verwenden Sie `NotificationCompat.Builder` Notification-Kompatibilität mit früheren Versionen von Android zu verwalten.

Richtlinien zum Entwerfen von Benachrichtigungen für Android, finden Sie unter [Benachrichtigungen](http://developer.android.com/guide/topics/ui/notifiers/notifications.html).


## <a name="related-links"></a>Verwandte Links

- [NotificationsLab (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Lokale Benachrichtigungen In Android exemplarischen Vorgehensweise](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Benachrichtigung des Benutzers](http://developer.android.com/training/notify-user/index.html)
- [Benachrichtigung](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
