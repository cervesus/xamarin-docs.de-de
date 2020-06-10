---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM) ist ein Dienst, der das Messaging zwischen mobilen apps und Server Anwendungen ermöglicht. Dieser Artikel bietet einen Überblick über die Funktionsweise von GCM und erläutert, wie Sie Google-Dienste so konfigurieren, dass Ihre APP GCM verwenden kann.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/02/2019
ms.openlocfilehash: d5231d57e84d57788f318c8ae04e592da57a0f5d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84566652"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

> [!WARNING]
> Google hat GCM seit dem 10. April 2018 als veraltet markiert. Die folgenden Dokumente und Beispiel Projekte werden möglicherweise nicht mehr verwaltet. Der GCM-Server und die Client-APIs von Google werden ab dem 29. Mai 2019 entfernt. Google empfiehlt, GCM-apps zu Firebase Cloud Messaging (FCM) zu migrieren. Weitere Informationen zur Veraltung und Migration von GCM finden Sie unter als [veraltet markierte Cloud-Messaging von Google](https://developers.google.com/cloud-messaging/).
>
> Informationen zur Verwendung von Firebase Cloud Messaging mit xamarin finden Sie unter [Firebase Cloud Messaging](firebase-cloud-messaging.md).

_Google Cloud Messaging (GCM) ist ein Dienst, der das Messaging zwischen mobilen apps und Server Anwendungen ermöglicht. Dieser Artikel bietet einen Überblick über die Funktionsweise von GCM und erläutert, wie Sie Google-Dienste so konfigurieren, dass Ihre APP GCM verwenden kann._

[![Google Cloud Messaging Logo](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Dieses Thema bietet eine allgemeine Übersicht darüber, wie Google Cloud Messaging Nachrichten zwischen Ihrer APP und einem App-Server weiterleitet, und bietet eine schrittweise Anleitung zum Abrufen von Anmelde Informationen, sodass Ihre APP GCM-Dienste verwenden kann.

## <a name="overview"></a>Übersicht

Google Cloud Messaging (GCM) ist ein Dienst, der das senden, das Routing und das Queuing von Nachrichten zwischen Server Anwendungen und mobilen Client-apps verarbeitet. Eine *Client-App* ist eine GCM-aktivierte APP, die auf einem Gerät ausgeführt wird. Der *App-Server* (der von Ihnen oder Ihrem Unternehmen bereitgestellt wird) ist der GCM-fähige Server, mit dem Ihre Client-App über GCM kommuniziert:

[![GCM befindet sich zwischen der Client-App und dem App-Server.](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

Mithilfe von GCM können App-Server Nachrichten an ein einzelnes Gerät, eine Gruppe von Geräten oder eine Reihe von Geräten senden, die ein Thema abonniert haben. Ihre Client-App kann GCM zum Abonnieren von downstreamnachrichten von einem App-Server (z. b. zum Empfangen von Remote Benachrichtigungen) verwenden. Außerdem ermöglicht GCM es Client-apps, upstreamnachrichten an den App-Server zurückzusenden.

## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging in Aktion

Wenn downstreamnachrichten von einem App-Server an eine Client-App gesendet werden, sendet der App-Server die Nachricht an einen *GCM-Verbindungs Server*. der GCM-Verbindungs Server leitet die Nachricht ihrerseits an ein Gerät weiter, auf dem Ihre Client-app ausgeführt wird. Nachrichten können über HTTP oder [XMPP](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) (erweiterbares Messaging- und Anwesenheitsprotokoll) gesendet werden. Da Client-apps nicht immer verbunden sind oder ausgeführt werden, stellt der GCM-Verbindungs Server Nachrichten in die Warteschlange Ebenso fügt GCM upstreamnachrichten aus der Client-App an den App-Server in die Warteschlange ein, wenn der App-Server nicht verfügbar ist.

GCM verwendet die folgenden Anmelde Informationen, um den App-Server und Ihre Client-App zu identifizieren, und verwendet diese Anmelde Informationen, um Nachrichten Transaktionen über GCM zu autorisieren:

- **API-Schlüssel** &ndash; Der *API-Schlüssel* ermöglicht dem App-Server den Zugriff auf Google-Dienste. GCM verwendet diesen Schlüssel zum Authentifizieren des App-Servers.
    Bevor Sie den GCM-Dienst verwenden können, müssen Sie zunächst einen API-Schlüssel aus der [Google Developer Console](https://console.developers.google.com/) abrufen, indem Sie ein *Projekt*erstellen. Der API-Schlüssel sollte sicher aufbewahrt werden. Weitere Informationen zum Schutz Ihres API-Schlüssels finden Sie unter [bewährte Methoden für die sichere Verwendung von API-Schlüsseln](https://support.google.com/cloud/answer/6310037?hl=en).

- **Absender-ID** &ndash; Die *Absender-ID* autorisiert den App-Server für Ihre Client-App &ndash; . es handelt sich um eine eindeutige Nummer, mit der der App-Server identifiziert wird, der Nachrichten an Ihre Client-App senden darf.
    Die Absender-ID ist auch Ihre Projekt Nummer. Sie erhalten die Absender-ID von der Google Developers Console, wenn Sie Ihr Projekt registrieren.

- **Registrierungs Token** &ndash; Das *Registrierungs Token* ist die GCM-Identität ihrer Client-App auf einem bestimmten Gerät. Das Registrierungs Token wird zur Laufzeit generiert, &ndash; Wenn Ihre APP ein Registrierungs Token erhält, wenn es bei der Ausführung auf einem Gerät zum ersten Mal bei GCM registriert wird. Das Registrierungs Token autorisiert eine Instanz Ihrer Client-app (auf diesem bestimmten Gerät ausgeführt) zum Empfangen von Nachrichten von GCM.

- **Anwendungs-ID** &ndash; Die Identität ihrer Client-app (unabhängig von einem bestimmten Gerät), die für den Empfang von Nachrichten von GCM registriert wird. Unter Android ist die Anwendungs-ID der Paketname, der in " **androidmanifest. XML**" aufgezeichnet wird, z `com.xamarin.gcmexample` . b..

Das [Einrichten von Google Cloud Messaging](#settingup) (später in diesem Handbuch) enthält ausführliche Anweisungen zum Erstellen eines Projekts und zum Erstellen dieser Anmelde Informationen.

In den folgenden Abschnitten wird erläutert, wie diese Anmelde Informationen verwendet werden, wenn Client-Apps über GCM mit App-Servern kommunizieren.

### <a name="registration-with-gcm"></a>Registrierung bei GCM

Eine Client-App, die auf einem Gerät installiert ist, muss zuerst bei GCM registriert werden, bevor das Messaging erfolgen kann. Die Client-App muss die im folgenden Diagramm gezeigten Registrierungsschritte ausführen:

[![Schritte zur APP-Registrierung](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1. Die Client-App kontaktiert GCM zum Abrufen eines Registrierungs Tokens und übergibt die Absender-ID an GCM.

2. GCM gibt ein Registrierungs Token an die Client-App zurück.

3. Die Client-App leitet das Registrierungs Token an den App-Server weiter.

Der App-Server speichert das Registrierungs Token für die nachfolgende Kommunikation mit der Client-App zwischen. Optional kann der App-Server eine Bestätigung zurück an die Client-App senden, um anzugeben, dass das Registrierungs Token empfangen wurde. Nachdem dieser Handshake stattfindet, kann die Client-App Nachrichten von dem App-Server empfangen (oder Nachrichten an ihn senden).

Wenn die Client-App keine Nachrichten mehr vom App-Server empfangen will, kann Sie eine Anforderung an den App-Server senden, um das Registrierungs Token zu löschen. Wenn die Client-App Themen Meldungen empfängt (wird weiter unten in diesem Artikel erläutert), kann Sie das Abonnement kündigen.
Wenn die Client-App auf einem Gerät deinstalliert wird, erkennt GCM dies und benachrichtigt den App-Server automatisch, das Registrierungs Token zu löschen.

### <a name="downstream-messaging"></a>Downstream-Messaging

Wenn der App-Server eine downstreamnachricht an die Client-App sendet, werden die in der folgenden Abbildung dargestellten Schritte ausgeführt:

[![Diagramm zum Speichern und Weiterleiten von downstreammessaging](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1. Die Nachricht wird vom App-Server an GCM gesendet.

2. Wenn das Client Gerät nicht verfügbar ist, speichert der GCM-Server die Nachricht zur späteren Übertragung in einer Warteschlange.

3. Wenn das Client Gerät verfügbar ist, sendet GCM die Nachricht an die Client-App auf diesem Gerät.

4. Die Client-App empfängt die Nachricht von GCM und verarbeitet sie entsprechend. Wenn die Nachricht z. b. eine Remote Benachrichtigung ist, wird Sie dem Benutzer angezeigt.

In diesem Messaging Szenario (in dem der App-Server eine Nachricht an eine einzelne Client-App sendet) können Nachrichten eine Länge von bis zu 4 KB aufweisen.

Ausführliche Informationen (einschließlich Codebeispielen) zum Empfangen von nachgeschalteten GCM-Nachrichten unter Android finden Sie unter [Remote Benachrichtigungen](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).

#### <a name="topic-messaging"></a>Thema Messaging

*Thema Messaging* ist ein Typ von downstreammessaging, bei dem der App-Server eine einzelne Nachricht an mehrere Client-App-Geräte sendet, die ein Thema abonnieren (z. b. eine Wettervorhersage). Themen Nachrichten können bis zu 2 KB lang sein, und Topic Messaging unterstützt bis zu 1 Million Abonnements pro app. Wenn GCM nur für Themen Nachrichten verwendet wird, ist es nicht erforderlich, dass die Client-App ein Registrierungs Token an den App-Server sendet.

#### <a name="group-messaging"></a>Gruppieren von Nachrichten

Das *Gruppieren* von Nachrichten ist eine Art von downstreammessaging, bei dem der App-Server eine einzelne Nachricht an mehrere Client-App-Geräte sendet, die zu einer Gruppe gehören (z. b. eine Gruppe von Geräten, die zu einem einzelnen Benutzer gehören). Gruppen Nachrichten können für IOS-Geräte eine Länge von bis zu 2 KB und für Android-Geräte eine Länge von bis zu 4 KB aufweisen. Eine Gruppe ist auf maximal 20 Mitglieder beschränkt.

### <a name="upstream-messaging"></a>Upstream-Messaging

Wenn Ihre Client-App eine Verbindung mit einem Server herstellt, der [XMPP](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref)unterstützt, können Sie Nachrichten zurück an den App-Server senden, wie in der folgenden Abbildung dargestellt:

[![Upstream-Messaging-Diagramm](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1. Die Client-App sendet eine Nachricht an den GCM XMPP-Verbindungs Server.

2. Wenn der App-Server getrennt wird, speichert der GCM-Server die Nachricht in einer Warteschlange für die spätere Weiterleitung.

3. Beim erneuten Verbinden des App-Servers leitet GCM die Nachricht an den App-Server weiter.

4. Der App-Server analysiert die Nachricht, um die Identität der Client-App zu überprüfen, und sendet dann eine "ACK"-Bestätigung an GCM, um die Nachrichten Bestätigung zu bestätigen.

5. Der App-Server verarbeitet die Nachricht.

Die [upstreamnachrichten](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref#upstream) von Google erläutern, wie JSON-codierte Nachrichten strukturiert und an App-Server gesendet werden, die den XMPP-basierten cloudverbindungsserver von Google ausführen.

<a name="settingup"></a>

## <a name="setting-up-google-cloud-messaging"></a>Einrichten von Google Cloud Messaging

Bevor Sie GCM-Dienste in Ihrer APP verwenden können, müssen Sie zuerst Anmelde Informationen für den Zugriff auf die GCM-Server von Google erwerben. In den folgenden Abschnitten werden die Schritte beschrieben, die zum Durchführen dieses Vorgangs erforderlich sind:

### <a name="enable-google-services-for-your-app"></a>Aktivieren von Google-Diensten für Ihre APP

1. Melden Sie sich mit Ihrem Google-Konto (d.h. ihrer gmail-Adresse) bei der [Google Developers-Konsole](https://developers.google.com/mobile/add?platform=android) an, und erstellen Sie ein neues Projekt. Wenn Sie über ein vorhandenes Projekt verfügen, wählen Sie das Projekt aus, für das Sie GCM aktiviert werden möchten. Im folgenden Beispiel wird ein neues Projekt mit dem Namen **xamaringcm** erstellt:

    [![Xamaringcm-Projekt wird erstellt](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2. Geben Sie als nächstes den Paketnamen für Ihre APP ein (in diesem Beispiel lautet der Paketname **com. xamarin. gcmexample**), und klicken Sie **auf Weiter, um Dienste auszuwählen und zu konfigurieren**:

    [![Eingeben des Paket namens](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Beachten Sie, dass dieser Paketname auch die Anwendungs-ID für Ihre APP ist.

3. Im Abschnitt **Dienste auswählen und konfigurieren** werden die Google-Dienste aufgelistet, die Sie Ihrer APP hinzufügen können. Klicken Sie auf **Cloud Messaging**:

    [![Cloud-Messaging auswählen](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4. Klicken Sie anschließend auf **Google Cloud Messaging aktivieren**:

    [![Aktivieren von Google Cloud Messaging](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5. Für Ihre APP werden ein **Server-API-Schlüssel** und eine Absender- **ID** generiert. Notieren Sie sich diese Werte, und klicken Sie auf **Schließen**:

    [![Server-API-Schlüssel und Absender-ID angezeigt](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    Schützen Sie den API-Schlüssel, der &ndash; nicht für die öffentliche Verwendung vorgesehen ist. Wenn der API-Schlüssel kompromittiert ist, können nicht autorisierte Server Nachrichten an Client Anwendungen veröffentlichen.
    [Bewährte Methoden für die sichere Verwendung von API-Schlüsseln](https://support.google.com/cloud/answer/6310037?hl=en) bieten nützliche Richtlinien für den Schutz Ihres API-Schlüssels.

### <a name="view-your-project-settings"></a>Anzeigen der Projekteinstellungen

Sie können Ihre Projekteinstellungen jederzeit anzeigen, indem Sie sich bei der [Google Cloud Console](https://console.cloud.google.com/) anmelden und Ihr Projekt auswählen. Beispielsweise können Sie die Absender- **ID** anzeigen, indem Sie Ihr Projekt im Pulldownmenü oben auf der Seite auswählen (in diesem Beispiel wird das Projekt als **xamaringcm**bezeichnet). Die Absender-ID ist die Projekt Nummer, wie in diesem Screenshot gezeigt (die Absender-ID lautet **9349932736**):

[![Anzeigen der Absender-ID](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Um den **API-Schlüssel**anzuzeigen, klicken Sie auf **API-Manager** und dann auf **Anmelde**Informationen:

[![Anzeigen des API-Schlüssels](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)

## <a name="for-further-reading"></a>Weitere Informationen

- [RFC 6120](https://tools.ietf.org/html/rfc6120) und [RFC 6121](https://tools.ietf.org/html/rfc6121) erläutern und definieren das Extensible Messaging and Presence Protocol (XMPP).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel bietet eine Übersicht über Google Cloud Messaging (GCM). Es wurden die verschiedenen Anmelde Informationen erläutert, die zum Identifizieren und Autorisieren von Messaging zwischen App-Servern und Client-Apps verwendet werden. Es wurden die häufigsten Messaging Szenarien veranschaulicht, und es werden die Schritte zum Registrieren Ihrer APP bei GCM für die Verwendung von GCM-Diensten beschrieben.

## <a name="related-links"></a>Verwandte Links

- [Cloud-Messaging](https://developers.google.com/cloud-messaging/)
