---
title: Firebase Cloud Messaging
description: Firebase Cloud Messaging (SCM) ist ein Dienst, der das Messaging zwischen mobilen apps und Server Anwendungen ermöglicht. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM und erläutert, wie Sie Google-Dienste so konfigurieren, dass Ihre APP FCM verwenden kann.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/31/2018
ms.openlocfilehash: 4cf32bae208efa67acbb08f2e4525e4571b14b16
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723781"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (SCM) ist ein Dienst, der das Messaging zwischen mobilen apps und Server Anwendungen ermöglicht. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM und erläutert, wie Sie Google-Dienste so konfigurieren, dass Ihre APP FCM verwenden kann._

[![Firebase Cloud Messaging Hero-Image](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Dieses Thema enthält eine allgemeine Übersicht über die Weiterleitung von Nachrichten zwischen Ihrer Xamarin Android-App und einem App-Server durch Firebase Cloud Messaging. Außerdem erhalten Sie eine Schritt-für-Schritt-Anleitung zum Abrufen von Anmeldeinformationen, damit Ihre App die Dienstprinzipalnamen verwenden kann.

## <a name="overview"></a>Übersicht über

Firebase Cloud Messaging (FCM) ist ein plattformübergreifender Dienst, der das Senden, Weiterleiten und Einreihen von Nachrichten zwischen Serveanwendungen und mobilen Client-Apps verarbeitet. FCM ist der Nachfolger von Google Cloud Messaging (GCM) und basiert auf Google Play Services.

Wie im folgenden Diagramm dargestellt, fungiert dieser als Vermittler zwischen Nachrichtensendern und-Clients. Bei einer *Client-App* handelt es sich um eine auf einem Gerät ausgeführte App. Der *App-Server* (der von Ihnen oder Ihrem Unternehmen bereitgestellt wird) ist der Server mit aktiviertem Server, mit dem Ihre Client-App über FCM kommuniziert. Anders als GCM ermöglicht FCM es Ihnen, Nachrichten direkt über die Konsolenbenachrichtigungs-GUI von Firebase an Client-Apps zu senden:

[![-Datei-App zwischen der Client-App und einem App-Server](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

Mithilfe von FCM können App-Server Nachrichten an ein einzelnes Gerät, an eine Gruppe von Geräten oder an eine Reihe von Geräten senden, die ein Thema abonniert haben. Eine Client-App kann FCM zum Abonnieren von downstreamnachrichten von einem App-Server (z. b. zum Empfangen von Remote Benachrichtigungen) verwenden. Weitere Informationen zu den verschiedenen Typen von Firebase-Meldungen finden Sie unter [Informationen zu Nachrichten](https://firebase.google.com/docs/cloud-messaging/concept-options).

## <a name="fcm-in-action"></a>Firebase Cloud Messaging in Aktion

Wenn eine Downstreamnachricht von einem App-Server an eine Client-App gesendet wird, sendet der App-Server die Nachricht an einen *FCM-Verbindungs Server*, der von Google bereitgestellt wird. Der FCM-Verbindungsserver leitet die Nachricht seinerseits an ein Gerät weiter, auf dem die Client-App ausgeführt wird. Nachrichten können über HTTP oder [XMPP](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) (erweiterbares Messaging- und Anwesenheitsprotokoll) gesendet werden. Da Client-Apps nicht immer verbunden sind oder ausgeführt werden, werden vom FCM-Verbindungsserver Nachrichten in die Warteschlange eingereiht und gespeichert, und sie werden an Client-apps gesendet, wenn sie wieder eine Verbindung herstellen. Ebenso fügt FCM Upstreamnachrichten aus der Client-App an den App-Server in die Warteschlange ein, wenn der App-Server nicht verfügbar ist. Weitere Informationen zu den FCM-Verbindungsservern finden Sie unter [Informationen zu Firebase Cloud Messaging Server](https://firebase.google.com/docs/cloud-messaging/server).

FCM verwendet die folgenden Anmelde Informationen, um den App-Server und die Client-App zu identifizieren, und verwendet diese Anmelde Informationen, um Nachrichten Transaktionen über den FCM zu autorisieren:

- <a name="fcm-in-action-sender-id"></a>**Absender-ID** &ndash; die *Absender-ID* ist ein eindeutiger numerischer Wert, der beim Erstellen des Firebase-Projekts zugewiesen wird. Die Absender-ID wird verwendet, um jeden App-Server zu identifizieren, der Nachrichten an die Client-App senden kann. Die Absender-ID ist auch Ihre Projekt Nummer. Sie erhalten die Absender-ID bei der Registrierung des Projekts über die Firebase-Konsole. Ein Beispiel für eine Absender-ID ist `496915549731`.

- <a name="fcm-in-action-api-key"></a>**API-Schlüssel** &ndash; der *API-Schlüssel* gewährt dem App-Server Zugriff auf Firebase-Dienste. FCM verwendet diesen Schlüssel zum Authentifizieren des App-Servers. Diese Anmelde Informationen werden auch als *Server Schlüssel* oder Web-API- *Schlüssel*bezeichnet. Ein Beispiel für einen API-Schlüssel ist `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

- <a name="fcm-in-action-app-id"></a>**App-ID** &ndash; die Identität ihrer Client-app (unabhängig von einem bestimmten Gerät), die für den Empfang von Nachrichten von FCM registriert wird. Ein Beispiel für eine APP-ID ist `1:415712510732:android:0e1eb7a661af2460`.

- <a name="fcm-in-action-registration-token"></a>Das **Registrierungs Token &ndash; das** *Registrierungs Token* (auch als Instanz- *ID*bezeichnet) ist die FCM-Identität ihrer Client-App auf einem bestimmten Gerät. Das Registrierungs Token wird zur Laufzeit generiert, &ndash; Ihre APP ein Registrierungs Token erhält, wenn Sie sich beim ersten Mal bei der Ausführung auf einem Gerät bei f cm registriert. Das Registrierungs Token autorisiert eine Instanz Ihrer Client-app (auf diesem bestimmten Gerät ausgeführt) zum Empfangen von Nachrichten von FCM.
    Ein Beispiel für ein Registrierungs Token ist `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (eine sehr lange Zeichenfolge).

[Einrichten von Firebase Cloud Messaging](#setup_fcm) (später in diesem Handbuch) enthält ausführliche Anweisungen zum Erstellen eines Projekts und zum Erstellen dieser Anmelde Informationen. Wenn Sie in der [Firebase-Konsole](https://console.firebase.google.com/)ein neues Projekt erstellen, wird eine Anmelde Informationsdatei mit dem Namen " **Google-Services. JSON** " erstellt, &ndash; diese Datei dem xamarin. Android-Projekt hinzuzufügen, wie in [Remote Benachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)erläutert.

In den folgenden Abschnitten wird erläutert, wie diese Anmelde Informationen verwendet werden, wenn die Kommunikation zwischen Client-apps und App-Servern über den-

<a name="registration" />

### <a name="registration-with-fcm"></a>Registrierung bei "ficm"

Eine Client-App muss zuerst bei der Verwendung von "ficm" registriert werden, bevor das Messaging erfolgt. Die Client-App muss die im folgenden Diagramm gezeigten Registrierungsschritte ausführen:

[Diagramm der ![App-Registrierungsschritte](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1. Die Client-App kontaktiert FCM zum Abrufen eines Registrierungs Tokens und übergibt die Absender-ID, den API-Schlüssel und die APP-ID an den FCM.

2. FCM gibt ein Registrierungs Token an die Client-App zurück.

3. Die Client-app (optional) leitet das Registrierungs Token an den App-Server weiter.

Der App-Server speichert das Registrierungs Token für die nachfolgende Kommunikation mit der Client-App zwischen. Der App-Server kann eine Bestätigung zurück an die Client-App senden, um anzugeben, dass das Registrierungs Token empfangen wurde. Nachdem dieser Handshake stattfindet, kann die Client-App Nachrichten von dem App-Server empfangen (oder Nachrichten an ihn senden). Die Client-App erhält möglicherweise ein neues Registrierungs Token, wenn das alte Token kompromittiert wurde (Weitere Informationen finden Sie unter [Remote Benachrichtigungen mit](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) dem Code, um zu erfahren, wie eine APP registrierungstokenaktualisierungen empfängt).

Wenn die Client-App keine Nachrichten mehr vom App-Server empfangen will, kann Sie eine Anforderung an den App-Server senden, um das Registrierungs Token zu löschen. Wenn die Client-App auf einem Gerät deinstalliert wird, erkennt FCM dies und benachrichtigt den App-Server automatisch, das Registrierungs Token zu löschen.

### <a name="downstream-messaging"></a>Downstream-Messaging

Im folgenden Diagramm wird veranschaulicht, wie Firebase Cloud Messaging downstreamnachrichten speichert und weiterleitet:

[![-Datei "Store" und "Weiterleiten" für downstreammessaging](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Wenn der App-Server eine downstreamnachricht an die Client-App sendet, werden die folgenden Schritte verwendet, wie im obigen Diagramm aufgelistet:

1. Der App-Server sendet die Nachricht an den FCM.

2. Wenn das Client Gerät nicht verfügbar ist, speichert der BCM-Server die Nachricht zur späteren Übertragung in einer Warteschlange. Nachrichten werden für einen maximalen Wert von 4 Wochen im Speicher für die Speicher Beibehaltung aufbewahrt (Weitere Informationen finden Sie unter [Festlegen der Lebensdauer einer Nachricht](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3. Wenn das Client Gerät verfügbar ist, leitet FCM die Nachricht an die Client-App auf diesem Gerät weiter.

4. Die Client-App empfängt die Nachricht von FCM, verarbeitet sie und zeigt Sie dem Benutzer an. Wenn die Nachricht z. b. eine Remote Benachrichtigung ist, wird Sie dem Benutzer im Benachrichtigungsbereich angezeigt.

In diesem Messaging Szenario (in dem der App-Server eine Nachricht an eine einzelne Client-App sendet) können Nachrichten eine Länge von bis zu 4 KB aufweisen.

Ausführliche Informationen zum Empfangen von nachgeschalteten Nachrichten mit einem SMS-Gerät unter Android finden Sie unter [Remote Benachrichtigungen mit dem](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)Befehl "f

### <a name="topic-messaging"></a>Thema Messaging

*Thema Messaging* ermöglicht einem App-Server, eine Nachricht an mehrere Geräte zu senden, die ein bestimmtes Thema abonniert haben. Sie können auch Themen Meldungen über die Firebase-Konsolen Benachrichtigungs-GUI verfassen und senden. FCM verarbeitet das Routing und die Übermittlung von Themen Meldungen an abonnierte Clients. Diese Funktion kann für Nachrichten verwendet werden, wie z. b. Wetterwarnungen, Aktienkurse und Nachrichten Nachrichten.

[Messaging Diagramm für ![Thema](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

Die folgenden Schritte werden im Thema Messaging verwendet (nachdem die Client-App ein Registrierungs Token abgerufen hat, wie bereits erläutert):

1. Die Client-App abonniert ein Thema, indem eine abonnierende Nachricht an FCM gesendet wird.

2. Der App-Server sendet Themen Meldungen zur Verteilung an FCM.

3. FCM leitet Themen Meldungen an Clients weiter, die dieses Thema abonniert haben.

Weitere Informationen zu Firebase Topic Messaging finden Sie im Google- [Thema Messaging unter Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).

<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Einrichten von Firebase Cloud Messaging

Bevor Sie in Ihrer APP-Dienst-Dienste verwenden können, müssen Sie über die [Firebase-Konsole](https://console.firebase.google.com/)ein neues Projekt erstellen (oder ein vorhandenes Projekt importieren). Verwenden Sie die folgenden Schritte, um ein Firebase Cloud Messaging-Projekt für Ihre APP zu erstellen:

1. Melden Sie sich mit Ihrem Google-Konto (d.h. ihrer gmail-Adresse) bei der [Firebase-Konsole](https://console.firebase.google.com/) an, und klicken Sie auf **Neues Projekt erstellen**:

    [![Schaltfläche "Neues Projekt erstellen"](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Wenn Sie über ein vorhandenes Projekt verfügen, klicken Sie auf **Google-Projekt importieren**.

2. Geben Sie im Dialogfeld **Projekt erstellen** den Namen des Projekts ein, und klicken Sie auf **Projekt erstellen**. Im folgenden Beispiel wird ein neues Projekt mit dem Namen **xamarinfcm** erstellt:

    [![Projekt Dialogfeld erstellen](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3. Klicken Sie in der **Übersicht über**die Firebase-Konsole **auf Firebase zu Ihrer Android-App hinzufügen**:

    [![Hinzufügen von Firebase zu Ihrer Android-App](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4. Geben Sie im nächsten Bildschirm den Paketnamen Ihrer APP ein. In diesem Beispiel lautet der Paketname **com. xamarin. fcmexample**. Dieser Wert muss mit dem Paketnamen Ihrer Android-App identisch sein. Ein App-Spitzname kann auch im Feld für die **App-Spitzname** eingegeben werden:

    [![eingeben von "ficm example" als App-Spitzname](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5. Wenn Ihre APP dynamische Verknüpfungen, Einladungen oder Google auth verwendet, müssen Sie auch Ihr debugsignaturzertifikat eingeben. Weitere Informationen zur Suche nach Ihrem Signaturzertifikat finden Sie unter [Ermitteln der MD5-oder SHA1-Signatur ihres Keystores](~/android/deploy-test/signing/keystore-signature.md).
    In diesem Beispiel wird das Signaturzertifikat leer gelassen.

6. Klicken Sie auf **app hinzufügen**:

    [![klicken auf die Schaltfläche app hinzufügen](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Für die APP werden automatisch ein Server-API-Schlüssel und eine Client-ID generiert. Diese Informationen werden in eine **Google-Services. JSON** -Datei gepackt, die automatisch heruntergeladen wird, wenn Sie auf **app hinzufügen**klicken.
    Stellen Sie sicher, dass Sie diese Datei an einem sicheren Ort speichern.

Ein ausführliches Beispiel zum Hinzufügen von " **Google-Services. JSON** " zu einem App-Projekt zum Empfangen von FCM-pushbenachrichtigungsnachrichten unter Android finden Sie unter [Remote Benachrichtigungen mit FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

## <a name="for-further-reading"></a>Weitere Informationen

- Das [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) von Google bietet einen Überblick über die wichtigsten Funktionen von Firebase Cloud Messaging, eine Erläuterung der Funktionsweise sowie Anweisungen zum Setup.

- Die Erstellungs Anforderungen von Google [Build App Server](https://firebase.google.com/docs/cloud-messaging/send-message) erläutern, wie Nachrichten mit dem App-Server gesendet werden.

- [RFC 6120](https://tools.ietf.org/html/rfc6120) und [RFC 6121](https://tools.ietf.org/html/rfc6121) erläutern und definieren das Extensible Messaging and Presence Protocol (XMPP).

- [Informationen zu](https://firebase.google.com/docs/cloud-messaging/concept-options) den unterschiedlichen Arten von Nachrichten, die mit Firebase Cloud Messaging gesendet werden können, finden Sie unter.

## <a name="summary"></a>Summary

Dieser Artikel bietet eine Übersicht über Firebase Cloud Messaging (SCM). Es wurden die verschiedenen Anmelde Informationen erläutert, die zum Identifizieren und Autorisieren von Messaging zwischen App-Servern und Client-Apps verwendet werden. Es wurden die Szenarien für Registrierung und downstreammessaging veranschaulicht, und es werden die Schritte zum Registrieren Ihrer APP bei FCM für die Verwendung von FCM-Diensten erläutert.

## <a name="related-links"></a>Verwandte Themen

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
