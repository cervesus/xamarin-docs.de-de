---
title: Lokale Benachrichtigungen
description: "In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in Xamarin.Android implementiert wird. Es wird erläutert, die verschiedenen Benutzeroberflächenelemente der Android-Benachrichtigung und behandelt die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7566ebac0f487ef321c512c988c79f34e50777ac
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="local-notifications"></a>Lokale Benachrichtigungen

_In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in Xamarin.Android implementiert wird. Es wird erläutert, die verschiedenen Benutzeroberflächenelemente der Android-Benachrichtigung und behandelt die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt._

## <a name="local-notifications-overview"></a>Übersicht über die lokale Benachrichtigungen

In diesem Thema wird erläutert, wie lokale Benachrichtigungen in einer Xamarin.Android-Anwendung implementiert wird. Es wird erläutert, die verschiedenen Teile des Android-Benachrichtigung, es wird erläutert, die anderen Benachrichtigung-Formate, die Anwendungsentwicklern zur Verfügung stehen und es eine Einführung in die APIs, die zum Erstellen und Veröffentlichen von Benachrichtigungen verwendet werden.

Android bietet zwei System gesteuert Bereiche für Benachrichtigungssymbole und die Informationen zu dem Benutzer angezeigt. Wenn zunächst eine Benachrichtigung veröffentlicht wird, wird das Symbol angezeigt, der *Infobereich*, wie im folgenden Screenshot gezeigt:

![Beispiel-Infobereich auf einem Gerät](local-notifications-images/01-notification-shade.png)

Um Details zu der Benachrichtigung zu erhalten, kann der Benutzer (der jeder Benachrichtigungssymbol zum Offenlegen von Benachrichtigungsinhalt erweitert wird) öffnen Sie die Benachrichtigung öffnen, und führen Sie alle Aktionen, die Benachrichtigungen zugeordnet. Die folgende Abbildung zeigt eine *öffnen Benachrichtigung* , entspricht der Infobereich oben angezeigt:

