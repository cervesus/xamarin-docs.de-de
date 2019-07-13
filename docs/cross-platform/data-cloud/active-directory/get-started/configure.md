---
title: Schritt 2 Konfigurieren des Service-Zugriffs für Mobile Anwendung
description: In diesem Dokument wird beschrieben, wie zu einer Xamarin-Anwendung mit Zugriff auf eine Azure-Anwendung durch Azure Active Directory gesichert wird.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e0015316b7be3462982ee0959862250c0c27dc74
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864659"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Schritt 2 Konfigurieren des Service-Zugriffs für Mobile Anwendung

Wenn alle Ressourcen, z. B. die Webanwendung, Webdienst usw. muss durch Azure Active Directory gesichert wird, muss sie registriert werden. Alle Anwendungen oder Dienste finden Sie unter **Anwendungen** Registerkarte. Hier können Sie die Anwendung auswählen, die von der mobilen Anwendung darauf zugegriffen werden und Zugriff darauf gewähren muss.

1. Auf der **konfigurieren** Registerkarte **Berechtigungen für andere Anwendungen** Abschnitt:

   ![](configure-images/2.1-configure.png "Suchen Sie auf der Registerkarte \"konfigurieren\" Berechtigungen für andere Anwendungen im Abschnitt")

2. Klicken Sie auf **Anwendung hinzufügen** Schaltfläche. Auf dem nächsten Bildschirm Popupfenster sehen Sie die Liste aller Anwendungen die von Azure Active Directory gesichert werden. Wählen Sie die Anwendungen, die aus der mobilen Anwendung zugegriffen werden muss.

   ![](configure-images/2.2-add-application.png "Wählen Sie die Anwendungen, die von der mobilen Anwendung zugegriffen werden muss")

3. Wählen Sie nach dem Auswählen der Anwendung erneut die neu hinzugefügten Anwendung im **Berechtigungen für andere Anwendungen** aus, und geben Sie die entsprechende Berechtigungen.

   ![](configure-images/2.3-permissions.png "Nach dem Auswählen der Anwendung wieder wählen Sie die neu hinzugefügten Anwendung in Berechtigungen für andere Anwendungen im Abschnitt und geben Sie die entsprechenden Rechte")

4. Zum Schluss **speichern** der Konfiguration. Diese Dienste sollte jetzt in mobilen Anwendungen verfügbar sein!



## <a name="related-links"></a>Verwandte Links

- [Microsoft NativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
