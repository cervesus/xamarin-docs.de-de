---
title: Schritt 2. Dienst Zugriff für mobile Anwendungen konfigurieren
description: In diesem Dokument wird beschrieben, wie Sie eine xamarin-Anwendung mit Zugriff auf eine durch Azure Active Directory gesicherte Azure-Anwendung bereitstellen.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: eeac7b0b70b2f11304a374de7522f28d4bcad6c6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016669"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Schritt 2. Dienst Zugriff für mobile Anwendungen konfigurieren

Jedes Mal, wenn eine Ressource, z. b. Webanwendung, Webdienst usw., durch Azure Active Directory gesichert werden muss, muss Sie registriert werden. Alle sicheren Anwendungen oder Dienste können auf der Registerkarte " **Anwendungen** " angezeigt werden. Hier können Sie die Anwendung auswählen, auf die von der mobilen Anwendung aus zugegriffen werden muss, und Zugriff darauf erhalten.

1. Suchen Sie auf der Registerkarte **Konfigurieren** den Abschnitt **Berechtigungen für andere Anwendungen** :

   ![](configure-images/2.1-configure.png "On the Configure tab, locate permissions to other applications section")

2. Klicken Sie auf die Schaltfläche **Anwendung hinzufügen** . Auf dem nächsten Bildschirm wird die Liste aller Anwendungen angezeigt, die durch Azure Active Directory gesichert sind. Wählen Sie die Anwendungen aus, auf die von der mobilen Anwendung aus zugegriffen werden muss.

   ![](configure-images/2.2-add-application.png "Select the applications that needs to be accessed from the mobile application")

3. Nachdem Sie die Anwendung ausgewählt haben, wählen Sie die neu hinzugefügte Anwendung im Abschnitt **Berechtigungen für andere Anwendungen** aus, und übermitteln Sie die entsprechenden Rechte.

   ![](configure-images/2.3-permissions.png "After selecting the application, once again select the newly-added application in permissions to other   applications section and give appropriate rights")

4. **Speichern** Sie abschließend die Konfiguration. Diese Dienste sollten jetzt in mobilen Anwendungen verfügbar sein.

## <a name="related-links"></a>Verwandte Links

- [Microsoft nativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
