---
title: Google Messaging
description: Dieser Abschnitt enthält Anleitungen, die beschreiben, wie xamarin. Android-Apps mithilfe von Google Messaging Services implementiert werden.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: bfcc526d1787caede4361030f5bf6718a59672b1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754394"
---
# <a name="google-messaging"></a>Google Messaging

_Dieser Abschnitt enthält Anleitungen, die beschreiben, wie xamarin. Android-Apps mithilfe von Google Messaging Services implementiert werden._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Firebase Cloud Messaging](firebase-cloud-messaging.md)

Firebase Cloud Messaging (SCM) ist ein Dienst, der das Messaging zwischen mobilen apps und Server Anwendungen ermöglicht. FCM ist der Nachfolger von Google für Google Cloud Messaging. Dieser Artikel bietet eine Übersicht über die Funktionsweise von "ficm" und bietet eine schrittweise Anleitung zum Abrufen von Anmelde Informationen, sodass Ihre APP die Verwendung von-Dienst-Diensten unterstützt.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Remote Benachrichtigungen mit Firebase Cloud Messaging](remote-notifications-with-fcm.md)

Diese exemplarische Vorgehensweise enthält eine schrittweise Erläuterung zur Verwendung von Firebase Cloud Messaging zum Implementieren von Remote Benachrichtigungen (auch als Pushbenachrichtigungen bezeichnet) in einer xamarin. Android-Anwendung. Es wird veranschaulicht, wie die verschiedenen Klassen implementiert werden, die für die Kommunikation mit Firebase Cloud Messaging (FCM) benötigt werden, und es werden Beispiele für die Konfiguration des Android-Manifests für den Zugriff auf den FCM und das downstreammessaging mithilfe von Firebase veranschaulicht. TS.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Google Cloud Messaging](google-cloud-messaging.md)

Dieser Abschnitt enthält eine allgemeine Übersicht darüber, wie Google Cloud Messaging (GCM) Nachrichten zwischen Ihrer APP und einem App-Server weiterleitet, und bietet ein schrittweises Verfahren zum Abrufen von Anmelde Informationen, sodass Ihre APP GCM-Dienste verwenden kann. (Beachten Sie, dass GCM von einem anderen Computer als "f" ersetzt wurde.)

> [!NOTE]
> GCM wurde von [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (SCM) abgelöst.
> Die GCM-Server-und Client-APIs [sind veraltet](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) und werden nicht mehr ab dem 11. April 2019 verfügbar sein.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Remote Benachrichtigungen mit Google Cloud Messaging](remote-notifications-with-gcm.md)

In diesem Abschnitt wird Schritt für Schritt erläutert, wie Remote Benachrichtigungen in xamarin. Android mithilfe von Google Cloud Messaging implementiert werden.
Es werden die verschiedenen Komponenten erläutert, die genutzt werden müssen, um Google Cloud Messaging in einer Android-Anwendung zu aktivieren.
