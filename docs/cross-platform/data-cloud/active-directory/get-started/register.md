---
title: Schritt 1. Registrieren einer APP für die Verwendung Azure Active Directory
description: In diesem Dokument wird beschrieben, wie Sie eine Azure-Anwendung mit Azure Active Directory registrieren, damit Mobile Clients sicher darauf zugreifen können.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 2811297e24aac726ae3c91e5354675499d4b9ddb
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290077"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Schritt 1. Registrieren einer APP für die Verwendung Azure Active Directory

1. Navigieren Sie zu [windowsazure.com](https://manage.windowsazure.com) , und melden Sie sich mit Ihrem Microsoft-Konto oder Organisations Konto im Azure-Portal an. Wenn Sie über kein Azure-Abonnement verfügen, können Sie eine Testversion von [Azure.com](https://www.azure.com) erhalten.

2. Wechseln Sie nach der Anmeldung zum Abschnitt **Active Directory** (1), und wählen Sie das Verzeichnis aus, in dem Sie die Anwendung registrieren möchten (2).

   [![](register-images/01.-active-directory-in-azure-portal-sml.jpg "aus, und wählen Sie das Verzeichnis aus, in dem Sie die Anwendung registrieren möchten.")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Klicken Sie auf **Hinzufügen** , um eine neue Anwendung zu erstellen, und wählen Sie dann **eine von meiner Organisation entwickelte Anwendung**

   [![](register-images/02.-add-new-application-sml.jpg "Eine von meinem Unternehmen entwickelte Anwendung hinzufügen")](register-images/02.-add-new-application.jpg#lightbox)

4. Legen Sie auf dem nächsten Bildschirm Ihrer APP einen Namen (z. b. XAM-DEMO).
   Stellen Sie sicher, dass Sie die **Native Client-Anwendung** als Anwendungstyp auswählen.

   ![](register-images/03.-app-name.jpg "Stellen Sie sicher, dass Sie die Native Client-Anwendung als Anwendungstyp auswählen.")

5. Geben Sie auf dem letzten Bildschirm einen **Umleitungs-URI* an, der für Ihre Anwendung eindeutig ist, da er nach Abschluss der Authentifizierung an diesen URI zurückgegeben wird.

   ![](register-images/04.-app-redirect.jpg "Geben Sie auf dem letzten Bildschirm einen Umleitungs-URI an, der für Ihre Anwendung eindeutig ist, da Sie nach Abschluss der Authentifizierung an diesen URI zurückgegeben wird.")

6. Nachdem die App erstellt wurde, navigieren Sie zur Registerkarte **Konfigurieren** . Notieren Sie sich die **Client-ID** , die wir später in unserer Anwendung verwenden werden. Auf diesem Bildschirm können Sie außerdem Ihrer mobilen Anwendung Zugriff auf Active Directory oder eine weitere Anwendung hinzufügen, wie z. b. Web-API oder Office 365, die von der mobilen Anwendung verwendet werden kann, sobald die Authentifizierung vollständig ist.

   ![](register-images/05.-configure.jpg "Außerdem können Sie auf diesem Bildschirm Ihrer mobilen Anwendung Zugriff auf Active Directory oder eine weitere Anwendung wie Web-API oder Office 365 hinzufügen.")



## <a name="related-links"></a>Verwandte Links

- [Microsoft nativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
