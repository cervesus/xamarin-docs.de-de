---
title: Schritt 1. Registrieren einer app zum Azure Active Directory verwenden
description: Dieses Dokument beschreibt, wie Sie eine Azure-Anwendung in Azure Active Directory zu registrieren, sodass es von mobilen Clients sicher zugegriffen werden kann.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fd3cb0e8201a6e84c726b211852013bd5f35fba1
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675110"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Schritt 1. Registrieren einer app zum Azure Active Directory verwenden

1. Navigieren Sie zu [windowsazure.com](https://manage.windowsazure.com) und melden Sie sich mit Ihrem Microsoft Account oder Organisations-Konto im Azure-Portal. Wenn Sie nicht über ein Azure-Abonnement verfügen, erhalten Sie eine Testversion von [azure.com](http://www.azure.com)

2. Nach der Anmeldung finden Sie unter den **Active Directory** Abschnitt (1), und wählen Sie das Verzeichnis, in der Anwendung (2)

  [![](register-images/01.-active-directory-in-azure-portal-sml.jpg "Abschnitt, und wählen Sie das Verzeichnis, in dem Sie die Anwendung registrieren möchten")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Klicken Sie auf **hinzufügen** um die neue Anwendung erstellen, und wählen Sie dann **eine von meinem Unternehmen entwickelte Anwendung hinzufügen**

  [![](register-images/02.-add-new-application-sml.jpg "Fügen Sie eine von meinem Unternehmen entwickelte Anwendung hinzu")](register-images/02.-add-new-application.jpg#lightbox)

4. Geben Sie auf dem nächsten Bildschirm der app einen Namen (z. b. XAM-DEMO).
  Stellen Sie sicher, dass Sie **systemeigene Clientanwendung** als den Typ der Anwendung.

  ![](register-images/03.-app-name.jpg "Stellen Sie sicher, dass Sie eine systemeigene Clientanwendung mit dem Typ der Anwendung auswählen")

5. Geben Sie auf der letzten Seite, ein **Umleitungs-URI* , die für Ihre Anwendung eindeutig ist, wie sie an diesen URI zurückgibt, wenn es sich bei Authentifizierung abgeschlossen ist.

  ![](register-images/04.-app-redirect.jpg "Geben Sie auf der letzten Seite ein Umleitungs-URI, der für Ihre Anwendung eindeutig ist, wie sie an diesen URI zurückgibt, wenn es sich bei Authentifizierung abgeschlossen ist")

6. Sobald die app erstellt wurde, navigieren Sie zu der **konfigurieren** Registerkarte. Notieren Sie sich die **Client-ID** die wir in unserer Anwendung später verwenden werden. Sie können auch auf diesem Bildschirm erhalten Ihre mobile Anwendungszugriff auf Active Directory oder Hinzufügen einer anderen Anwendung wie Web-API oder Office 365, die durch die Verwaltungsrichtlinie für mobile Anwendungen verwendet werden kann, nachdem die Authentifizierung abgeschlossen ist.

    ![](register-images/05.-configure.jpg "Darüber hinaus auf diesem Bildschirm können Sie Ihre mobile Anwendungszugriff auf Active Directory gewähren oder Hinzufügen einer anderen Anwendung wie Web-API oder Office 365")



## <a name="related-links"></a>Verwandte Links

- [Microsoft NativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
