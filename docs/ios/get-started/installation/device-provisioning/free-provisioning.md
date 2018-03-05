---
title: Kostenlose Bereitstellung
description: "Die Version Xcode 7 von Apple brachte für alle iOS- und Mac-Entwickler ein wichtige Neuerung: die freie Bereitstellung."
ms.topic: article
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: baf1ae7d4cc533af0db482e8d7c31fc3c8b4edbf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="free-provisioning"></a>Kostenlose Bereitstellung

_Die Version Xcode 7 von Apple brachte für alle iOS- und Mac-Entwickler ein wichtige Neuerung: die freie Bereitstellung._

Mit der freien Bereitstellung können Entwickler ihre Xamarin.iOS-Anwendung auf ihrem iOS-Gerät bereitstellen, **ohne** an einem **Apple-Entwicklerprogramm** teilzunehmen. Dies bringt für Entwickler bedeutende Vorteile, da das Testen auf einem Gerät gegenüber dem Testen im Simulator zahlreiche Vorzüge besitzt, wie etwa Arbeitsspeicher, Speicher, Netzwerkkonnektivität u.a.

Das Bereitstellen ohne Apple-Entwicklerkonto muss über Xcode ausgeführt werden. Dabei werden eine *Signierungsidentität* (mit einem Entwicklerzertifikat und einem privaten Schlüssel) sowie ein *Bereitstellungsprofil* (mit einer eindeutigen App-ID und der benutzerdefinierten ID Ihres verbundenen iOS-Geräts) erstellt.

## <a name="requirements"></a>Anforderungen

Damit Sie die Vorteile der freien Bereitstellung Ihrer Xamarin.iOS-Anwendungen auf einem Gerät auch nutzen können, müssen Sie Xcode 7 oder höher verwenden.

**Die verwendete Apple-ID darf nicht mit einem Apple-Entwicklerprogramm verbunden sein.**

Die in Ihrer App verwendete Bundle-ID muss eindeutig sein und darf nicht schon in einer anderen App verwendet worden sein. Eine mit der freien Bereitstellung verwendete Bundle-ID DARF NICHT erneut verwendet werden. Wenn Sie eine App bereits verteilt haben, können Sie diese App nicht mehr mit der freien Bereitstellung bereitstellen. 

Weitere Informationen finden Sie in den [Leitfäden zur App-Verteilung](~/ios/deploy-test/app-distribution/index.md).