[![Öffnen der Beispiel-Benachrichtigung drei Benachrichtigungen anzeigen](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png)

Android-Benachrichtigungen werden zwei Typen von Layouts verwenden:

-   ***Grundlegende Layout*** &ndash; ein kompaktes, feste Präsentationsformat.

-   ***Erweiterte Layout*** &ndash; ein Darstellungsformat aus, die zum Offenlegen von Informationen zu einem größeren erweitert werden kann.

Jeder dieser Typen Layout (und deren Erstellung) werden in den folgenden Abschnitten erläutert.

<a name="base-layout" />

### <a name="base-layout"></a>Basis-Layout

Alle Android-Benachrichtigungen werden basierend auf der Basis Layoutformat erstellt, die mindestens die folgenden Elemente enthält:

1.  Ein *Benachrichtigungssymbol*, der die ursprünglichen app oder der Benachrichtigungstyp darstellt, wenn die app auf verschiedenen Arten von Benachrichtigungen unterstützt.

2.  Die Benachrichtigung *Titel*, oder den Namen des Absenders, wenn die Benachrichtigung über eine persönliche Nachricht ist.

3.  Die Benachrichtigung.

4.  Ein *Zeitstempel*.

Diese Elemente werden angezeigt, wie im folgenden Diagramm dargestellt:

[![Speicherort der Benachrichtigung Elemente](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png)

Basis Layouts sind auf 64 Dichte typunabhängig Pixel (dp) hoch beschränkt. Android dieser Stil grundlegende Benachrichtigung wird standardmäßig erstellt.

Benachrichtigungen können optional ein großes Symbol angezeigt, das die Anwendung oder ein Foto des Absenders darstellt. Wenn ein großes Symbol in einer Benachrichtigung in Android 5.0 und höher verwendet wird, wird das kleine Benachrichtigungssymbol als ein Signal über die "große Symbole" angezeigt:

![Einfache Benachrichtigung Foto](local-notifications-images/04-simple-notification-photo.png)

Beginnen mit dem Android 5.0, können Benachrichtigungen auch auf die für den sperrbildschirmhintergrund angezeigt:

[![Beispiel für den sperrbildschirmhintergrund-Benachrichtigung](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png)

Der Benutzer kann Doppeltippen Sie auf die Benachrichtigung für den sperrbildschirmhintergrund entsperren Sie das Gerät, und springen Sie zu der app, die diese Benachrichtigung stammt oder Streifen verwerfen die Benachrichtigung. Apps können das Maß an Sichtbarkeit eine Benachrichtigung zu steuern, was die für den sperrbildschirmhintergrund angezeigt werden, und Benutzer können auswählen, ob vertraulichem Inhalt für den sperrbildschirmhintergrund Benachrichtigungen angezeigt werden können.

Android 5.0.x eingeführt, eine Benachrichtigung mit hoher Priorität Präsentationsformat aufgerufen *Heads-Up*. Heads-Up-Benachrichtigungen vom oberen Rand des Bildschirms für einige Sekunden nach unten schieben, und klicken Sie dann auf den Benachrichtigungsbereich sichern zurückzukehren:

[![Beispiel-heads-up-Benachrichtigung](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png)

Heads-Up-Benachrichtigungen ermöglichen es dem System UI, wichtige Informationen vor der Benutzer ohne Unterbrechung des Status der derzeit ausgeführten Aktivität eingefügt werden soll.

Android bietet Unterstützung für Benachrichtigung Metadaten, sodass Benachrichtigungen sortiert und auf intelligente Weise angezeigt werden können. Metadaten von abfragebenachrichtigungen wird auch gesteuert, wie Benachrichtigungen auf der für den sperrbildschirmhintergrund und Heads-Up-Format dargestellt werden. Anwendungen können die folgenden Arten von Metadaten von abfragebenachrichtigungen festlegen:

-   **Priorität** &ndash; die Prioritätsstufe bestimmt, wie und wann Benachrichtigungen angezeigt werden. Beispielsweise wird im Android 5.0 werden Benachrichtigungen mit hoher Priorität als Heads-Up-Benachrichtigungen angezeigt.

-   **Sichtbarkeit** &ndash; gibt an, wie viel Benachrichtigungsinhalt angezeigt werden, wenn die Benachrichtigung auf der für den sperrbildschirmhintergrund angezeigt wird.

-   **Kategorie** &ndash; informiert das System wie die Benachrichtigung in verschiedenen Situationen, z. B. wenn das Gerät wird behandelt *nicht stören* Modus.

**Hinweis:** **Sichtbarkeit** und **Kategorie** wurden eingeführt Android 5.0 und in früheren Versionen von Android nicht verfügbar sind. Beginnend mit Android 8.0 [benachrichtigungskanäle](#notif-chan) werden verwendet, um zu steuern, wie dem Benutzer Benachrichtigungen angezeigt werden.

<a name="expanded-layouts" />

### <a name="expanded-layouts"></a>Erweiterte Layouts

Beginnend mit Android 4.1, können mit erweiterten Layout Stilen Benachrichtigungen konfiguriert werden, mit die den Benutzer die Höhe der Benachrichtigung zum Anzeigen von mehr Inhalt erweitern können. Das folgende Beispiel zeigt z. B. eine erweiterte Layout Benachrichtigung im vertraglich-Modus:

![Vertraglich Benachrichtigung](local-notifications-images/07-contracted-notification.png)

Wenn diese Benachrichtigung erweitert wird, zeigt es die gesamte Nachricht:

![Erweiterte Benachrichtigung](local-notifications-images/08-expanded-notification.png)

Android unterstützt drei erweiterten Layout Stile für einzelnes Ereignis Benachrichtigungen:

-   ***Große Text*** &ndash; In vertraglich Modus zeigt einen Auszug aus der ersten Zeile der Nachricht ein, gefolgt von zwei Punkte. Im erweiterten Modus zeigt die gesamte Nachricht (wie im Beispiel oben dargestellt).

-   ***Eingangsbox*** &ndash; In vertraglich Modus zeigt die Anzahl neuer Nachrichten. Im erweiterten Modus zeigt die erste e-Mail-Nachricht oder eine Liste der Nachrichten im Posteingang.

-   ***Bild*** &ndash; im vertraglich Modus nur zeigt den Meldungstext an. Im erweiterten Modus können Sie den Text und ein Bild anzeigt.

[Über die grundlegenden Benachrichtigung](#beyond-the-basic-notification) (weiter unten in diesem Artikel) erläutert, wie *Big Text*, *Posteingang*, und *Image* Benachrichtigungen.

<a name="notification-creation" />

## <a name="notification-creation"></a>Benachrichtigung erstellen

Um eine Benachrichtigung in Android zu erstellen, verwenden Sie die [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) Klasse. `Notification.Builder` Android 3.0 zum Vereinfachen der Erstellung der Benachrichtigung Objekte eingeführt wurde. Um Benachrichtigungen zu erstellen, die mit älteren Versionen von Android kompatibel sind, können Sie die [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) -Klasse statt `Notification.Builder` (finden Sie unter [Kompatibilität](#compatibility) weiter unten in diesem Thema Weitere Informationen zur Verwendung von `NotificationCompat.Builder`).
`Notification.Builder` bietet Methoden für die verschiedenen Optionen wie z. B. in einer Benachrichtigung festlegen:

-   Der Inhalt, einschließlich Titel, den Nachrichtentext und das Symbol "Benachrichtigung".

-   Das Format der Benachrichtigung, z. B. *Big Text*, *Posteingang*, oder *Image* Stil.

-   Die Priorität der Benachrichtigung: Minimum, Niedrig, in der Standardeinstellung hoch oder maximale.

-   Die Sichtbarkeit der Benachrichtigung auf der für den sperrbildschirmhintergrund: öffentliche, Private oder geheimen Schlüssel.

-   Kategorie-Metadaten, die der Identifikation von Android klassifizieren und Filtern die Benachrichtigung.

-   Optionale Priorität, der eine Aktivität zum Starten, wenn die Benachrichtigung abgezweigten angibt.

Nach dem Festlegen dieser Optionen im Generator generieren Sie eine Benachrichtigungsobjekt, das die Einstellungen enthält. Um die Benachrichtigung zu veröffentlichen, übergeben Sie diese Benachrichtigungsobjekt, das *Warnungs-Manager*. Android bietet die [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) -Klasse, die für die Veröffentlichung von Benachrichtigungen und sie dem Benutzer angezeigt wird. Ein Verweis auf diese Klasse kann aus einem beliebigen Kontext, z. B. eine Aktivität oder einem Dienst abgerufen werden.

<a name="how-to-generate" />

### <a name="how-to-generate-a-notification"></a>Gewusst wie: Generieren Sie eine Benachrichtigung

Gehen Sie folgendermaßen vor, um eine Benachrichtigung in Android zu generieren:

1.  Instanziieren einer `Notification.Builder` Objekt.

2.  Rufen Sie die verschiedenen Methoden auf die `Notification.Builder` Objekt Benachrichtigungsoptionen festlegen.

3.  Rufen Sie die [erstellen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) Methode der `Notification.Builder` Objekt zum Instanziieren eines Benachrichtigungsobjekts.

4.  Rufen Sie die [benachrichtigen](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) Methode der vom Warnungs-Manager, um die Benachrichtigung zu veröffentlichen.

Sie müssen mindestens die folgende Informationen für jede Benachrichtigung angeben:

-   Ein kleines Symbol (24 x 24 dp Größe)

-   Einen kurzen Titel

-   Der Text der Benachrichtigung

Das folgende Codebeispiel zeigt, wie `Notification.Builder` um eine grundlegende Benachrichtigung zu generieren. Beachten Sie, dass `Notification.Builder` Methoden unterstützen [methodenverkettung](http://en.wikipedia.org/wiki/Method_chaining); d. h. jede Methode das Builder-Objekt zurückgibt, damit Sie das Ergebnis des letzten Methodenaufrufs verwenden können, der nächsten Aufruf der-Methode aufgerufen:

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
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

In diesem Beispiel wird ein neues `Notification.Builder` bezeichnetes Objekt `builder` wird instanziiert, Titel und Text, der die Benachrichtigung festgelegt sind, und das Symbol "Benachrichtigung" geladen wird **Resources/drawable/ic_notification.png**. Der Aufruf von des Benachrichtigung Generators `Build` Methode ein Benachrichtigungsobjekts mit diesen Einstellungen erstellt. Der nächste Schritt ist das Aufrufen der `Notify` Methode des benachrichtigungs-Managers. Um den Warnungs-Manager zu suchen, rufen Sie `GetSystemService`, wie oben gezeigt.

Die `Notify` Methode akzeptiert zwei Parameter: die benachrichtigungs-ID und das Benachrichtigungsobjekt. Der Bezeichner für die Benachrichtigung ist eine eindeutige ganze Zahl, die die Benachrichtigung, um die Anwendung identifiziert. In diesem Beispiel wird der Bezeichner für die Benachrichtigung festgelegt, auf Null (0); Allerdings möchten Sie in einer produktionsanwendung werden jede Benachrichtigung geben Sie einen eindeutigen Bezeichner. Wiederverwenden der vorherige Bezeichnerwert in einem Aufruf von `Notify` bewirkt, dass die letzte Benachrichtigung überschrieben werden.

Wenn dieser Code auf einem Android 5.0-Gerät ausgeführt wird, generiert er eine Benachrichtigung, die im folgenden Beispiel ähnelt:

![Benachrichtigungsergebnis für den Beispielcode](local-notifications-images/09-hello-world.png)

Das Symbol "Benachrichtigung" wird angezeigt, auf der linken Seite der Benachrichtigung &ndash; dieses Bild von einer Eingekreister &ldquo;ich&rdquo; verfügt über einen alpha-Kanal, sodass Android zirkulären grauen Hintergrund dahinter zeichnen kann. Sie können auch ein Symbol ohne einen alpha-Kanal angeben. Um ein Foto als Symbol anzuzeigen, finden Sie unter [Symbol großformatiges](#large-icon-format) weiter unten in diesem Thema.

Der Zeitstempel wird automatisch festgelegt, aber Sie können diese Einstellung überschreiben, durch Aufrufen der [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) -Methode des Generators für Benachrichtigung. Beispielsweise legt im folgenden Codebeispiel wird den Zeitstempel auf die aktuelle Zeit:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```
<a name="sound-and-vibr" />

### <a name="enabling-sound-and-vibration"></a>Aktivieren der Sound und Vibrationen

Wenn Sie Ihre Benachrichtigung auch einen Sound wiedergeben möchten, können, rufen Sie die Benachrichtigung-Generator [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) -Methode und übergeben Sie die `NotificationDefaults.Sound` Kennzeichen:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Dieser Aufruf `SetDefaults` führt dazu, dass das Gerät einen Sound wiedergegeben, wenn die Benachrichtigung veröffentlicht wird. Wenn Sie möchten das Gerät Vibrieren, anstatt einen Sound wiedergeben, können Sie übergeben `NotificationDefaults.Vibrate` auf `SetDefaults.` , wenn Sie möchten das Gerät einen Sound wiedergeben und Vibrieren des Geräts, können Sie beide Flags übergeben `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Wenn Sie Sound aktivieren, ohne einen Sound wiedergeben, verwendet Android die Standard-Systemsounds Benachrichtigung. Sie können jedoch ändern, den Sound, der wiedergegeben wird, durch Aufrufen des Benachrichtigung Generators [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) Methode. Beispielsweise spielt den Alarm sound mit der Benachrichtigung (statt der Benachrichtigung Standardsound), erhalten Sie den URI für den Alarm sound aus der [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) und übergeben sie an `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternativ können Sie das System standardmäßig Klingelton sound für Ihre Benachrichtigung verwenden:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Nach der Erstellung eines Benachrichtigungsobjekts ist es möglich, Benachrichtigungseigenschaften für das Benachrichtigungsobjekt festzulegen (statt sie im Voraus über konfigurieren `Notification.Builder` Methoden). Z. B. statt der `SetDefaults` Methode zum Aktivieren von Vibrationen auf eine Benachrichtigung, können Sie das Bitflag, der der benachrichtigungs direkt ändern [standardmäßig](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) Eigenschaft:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

In diesem Beispiel bewirkt, dass das Gerät Vibrieren, wenn die Benachrichtigung veröffentlicht wird.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Aktualisieren eine Benachrichtigung

Wenn Sie möchten den Inhalt einer Benachrichtigung aktualisieren, nachdem er veröffentlicht wurde, können Sie wiederverwenden, die vorhandene `Notification.Builder` Objekt zum Erstellen eines neuen Benachrichtigungsobjekts und veröffentlichen diese Benachrichtigung mit dem Bezeichner der letzten Benachrichtigung. Zum Beispiel:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

In diesem Beispiel wird die vorhandene `Notification.Builder` Objekt wird verwendet, um ein neues Benachrichtigungsobjekt mit einem anderen Titel und die Nachricht zu erstellen.
Das neue Benachrichtigungsobjekt wird unter Verwendung des Bezeichners der vorherige Benachrichtigung veröffentlicht und aktualisiert den Inhalt der Benachrichtigung zuvor veröffentlichten:

![Aktualisierte Benachrichtigung](local-notifications-images/12-updated-notification.png)

Der Text der vorherigen Benachrichtigung wiederverwendet &ndash; nur den Titel und den Text der Benachrichtigung ändert, während die Benachrichtigung im Öffnen Sie Benachrichtigung angezeigt wird. Der Titeltext ändert sich von "Beispiel-Benachrichtigung" auf "Benachrichtigung aktualisiert" und der Meldungstext ändert sich von "Hello World! Dies ist mein erster Benachrichtigung!" um "Auf diese Meldung geändert."

Eine Benachrichtigung weiterhin angezeigt, bis eines der drei Dinge Ereignisse auftritt:

-   Der Benutzer schließt die Benachrichtigung (oder tippt *alle löschen*).

-   Die Anwendung führt einen Aufruf von `NotificationManager.Cancel`, und übergeben Sie die eindeutige Benachrichtigungsdienst-ID, der zugewiesen wurde, wenn die Benachrichtigung veröffentlicht wurde.

-   Ruft die Anwendung `NotificationManager.CancelAll`.

Weitere Informationen zum Aktualisieren von Android-Benachrichtigungen finden Sie unter [ändern Sie eine Benachrichtigung](http://developer.android.com/training/notify-user/managing.html#Updating).

<a name="starting-an-activity" />

### <a name="starting-an-activity-from-a-notification"></a>Starten Sie eine Aktivität aus einer Benachrichtigung

In Android-Geräten ist es üblich, für eine Benachrichtigung verknüpft werden soll ein *Aktion* &ndash; eine Aktivität, die beim Tippen auf der benachrichtigungs gestartet wird. Diese Aktivität kann in einer anderen Anwendung oder sogar in einer anderen Aufgabe befinden. Um eine Benachrichtigung eine Aktion hinzugefügt haben, erstellen Sie eine [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) Objekt, und ordnen Sie die `PendingIntent` bei der Benachrichtigung. Ein `PendingIntent` ist ein besonderer Typ der Absicht, die von der empfangenden Anwendung einen vordefinierten Teil des Codes mit den Berechtigungen der sendenden Anwendung ausgeführt ermöglicht. Wenn der Benutzer die Benachrichtigung tippt, Android startet die Aktivität, die gemäß der `PendingIntent`.

Der folgende Codeausschnitt veranschaulicht, wie erstellen Sie eine Benachrichtigung mit einem `PendingIntent` startet, die die Aktivität von der ursprünglichen app `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
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

Dieser Code ist vergleichbar mit der Benachrichtigungscode im vorherigen Abschnitt, außer dass eine `PendingIntent` an das Benachrichtigungsobjekt hinzugefügt wird. In diesem Beispiel wird die `PendingIntent` bezieht sich auf die Aktivität der ursprünglichen app vor der Übergabe des Notification-Generators [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) Methode. Die `PendingIntentFlags.OneShot` Flag zum Übergeben der `PendingIntent.GetActivity` Methode, damit die `PendingIntent` nur einmal verwendet wird. Wenn dieser Code ausgeführt wird, wird die folgende Benachrichtigung angezeigt:

![Erste Aktion-Benachrichtigung](local-notifications-images/10-first-action-notification.png)

Tippen Sie auf diese Benachrichtigung wird der Benutzer zurück zur ursprünglichen Aktivität.

In einer Produktions-app muss die app verarbeiten die *rückwärts-* Wenn der Benutzer drückt die **wieder** Schaltfläche innerhalb der Benachrichtigungsaktivität (Wenn Sie nicht mit Android Aufgaben und die rückwärts-vertraut sind, finden Sie unter [ Aufgaben und rückwärts-](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
In den meisten Fällen sollte die Rückwärts navigieren, aus der Benachrichtigungsaktivität den Benutzer aus einer app und zurück zum Startbildschirm zurückgeben. Back Stapel, verwendet Ihre app verwalten die [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) -Klasse zum Erstellen einer `PendingIntent` mit einem Stapel zurück.

Ein weiterer Aspekt der realen Welt ist, dass die ursprüngliche Aktivität möglicherweise Daten an die Benachrichtigungsaktivität senden. Z. B. möglicherweise die Benachrichtigung, dass eine Textnachricht eingegangen und Notification-Aktivität (eine Nachricht Bildschirm anzeigen), erfordert die ID der Nachricht an die Nachricht an den Benutzer anzuzeigen. Die Aktivität, erstellt der `PendingIntent` können die [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) Methode zum Hinzufügen von Daten (z. B. eine Zeichenfolge), die den Zweck, sodass diese Daten an die Benachrichtigungsaktivität übergeben werden.

Das folgende Codebeispiel zeigt, wie `TaskStackBuilder` zum Verwalten von die rückwärts- und es enthält ein Beispiel dafür, wie eine Benachrichtigungsaktivität mit dem Namen einer einzelnen Meldungszeichenfolge an `SecondActivity`:

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
Notification.Builder builder = new Notification.Builder (this)
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

In diesem Codebeispiel wird die app besteht aus zwei Aktivitäten: `MainActivity` (enthält die oben genannten Benachrichtigungscode), und `SecondActivity`, den Bildschirm, der dem Benutzer nach Tippen auf die Benachrichtigung angezeigt wird. Wenn dieser Code ausgeführt wird, wird eine einfache Benachrichtigung (ähnlich wie im vorherigen Beispiel) dargestellt. Tippen Sie auf die Benachrichtigung wird der Benutzer auf die `SecondActivity` Bildschirm:

![Screenshot der zweiten Aktivität](local-notifications-images/11-second-activity.png)

Die Zeichenfolgennachricht (der Absicht übergebenen `PutExtra` Methode) wird in abgerufen `SecondActivity` über diese Codezeile:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Diese Meldung abgerufenen "Greetings aus Verwendung des Layoutnamens!" wird angezeigt, der `SecondActivity` Bildschirm, wie im obigen Screenshot dargestellt. Wenn der Benutzer drückt die **wieder** Schaltfläche in `SecondActivity`, Navigation leads aus einer app und dann wieder auf dem Bildschirm vor Start der app.

Weitere Informationen zum Erstellen von ausstehende Intents finden Sie unter [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan" />

## <a name="notification-channels"></a>Unter einem Benachrichtigungskanal versteht

Beginnen mit dem Android 8.0 (Oreo), können Sie die *benachrichtigungskanäle* Funktion zum Erstellen eines benutzerdefinierten Kanals für jeden Typ der Benachrichtigung, die Sie anzeigen möchten. Unter einem Benachrichtigungskanal versteht ermöglichen Sie Gruppe Benachrichtigungen, damit alle Benachrichtigungen eine Anlage Kanal das gleiche Verhalten gebucht. Beispielsweise müssen Sie möglicherweise ein Benachrichtigungskanal, der für Benachrichtigungen vorgesehen ist, die sofortige Aufmerksamkeit erfordern, und einen separaten "ruhigerer" Kanal, der für informationsmeldungen verwendet wird.

Die **YouTube** mit Android Oreo installierten app sind zwei Benachrichtigungskategorien aufgeführt: **herunterladen Benachrichtigungen** und **allgemeine Benachrichtigungen**:

[![Benachrichtigung Bildschirme für YouTube in Android Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png)

Jede dieser Kategorien entspricht ein Benachrichtigungskanal. Implementiert die YouTube-app eine **herunterladen Benachrichtigungen** Kanal und ein **allgemeine Benachrichtigungen** Kanal. Der Benutzer tippen kann **herunterladen Benachrichtigungen**, dem Bildschirm "Einstellungen" angezeigt, für der app Benachrichtigungen Kanal herunterladen:

[![Benachrichtigungen Bildschirm für YouTube-app herunterladen](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png)

Der Benutzer kann auf diesem Bildschirm das Verhalten der Ändern der **herunterladen** Benachrichtigungen channel folgendermaßen aus:

-   Stellen Sie die Wichtigkeit auf **dringend**, **hohe**, **Mittel**, oder **niedrig**, der die Ebene der Unterbrechung von Audio- und visuellen konfiguriert.

-   Aktivieren Sie die Benachrichtigung Punkt aus, oder deaktivieren.

-   Aktivieren Sie die LED blinkende, oder deaktivieren.

-   Ein- und Ausblenden von Benachrichtigungen auf dem Sperrbildschirm.

-   Überschreiben Sie die **nicht stören** Einstellung.

Die **allgemeine Benachrichtigungen** Kanal hat ähnliche Einstellungen:

[![Allgemeine Benachrichtigungen Bildschirm für YouTube-app](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png)

Beachten Sie, dass Sie keine absolute Kontrolle über Ihre benachrichtigungskanäle wie mit dem Benutzer interagieren &ndash; der Benutzer die Einstellungen für alle Benachrichtigungskanal auf dem Gerät ändern kann, wie in den oben genannten Screenshots dargestellt. Allerdings können Sie Standardwerte konfigurieren, (wie unten beschrieben). Da diese Beispiele veranschaulichen, macht es die neue Kanäle Benachrichtigungsfunktion möglich, dass Sie Benutzern eine präzisere Kontrolle über die verschiedenen Arten von Benachrichtigungen gewähren.

Sollten Sie Unterstützung für einen Benachrichtigungskanal zu Ihrer app hinzufügen? Wenn Sie Android 8.0, Ihre app als Ziel bestimmen *müssen* benachrichtigungskanäle implementieren.
Eine app, die als Ziel für Oreo, die versucht, eine lokale Benachrichtigung für den Benutzer zu senden, ohne Verwendung eines benachrichtigungskanals schlägt fehl, die Benachrichtigung auf Oreo Geräte angezeigt werden sollen. Bei Android 8.0 nicht, wird Ihre app weiterhin unter Android 8.0, jedoch mit dem gleichen Benachrichtigungsverhalten wie ausgeführt es aufweisen würde, wenn für Android 7.1 oder früher ausgeführt wird.

<a name="notif-chan-create" />

### <a name="creating-a-notification-channel"></a>Erstellen eines Benachrichtigungskanals

Um einen Benachrichtigungskanal zu erstellen, führen Sie folgende Schritte aus:

1. Erstellen einer [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) Objekt mit den folgenden:

    - Ein ID-Zeichenfolge, die innerhalb des Pakets eindeutig ist. Im folgenden Beispiel wird die Zeichenfolge `com.xamarin.myapp.urgent` verwendet wird.

    - Der Benutzer sichtbare Name des Kanals (weniger als 40 Zeichen). Im folgenden Beispiel den Namen **dringend** verwendet wird.

    - Die Wichtigkeit des Kanals, die steuert, wie die Ausführung einer Benachrichtigungen bereitgestellt die `NotificationChannel`. Die Wichtigkeit kann `Default`, `High`, `Low`, `Max`, `Min`, `None`, oder `Unspecified`.

    Die Werte zum Übergeben der [Konstruktor](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (in diesem Beispiel `Resource.String.noti_chan_urgent` festgelegt ist, um **dringend**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Konfigurieren der `NotificationChannel` Objekt mit den initialeinstellungen.
    Der folgende Code beispielsweise konfiguriert die `NotificationChannel` Objekt, sodass Benachrichtigungen an diesen Kanal gesendeten des Geräts Vibrieren und auf dem Sperrbildschirm standardmäßig angezeigt werden:

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Übermitteln Sie das Benachrichtigungsobjekt-Kanal für den benachrichtigungs-Manager:

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```

<a name="notif-chan-post" />

### <a name="posting-to-a-notifications-channel"></a>Posten von Beiträgen in einem Kanal Benachrichtigungen

Führen Sie folgende Schritte aus, um eine Benachrichtigung an einen Kanal zu senden:

1.  Konfigurieren Sie das Benachrichtigung mit der `Notification.Builder`, und übergeben Sie die Kanal-ID, die die `SetChannelId` Methode. Zum Beispiel:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  Erstellen und Ausstellen der Benachrichtigung wird über den Warnungs-Manager [benachrichtigen](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) Methode:

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

Sie können die oben genannten Schritte zum Erstellen von einem anderen Benachrichtigungskanal für informationsmeldungen wiederholen. Dieser zweite Kanal konnte, wird standardmäßig deaktivieren Vibrationen, legen die standardsichtbarkeit für Sperren Bildschirm auf `Private`, und legen Sie die Wichtigkeit der Benachrichtigung auf `Default`.

Ein vollständiges Codebeispiel von Benachrichtigungskanälen. Android Oreo in Aktion, finden Sie unter der [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Sample; diese Beispiel-app verwaltet zwei Kanäle und legt Weitere Benachrichtigungsoptionen.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Über die grundlegenden Benachrichtigung

Benachrichtigungen als Standardwert ein einfaches Basis Layoutformat in Android, aber Sie können diese Grundformat erhöhen, indem Sie zusätzliche ausführenden `Notification.Builder` Methodenaufrufe. In diesem Abschnitt erfahren Sie, wie die Benachrichtigung ein großes Foto-Symbol hinzu, und Sie finden Beispiele dafür, wie erweiterte Layout Benachrichtigungen erstellt.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>"Große Symbole"-Format

Android-Benachrichtigungen ein Symbol in der Regel der ursprünglichen app (auf der linken Seite der Benachrichtigung). Benachrichtigungen können jedoch anzeigen, ein Bild oder ein Foto (eine *"große Symbole"*) anstelle des standardmäßigen kleinen Symbols an. Eine messaging-app konnte z. B. ein Foto des Absenders anstatt das Symbol "app" angezeigt werden.

Hier ist ein Beispiel einer einfachen Android 5.0-Benachrichtigung &ndash; nur das kleine app-Symbol angezeigt:

![Normale Beispiel-Benachrichtigung](local-notifications-images/13-sample-notification.png)

Und hier ist ein Screenshot, der die Benachrichtigung nach dem Ändern sie ein großes Symbol anzuzeigende &ndash; verwendet ein Symbol, das aus einem Image der Affe eine Xamarin-Code erstellt:

![Beispiel "große Symbole" Benachrichtigung](local-notifications-images/14-large-icon-sample.png)

Beachten Sie, dass wenn eine Benachrichtigung im Format "große Symbole" dargestellt wird, das Symbol "kleine app" als ein Signal in der unteren rechten Ecke des großes Symbol angezeigt wird.

Um ein Bild als ein großes Symbol in einer Benachrichtigung verwenden möchten, rufen Sie die Benachrichtigung-Generator [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) -Methode und übergeben Sie eine Bitmap des Bilds. Im Gegensatz zu `SetSmallIcon`, `SetLargeIcon` akzeptiert nur eine Bitmap. Zum Konvertieren einer Abbilddatei in eine Bitmap, die Sie verwenden die [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) Klasse. Zum Beispiel:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Dieser Beispielcode öffnet die Abbilddatei an **Resources/drawable/monkey_icon.png**, konvertiert es in eine Bitmap und übergibt das resultierende Bitmap `Notification.Builder`. In der Regel die bildauflösung Quelle ist größer als das kleine Symbol &ndash; aber nicht sehr viel größere. Ein Bild, das zu groß ist möglicherweise unnötige größenänderungsvorgängen, die die Benachrichtigung die Buchung verzögert werden kann.
Weitere Informationen zur Benachrichtigung Symbolgrößen in Android finden Sie unter [Benachrichtigungssymbole](http://developer.android.com/design/style/iconography.html#notification).

<a name="big-text-style" />

### <a name="big-text-style"></a>Große Textart

Die *Big Text* Stil ist eine erweiterte Layoutvorlage aus, die Sie zum Anzeigen von lange Meldungen in Benachrichtigungen verwenden. Wie alle erweiterten Layout-Benachrichtigungen wird der große textbenachrichtigung in ein Präsentationsformat compact anfänglich angezeigt:

![Beispiel für große textbenachrichtigung](local-notifications-images/15-big-text-notification.png)

In diesem Format wird nur ein Auszug aus der Nachricht angezeigt, durch zwei Punkte beendet. Wenn der Benutzer auf die Benachrichtigung, nach unten zieht erweitert und es werden die gesamte Nachricht anzuzeigen:

![Erweiterte Big textbenachrichtigung](local-notifications-images/16-big-text-expanded.png)

Diese erweiterten Layoutformat schließt außerdem summary-Text am unteren Rand der Benachrichtigung. Die maximale Höhe der *Big Text* Benachrichtigung ist 256 dp.

Erstellen eine *Big Text* Benachrichtigung, instanziieren Sie eine `Notification.Builder` Objekt, wie zuvor, und klicken Sie dann zu instanziieren und Hinzufügen einer [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) -Objekt an die `Notification.Builder` Objekt. Zum Beispiel:

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

In diesem Beispiel dem Meldungstext und der Zusammenfassungstext befinden sich der `BigTextStyle` Objekt (`textStyle`) vor der Übergabe an `Notification.Builder.`

<a name="image-style" />

### <a name="image-style"></a>Bild-Format

Die *Image* Stil (so genannte der *kompletten* Stil) ist eine erweiterte benachrichtigungsformat, die Sie verwenden können, um ein Bild im Text einer Benachrichtigung anzuzeigen. Z. B. eine bildschirmabbildung von app oder eine Foto-Apps können die *Image* Benachrichtigung Bild-Formatvorlage, die dem Benutzer eine Benachrichtigung des letzten bereitzustellen, die erfasst wurden. Beachten Sie, dass die maximale Höhe der *Image* Benachrichtigung ist 256 dp &ndash; Android wird das Bild, das in dieser Einschränkung maximale Höhe innerhalb der Grenzen des verfügbaren Arbeitsspeichers passt die Größe.

Wie bei allen erweitert Layout-Benachrichtigungen *Image* Benachrichtigungen werden zuerst angezeigt, in einem kompakten Format, in dem einen Auszug aus dem zugehörigen Nachrichtentext angezeigt:

![Compact Image Benachrichtigung dargestellt keine Abbildung.](local-notifications-images/17-image-compact.png)

Wenn der Benutzer zieht nach unten auf der *Image* Benachrichtigung erweitert wird, um ein Bild anzuzeigen. Hier ist z. B. die erweiterte Version des vorherigen Benachrichtigung:

![Erweiterte Bild Benachrichtigung ergibt Bild](local-notifications-images/18-image-expanded.png)

Beachten Sie, dass wenn die Benachrichtigung in einem komprimierten Format angezeigt wird, er Benachrichtigungstext zeigt (der Text, der dem Notification-Generator übergeben wird `SetContentText` Methode, wie weiter oben gezeigt). Wenn die Benachrichtigung erweitert wird, um das Bild anzuzeigen, zeigt jedoch Zusammenfassungstext oberhalb des Bilds.

Erstellen eine *Image* Benachrichtigung, instanziieren Sie ein `Notification.Builder` Objekt wie zuvor, und erstellen und Einfügen ein [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) -Objekt in der `Notification.Builder` Objekt. Zum Beispiel:

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

Wie die `SetLargeIcon` Methode `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) Methode `BigPictureStyle` erfordert eine Bitmap des Bilds, das im Text der Benachrichtigung angezeigt werden soll. In diesem Beispiel wird die [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) Methode `BitmapFactory` liest die Bilddatei controllerarbeitsverzeichnis **Resources/drawable/x_bldg.png** und konvertiert ihn in eine Bitmap.

Sie können Bilder, die nicht gepackt werden auch als Ressource anzeigen. Beispielsweise der folgende Beispielcode lädt ein Bild aus der lokalen SD-Karte, und zeigt ihn in ein *Image* Benachrichtigung:

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

In diesem Beispiel befindet sich die Abbilddatei an **/sdcard/Pictures/my-tshirt.jpg** geladen, auf die Hälfte der ursprünglichen Größe angepasst und dann in eine Bitmap für die Verwendung in der Benachrichtigung konvertiert:

![Abbildung eines Beispiel T-shirt Benachrichtigung](local-notifications-images/19-tshirt-notification.png)

Wenn Sie die Größe der Bilddatei nicht im Voraus kennen, ist es eine gute Idee, umschließen den Aufruf von [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) in einem Ausnahmehandler &ndash; ein `OutOfMemoryError` Ausnahme kann ausgelöst werden, wenn das Bild für zu groß ist. Android Größe ändern.

Weitere Informationen zum Laden und Decodieren von Bildern für große Bitmap finden Sie unter [laden große Bitmaps effiziente](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently).

<a name="inbox-style" />

### <a name="inbox-style"></a>Eingangsbox-Stil

Die *Posteingang* Format ist eine erweiterte Layoutvorlage für die Anzeige von separaten Textzeilen (z. B. eine e-Mail-Posteingang Zusammenfassung) im Text der Benachrichtigung vorgesehen. Die *Posteingang* Format-Benachrichtigung wird zunächst in einem kompakten Format angezeigt:

![Beispiel compact Posteingang-Benachrichtigung](local-notifications-images/20-inbox-compact.png)

Wenn der Benutzer auf die Benachrichtigung nach unten zieht, erweitert und es werden eine e-Mail-Zusammenfassung, wie im folgenden Screenshot sehen Offenlegen:

![Beispiel-Posteingang-Benachrichtigung erweitert](local-notifications-images/21-inbox-expanded.png)

Erstellen einer *Posteingang* Benachrichtigung, instanziieren Sie eine `Notification.Builder` -Objekt, wie zuvor, und fügen Sie ein [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) -Objekt an der `Notification.Builder`. Zum Beispiel:

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

Um neue Textzeilen in den Text der Benachrichtigung hinzuzufügen, rufen Sie die [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) Methode der `InboxStyle` Objekt (die maximale Höhe des der *Posteingang* Benachrichtigung ist 256 dp). Beachten Sie, dass im Gegensatz zu *Big Text* Stil, die *Posteingang* Stil unterstützt einzelne Textzeilen in den Textkörper der Benachrichtigung.

Sie können auch die *Posteingang* Stil für Benachrichtigungen, die einzelne Textzeilen in einem erweiterten Format anzeigen. Z. B. die *Posteingang* Benachrichtigung-Stil kann verwendet werden, um mehrere ausstehende Benachrichtigungen in einer zusammenfassungsbenachrichtigung kombinieren &ndash; können Sie ein einzelnes aktualisieren *Posteingang* Benachrichtigung über neu formatieren Zeilen mit Benachrichtigungsinhalt (finden Sie unter [aktualisieren eine Benachrichtigung](#updating-a-notification) oben), sondern als einen kontinuierlichen Strom von neuen, größtenteils ähnlich Benachrichtigungen zu generieren. Weitere Informationen zu diesem Verfahren finden Sie unter [zusammenfassen Ihre Benachrichtigungen](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).

<a name="configuring-metadata" />

## <a name="configuring-metadata"></a>Konfigurieren von Metadaten

`Notification.Builder` enthält Methoden, die Sie aufrufen können, um Metadaten über die Benachrichtigung, wie z. B. Sichtbarkeit, Priorität und Kategorie festgelegt. Android verwendet diese Informationen &mdash; zusammen mit benutzereinstellungseinstellungen &mdash; um zu bestimmen, wie und wann Benachrichtigungen angezeigt.

<a name="priority-settings" />

### <a name="priority-settings"></a>Prioritätseinstellungen

Die Einstellung der Priorität einer Benachrichtigung bestimmt zwei Ergebnisse, wenn die Benachrichtigung veröffentlicht wird:

-   Wobei die Benachrichtigung in Bezug auf andere Benachrichtigungen angezeigt wird.
    Beispielsweise werden hohe Priorität Benachrichtigungen über niedrigere Priorität Benachrichtigungen in den Bereich Benachrichtigungen unabhängig von angezeigt, wenn jede Benachrichtigung veröffentlicht wurde.

-   Gibt an, ob die Benachrichtigung in der Heads-up benachrichtigungsformat (Android 5.0 und höher) angezeigt wird. Nur *hohe* und *maximale* Priorität Benachrichtigungen als Heads-Up-Benachrichtigungen angezeigt werden.

Xamarin.Android definiert die folgenden Enumerationen für Benachrichtigungspriorität festlegen:

-   `NotificationPriority.Max` &ndash; Weist den Benutzer auf eine dringende oder der kritische Bedingung (z. B. ein eingehender Anruf, Aktivieren von Turn Richtungen oder ein Notfall Warnung). Auf Android 5.0 und höher werden die maximalen Priorität Benachrichtigungen Heads-Up-Format angezeigt.

-   `NotificationPriority.High` &ndash; Informiert den Benutzer von wichtigen Ereignissen (z. B. wichtige e-Mails oder den Empfang von Chatnachrichten in Echtzeit). Auf Android 5.0 und höher werden Benachrichtigungen mit hoher Priorität Heads-Up-Format angezeigt.

-   `NotificationPriority.Default` &ndash; Benachrichtigt den Benutzer von Bedingungen, die eine mittlere Wichtigkeit haben.

-   `NotificationPriority.Low` &ndash; Für nicht dringende Informationen, die der Benutzer (z. B. Software Update Erinnerungen oder soziale Netzwerke Updates) informiert werden muss.

-   `NotificationPriority.Min` &ndash; Hintergrundinformationen, dass der Benutzer nur, wenn Benachrichtigungen Anzeigen von Benachrichtigungen (z. B. Speicherort oder Wetter-Informationen).

Um die Priorität einer Benachrichtigung festlegen möchten, rufen die [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) Methode der `Notification.Builder` Objekts und übergibt dabei die Prioritätsstufe. Zum Beispiel:

```csharp
builder.SetPriority (NotificationPriority.High);
```

Das folgende Beispiel, die Benachrichtigung hoher Priorität, "Eine wichtige Meldung!" Öffnen Sie die Benachrichtigung oben angezeigt:

![Beispiel hoher Priorität-Benachrichtigung](local-notifications-images/22-hi-priority-drawer.png)

Da es sich um eine Benachrichtigung hoher Priorität handelt, wird er auch als ein Heads-Up-Benachrichtigung über die aktuelle Aktivität Benutzerbildschirms Android 5.0 angezeigt:

![Beispiel-Heads-Up-Benachrichtigung](local-notifications-images/23-heads-up-example.png)

Im nächsten Beispiel wird die mit niedriger Priorität "Für den Tag betrachtet" Benachrichtigung unter eine höhere Priorität Akku Ebene Benachrichtigung angezeigt:

![Beispiel mit niedriger Priorität-Benachrichtigung](local-notifications-images/24-lo-priority-drawer.png)

Da die Benachrichtigung ", um herauszufinden, für den Tag" eine Benachrichtigung mit niedriger Priorität ist, wird Sie von Android keine Heads-up Format angezeigt.

<a name="visibility-settings" />

### <a name="visibility-settings"></a>Sichtbarkeitseinstellungen

Android 5.0.x ab der *Sichtbarkeit* Einstellung kann steuern, wie viel Inhalt der Benachrichtigung auf die sicheren für den sperrbildschirmhintergrund angezeigt wird.
Xamarin.Android definiert die folgenden Enumerationen für die Sichtbarkeit der Benachrichtigung festlegen:

-   `NotificationVisibility.Public` &ndash; Der vollständige Inhalt der Benachrichtigung wird auf die sicheren für den sperrbildschirmhintergrund angezeigt.

-   `NotificationVisibility.Private` &ndash; Nur wichtige Informationen für die sichere für den sperrbildschirmhintergrund (z. B. das Benachrichtigungssymbol und den Namen der app, die sie zurückgesendet) angezeigt, aber die restlichen Details für die Benachrichtigung sind ausgeblendet. Alle Benachrichtigungen standardmäßig `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Auf die sicheren für den sperrbildschirmhintergrund, auch nicht auf das Benachrichtigungssymbol wird nichts angezeigt. Den Inhalt der Benachrichtigung ist verfügbar, nachdem der Benutzer das Gerät entsperrt.

Um die Sichtbarkeit einer Benachrichtigung, apps Aufruf festzulegen der `SetVisibility` Methode der `Notification.Builder` Objekt, und der Einstellung "Sichtbarkeit" übergeben. Beispielsweise dieser Aufruf `SetVisibility` stellt die Benachrichtigung `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Wenn eine `Private` Benachrichtigung gesendet wird, wird nur der Name und der app-Symbol für die sichere für den sperrbildschirmhintergrund angezeigt. Anstatt die Nachricht erkennt der Benutzer "Entsperren Ihres Geräts zu dieser Benachrichtigung finden Sie unter":

![Entsperren Sie Ihr Gerät-Benachrichtigung](local-notifications-images/25-lockscreen-private.png)

In diesem Beispiel **NotificationsLab** ist der Name der ursprünglichen app. Diese geschwärzten Version der Benachrichtigung angezeigt wird, nur wenn die für den sperrbildschirmhintergrund sicher ist (d. h. per PIN, Muster oder ein Kennwort geschützt) &ndash; der für den sperrbildschirmhintergrund nicht sicher ist, der vollständigen Inhalt der Benachrichtigung auf der für den sperrbildschirmhintergrund verfügbar ist.

<a name="category-settings" />

### <a name="category-settings"></a>Kategorie-Einstellungen

Beginnen mit dem Android 5.0, sind die vordefinierte Kategorien für Zuweisen von Rängen und Filtern von Benachrichtigungen verfügbar. Xamarin.Android bietet die folgenden Enumerationen für diese Kategorien:

-   `Notification.CategoryCall` &ndash; Eingehenden Telefonanruf.

-   `Notification.CategoryMessage` &ndash; Eingehende Textnachricht.

-   `Notification.CategoryAlarm` &ndash; Ein Alarm Bedingung oder einen Zeitgeber ablaufen.

-   `Notification.CategoryEmail` &ndash; Eingehende e-Mail-Nachricht.

-   `Notification.CategoryEvent` &ndash; Ein Kalenderereignis.

-   `Notification.CategoryPromo` &ndash; Ein Werbe-Nachricht oder eine Ankündigung.

-   `Notification.CategoryProgress` &ndash; Der Status eines Hintergrundvorgangs im.

-   `Notification.CategorySocial` &ndash; Update für soziale Netzwerke.

-   `Notification.CategoryError` &ndash; Fehler beim Vorgang oder die Authentifizierung ein Hintergrundprozess.

-   `Notification.CategoryTransport` &ndash; Update für Media-Wiedergabe.

-   `Notification.CategorySystem` &ndash; Reserviert für die Verwendung durch das System (Systems oder Geräts Status).

-   `Notification.CategoryService` &ndash; Gibt an, dass ein Hintergrunddienst im ausgeführt wird.

-   `Notification.CategoryRecommendation` &ndash; Eine Empfehlung-Meldung, die im Zusammenhang mit der laufenden app.

-   `Notification.CategoryStatus` &ndash; Informationen über das Gerät.

Wenn Benachrichtigungen sortiert sind, hat die Benachrichtigung Priorität Vorrang vor seine Kategorie-Einstellung. Eine Benachrichtigung hoher Priorität werden z. B. als Heads-Up angezeigt werden, auch wenn er gehört die `Promo` Kategorie. Um die Kategorie der Benachrichtigung festlegen möchten, rufen Sie die `SetCategory` Methode der `Notification.Builder` Objekt, und der Einstellung "Kategorie" übergeben. Zum Beispiel:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

Die *nicht stören* Feature (neu in Android 5.0.x) filtert Benachrichtigungen basierend auf Kategorien. Z. B. die *nicht stören* im Bildschirm **Einstellungen** ermöglicht es dem Benutzer ausgenommen Benachrichtigungen für Anrufe und Nachrichten:

![Bildschirm Switches nicht stören](local-notifications-images/26-do-not-disturb.png)

Wenn der Benutzer konfiguriert *nicht stören* um alle Interrupts außer Telefonanrufe (wie im obigen Screenshot dargestellt) zu blockieren, Android Benachrichtigungen Kategorie diese Einstellung ermöglicht `Notification.CategoryCall` währen das Gerät dargestellt werden soll befindet sich im *nicht stören* Modus. Beachten Sie, dass `Notification.CategoryAlarm` Benachrichtigungen werden nie in blockiert *nicht stören* Modus.


<a name="compatibility" />

## <a name="compatibility"></a>Kompatibilität

Wenn Sie eine app erstellen führen Sie auch in früheren Versionen von Android (so früh wie API-Ebene 4), verwenden Sie die [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) -Klasse statt `Notification.Builder`. Beim Erstellen von Benachrichtigungen mit `NotificationCompat.Builder`, Android wird sichergestellt, dass grundlegende Benachrichtigungsinhalt auf ältere Geräte ordnungsgemäß angezeigt wird. Jedoch, da einige erweiterte Benachrichtigungsfunktionen nicht auf älteren Versionen von Android verfügbar sind, muss der Code verarbeiten explizit Kompatibilitätsprobleme für eine Benachrichtigung erweiterten Stile, Kategorien und Sichtbarkeit wie nachstehend beschrieben.

Mit `NotificationCompat.Builder` in der app, und fügen Sie der [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet in Ihrem Projekt.

Im folgenden Codebeispiel wird veranschaulicht, wie zum Erstellen eines grundlegenden Benachrichtigung mithilfe `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

Methodenaufrufe für wichtige Benachrichtigungsoptionen sind identisch mit denen der, wie in diesem Beispiel wird verdeutlicht, `Notification.Builder`. Allerdings hat der Code nach unten weisenden-Kompatibilitätsprobleme für komplexere Benachrichtigungen (im nächsten Abschnitt beschrieben) behandelt.

Die [LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) Beispiel veranschaulicht, wie `NotificationCompat.Builder` um eine zweite Aktivität von einer Benachrichtigung zu starten. In diesem Beispielcode wird erläutert, der [mithilfe von lokalen Benachrichtigungen in Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) Exemplarische Vorgehensweise.

<a name="notification-styles" />

### <a name="notification-styles"></a>Benachrichtigung Stile

Zum Erstellen *Big Text*, *Image*, oder *Posteingang* Formatieren von Benachrichtigungen mit `NotificationCompat.Builder`, Ihre app muss die Kompatibilität Versionen dieser Stile verwenden. Beispielsweise verwenden die *Big Text* formatieren, instanziieren `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Auf ähnliche Weise können Ihre app `NotificationCompat.InboxStyle` und `NotificationCompat.BigPictureStyle` für *Posteingang* und *Image* Stile, bzw.

<a name="priority-and-category" />

### <a name="notification-priority-and-category"></a>Benachrichtigungspriorität und Kategorie

`NotificationCompat.Builder` unterstützt die `SetPriority` Methode (verfügbar ab mit Android 4.1). Allerdings die `SetCategory` Methode ist *nicht* von unterstützten `NotificationCompat.Builder` da Kategorien neue Benachrichtigung Metadatensystem gehören, die in Android 5.0 eingeführt wurde.

Zur Unterstützung von älterer Versionen von Android, Where `SetCategory` ist nicht verfügbar ist, der Code zur Laufzeit bedingt Aufrufen der API-Ebene überprüfen kann `SetCategory` beim API-Ebene ist, gleich oder größer als Android 5.0.x (API-Ebene 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

In diesem Beispiel wird die app des **Zielframework** auf Android 5.0 festgelegt ist und die **Mindestversion Android** festgelegt ist, um **Android 4.1 (API Level 16)**. Da `SetCategory` wird in API-Ebene 21 und höher verfügbar, in diesem Beispielcode ruft `SetCategory` nur, wenn es verfügbar ist &ndash; kein Aufruf `SetCategory` beim API-Ebene ist kleiner als
21.

<a name="lockscreen-visibility" />

### <a name="lockscreen-visibility"></a>Sichtbarkeit des Sperrbildschirms

Da Android keine für den sperrbildschirmhintergrund Benachrichtigungen vor Android 5.0.x (API-Ebene 21), unterstützen `NotificationCompat.Builder` unterstützt nicht die `SetVisibility` Methode. Wie bereits dargelegt für `SetCategory`, Codes sehen die API-Ebene zur Laufzeit und rufen `SetVisiblity` nur, wenn es verfügbar ist:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```

<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie lokale Benachrichtigungen in Android zu erstellen. Er den Aufbau einer Benachrichtigung beschrieben, es wird erläutert, wie mit `Notification.Builder` zum Erstellen von Benachrichtigungen, Stil-Benachrichtigungen in "große Symbole", wie *Big Text*, *Image* und *Posteingang*  Formate, wie Benachrichtigung Metadateneinstellungen wie z. B. Sichtbarkeit, Priorität und Kategorie festgelegt und wie eine Aktivität aus einer Benachrichtigung gestartet. In diesem Artikel auch beschrieben, wie diese Benachrichtigungseinstellungen mit der neuen Heads-Up, für den sperrbildschirmhintergrund, arbeiten und *nicht stören* Android 5.0 eingeführten Funktionen. Schließlich haben Sie gelernt, wie verwenden `NotificationCompat.Builder` Benachrichtigung Kompatibilität mit früheren Versionen von Android verwalten.

Richtlinien zum Entwerfen von Benachrichtigungen für Android finden Sie unter [Benachrichtigungen](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>Verwandte Links

- [NotificationsLab (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Lokale Benachrichtigungen In Android Exemplarische Vorgehensweise](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Benachrichtigungen](http://developer.android.com/design/patterns/notifications.html)
- [Benachrichtigen des Benutzers](http://developer.android.com/training/notify-user/index.html)
- [Benachrichtigung](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
