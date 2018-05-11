---
title: Schritt 2 Konfigurieren des Service-Zugriffs für Mobile Anwendung
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fd6436a664fde7a610b29bba31d0baf35cf88dad
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Schritt 2 Konfigurieren des Service-Zugriffs für Mobile Anwendung

Wenn Ressource, z. B. die Webanwendung, Webdienst usw. von Azure Active Directory gesichert werden muss, muss sie registriert werden. Die sichere Anwendungen oder Dienste können unter betrachtet werden **Anwendungen** Registerkarte. Hier können Sie die Anwendung auswählen, der von der mobilen Anwendung darauf zugegriffen werden und für den Zugriff darauf muss.

1. Auf der **konfigurieren** Registerkarte **Berechtigungen für andere Anwendungen** Abschnitt:

  ![](configure-images/2.1-configure.png "Suchen Sie auf der Registerkarte "konfigurieren" "Berechtigungen für andere Anwendungen"")

2.  Klicken Sie auf **Anwendung hinzufügen** Schaltfläche. Auf dem nächsten Bildschirm Popup sollte Liste aller Anwendungen angezeigt werden, die von Azure Active Directory geschützt sind. Wählen Sie die Anwendungen, die von der mobilen Anwendung zugegriffen werden muss.

  ![](configure-images/2.2-add-application.png "Wählen Sie die Anwendungen, die von der mobilen Anwendung zugegriffen werden muss")

3. Nach der Auswahl der Anwendung, und wählen Sie erneut die neu hinzugefügte Anwendung in **Berechtigungen für andere Anwendungen** Abschnitt, und geben Sie die erforderlichen Rechte verfügt.

  ![](configure-images/2.3-permissions.png "Nach der Auswahl der Anwendung, erneut wählen Sie die Anwendung neu hinzugefügten in "Berechtigungen für andere Anwendungen" und geben Sie die entsprechenden Rechte")

4. Schließlich **speichern** der Konfiguration. Diese Dienste sollten jetzt in mobilen Anwendungen zur Verfügung!



## <a name="related-links"></a>Verwandte Links

- [Microsoft NativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