Wenn Ihre App App Services verwendet, müssen Sie ein Bereitstellungsprofil erstellen. Eine Anleitung hierzu finden Sie im Leitfaden zur [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md#appservices). Weitere Einschränkungen werden im [entsprechenden Abschnitt](#limitations) weiter unten aufgeführt.


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> Starten der App

Für die freie Bereitstellung einer Anwendung auf einem Gerät müssen Sie in Xcode die Signierungsidentität und Bereitstellungsprofile erstellen. Anschließend verwenden Sie in Visual Studio für Mac oder Visual Studio das richtige Profil zum Signieren Ihrer App. Führen Sie dazu die folgenden Schritte aus:

1. Wenn Sie noch keine Apple-ID besitzen, erstellen Sie eine Apple-ID unter [appleid.apple.com](https://appleid.apple.com/account).
2. Öffnen Sie Xcode, und gehen Sie zu **Xcode > Einstellungen**.
3. Klicken Sie unter **Konten** auf die **+**-Schaltfläche, um Ihre vorhandene Apple-ID hinzuzufügen. Dies sollte ungefähr wie folgt aussehen:

  [ ![](free-provisioning-images/launchapp1.png "Xcode > Voreinstellungen > Konten")](free-provisioning-images/launchapp1.png)

4. Schließen Sie das iOS-Gerät an, auf dem die Bereitstellung erfolgen soll, und erstellen Sie eine neue leere Einzelansicht für Ihr iOS-Projekt in Xcode. Wählen Sie in der Dropdownliste **Team** die zuvor hinzugefügte Apple-ID aus. Das Format sollte etwa wie `your name (Personal Team - your Apple ID)` aussehen:

  [ ![](free-provisioning-images/launchapp2.png "Erstellen der Signierungsidentität")](free-provisioning-images/launchapp2.png)

5. Stellen Sie sicher, dass der Bündelbezeichner im Abschnitt **Allgemein > Identität** _genau_ dem Bündelbezeichner Ihrer Xamarin.iOS-App entspricht und dass das Bereitstellungsziel dem verbundenen iOS-Gerät entweder entspricht oder niedriger ist. Dieser Schritt ist äußerst wichtig, da ein Bereitstellungsprofil in Xcode nur mit einer eindeutigen App-ID erstellt wird:

  [![](free-provisioning-images/launchapp5.png "Erstellen eines Bereitstellungsprofils mit einer expliziten App-ID")](free-provisioning-images/launchapp5.png)

6. Wählen Sie im Abschnitt „Signierung“ die Option **Signierung automatisch verwalten** aus, und wählen Sie anschließend aus der Dropdownliste Ihr Team aus:

  [![](free-provisioning-images/launchapp6.png "Auswählen der Option zum automatischen Verwalten der Signierung und Ihres Teams aus der Dropdownliste")](free-provisioning-images/launchapp6.png)

7. Dadurch werden automatisch ein Bereitstellungsprofil und eine Signierungsidentität für Sie erstellt. Diese werden durch Klicken auf das Informationssymbol neben dem Bereitstellungsprofil angezeigt:

  [![](free-provisioning-images/launchapp7.png "Anzeigen des Bereitstellungsprofils")](free-provisioning-images/launchapp7.png)

8. Stellen Sie die leere Anwendung durch Klicken auf die Schaltfläche „Ausführen“ auf Ihrem Gerät bereit, um in Xcode zu testen.

9. Gehen Sie zurück zu Ihrer IDE, und lassen Sie das Gerät angeschlossen. Klicken Sie mit der rechten Maustaste auf den Namen Ihres Xamarin.iOS-Projekts, um das Dialogfeld **Projektoptionen** zu öffnen. Gehen Sie zum Abschnitt „iOS-Bundle-Signierung“, und legen Sie Ihre Signierungsidentität und Ihr Bereitstellungsprofil eindeutig fest:

  [![](free-provisioning-images/launchapp8.png "Festlegen der Signierungsidentität und des Bereitstellungsprofils")](free-provisioning-images/launchapp8.png)

Wird Ihre Signierungsidentität oder das korrekte Bereitstellungsprofil in Ihrer IDE nicht angezeigt, müssen Sie die IDE möglicherweise neu starten.


## <a name="a-namelimitations-limitations"></a><a name="limitations" />Einschränkungen

Es existieren einige Einschränkungen, wann und wie Sie die freie Bereitstellung verwenden können, um Ihre Anwendung auf einem iOS-Gerät auszuführen. Dadurch will Apple sicherstellen, dass die Bereitstellung nur auf *Ihrem* Gerät erfolgt. Die Einschränkungen werden in diesem Abschnitt aufgeführt.

Auch der Zugriff auf iTunes Connect ist eingeschränkt, wodurch Dienste wie etwa das Veröffentlichen im App Store und TestFlight für Entwickler, die für ihre Anwendungen die freie Bereitstellung verwenden, nicht verfügbar sind. Für die Verteilung über Ad-hoc- und interne Mittel ist ein Apple-Entwicklerkonto erforderlich (Enterprise- oder persönliches Konto).

Auf diese Weise erstellte Bereitstellungsprofile laufen nach einer Woche ab, Signierungsidentitäten nach einem Jahr. Außerdem können Bereitstellungsprofile nur mit einer eindeutigen App-ID erstellt werden, weshalb Sie für jede App, die Sie installieren möchten, die [oben](#launching) aufgeführten Anweisungen befolgen müssen.

Auch die meisten Anwendungsdienste können nicht mit der freien Bereitstellung bereitgestellt werden. Dies umfasst Folgendes:

- Apple Pay
- Game Center
- iCloud
- In-App-Käufe
- Pushbenachrichtigungen
- Wallet (früher Passbook)

Eine vollständige Auflistung finden Sie im Leitfaden [Supported Capabilities](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1) (Unterstützte Funktionen) von Apple. Wenn Sie Ihre App für die Verwendung mit Anwendungsdiensten bereitstellen möchten, finden Sie Informationen dazu in den Leitfäden [Working with Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md) (Arbeiten mit Funktionen).


## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden werden die Vorteile und Einschränkungen der freien Bereitstellung bei der Installation von Anwendungen auf einem iOS-Gerät beschrieben. Außerdem wird die Installation einer Xamarin.iOS-App mit der freien Bereitstellung detailliert dargestellt.

## <a name="related-links"></a>Verwandte Links

- [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md)
- [Bereitstellung für Anwendungsdienste](~/ios/get-started/installation/device-provisioning/index.md#appservices)
