---
title: Von verbundenen Diensten in Visual Studio für Mac
description: Hinzufügen von Azure Datenspeicher, Authentifizierung und Pushbenachrichtigungen auf mobile apps aus Visual Studio für Mac
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b9aaa197ccf01c59d6e4bbb0476d10295a108f89
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="connected-services-walkthrough"></a>Verbundene Dienste Exemplarische Vorgehensweise

Der verbundene Dienste Workflow kann das Azure Portal Workflow in Visual Studio für Mac, daher ist es nicht, um das Projekt, um Dienste hinzufügen zu lassen.

Diese exemplarische Vorgehensweise veranschaulicht das hinzufügen einen Azure-Back-End-Dienst untergebracht cloudspeicherung von Daten, Authentifizierung und Pushbenachrichtigungen an eine plattformübergreifende Xamarin.Forms Portable Klassenbibliothek (PCL)-Anwendung.


1.  Starten Sie durch Doppelklicken auf die **verbundene Dienste** Knoten in der Projektmappe öffnen wird die **Services-Katalog**.
  Dies ist eine Liste aller verfügbaren Dienste für diesen Anwendungstyp. Wählen Sie einen Dienst (z. B. **Mobile Back-End-mit Azure App Service**) indem Sie darauf klicken.

  [![](connected-services-images/image001-sml.png "Dienstknoten in Visual Studio für Mac verbunden sind.")](connected-services-images/image001.png#lightbox)

2. Die Detailseite Dienst weist eine Beschreibung des Diensts und die Abhängigkeiten installiert werden.
  Klicken Sie auf die **hinzufügen** , um die app die Abhängigkeiten hinzuzufügen:

  [![](connected-services-images/image002-sml.png "Mobile Back-End-mit Azure")](connected-services-images/image002.png#lightbox)

3. Die Abhängigkeiten müssen sowohl die PCL und den plattformspezifischen Projekten arbeiten hinzugefügt werden.
  Aktivieren Sie die Kontrollkästchen, um den Dienst auf jedes Projekt hinzuzufügen, die darauf verweisen (entweder direkt oder indirekt):

  [![](connected-services-images/image003-sml.png "Überprüfen Sie alle Projekte, die den Dienst verweisen soll")](connected-services-images/image003.png#lightbox)

4. Wählen Sie **Accept** auf die **akzeptieren der Lizenzbedingungen** Dialogfelder für die NuGet-Pakete.
  Es können zwei Dialogfelder annehmen, eine für die MobileClient und Abhängigkeiten und die andere für SQLiteStore, was für die offline-datensynchronisierung erforderlich ist, vorhanden sein:

  [![](connected-services-images/image004-sml.png "Lizenzvereinbarungen akzeptieren")](connected-services-images/image004.png#lightbox)

  ![](connected-services-images/image005.png "Lizenz Annahme Fenster")

5. Sobald die Abhängigkeiten hinzugefügt wurden, werden Sie aufgefordert, sich mit dem Konto anmelden, die Sie für die Kommunikation mit Azure verwenden möchten.
  Wenn Sie bereits mit einer Microsoft-ID angemeldet sind, versucht Visual Studio für Mac Ihre Azure-Abonnements und alle app-Dienste, die zugeordnet werden sollen. Wenn Sie nicht über Abonnements verfügen, können Sie sich für eine kostenlose Testversion anmelden, oder erwerben ein Abonnement im Azure-Portal hinzufügen.

6. Wählen Sie einen app-Dienst aus der Liste aus. Ausfüllen dieser den Vorlagencode für die `MobileServiceClient` Objekt durch die entsprechende URL der app-Dienst in Azure:

  [![](connected-services-images/image006-sml.png "Wählen Sie aus der app service")](connected-services-images/image006.png#lightbox)

  Wenn es keine Dienste aufgeführt sind, klicken Sie auf die **neu** Schaltfläche (siehe Schritt 9).

7. Kopieren Sie den Vorlagencode für die `MobileServiceClient` in der PCL. Die Datei-Speicherort-app in nicht wichtig ist, solange es nur eine Instanz des Zertifikats ist.
  Die empfohlene Vorgehensweise ist die Erstellung einer `AzureService` -Klasse, die alle Azure Interaktionen verarbeitet und verwendet die `MobileServiceClient`:

  ![](connected-services-images/image007.png "Kopieren Sie die Config-Code in der app")

8. Befolgen Sie die Dokumentation im **Arbeitsschritte** zum Hinzufügen von Daten, die offline-Synchronisierung, Authentifizierung und Pushbenachrichtigungen an Ihre app:

  [![](connected-services-images/image008-sml.png "Lesen Sie die nächsten Schritte-Anweisungen")](connected-services-images/image008.png#lightbox)

10. Wenn Sie alle vorhandenen app-Dienste besitzen, können Sie neue Dienste von innerhalb von Visual Studio für Mac erstellen.
  Klicken Sie auf die **neu** Schaltfläche in der linken unteren Rand der Liste der Dienste zu öffnen die **neuen App Service** Dialogfeld:

  [![](connected-services-images/image009-sml.png "Erstellen Sie einen neuen app-Dienst in Visual Studio für Mac")](connected-services-images/image009.png#lightbox)

Ein neuer Dienst erfordert die folgenden Parameter:

-   **Name des App Service** – eindeutige Namen/Id für den Plan
-   **Abonnement** – das Abonnement, das Sie bezahlen für den Dienst verwenden möchten
-   **Ressourcengruppe** – einen unidirektionalen oder einen Organisieren Ihrer Azure Ressourcen für ein Projekt. Option, um vorhandene oder eine neue erstellen. Wenn dies Ihre erste Azure-Dienst ist, erstellen Sie eine neue.
-   **Service-Plan** – bestimmt den Speicherort und den Kosten aller Ressourcen, die sie verwenden. Option, um vorhandene oder eine neue erstellen. Ist dies Ihre erste Azure-Dienst, verwenden Sie den Standardwert, oder erstellen Sie eine neue im free-Tarif (F1).

Besuchen Sie die [Azure App Service-Dokumentation](https://docs.microsoft.com/azure/app-service/) für Weitere Informationen


## <a name="related-links"></a>Verwandte Links

- [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
