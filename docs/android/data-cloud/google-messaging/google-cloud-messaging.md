---
title: Google Cloud Messaging
description: "Google Cloud Messaging (GCM) ist ein Dienst, der erleichtert das messaging zwischen mobilen apps und serveranwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von GCM, und es wird erläutert, wie Google-Dienste konfigurieren, damit Ihre app GCM verwenden kann."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 225662fe64c92b77af3e75cbee865561118692a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

_Google Cloud Messaging (GCM) ist ein Dienst, der erleichtert das messaging zwischen mobilen apps und serveranwendungen. Dieser Artikel bietet einen Überblick über die Funktionsweise von GCM, und es wird erläutert, wie Google-Dienste konfigurieren, damit Ihre app GCM verwenden kann._

[![Google Cloud Messaging-logo](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png)

Dieses Thema bietet einen allgemeinen Überblick darüber, wie Google Cloud Messaging Nachrichten zwischen Ihrer app und ein app-Server weiterleitet, und es bietet eine schrittweise Anleitung für den Erwerb von Anmeldeinformationen, sodass Ihre app GCM-Dienste verwenden kann.

<a name="overview" />

## <a name="overview"></a>Übersicht

Google Cloud Messaging (GCM) ist ein Dienst, der das Senden, routing und Message Queueing von Nachrichten zwischen serveranwendungen und mobilen Clientanwendungen behandelt. Ein *Client-app* ist eine GCM-fähige Anwendung, die auf einem Gerät ausgeführt wird. Die *Anwendungsserver* (bereitgestellt durch Sie oder Ihr Unternehmen) die GCM-fähigen Servers, mit denen Ihre Client-app durch GCM kommuniziert wird:

[![GCM befindet sich zwischen dem Client-app und app-Servers](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png)

Mithilfe von GCM, können app-Servern senden Nachrichten an ein einzelnes Gerät, eine Gruppe von Geräten oder eine Reihe von Geräten, die an ein Thema abonniert haben. Ihre Client-app können GCM downstream-Nachrichten von einem app-Server (z. B. remote Benachrichtigungen zu empfangen) abonniert. Darüber hinaus vereinfacht GCM für Client-apps zum Senden von upstream Nachrichten zurück an den Anwendungsserver.

Informationen zum Implementieren eines app-Servers für GCM finden Sie unter [zu GCM-Verbindungsserver](https://developers.google.com/cloud-messaging/server).


<a name="inaction" />

## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging in Aktion

Wenn nachfolgende Nachrichten an einen Client-app von einem app-Server gesendet werden, handelt es sich bei der app-Server sendet die Nachricht an einen *GCM-Verbindungsserver*; die GCM-Verbindungsservers wiederum leitet die Nachricht an ein Gerät, das Ihre Client-app ausgeführt wird. Nachrichten über HTTP gesendet werden können oder [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging und Anwesenheit-Protokoll). Da Client-apps nicht immer verbunden sind oder ausgeführt wird, handelt es sich bei den GCM Verbindung Server reiht und speichert Nachrichten senden an die Client-apps, wie sie erneut eine Verbindung herstellen und zur Verfügung stehen. Auf ähnliche Weise GCM wird in die Warteschlange einzureihen upstream Nachrichten über die Client-app an den Anwendungsserver, wenn der Anwendungsserver nicht verfügbar ist.

GCM verwendet die folgenden Anmeldeinformationen auf um dem app-Server und Client-app zu identifizieren und verwendet diese Anmeldeinformationen Nachrichtentransaktionen durch GCM autorisieren:

-   **API-Schlüssel** &ndash; der *API-Schlüssel* ermöglicht Ihrer app-Server den Zugriff auf Google-Dienste; GCM verwendet diesen Schlüssel zum Authentifizieren Ihrer app-Servers.
    Bevor Sie den GCM-Dienst verwenden können, benötigen Sie zunächst einen API-Schlüssel aus der [Google-Entwicklerkonsole](https://console.developers.google.com/) durch das Erstellen einer *Projekt*. API-Schlüssel sollte gesichert werden; Weitere Informationen zu Ihren API-Schlüssel zu schützen, finden Sie unter [bewährte Methoden für die sichere Verwendung von API-Schlüssel](https://support.google.com/cloud/answer/6310037?hl=en).

-   **Absender-ID** &ndash; der *Absender-ID* autorisiert den Anwendungsserver auf Ihrem Client-app &ndash; ist eine eindeutige Nummer, die app-Servers identifiziert, die zum Senden von Nachrichten an Ihre Client-app berechtigt ist.
    Die Absender-ID ist auch die Projektnummer Ihres; Sie abrufen die Absender-ID aus dem Google-Entwicklerkonsole, wenn Sie Ihr Projekt registrieren.

-   **Registrierung Token** &ndash; der *Registrierung Token* ist die GCM-Identität der Client-app auf einem bestimmten Gerät. Das Token für die Registrierung wird zur Laufzeit generiert &ndash; Ihrer app empfängt eine Registrierung-Token auf, wenn sie sich zuerst bei GCM registriert, während der Ausführung auf einem Gerät. Das Token für die Registrierung wird eine Instanz der Client-app (das betreffende Gerät ausgeführt) zum Empfangen von Nachrichten von GCM autorisiert.

-   **ID der Anwendung** &ndash; die Identität des Client-app (unabhängig von einem beliebigen angegebenen Gerät), der für den Empfang von Nachrichten von GCM registriert. Auf Android-Geräten wird die Anwendungs-ID den Paketnamen in aufgezeichnet **AndroidManifest.xml**, wie z. B. `com.xamarin.gcmexample`.

[Einstellung von Google Cloud Messaging](#settingup) (weiter unten in diesem Handbuch) bietet detaillierte Anweisungen zum Erstellen eines Projekts, und diese Anmeldeinformationen generiert.

In den folgenden Abschnitten wird erläutert, wie diese Anmeldeinformationen verwendet werden, wenn Client-apps mit app-Servern über GCM kommunizieren.


<a name="registration" />

### <a name="registration-with-gcm"></a>Die Registrierung mit GCM

Eine Client-app auf einem Gerät installiert, muss zuerst bei GCM registrieren, bevor messaging stattfinden kann. Die Client-app muss die Registrierung in der folgenden Abbildung gezeigten Schritte:

[![App-Registrierungsschritte](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png)

1.  Die Clientanwendung kontaktiert GCM zum Abrufen eines Registrierung-Token, die Absender-ID an GCM übergeben.

2.  GCM gibt ein Token für die Registrierung für die Clientanwendung zurück.

3.  Die Client-app weiterleitet, die Registrierung Token an den Anwendungsserver.

Der app-Server speichert den Registrierung-Token für nachfolgende Kommunikation mit dem Client-app. Optional kann der app-Server eine Bestätigung senden, zurück an den Client-app, um anzugeben, dass das Token Registrierung empfangen wurde. Nachdem diese Handshake durchgeführt wurde, kann die Client-app empfangen von Nachrichten aus (oder Senden von Nachrichten an) dem Anwendungsserver.

Wenn die Client-app nicht mehr Nachrichten aus dem app-Server empfangen möchte, können sie eine Anforderung an den Anwendungsserver So löschen Sie das Token für die Registrierung senden. Wenn die Client-app themennachrichten (weiter unten in diesem Artikel erläutert) empfängt, können sie aus dem Thema kündigen.
Wenn die Client-app von einem Gerät deinstalliert wird, wird GCM erkennt dies und benachrichtigt automatisch auf den Anwendungsserver das Token für die Registrierung zu löschen.

Google [Client-Apps registrieren](https://developers.google.com/cloud-messaging/registration) wird erläutert, die während der Registrierung im Detail; es wird erläutert, die Registrierung und das Aufheben des Abonnements und den Prozess der Aufhebung der Registrierung bei der Deinstallation von einer Client-app beschrieben.


<a name="downstream" />

### <a name="downstream-messaging"></a>Downstream Messaging

Wenn der Anwendungsserver an den Client-app eine nachfolgende Nachricht sendet, befolgt er die Schritte im folgenden Diagramm dargestellt:

[![Speichern und Weiterleiten Diagramm Downstream-messaging](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png)

1.  Die app-Server sendet die Nachricht an GCM.

2.  Wenn der Client-Gerät nicht verfügbar ist, speichert die GCM-Server die Nachricht in einer Warteschlange für die spätere Übermittlung an.

3.  Wenn der Client-Gerät verfügbar ist, sendet GCM die Nachricht an den Client-app auf diesem Gerät an.

4.  Die Clientanwendung empfängt die Nachricht von GCM und entsprechend behandelt wird. Wenn die Nachricht eine Benachrichtigung remote ist, wird sie z. B. dem Benutzer angezeigt.

In diesem messaging-Szenario (, in dem die app-Server sendet eine Nachricht an einen einzelnen Client-app), Nachrichten können bis zu 4kB lang sein.

Ausführliche Informationen (einschließlich Codebeispiele) zum Empfangen von downstreamer GCM-Nachrichten auf Android-Geräten finden Sie unter [Remote Benachrichtigungen](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


<a name="topic" />

#### <a name="topic-messaging"></a>Thema Messaging

*Thema Messaging* ist eine Art von downstream-messaging, in dem die app-Server sendet eine einzelne Nachricht auf mehrere Client-app-Geräte, die an ein Thema (z. B. einem Wetterbericht) zu abonnieren. Thema Nachrichten können bis zu 2KB lang sein und Thema messaging unterstützt bis zu einer Million Abonnements pro app. Wenn GCM nur für messaging Thema verwendet wird, wird die Client-app ist nicht erforderlich, ein Token für die Registrierung mit dem app-Server zu senden. Google [implementieren Thema Messaging](https://developers.google.com/cloud-messaging/topic-messaging) wird erläutert, wie Nachrichten von einem app-Server auf mehreren Geräten senden, die ein bestimmtes Thema abonniert haben.


<a name="group" />

#### <a name="group-messaging"></a>Messaging-Gruppe

*Gruppieren von Messaging* ist eine Art von downstream-messaging, in dem die app-Server sendet eine einzelne Nachricht für mehrere Client-app-Geräte, die zu einer Gruppe (z. B. eine Gruppe von Geräten, die auf einen einzelnen Benutzer gehören) gehören. Gruppieren von Nachrichten kann bis zu 2KB lang für iOS-Geräte und bis zu 4KB lang für Android-Geräte. Eine Gruppe ist auf maximal 20 Mitglieder beschränkt. Google [Device Gruppe Messaging](https://developers.google.com/cloud-messaging/notifications) wird erläutert, wie app-Servern senden können eine einzelne Nachricht an mehrere Instanzen des Client-app auf Geräten, die zu einer Gruppe gehören.


<a name="upstream" />

### <a name="upstream-messaging"></a>Upstream-Messaging

Wenn Ihre Client-app auf einem Server verbindet, das unterstützt [XMPP](https://developers.google.com/cloud-messaging/ccs), es kann Nachrichten zurück an den Anwendungsserver senden, wie im folgenden Diagramm dargestellt:

[![Upstream-messaging-Diagramm](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png)

1.  Die Clientanwendung sendet eine Nachricht an den Server der GCM XMPP-Verbindung.

2.  Wenn der Anwendungsserver getrennt ist, speichert die GCM-Server die Nachricht in einer Warteschlange für die Weiterleitung von einem späteren Zeitpunkt.

3.  Bei der app-Server erneut eine Verbindung hergestellt ist, leitet die Nachricht an den Anwendungsserver von GCM weiter.

4.  App-Servers analysiert die Meldung zum Überprüfen der Identität des Client-app, und er sendet eine "Bestätigung" an GCM zum Empfang der Nachricht zu bestätigen.

5.  App-Servers wird die Nachricht verarbeitet.

Google [Upstream Nachrichten](https://developers.google.com/cloud-messaging/ccs#upstream) erläutert die Struktur der JSON-codierte Nachrichten und senden sie an app-Servern, auf denen Server Cloud Google XMPP-basierte Verbindung ausgeführt.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Setting Up Google Cloud Messaging

Bevor Sie GCM-Dienste in Ihrer app verwenden können, müssen Sie zunächst Anmeldeinformationen für den Zugriff auf Google GCM-Server erwerben. Die folgenden Abschnitte beschreiben die erforderlichen Schritte zum Abschließen dieses Vorgangs an:


<a name="googleservices" />

### <a name="enable-google-services-for-your-app"></a>Google-Dienste für Ihre App aktivieren

1.  Melden Sie sich die [Google-Entwicklerkonsole](https://developers.google.com/mobile/add?platform=android) mit Ihrem Google-Konto (d. h., Ihre Adresse Gmail), und erstellen Sie ein neues Projekt. Wenn Sie ein vorhandenes Projekt haben, wählen Sie das Projekt, das GCM aktiviert werden soll. Im folgenden Beispiel ein neues Projekt aufgerufen **XamarinGCM** wird erstellt:

    [![XamarinGCM-Projekt erstellen](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png)

2.  Als Nächstes geben Sie den Paketnamen für Ihre app (in diesem Beispiel ist der Paketname **com.xamarin.gcmexample**), und klicken Sie auf **weiter, um auswählen und Konfigurieren der Dienste**:

    [![Der Paketname eingeben](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png)

    Beachten Sie, dass diese Paketname auch die Anwendungs-ID für Ihre app ist.

3.  Die **auswählen und Konfigurieren der Dienste** Abschnitt listet die Google-Dienste, die Sie zu Ihrer app hinzufügen können. Klicken Sie auf **Cloud Messaging**:

    [![Wählen Sie die Cloud Messaging](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png)

4.  Klicken Sie anschließend auf **ENABLE GOOGLE CLOUD MESSAGING**:

    [![Google Cloud Messaging zu aktivieren](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png)

5.  Ein **Server API-Schlüssel** und ein **Absender-ID** für Ihre app generiert werden. Zeichnen Sie diese Werte, und klicken Sie auf **schließen**:

    [![Server-API-Schlüssel und die Absender-ID angezeigt](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png)

    Schützen den API-Schlüssel &ndash; es ist nicht für die öffentliche Verwendung vorgesehen. Wenn die API-Schlüssel gefährdet ist, konnte nicht autorisierte Server Nachrichten für Clientanwendungen veröffentlicht.
    [Bewährte Methoden für die sichere Verwendung von API-Schlüssel](https://support.google.com/cloud/answer/6310037?hl=en) bietet nützliche Richtlinien für den Schutz von Ihren API-Schlüssel.


<a name="projectsettings" />

### <a name="view-your-project-settings"></a>Zeigen Sie die Projekteinstellungen an.

Sie können die projekteinstellungen jederzeit anzeigen, indem Sie bei der Anmeldung bei der [Konsole der Google Cloud](https://console.cloud.google.com/) , und wählen das Projekt. Sie können z. B. Anzeigen der **Absender-ID** dazu Ihr Projekt in der Pull-Menü am oberen Rand der Seite "(in diesem Beispiel heißt das Projekt **XamarinGCM**). Die Absender-ID ist die Nummer des Projekts in diesem Screenshot gezeigten (die Absender-ID ist **9349932736**):

[![Anzeigen der Absender-ID](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png)

Anzeigen der **API-Schlüssel**, klicken Sie auf **API Manager** , und klicken Sie dann auf **Anmeldeinformationen**:

[![API-Schlüssel anzeigen](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png)


<a name="furtherreading" />

## <a name="for-further-reading"></a>Weitere Informationen

-   Google [Client-Apps registrieren](https://developers.google.com/cloud-messaging/registration) beschreibt den Prozess im Detail, Registrierung und es stellt Informationen über die automatische Wiederholung konfigurieren und Synchronisieren von den Registrierungsstatus.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) und [RFC 6121](https://tools.ietf.org/html/rfc6121) erläutern und definieren Sie die erweiterbare Messaging und Anwesenheit-Protokoll (XMPP).


<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel bereitgestellt, eine Übersicht über die von Google Cloud Messaging (GCM). Diese erläutert der verschiedenen Anmeldeinformationen, die verwendet werden, um zu identifizieren und zu autorisieren, messaging zwischen app-Servern und Client-apps. Es veranschaulicht die am häufigsten verwendeten messaging-Szenarien, und sie detailliert die Schritte zum Registrieren Ihrer app mit GCM GCM-Dienste verwendet.


## <a name="related-links"></a>Verwandte Links

- [Cloud-Messaging](https://developers.google.com/cloud-messaging/)
