---
title: Schritt 1. Registrieren einer app zur Verwendung von Azure Active Directory
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 52d06dc6125f91f98e8f3ee8b4f91ad7b52347a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Schritt 1. Registrieren einer app zur Verwendung von Azure Active Directory

1. Navigieren Sie zu [windowsazure.com](https://manage.windowsazure.com) und melden Sie sich mit Ihrem Microsoft-Account oder Organisations-Konto im Azure-Portal. Wenn Sie nicht über ein Azure-Abonnement verfügen, erhalten Sie eine Testversion von [azure.com](http://www.azure.com)

2. Nach der Anmeldung, wechseln Sie zu der **Active Directory** Abschnitt (1), und wählen Sie das Verzeichnis, in dem die Anwendung (2) zu registrieren werden sollen,

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "im Abschnitt, und wählen Sie das Verzeichnis, in dem die Anwendung registriert werden sollen")](register-images/01.-active-directory-in-azure-portal.jpg)

3. Klicken Sie auf **hinzufügen** um die neue Anwendung erstellt haben, und wählen Sie dann **eine von meinem Unternehmen entwickelte Anwendung hinzufügen**

  [ ![](register-images/02.-add-new-application-sml.jpg "Fügen Sie eine von meinem Unternehmen entwickelte Anwendung hinzu")](register-images/02.-add-new-application.jpg)

4. Geben Sie auf dem nächsten Bildschirm Ihre app einen Namen (z. b. XAM-DEMO).
  Stellen Sie sicher, dass die Auswahl **systemeigene Clientanwendung** als Typ der Anwendung.

  ![](register-images/03.-app-name.jpg "Stellen Sie sicher, dass Sie systemeigene Clientanwendung mit dem Typ der Anwendung auswählen")

5. Geben Sie auf der letzten Seite eine **Umleitungs-URI* , die für Ihre Anwendung eindeutig ist, wie es beim Abschluss der Authentifizierung an diesen URI zurückgegeben werden sollen.

  ![](register-images/04.-app-redirect.jpg "Geben Sie auf der letzten Seite eine Umleitungs-URI, die für Ihre Anwendung eindeutig ist, da es an diesen URI zurückgegeben wird, wenn Authentifizierung abgeschlossen ist.")

6. Sobald die app erstellt wurde, navigieren Sie zu der **konfigurieren** Registerkarte. Notieren Sie sich die **Clientkennung** das in der vorliegenden Anwendung später verwendet wird. Sie können auch auf diesem Bildschirm übergeben Ihren mobilen Anwendungszugriff auf Active Directory oder Hinzufügen von einer anderen Anwendung wie Web-API oder Office 365, der nach Abschluss der Authentifizierung von mobilen Anwendung verwendet werden kann.

    ![](register-images/05.-configure.jpg "Darüber hinaus auf diesem Bildschirm können Sie Ihre mobile Anwendungszugriff auf Active Directory gewähren oder Hinzufügen einer anderen Anwendung wie Web-API oder Office 365")



## <a name="related-links"></a>Verwandte Links

- [Microsoft NativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
