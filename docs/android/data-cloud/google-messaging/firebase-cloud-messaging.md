---
title: Firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM) ist ein Dienst, der ermöglicht, Nachrichten zwischen mobile apps und Server-Anwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM, und es wird erläutert, wie Google Services so konfigurieren, dass Ihre app FCM verwenden kann.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 92c402085627bbe67d4cd8ccaf60a68aa979f3e7
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514294"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) ist ein Dienst, der ermöglicht, Nachrichten zwischen mobile apps und Server-Anwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM, und es wird erläutert, wie Google Services so konfigurieren, dass Ihre app FCM verwenden kann._

[![Firebase Cloud Messaging Hero-Bild](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Dieses Thema enthält einen allgemeinen Überblick darüber, wie das Firebase Cloud Messaging Nachrichten zwischen Ihrer Xamarin.Android-app und eines app-Servers leitet ein, und es enthält ein schrittweises Verfahren zum Abrufen von Anmeldeinformationen, damit Ihre app FCM-Dienste verwenden kann.

## <a name="overview"></a>Übersicht

Firebase Cloud Messaging (FCM) ist ein plattformübergreifender Dienst, der die senden, routing und Queuing von Nachrichten zwischen Server-Anwendungen und mobile Client-apps behandelt. FCM ist der Nachfolger zu Google Cloud Messaging (GCM), und die Sprache baut auf Google Play-Dienste.

Wie im folgenden Diagramm dargestellt wird, fungiert FCM als Vermittler zwischen Clients und Absender von Nachrichten an. Ein *Client-app* ist eine FCM-fähige app, die auf einem Gerät ausgeführt wird. Die *Anwendungsserver* (bereitgestellt durch Sie oder Ihr Unternehmen) ist der FCM-fähigen Server, mit denen der Client-app über FCM kommuniziert. Im Gegensatz zu GCM ist es bei FCM zum Senden von Nachrichten auf Client-apps direkt über die Firebase-Konsole Benachrichtigungen GUI möglich:

[![FCM befindet sich zwischen dem Client-app und eines app-Servers](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

Per FCM, können app-Server senden Nachrichten an ein einzelnes Gerät, um eine Gruppe von Geräten oder auf eine Anzahl von Geräten, die an ein Thema abonniert haben. Eine Client-app können FCM zum Abonnieren von downstream-Nachrichten von einem app-Server (z. B. um remotebenachrichtigungen zu erhalten). Weitere Informationen zu den verschiedenen Typen von Firebase-Nachrichten finden Sie unter [About FCM Messages](https://firebase.google.com/docs/cloud-messaging/concept-options).

## <a name="fcm-in-action"></a>Firebase Cloud Messaging in Aktion

Wenn eine downstreamverarbeitung von Nachrichten an einen Client-app von einem app-Server gesendet wird, handelt es sich bei der app-Server sendet die Nachricht an ein *FCM-Verbindungsserver* von Google; bereitgestellt wird der Server für den FCM-Verbindung, wiederum leitet die Nachricht an ein Gerät, das ausgeführt wird die Client-app. Es können Nachrichten über HTTP gesendet werden oder [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging und Vorhandensein-Protokoll). Da der Client-apps nicht immer verbunden sind oder ausgeführt wird, handelt es sich bei der FCM Verbindung Server in die Warteschlange einreiht und speichert Nachrichten senden an den Client-apps verfügbar und erneut eine Verbindung herzustellen. Auf ähnliche Weise FCM wird in die Warteschlange einzureihen upstream Nachrichten vom Client-app in der app-Server, wenn der app-Server nicht verfügbar ist. Weitere Informationen zu FCM-Verbindung-Servern finden Sie unter [zu Firebase Cloud Messaging-Server](https://firebase.google.com/docs/cloud-messaging/server).

FCM verwendet die folgenden Anmeldeinformationen zum Identifizieren der app-Server und die Client-app, und sie diese Anmeldeinformationen verwendet, um Transaktionen über FCM Nachrichten zu autorisieren:

-   <a name="fcm-in-action-sender-id"></a>**Absender-ID** &ndash; der *Absender-ID* ist ein eindeutiger numerischer Wert, der zugewiesen wird, wenn Sie Ihrem Firebase-Projekt erstellen. Die Absender-ID wird verwendet, jede app-Server zu identifizieren, die Nachrichten an die Client-app senden können. Die Absender-ID ist auch Ihre Projektnummer. Sie erhalten die Absender-ID über die Firebase-Konsole, wenn Sie Ihr Projekt registrieren. Ein Beispiel für eine Absender-ID ist `496915549731`.

-   <a name="fcm-in-action-api-key"></a>**API-Schlüssel** &ndash; der *API-Schlüssel* ermöglicht der app-Server den Zugriff auf Firebase-Diensten FCM verwendet diesen Schlüssel zum Authentifizieren des app-Servers. Diese Anmeldeinformationen werden auch als bezeichnet die *Serverschlüssel* oder *Web API-Schlüssel*. Ein Beispiel für einen API-Schlüssel ist `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   <a name="fcm-in-action-app-id"></a>**App-ID** &ndash; die Identität Ihrer Client-App (unabhängig von einem beliebigen angegebenen Gerät), der zum Empfangen von Nachrichten von FCM registriert. Ein Beispiel für eine App-ID ist `1:415712510732:android:0e1eb7a661af2460`.

-   <a name="fcm-in-action-registration-token"></a>**Registrierungstoken** &ndash; der *Registrierung Token* (auch bezeichnet als die *Instanz-ID*) ist die Identität FCM Ihrer Client-App auf einem Gerät. Das Registrierungstoken wird zur Laufzeit generiert &ndash; Ihre app empfängt ein Registrierungstoken aus, wenn er sich zunächst bei FCM registriert, während der Ausführung auf einem Gerät. Das Registrierungstoken wird eine Instanz Ihrer Client-App (ausgeführt auf dem bestimmten Gerät) zum Empfangen von Nachrichten von FCM autorisiert.
    Ein Beispiel eines registrierungstokens ist `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (eine lange Zeichenfolge).

[Einstellung von Firebase Cloud Messaging](#setup_fcm) (weiter unten in diesem Leitfaden) enthält ausführliche Anweisungen zum Erstellen eines Projekts, und generieren mit diesen Anmeldeinformationen. Beim Erstellen eines neuen Projekts in der [Firebase-Konsole](https://console.firebase.google.com/), eine Datei mit dem Namen **Google-services.json** erstellt &ndash; diese Datei zu Ihrer Xamarin.Android-Projekt hinzufügen, wie unter [ Remotebenachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

In den folgenden Abschnitten wird erläutert, wie diese Anmeldeinformationen verwendet werden, wenn der Client-apps mit app-Server über FCM kommunizieren.


<a name="registration" />

### <a name="registration-with-fcm"></a>Registrierung bei FCM

Eine Client-app muss zuerst bei FCM registrieren, bevor messaging stattfinden kann. Die Client-app muss die Schritte zur Registrierung in der folgenden Abbildung gezeigten ausführen:

[![App-Registrierung-Schritte-Diagramm](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  Die Client-app kontaktiert FCM zum Abrufen eines registrierungstokens, die Absender-ID, die API-Schlüssel und die App-ID an FCM übergeben.

2.  FCM gibt ein Registrierungstoken zur Client-app zurück.

3.  Die Client-app weiterleitet (optional) das Registrierungstoken für die app-Server.

Der app-Server speichert das Registrierungstoken für die weitere Kommunikation mit dem Client-app. Der app-Server kann eine Bestätigung senden, an die Client-app, um anzugeben, dass das Registrierungstoken empfangen wurde. Nachdem dieser Handshake durchgeführt wurde, kann die Client-app empfangen von Nachrichten aus (oder Senden von Nachrichten an) der app-Server. Die Client-app möglicherweise ein neues Registrierungstoken, wenn das alte Token gefährdet ist (finden Sie unter [Remotebenachrichtigungen bei FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) für verdeutlicht, wie eine Anwendung token registrierungsaktualisierungen empfängt).

Wenn die Client-app, die nicht mehr zum Empfangen von Nachrichten aus der app-Server benötigt wird, können sie eine Anforderung an die Anwendungsserver So löschen Sie das Registrierungstoken senden. Wenn die Client-app von einem Gerät deinstalliert wird, wird von FCM erkennt dies und benachrichtigt automatisch der app-Server So löschen Sie das Registrierungstoken.



### <a name="downstream-messaging"></a>Downstream-messaging

Das folgende Diagramm veranschaulicht, wie Firebase Cloud Messaging und speichert downstream Nachrichten weiterleitet:

[![FCM verwendet Speicher- und Weiterleitungsmodus für downstream-messaging](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Wenn der app-Server eine downstreamverarbeitung von Nachrichten an die Client-app sendet, verwendet es die folgenden Schritte aus, wie in der obigen Abbildung aufgelisteten:

1.  Der app-Server sendet die Nachricht zu FCM.

2.  Wenn der Client-Gerät nicht verfügbar ist, speichert der FCM-Server die Nachricht in einer Warteschlange für die spätere Übertragung. Nachrichten werden im FCM-Speicher für bis zu 4 Wochen beibehalten (Weitere Informationen finden Sie unter [Festlegen der Lebensdauer einer Nachricht](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Wenn der Client-Gerät verfügbar ist, leitet FCM die Nachricht an die Client-app auf dem Gerät.

4.  Die Client-app empfängt die Nachricht von FCM, verarbeitet sie und die Benutzer angezeigt. Wenn die Nachricht eine remotebenachrichtigung ist, ist es z. B. für den Benutzer im Infobereich der Taskleiste angezeigt.

In diesem messaging-Szenario (wobei der app-Server sendet eine Nachricht an eine einzelne Client-app), Nachrichten können bis zu 4kB lang sein.

Ausführliche Informationen zur downstream FCM-Nachrichten unter Android, finden Sie unter [Remotebenachrichtigungen bei FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Thema messaging

*Thema Messaging* ermöglicht es einer app-Server zum Senden einer Nachricht an mehrere Geräte, die sich auf ein bestimmtes Thema entschieden haben. Sie können auch verfassen und Senden von Nachrichten von Thema über die grafische Benutzeroberfläche Benachrichtigungen von Firebase-Konsole. FCM verarbeitet das routing und die Bereitstellung des themanachrichten gesendet werden für abonnierte Clients. Dieses Feature kann für Nachrichten, z. B. Wetter-Warnungen, Aktienkurse und Schlagzeilen verwendet werden.

[![Thema-messaging-Diagramm](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

Die folgenden Schritte werden in Thema messaging verwendet, (nachdem Sie die Client-app ein Registrierungstoken wie bereits erwähnt erhält):

1.  Die Client-app, die durch Senden einer Nachricht abonnieren, FCM an ein Thema abonniert werden.

2.  Der app-Server sendet themanachrichten gesendet werden, FCM, für die Verteilung.

3.  FCM leitet themanachrichten gesendet werden, für Clients, die für dieses Thema abonniert haben.

Weitere Informationen zu Firebase Thema messaging, finden Sie unter Google [Thema Messaging unter Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Einrichten von Firebase Cloud Messaging

Bevor Sie den FCM-Dienste in Ihrer app verwenden können, müssen Sie ein neues Projekt erstellen (oder ein vorhandenes Projekt importieren) über die [Firebase-Konsole](https://console.firebase.google.com/). Verwenden Sie zum Erstellen eines Firebase Cloud Messaging-Projekts für Ihre app die folgenden Schritte aus:

1.  Melden Sie sich bei der [Firebase-Konsole](https://console.firebase.google.com/) mit Ihrem Google-Konto (d. h. Ihre Gmail-Adresse) und klicken Sie auf **Erstellen eines neuen PROJEKTS**:

    [![Erstellen Sie die Schaltfläche "Neues Projekt"](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Wenn Sie ein vorhandenes Projekt haben, klicken Sie auf **ein Google-Projekt importieren**.

2.  In der **erstellen Sie ein Projekt** Dialogfeld Geben Sie den Namen des Projekts, und klicken Sie auf **Erstellen eines PROJEKTS**. Im folgenden Beispiel ist ein neues Projekt namens **XamarinFCM** wird erstellt:

    [![Erstellen Sie ein Dialogfeld "Projekt"](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  In der Firebase-Konsole **Übersicht**, klicken Sie auf **Hinzufügen von Firebase zu Ihrer Android-app**:

    [![Hinzufügen von Firebase zu Ihrer Android-app](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Geben Sie im nächsten Bildschirm den Paketnamen der app ein. In diesem Beispiel ist der Paketname ist **com.xamarin.fcmexample**. Dieser Wert muss es sich um den Paketnamen der Ihrer Android-app übereinstimmen. Ein app-Spitzname kann auch eingegeben werden, der **App Spitzname** Feld:

    [![Eingeben von FCM-Beispiel als den Kurznamen der app](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Wenn Ihre app dynamische Links, Einladungen oder Google-Authentifizierung verwendet, müssen Sie auch Ihre Debug Signaturzertifikat eingeben. Weitere Informationen zum Suchen Ihres Signaturzertifikats finden Sie unter [Suchen von MD5 oder SHA1-Signatur Ihres Keystores des](~/android/deploy-test/signing/keystore-signature.md).
    In diesem Beispiel wird das Signaturzertifikat leer gelassen.

6.  Klicken Sie auf **APP hinzufügen**:

    [![Klicken Sie auf die Schaltfläche "App hinzufügen"](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Ein Server-API-Schlüssel und eine Client-ID werden für die app automatisch generiert. Diese Informationen innerhalb einer **Google-services.json** -Datei, die automatisch heruntergeladen wird, wenn Sie auf **APP hinzufügen**.
    Achten Sie darauf, dass Sie diese Datei an einem sicheren Ort zu speichern.

Ein ausführliches Beispiel zum Hinzufügen von **Google-services.json** zu einem app-Projekt zum Empfangen von FCM-Push-Benachrichtigungen unter Android, finden Sie unter [Remotebenachrichtigungen bei FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Weitere Informationen

-   Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) bietet einen Überblick über die Firebase Cloud Messaging die wichtigsten Funktionen, eine Erläuterung der Funktionsweise und Anweisungen zur Einrichtung.

-   Google [erstellen App senden Serveranforderungen](https://firebase.google.com/docs/cloud-messaging/send-message) wird erläutert, wie Nachrichten mit dem app-Server zu senden.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) und [RFC 6121](https://tools.ietf.org/html/rfc6121) erläutern und dem Extensible Messaging and Presence Protocol (XMPP) zu definieren.

-   [Informationen zu FCM-Nachrichten](https://firebase.google.com/docs/cloud-messaging/concept-options) beschreibt die verschiedenen Typen von Nachrichten, die gesendet werden, können mit Firebase Cloud Messaging.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine Übersicht über die von Firebase Cloud Messaging (FCM). Es erläutert die verschiedenen Anmeldeinformationen, die zur Identifizierung und Autorisierung von Nachrichten zwischen app-Server und Client-apps verwendet werden. Es veranschaulicht werden, die Registrierung und downstream-messaging-Szenarios, und sie die Schritte zum Registrieren Ihrer app bei FCM von FCM Services beschrieben.


## <a name="related-links"></a>Verwandte Links

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
