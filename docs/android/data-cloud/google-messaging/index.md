---
title: Google Messaging
description: Dieser Abschnitt enthält Anleitungen, die beschreiben, wie Sie Xamarin.Android-apps mit Google-messaging-Dienste zu implementieren.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: 8c2e3705f0f867b6993d0bdcb22f6672c853498e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61016738"
---
# <a name="google-messaging"></a>Google Messaging

_Dieser Abschnitt enthält Anleitungen, die beschreiben, wie Sie Xamarin.Android-apps mit Google-messaging-Dienste zu implementieren._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Firebase Cloud Messaging](firebase-cloud-messaging.md)

Firebase Cloud Messaging (FCM) ist ein Dienst, der ermöglicht, Nachrichten zwischen mobile apps und Server-Anwendungen. FCM wird Googles-Nachfolger von Google Cloud Messaging. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM, und es enthält ein schrittweises Verfahren zum Abrufen von Anmeldeinformationen, damit Ihre app FCM-Dienste verwenden kann.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Remotebenachrichtigungen mit Firebase Cloud Messaging](remote-notifications-with-fcm.md)

Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Firebase Cloud Messaging verwenden, um remotebenachrichtigungen (auch als "Pushbenachrichtigungen" bezeichnet) implementieren in einer Xamarin.Android-Anwendung. Es wird veranschaulicht, wie die verschiedenen Klassen implementieren, die erforderlich sind, für die Kommunikation mit Firebase Cloud Messaging (FCM) und enthält Beispiele zum Konfigurieren der Android-Manifest für den Zugriff auf FCM veranschaulicht downstream-messaging über die Firebase Konsole.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Google Cloud Messaging](google-cloud-messaging.md)

Dieser Abschnitt enthält einen allgemeinen Überblick darüber, wie Nachrichten zwischen Ihrer app und eines app-Servers von Google Cloud Messaging (GCM) weitergeleitet, und es enthält ein schrittweises Verfahren zum Abrufen von Anmeldeinformationen, damit Ihre app GCM-Dienste verwenden kann. (Beachten Sie, dass es sich bei GCM durch FCM ersetzt wurde.)

> [!NOTE]
> GCM wurde ersetzt wurde, indem [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM-Server und Client-APIs [sind veraltet](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) und wird nicht mehr zur Verfügung so schnell wie vom 11. April 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Remotebenachrichtigungen mit Google Cloud Messaging](remote-notifications-with-gcm.md)

Dieser Abschnitt enthält eine schrittweise Erklärung der Implementierung von remotebenachrichtigungen in Xamarin.Android mit Google Cloud Messaging.
Es wird erläutert, die verschiedenen Komponenten, die zum Aktivieren von Google Cloud Messaging in einer Android-Anwendung genutzt werden müssen.


