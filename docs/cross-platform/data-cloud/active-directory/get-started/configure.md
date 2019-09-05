---
title: Schritt 2 Dienst Zugriff für mobile Anwendungen konfigurieren
description: In diesem Dokument wird beschrieben, wie Sie eine xamarin-Anwendung mit Zugriff auf eine durch Azure Active Directory gesicherte Azure-Anwendung bereitstellen.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 1f0cdec005dc210600977d5c8f5606cff6570989
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290010"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Schritt 2 Dienst Zugriff für mobile Anwendungen konfigurieren

Jedes Mal, wenn eine Ressource, z. b. Webanwendung, Webdienst usw., durch Azure Active Directory gesichert werden muss, muss Sie registriert werden. Alle sicheren Anwendungen oder Dienste können auf der Registerkarte " **Anwendungen** " angezeigt werden. Hier können Sie die Anwendung auswählen, auf die von der mobilen Anwendung aus zugegriffen werden muss, und Zugriff darauf erhalten.

1. Suchen Sie auf der Registerkarte **Konfigurieren** den Abschnitt **Berechtigungen für andere Anwendungen** :

   ![](configure-images/2.1-configure.png "Suchen Sie auf der Registerkarte konfigurieren den Abschnitt Berechtigungen für andere Anwendungen.")

2. Klicken Sie auf die Schaltfläche **Anwendung hinzufügen** . Auf dem nächsten Bildschirm wird die Liste aller Anwendungen angezeigt, die durch Azure Active Directory gesichert sind. Wählen Sie die Anwendungen aus, auf die von der mobilen Anwendung aus zugegriffen werden muss.

   ![](configure-images/2.2-add-application.png "Wählen Sie die Anwendungen aus, auf die von der mobilen Anwendung aus zugegriffen werden muss.")

3. Nachdem Sie die Anwendung ausgewählt haben, wählen Sie die neu hinzugefügte Anwendung im Abschnitt **Berechtigungen für andere Anwendungen** aus, und übermitteln Sie die entsprechenden Rechte.

   ![](configure-images/2.3-permissions.png "Nachdem Sie die Anwendung ausgewählt haben, wählen Sie die neu hinzugefügte Anwendung im Abschnitt Berechtigungen für andere Anwendungen aus, und übermitteln Sie die entsprechenden Rechte.")

4. **Speichern** Sie abschließend die Konfiguration. Diese Dienste sollten jetzt in mobilen Anwendungen verfügbar sein.



## <a name="related-links"></a>Verwandte Links

- [Microsoft nativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
