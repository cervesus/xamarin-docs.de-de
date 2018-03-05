---
title: Firebase Cloud-Messaging
description: "Firebase Cloud Messaging (FCM) ist ein Dienst, der erleichtert das messaging zwischen mobilen apps und serveranwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM, und es wird erläutert, wie Google-Dienste konfigurieren, damit Ihre app FCM verwenden kann."
ms.topic: article
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2017
ms.openlocfilehash: 9f084899f44e0104d0aa2d4b3c0509812bd3fdd2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud-Messaging

_Firebase Cloud Messaging (FCM) ist ein Dienst, der erleichtert das messaging zwischen mobilen apps und serveranwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM, und es wird erläutert, wie Google-Dienste konfigurieren, damit Ihre app FCM verwenden kann._

[![Firebase Cloud Messaging Hero Bild](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png)

Dieses Thema bietet einen allgemeinen Überblick darüber, wie Nachrichten zwischen Ihrer app Xamarin.Android und ein app-Server Firebase Cloud Messaging leitet, und es bietet eine schrittweise Anleitung für den Erwerb von Anmeldeinformationen, damit Ihre app FCM-Dienste verwenden kann.


<a name="overview" />

## <a name="overview"></a>Übersicht

Firebase Cloud Messaging (FCM) ist eine plattformübergreifende-Dienst, der das Senden, routing und Message Queueing von Nachrichten zwischen serveranwendungen und mobilen Clientanwendungen behandelt. FCM ist der Nachfolger zu Google Cloud Messaging (GCM), und es wird auf Google Play-Dienste erstellt.

Wie im folgenden Diagramm dargestellt, fungiert FCM als Mittler zwischen Absender von Nachrichten und Clients. Ein *Client-app* ist eine FCM-fähige Anwendung, die auf einem Gerät ausgeführt wird. Die *Anwendungsserver* (bereitgestellt durch Sie oder Ihr Unternehmen) die FCM-fähigen Servers, mit denen Ihre Client-app über FCM kommuniziert wird. Im Gegensatz zu GCM macht es bei FCM möglich, dass Sie Nachrichten an den Client-apps direkt über die GUI Firebase Konsole Benachrichtigungen senden:

[![FCM befindet sich zwischen dem Client-app und eines app-Servers](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png)

Mit FCM können app-Servern senden Nachrichten und ein einziges Gerät, um eine Gruppe von Geräten oder auf eine Anzahl von Geräten, die an ein Thema abonniert haben. Eine Client-app können FCM downstream-Nachrichten von einem app-Server (z. B. remote Benachrichtigungen zu empfangen) abonniert. Weitere Informationen zu den verschiedenen Typen von Firebase Nachrichten finden Sie unter [zu FCM Nachrichten](https://firebase.google.com/docs/cloud-messaging/concept-options).


<a name="inaction" />

## <a name="firebase-cloud-messaging-in-action"></a>Firebase Cloud Messaging in Aktion

Wenn eine nachfolgende Nachricht an eine Client-app von einem app-Server gesendet wird, handelt es sich bei der app-Server sendet die Nachricht an eine *FCM-Verbindungsserver* gebotenen Google; der FCM Verbindung-Server wiederum leitet die Nachricht an ein Gerät, das ausgeführt wird die Client-app. Nachrichten über HTTP gesendet werden können oder [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging und Anwesenheit-Protokoll). Da Client-apps nicht immer verbunden sind oder ausgeführt wird, handelt es sich bei der FCM Verbindung Server reiht und speichert Nachrichten, sie an Client-apps zu senden, wie sie erneut eine Verbindung herstellen und zur Verfügung stehen. Auf ähnliche Weise FCM wird in die Warteschlange einzureihen upstream Nachrichten über die Client-app an den Anwendungsserver, wenn der Anwendungsserver nicht verfügbar ist. Weitere Informationen zu Servern an FCM Verbindung finden Sie unter [zu Firebase Messaging Cloudserver](https://firebase.google.com/docs/cloud-messaging/server).

FCM verwendet die folgenden Anmeldeinformationen, um app-Servers und der Client-app zu identifizieren und verwendet diese Anmeldeinformationen zum Autorisieren von Transaktionen über FCM Nachrichten:

-   **Absender-ID** &ndash; der *Absender-ID* ist ein eindeutiger numerischer Wert, der bei der Erstellung des Projekts Firebase zugewiesen ist. Die Absender-ID wird verwendet, um jede app-Servers zu identifizieren, die für das Senden von Nachrichten an die Client-app, können. Die Absender-ID ist auch die Projektnummer Ihres; Sie abrufen die Absender-ID aus der Konsole Firebase, wenn Sie Ihr Projekt registrieren. Ein Beispiel für eine Absender-ID ist `496915549731`.

-   **API-Schlüssel** &ndash; der *API-Schlüssel* ermöglicht der app-Server den Zugriff auf Firebase Dienste; FCM verwendet diesen Schlüssel zum Authentifizieren des app-Servers. Diese Anmeldeinformationen werden auch als bezeichnet den *Serverschlüssel* oder *Web API-Schlüssel*. Ein Beispiel für einen API-Schlüssel ist `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   **App-ID** &ndash; die Identität des Client-app (unabhängig von einem beliebigen angegebenen Gerät), der zum Empfangen von Nachrichten von FCM registriert. Ein Beispiel für eine App-ID ist `1:415712510732:android:0e1eb7a661af2460`.

-   **Registrierung Token** &ndash; der *Registrierung Token* (auch bezeichnet als das *Instanz-ID*) ist die Identität FCM Ihrer Client-App auf ein bestimmtes Gerät. Das Token für die Registrierung wird zur Laufzeit generiert &ndash; Ihrer app empfängt eine Registrierung-Token auf, wenn sie sich zuerst bei FCM registriert, während der Ausführung auf einem Gerät. Das Token für die Registrierung wird eine Instanz der Client-app (das betreffende Gerät ausgeführt) zum Empfangen von Nachrichten aus FCM autorisiert.
    Ein Beispiel für ein Token für die Registrierung ist `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (eine sehr lange Zeichenfolge).

[Einstellung von Firebase Cloud Messaging](#setup_fcm) (weiter unten in diesem Handbuch) bietet detaillierte Anweisungen zum Erstellen eines Projekts, und diese Anmeldeinformationen generiert. Beim Erstellen eines neuen Projekts in der [Firebase Konsole](https://console.firebase.google.com/), eine Datei namens **Google-services.json** wird erstellt &ndash; fügen Sie diese Datei dem Projekt Xamarin.Android, wie beschrieben[ Remote-Benachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

In den folgenden Abschnitten wird erläutert, wie diese Anmeldeinformationen verwendet werden, wenn Client-apps mit app-Servern über FCM kommunizieren.


<a name="registration" />

### <a name="registration-with-fcm"></a>Die Registrierung mit FCM

Eine Client-app muss zuerst bei FCM registrieren, bevor messaging stattfinden kann. Die Client-app muss die Registrierung in der folgenden Abbildung gezeigten Schritte:

[![App-Registrierung Schritte Diagramm](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png)

1.  Die Clientanwendung kontaktiert FCM zum Abrufen eines Registrierung-Token, die Absender-ID, die API-Schlüssel und die App-ID an FCM übergeben.

2.  FCM gibt ein Token für die Registrierung für die Clientanwendung zurück.

3.  Die Client-app weiterleitet (optional) den Registrierung-Token an den Anwendungsserver.

Der app-Server speichert den Registrierung-Token für nachfolgende Kommunikation mit dem Client-app. App-Servers kann eine Bestätigung zurück an die Client-app, um anzugeben, dass das Token Registrierung empfangen wurde gesendet. Nachdem diese Handshake durchgeführt wurde, kann die Client-app empfangen von Nachrichten aus (oder Senden von Nachrichten an) dem Anwendungsserver. Die Client-app möglicherweise ein neues Token für die Registrierung, wenn das alte Token gefährdet ist (finden Sie unter [Remote Benachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) ein Beispiel, wie eine app Registrierung token Updates empfängt).

Wenn die Client-app nicht mehr Nachrichten aus dem app-Server empfangen möchte, können sie eine Anforderung an den Anwendungsserver So löschen Sie das Token für die Registrierung senden. Wenn die Client-app von einem Gerät deinstalliert wird, wird FCM erkennt dies und benachrichtigt automatisch auf den Anwendungsserver das Token für die Registrierung zu löschen.


<a name="downstream" />

### <a name="downstream-messaging"></a>Downstream Messaging

Das folgende Diagramm veranschaulicht, wie Firebase Cloud Messaging speichert und nachfolgende Nachrichten weiterleitet:

[![FCM verwendet speichern und Weiterleiten für downstream-messaging](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png)

Wenn der Anwendungsserver an den Client-app eine nachfolgende Nachricht sendet, verwendet diese Funktion als in der Abbildung oben aufgelisteten die folgenden Schritte aus:

1.  Die app-Server sendet die Nachricht an FCM.

2.  Wenn der Client-Gerät nicht verfügbar ist, speichert der FCM-Server die Nachricht in einer Warteschlange für die spätere Übermittlung an. Nachrichten im FCM Speicher für maximal 4 Wochen beibehalten werden (Weitere Informationen finden Sie unter [Festlegen der Lebensdauer einer Nachricht](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Wenn der Client-Gerät verfügbar ist, leitet FCM die Nachricht an den Client-app auf dem Gerät.

4.  Die Clientanwendung empfängt die Nachricht von FCM, verarbeitet, und es dem Benutzer angezeigt. Wenn die Nachricht eine Benachrichtigung remote ist, wird sie z. B. der Benutzer im Infobereich angezeigt.

In diesem messaging-Szenario (, in dem die app-Server sendet eine Nachricht an einen einzelnen Client-app), Nachrichten können bis zu 4kB lang sein.

Ausführliche Informationen zu downstream FCM Nachrichtenempfang unter Android, finden Sie unter [Remote Benachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).


<a name="topic" />

### <a name="topic-messaging"></a>Thema Messaging

*Thema Messaging* ermöglicht es einem app-Server zum Senden einer Nachricht auf mehreren Geräten, die auf ein bestimmtes Thema abonniert haben. Sie können auch verfassen und Thema Nachrichten über die GUI Firebase Konsole Benachrichtigungen senden. FCM verarbeitet das routing und die Übermittlung von themennachrichten an abonnierte Clients. Diese Funktion kann für Nachrichten, z. B. Weather Warnungen, Aktienkurse und Schlagzeilen verwendet werden.

[![Thema messaging-Diagramm](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png)

Die folgenden Schritte werden in Thema messaging verwendet, (nachdem die Client-app ein Token Registrierung wie zuvor erklärt bezieht):

1.  Die Client-app, die durch Senden einer Nachricht abonnieren, FCM an ein Thema abonniert werden.

2.  Der app-Server sendet themennachrichten an FCM, für die Verteilung.

3.  FCM leitet Thema Nachrichten für Clients, die für dieses Thema abonniert haben.

Weitere Informationen zu Firebase Thema messaging, finden Sie in der Google [Thema Messaging unter Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Einrichten von Firebase Cloud-Messaging

Bevor Sie FCM Dienste in Ihrer app verwenden können, müssen Sie erstellen ein neues Projekt (oder ein vorhandenes Projekt importieren) über die [Firebase Konsole](https://console.firebase.google.com/). Verwenden Sie die folgenden Schritte aus, um ein Firebase Cloud Messaging-Projekt für Ihre app zu erstellen:

1.  Melden Sie sich die [Firebase Konsole](https://console.firebase.google.com/) mit Ihrem Google-Konto (d. h., Ihre Adresse Gmail) und auf **neues Projekt erstellen**:

    [![Neues Projekt Schaltfläche "erstellen"](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png)

    Wenn Sie ein vorhandenes Projekt haben, klicken Sie auf **eine Google-Projekt importieren**.

2.  In der **erstellen Sie ein Projekt** Dialogfeld, geben Sie den Namen des Projekts, und klicken Sie auf **Projekt erstellen**. Im folgenden Beispiel ein neues Projekt aufgerufen **XamarinFCM** wird erstellt:

    [![Erstellen Sie ein Dialogfeld "Projekt"](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png)

3.  In der Konsole Firebase **Übersicht**, klicken Sie auf **Firebase hinzufügen, um Ihre Android-app**:

    [![Firebase der Android-app hinzufügen](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png)

4.  Geben Sie im nächsten Bildschirm der Paketname der app ein. In diesem Beispiel ist der Paketname **com.xamarin.fcmexample**. Dieser Wert muss es sich um den Paketnamen von Android-Apps übereinstimmen. Ein app-Spitzname kann auch eingegeben werden, der **App Spitzname** Feld:

    [![FCM Beispiel als app Spitznamen eingeben](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png)

5.  Wenn Ihre app dynamische Links, Einladungen oder Google-Authentifizierung verwendet, müssen Sie auch Ihre Debug Signaturzertifikat eingeben. Weitere Informationen zu Ihrem Signaturzertifikat suchen, finden Sie unter [suchen Sie nach Ihrem Schlüsselspeicher MD5 oder SHA1-Signatur](~/android/deploy-test/signing/keystore-signature.md).
    In diesem Beispiel wird das Signaturzertifikat leer gelassen.

6.  Klicken Sie auf **ADD APP**:

    [![Klicken Sie auf die Schaltfläche "App hinzufügen"](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png)

    Ein Server-API-Schlüssel und eine Client-ID werden automatisch für die app generiert. Diese Informationen wird verpackt in einer **Google-services.json** -Datei, die automatisch heruntergeladen wird, wenn Sie auf **APP hinzufügen**.
    Achten Sie darauf, dass Sie diese Datei an einem sicheren Ort zu speichern.

Ein ausführliches Beispiel zum Hinzufügen von **Google-services.json** zu einem app-Projekt FCM Push-Benachrichtigungen auf Android-Geräten zu erhalten, finden Sie unter [Remote Benachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).


<a name="furtherreading" />

## <a name="for-further-reading"></a>Weitere Informationen

-   Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) bietet einen Überblick über die wichtigsten Funktionen Firebase Cloud Messaging, eine Erläuterung der Funktionsweise und einrichtungsanweisungen.

-   Google [erstellen App senden Serveranforderungen](https://firebase.google.com/docs/cloud-messaging/send-message) wird erläutert, wie zum Senden von Nachrichten mit dem app-Server.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) und [RFC 6121](https://tools.ietf.org/html/rfc6121) erläutern und definieren Sie die erweiterbare Messaging und Anwesenheit-Protokoll (XMPP).


<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel bereitgestellt, eine Übersicht der Firebase Cloud Messaging (FCM). Diese erläutert der verschiedenen Anmeldeinformationen, die verwendet werden, um zu identifizieren und zu autorisieren, messaging zwischen app-Servern und Client-apps. Es veranschaulicht die Registrierung und die downstream-messaging-Szenarien, und sie detailliert die Schritte zum Registrieren Ihrer app mit FCM FCM-Dienste verwendet.


## <a name="related-links"></a>Verwandte Links

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
