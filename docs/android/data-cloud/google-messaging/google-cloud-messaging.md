---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM) ist ein Dienst, der ermöglicht, Nachrichten zwischen mobile apps und Server-Anwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von GCM, und es wird erläutert, wie Google-Dienste konfigurieren, damit Ihre app GCM verwenden kann.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: c2ca567ffcb247622d1b3e8f3e0136c453723b96
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119104"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

_Google Cloud Messaging (GCM) ist ein Dienst, der ermöglicht, Nachrichten zwischen mobile apps und Server-Anwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von GCM, und es wird erläutert, wie Google-Dienste konfigurieren, damit Ihre app GCM verwenden kann._

[![Google Cloud Messaging-logo](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Dieses Thema enthält einen allgemeinen Überblick darüber, wie der Google Cloud Messaging-Nachrichten zwischen Ihrer app und eines app-Servers weitergeleitet, und es enthält ein schrittweises Verfahren zum Abrufen von Anmeldeinformationen, damit Ihre app GCM-Dienste verwenden kann.

> [!NOTE]
> GCM wurde ersetzt wurde, indem [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM-Server und Client-APIs [sind veraltet](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) und wird nicht mehr zur Verfügung so schnell wie vom 11. April 2019.

## <a name="overview"></a>Übersicht

Google Cloud Messaging (GCM) ist ein Dienst, der die senden, routing und Queuing von Nachrichten zwischen Server-Anwendungen und mobile Client-apps behandelt. Ein *Client-app* ist eine GCM-fähige app, die auf einem Gerät ausgeführt wird. Die *Anwendungsserver* (bereitgestellt durch Sie oder Ihr Unternehmen) ist der GCM-fähigen Server, mit denen der Client-app durch GCM kommuniziert:

[![GCM befindet sich zwischen dem Client-app und der app-server](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

Mithilfe von GCM, können app-Server senden Nachrichten an ein einzelnes Gerät, eine Gruppe von Geräten oder eine Anzahl von Geräten, die an ein Thema abonniert haben. Der Client-app können GCM zum Abonnieren von downstream-Nachrichten von einem app-Server (z. B. um remotebenachrichtigungen zu erhalten). Darüber hinaus ermöglicht GCM für Client-apps zum Senden von upstream Nachrichten zurück an den app-Server.

Informationen zum Implementieren eines app-Servers für GCM, finden Sie unter [über GCM-Verbindungsserver](https://developers.google.com/cloud-messaging/server).



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging in Aktion

Wenn downstream-Nachrichten an einen Client-app von einem app-Server gesendet werden, handelt es sich bei der app-Server sendet die Nachricht an eine *GCM-Verbindungsserver*; der GCM-Server-Verbindung wiederum leitet die Nachricht an ein Gerät, das der Client-app ausgeführt wird. Es können Nachrichten über HTTP gesendet werden oder [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging und Vorhandensein-Protokoll). Da der Client-apps nicht immer verbunden sind oder ausgeführt wird, handelt es sich bei den GCM-Verbindung Server in die Warteschlange einreiht und speichert Nachrichten auf Client-apps senden, während sie erneut eine Verbindung herstellen und verfügbar. Auf ähnliche Weise GCM wird in die Warteschlange einzureihen upstream Nachrichten vom Client-app in der app-Server, wenn der app-Server nicht verfügbar ist.

GCM verwendet die folgenden Anmeldeinformationen zum Identifizieren der app-Server und der Client-app, und sie diese Anmeldeinformationen verwendet, um Nachrichtentransaktionen über GCM zu autorisieren:

-   **API-Schlüssel** &ndash; der *API-Schlüssel* ermöglicht Ihrer app-Server den Zugriff auf Google-Diensten GCM verwendet diesen Schlüssel zum Authentifizieren Ihrer app-Servers.
    Bevor Sie den GCM-Dienst verwenden können, benötigen Sie zunächst einen API-Schlüssel aus der [Google Developer Console](https://console.developers.google.com/) durch das Erstellen einer *Projekt*. Der API-Schlüssel sollte gesichert werden; Weitere Informationen zu Ihren API-Schlüssel schützen, finden Sie unter [bewährte Methoden für die Verwendung von API-Schlüssel sicher](https://support.google.com/cloud/answer/6310037?hl=en).

-   **Absender-ID** &ndash; der *Absender-ID* autorisiert den app-Server auf Ihrem Client-app &ndash; es ist eine eindeutige Zahl, die den app-Server identifiziert, die zum Senden von Nachrichten an die Client-app berechtigt ist.
    Die Absender-ID ist auch Ihre Projektnummer. Sie abrufen die Absender-ID aus dem Google-Entwicklerkonsole, wenn Sie Ihr Projekt registrieren.

-   **Registrierungstoken** &ndash; der *Registrierung Token* ist die GCM-Identität Ihrer Client-App auf einem Gerät. Das Registrierungstoken wird zur Laufzeit generiert &ndash; Ihre app empfängt ein Registrierungstoken aus, wenn er sich zunächst bei GCM registriert, während der Ausführung auf einem Gerät. Das Registrierungstoken wird eine Instanz Ihrer Client-App (ausgeführt auf dem bestimmten Gerät) zum Empfangen von Nachrichten von GCM autorisiert.

-   **Anwendungs-ID** &ndash; die Identität Ihrer Client-App (unabhängig von einem beliebigen angegebenen Gerät), der zum Empfangen von Nachrichten von GCM registriert. Unter Android werden die Anwendungs-ID ist der Name des im aufgezeichnet **"androidmanifest.xml"**, z. B. `com.xamarin.gcmexample`.

[Einstellung von Google Cloud Messaging](#settingup) (weiter unten in diesem Leitfaden) enthält ausführliche Anweisungen zum Erstellen eines Projekts, und generieren mit diesen Anmeldeinformationen.

In den folgenden Abschnitten wird erläutert, wie diese Anmeldeinformationen verwendet werden, wenn der Client-apps mit app-Server über GCM kommunizieren.



### <a name="registration-with-gcm"></a>Registrierung bei GCM

Eine Client-app auf einem Gerät installiert, muss zuerst bei GCM registrieren, bevor messaging stattfinden kann. Die Client-app muss die Schritte zur Registrierung in der folgenden Abbildung gezeigten ausführen:

[![Schritte der App-Registrierung](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  Die Client-app kontaktiert GCM zum Abrufen eines registrierungstokens, die Absender-ID an GCM zu übergeben.

2.  GCM gibt ein Registrierungstoken zur Client-app zurück.

3.  Die Client-app leitet das Registrierungstoken für die app-Server weiter.

Der app-Server speichert das Registrierungstoken für die weitere Kommunikation mit dem Client-app. Optional kann der app-Server eine Bestätigung senden, an die Client-app, um anzugeben, dass das Registrierungstoken empfangen wurde. Nachdem dieser Handshake durchgeführt wurde, kann die Client-app empfangen von Nachrichten aus (oder Senden von Nachrichten an) der app-Server.

Wenn die Client-app, die nicht mehr zum Empfangen von Nachrichten aus der app-Server benötigt wird, können sie eine Anforderung an die Anwendungsserver So löschen Sie das Registrierungstoken senden. Wenn die Client-app themanachrichten gesendet werden (weiter unten in diesem Artikel erläutert) empfängt, können sie aus dem Thema wieder abmelden.
Wenn die Client-app von einem Gerät deinstalliert wird, wird von GCM erkennt dies und benachrichtigt automatisch der app-Server So löschen Sie das Registrierungstoken.

Google [Client-Apps registrieren](https://developers.google.com/cloud-messaging/registration) erläutert den Registrierungsprozess noch ausführlicher; es wird erläutert, Aufheben der Registrierung und das Aufheben des Abonnements und der Vorgang zum Aufheben der Registrierung bei der Deinstallation einer Client-app beschrieben.



### <a name="downstream-messaging"></a>Downstream-Messaging

Wenn der app-Server eine downstreamverarbeitung von Nachrichten an die Client-app sendet, folgt es die Schritte im folgenden Diagramm dargestellt:

[![Speichern und Weiterleiten Diagramm Downstream-messaging](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  Der app-Server sendet die Nachricht an GCM.

2.  Wenn das Client-Gerät nicht verfügbar ist, speichert der GCM-Server die Nachricht in einer Warteschlange für die spätere Übertragung.

3.  Wenn der Client-Gerät verfügbar ist, sendet GCM die Nachricht an die Client-app auf dem Gerät.

4.  Die Client-app empfängt die Nachricht von GCM und verarbeitet sie entsprechend. Wenn die Nachricht eine remotebenachrichtigung ist, ist es z. B. dem Benutzer angezeigt.

In diesem messaging-Szenario (wobei der app-Server sendet eine Nachricht an eine einzelne Client-app), Nachrichten können bis zu 4kB lang sein.

Ausführliche Informationen (einschließlich Codebeispiele) zum Empfangen von Nachgeschalteter GCM-Nachrichten auf Android finden Sie unter [Remotebenachrichtigungen](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


#### <a name="topic-messaging"></a>Thema Messaging

*Thema Messaging* ist ein downstream-messaging, in dem der app-Server sendet eine einzelne Nachricht an mehrere Client-app-Geräte, die an ein Thema (z. B. eine Wettervorhersage) abonnieren. Thema-Nachrichten können bis zu 2KB lang sein und Thema messaging unterstützt bis zu eine Million Abonnements pro app. Wenn es sich bei GCM nur für Thema messaging verwendet wird, wird die Client-app ist nicht erforderlich, um ein Registrierungstoken an der app-Server zu senden. Google [implementieren Thema Messaging](https://developers.google.com/cloud-messaging/topic-messaging) wird erläutert, wie Nachrichten von einem app-Server auf mehreren Geräten zu senden, die ein bestimmtes Thema abonnieren.



#### <a name="group-messaging"></a>Gruppe Messaging

*Gruppieren von Messaging* ist ein downstream-messaging, in dem der app-Server sendet eine einzelne Nachricht an mehrere Client-app-Geräte, die zu einer Gruppe (z. B. eine Gruppe von Geräten, die auf einen einzelnen Benutzer gehören) gehören. Gruppieren von Nachrichten können bis zu 2KB lang für iOS-Geräte sein und bis zu 4KB lang für Android-Geräte. Eine Gruppe ist auf maximal 20 Elemente beschränkt. Google [Device Messaging verwenden Gruppe](https://developers.google.com/cloud-messaging/notifications) wird erläutert, wie app-Server senden können eine einzelne Nachricht an mehrere Client-app-Instanzen, die auf Geräten, die zu einer Gruppe gehören.


### <a name="upstream-messaging"></a>Upstream-Messaging

Wenn der Client-app mit einem Server verbunden, die unterstützt [XMPP](https://developers.google.com/cloud-messaging/ccs), es kann Nachrichten an den app-Server senden, wie im folgenden Diagramm dargestellt:

[![Upstream-messaging-Diagramm](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  Die Client-app sendet eine Nachricht an den Server der GCM XMPP-Verbindung.

2.  Wenn der app-Server getrennt ist, speichert der GCM-Server die Nachricht in einer Warteschlange für das Weiterleiten von später noch mal.

3.  Wenn der app-Server erneut eine Verbindung hergestellt ist, leitet GCM die Nachricht an den app-Server weiter.

4.  Der app-Server wird die Meldung zum Überprüfen der Identität der Client-app analysiert und dann sendet er eine "Ack" an GCM zum Empfang der Nachricht zu bestätigen.

5.  Der app-Server verarbeitet die Nachricht.

Google [Upstream Nachrichten](https://developers.google.com/cloud-messaging/ccs#upstream) erläutert die Struktur der JSON-codierte Nachrichten aus, und senden sie an der app-Server, die Googles XMPP-basierten Cloud-Verbindungsserver ausgeführt werden.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Einrichten von Google Cloud Messaging

Bevor Sie GCM-Dienste in Ihrer app verwenden können, müssen Sie zuerst die Anmeldeinformationen für den Zugriff auf Google GCM-Server abrufen. Die folgenden Abschnitte beschreiben die erforderlichen Schritte zum Abschließen dieses Vorgangs an:



### <a name="enable-google-services-for-your-app"></a>Aktivieren von Google-Dienste für Ihre App

1.  Melden Sie sich bei der [Google Developers Console](https://developers.google.com/mobile/add?platform=android) mit Ihrem Google-Konto (d.h., Ihr Gmail-Adresse), und erstellen Sie ein neues Projekt. Wenn Sie ein vorhandenes Projekt haben, wählen Sie das Projekt, das GCM-aktiviert werden soll. Im folgenden Beispiel ist ein neues Projekt namens **XamarinGCM** wird erstellt:

    [![Erstellen des Projekts XamarinGCM](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  Geben Sie anschließend auf den Paketnamen für Ihre app (in diesem Beispiel ist der Paketname ist **com.xamarin.gcmexample**), und klicken Sie auf **fortsetzen auswählen und Konfigurieren von Diensten**:

    [![Geben Sie den Paketnamen](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Beachten Sie, dass diese Paketname auch die Anwendungs-ID für Ihre app ist.

3.  Die **auswählen und Konfigurieren von Diensten** Abschnitt listet die Google-Dienste, die Sie zu Ihrer app hinzufügen können. Klicken Sie auf **Cloudmessaging**:

    [![Wählen Sie die Cloud Messaging](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  Klicken Sie anschließend **GOOGLE CLOUD MESSAGING aktivieren**:

    [![Aktivieren von Google Cloud Messaging](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  Ein **-Server-API-Schlüssel** und **Absender-ID** für Ihre app generiert werden. Notieren Sie diese Werte, und klicken Sie auf **schließen**:

    [![Server-API-Schlüssel und Absender-ID angezeigt](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    Schützen den API-Schlüssel &ndash; es ist nicht für die öffentliche Verwendung vorgesehen. Wenn der API-Schlüssel gefährdet ist, konnte von nicht autorisierte Servern Nachrichten für Clientanwendungen veröffentlicht werden.
    [Bewährte Methoden für die Verwendung von API-Schlüssel sicher](https://support.google.com/cloud/answer/6310037?hl=en) bietet nützliche Richtlinien für den Schutz von Ihren API-Schlüssel.



### <a name="view-your-project-settings"></a>Zeigen Sie Ihre Projekteinstellungen ein.

Sie können die projekteinstellungen jederzeit anzeigen, melden Sie sich die [Google Cloud Console](https://console.cloud.google.com/) , und wählen Ihr Projekt. Sie können z. B. Anzeigen der **Absender-ID** dazu Ihr Projekt in das Pulldown-Menü am oberen Rand der Seite (in diesem Beispiel heißt das Projekt **XamarinGCM**). Die Absender-ID ist die Projektnummer, wie im folgenden Screenshot gezeigt (die Absender-ID ist **9349932736**):

[![Die Absender-ID anzeigen](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Anzeigen der **API-Schlüssel**, klicken Sie auf **API Manager** , und klicken Sie dann auf **Anmeldeinformationen**:

[![Anzeigen des API-Schlüssels](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>Weitere Informationen

-   Google [Client-Apps registrieren](https://developers.google.com/cloud-messaging/registration) beschreibt den Prozess im Detail, Registrierung und stellt Informationen zum Konfigurieren der automatische Wiederholung und zum Registrierungsstatus synchron zu halten.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) und [RFC 6121](https://tools.ietf.org/html/rfc6121) erläutern und dem Extensible Messaging and Presence Protocol (XMPP) zu definieren.



## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine Übersicht über die von Google Cloud Messaging (GCM). Es erläutert die verschiedenen Anmeldeinformationen, die zur Identifizierung und Autorisierung von Nachrichten zwischen app-Server und Client-apps verwendet werden. Es veranschaulicht die häufigsten Szenarien für messaging, und sie detailliert die Schritte zum Registrieren Ihrer app bei GCM GCM-Dienste verwenden.


## <a name="related-links"></a>Verwandte Links

- [Cloudmessaging](https://developers.google.com/cloud-messaging/)
