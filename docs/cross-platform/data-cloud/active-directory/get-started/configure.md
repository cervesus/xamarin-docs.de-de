---
title: 'Schritt 2: Dienst Zugriff für mobile Anwendungen konfigurieren'
description: In diesem Dokument wird beschrieben, wie Sie eine xamarin-Anwendung mit Zugriff auf eine durch Azure Active Directory gesicherte Azure-Anwendung bereitstellen.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 2e1a96b96ea8738e162a9c49a5f7c6927bd2b0d3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931754"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Schritt 2: Dienst Zugriff für mobile Anwendungen konfigurieren

Jedes Mal, wenn eine Ressource, z. b. Webanwendung, Webdienst usw., durch Azure Active Directory gesichert werden muss, muss Sie registriert werden. Alle sicheren Anwendungen oder Dienste können auf der Registerkarte " **Anwendungen** " angezeigt werden. Hier können Sie die Anwendung auswählen, auf die von der mobilen Anwendung aus zugegriffen werden muss, und Zugriff darauf erhalten.

1. Suchen Sie auf der Registerkarte **Konfigurieren** den Abschnitt **Berechtigungen für andere Anwendungen** :

   ![Suchen Sie auf der Registerkarte konfigurieren den Abschnitt Berechtigungen für andere Anwendungen.](configure-images/2.1-configure.png)

2. Klicken Sie auf die Schaltfläche **Anwendung hinzufügen** . Auf dem nächsten Bildschirm wird die Liste aller Anwendungen angezeigt, die durch Azure Active Directory gesichert sind. Wählen Sie die Anwendungen aus, auf die von der mobilen Anwendung aus zugegriffen werden muss.

   ![Wählen Sie die Anwendungen aus, auf die von der mobilen Anwendung aus zugegriffen werden muss.](configure-images/2.2-add-application.png)

3. Nachdem Sie die Anwendung ausgewählt haben, wählen Sie die neu hinzugefügte Anwendung im Abschnitt **Berechtigungen für andere Anwendungen** aus, und übermitteln Sie die entsprechenden Rechte.

   ![Nachdem Sie die Anwendung ausgewählt haben, wählen Sie die neu hinzugefügte Anwendung im Abschnitt Berechtigungen für andere Anwendungen aus, und übermitteln Sie die entsprechenden Rechte.](configure-images/2.3-permissions.png)

4. **Speichern** Sie abschließend die Konfiguration. Diese Dienste sollten jetzt in mobilen Anwendungen verfügbar sein.

## <a name="related-links"></a>Ähnliche Themen

- [Microsoft nativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
