---
title: App-Gruppen-Funktionen in Xamarin.iOS
description: Um Funktionen zu einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden Einstellungen beschrieben, die für App-Gruppen-Funktionen erforderlich sind.
ms.prod: xamarin
ms.assetid: 0A61220B-BBAC-492B-9D3B-578986E64064
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 4ce04f21a3e520fea9da5d538fb7cc0ac098ad31
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119837"
---
# <a name="app-group-capabilities-in-xamarinios"></a>App-Gruppen-Funktionen in Xamarin.iOS

_Um Funktionen einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden Einstellungen beschrieben, die für App-Gruppen-Funktionen erforderlich sind._

Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

*   [Apple Watch-Einstellungen](~/ios/watchos/app-fundamentals/settings.md)
*   [Freigegebene NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)
*   [Freigegebene Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files)

## <a name="configure-a-new-app-group"></a>Konfigurieren einer neuen App-Gruppe

Der freigegebene Speicherort wird mit einer  [App-Gruppe](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) konfiguriert. Diese wird im  [Apple Developer Center](https://developer.apple.com/account/) im Abschnitt  **Certificates, Identifiers & Profiles (Zertifikate, Bezeichner & Profile)**  eingerichtet. Auf den Speicherort muss auch in der Datei „Entitlements.plist“ des jeweiligen Projekts verwiesen werden.

Die App-Gruppe erhält einen Bezeichner, der sich üblicherweise aus der Bundle-ID und dem Gruppenpräfix („group.“) zusammensetzt. Beispielsweise könnte für die Bundle-ID `com.xamarin.WatchSettings`  die App-Gruppe  `group.com.xamarin.WatchSettings` verwendet werden.

Gehen Sie zum Erstellen einer neuen App-Gruppe wie folgt vor:

1.  Öffnen Sie im  [iOS Developer Center](https://developer.apple.com/account/) von Apple über  **Account**  Ihr Konto, und melden Sie sich an.
2.  Klicken Sie auf **Certificates, IDs & Profiles** (Zertifikate, Bezeichner & Profile).
3.  Wählen Sie unter  **Identifier**  (Bezeichner) die Option  **App Groups**  (App-Gruppen) aus, und klicken Sie auf die Schaltfläche  **+** , um eine neue Gruppe zu erstellen.
4.  Geben Sie einen  **Namen**  und einen  **Bezeichner**  für die neue Gruppe ein, und klicken Sie auf die Schaltfläche  **Continue**  (Weiter): 
   
    ![Hinzufügen von Details zu App-Gruppen](app-groups-capabilities-images/image52.png)

5.  Klicken Sie zum Erstellen der Gruppe auf die Schaltfläche  **Register**  (Registrieren) und anschließend auf  **Done (Fertig)** , um zur Liste der registrierten App-Gruppen zurückzukehren.

## <a name="configure-an-app-to-use-app-groups"></a>Konfigurieren einer App zur Verwendung mit App-Gruppen

Nach dem Erstellen der App-Gruppe müssen Sie die App-IDs erstellen, durch die Apps die App-Gruppe verwenden können.

Führen Sie folgende Schritte aus:

1.  Melden Sie sich im  [iOS Developer Center](https://developer.apple.com/account/) mit Ihrem Apple-Entwicklerkonto an.
2.  Klicken Sie im Menü  **Program Resources**  (Programmressourcen) auf  **Certificates, IDs & Profiles** (Zertifikate, Bezeichner & Profile).
3.  Wählen Sie unter  **Identifier**  (Bezeichner) die Option  **App IDs**  (App-IDs) aus, und klicken Sie auf die Schaltfläche  **+** , um eine neue ID zu erstellen.
4.  Geben Sie unter „Name“ einen Namen für die App-ID und unter „Explicit App ID“ eine explizite App-ID an.
5.  Aktivieren Sie unter  **App Services**  (App-Dienste) das Kontrollkästchen  **App Groups** (App-Gruppen), und klicken Sie anschließend auf die Schaltfläche „Continue“ (Weiter):

    ![Hinzufügen von App-Diensten zu App-Gruppen](app-groups-capabilities-images/image53.png)

6.  Überprüfen Sie die Einstellungen, und klicken Sie auf die Schaltfläche  **Register**  (Registrieren) zum Erstellen der App-ID.
7.  Klicken Sie auf die Schaltfläche  **Done**  (Fertig), um zur Liste der registrierten App-IDs zurückzukehren.
8.  Wählen Sie die neu erstellte App-ID aus der Liste aus, und klicken Sie auf die Schaltfläche  **Edit**  (Bearbeiten):

    ![Auswählen einer App-ID aus der Liste](app-groups-capabilities-images/image54.png)

9.  Klicken Sie unter dem Dienst  **App Group** (App-Gruppe) auf die Schaltfläche  **Edit**  (Bearbeiten):

    ![Auswählen einer App-ID aus der Liste](app-groups-capabilities-images/image55.png)

10. Wählen Sie die zuvor erstellte App-Gruppe aus, und klicken Sie auf die Schaltfläche  **Continue**  (Weiter):

    ![Hinzufügen von App-Gruppe](app-groups-capabilities-images/image56.png)

11. Klicken Sie zuerst auf die Schaltfläche  **Assign**  (Zuweisen) und anschließend auf  **Done**  (Fertig), um zur Liste der registrierten App-IDs zurückzukehren.
12. Wiederholen Sie diese Schritte für alle Apps (oder Erweiterungen), die mit dieser App-Gruppe verwendet werden sollen.

## <a name="next-steps"></a>Nächste Schritte
 
In der folgenden Liste werden mögliche weitere Schritte aufgeführt:

* Verwenden des Framework-Namespaces in Ihrer App
* Hinzufügen der erforderlichen Berechtigungen zu Ihrer App Informationen zu den erforderlichen Berechtigungen und wie sie hinzugefügt werden finden Sie im Leitfaden [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md).
* Stellen Sie im Bereich  **iOS-Bundle-Signierung** der App sicher, dass  **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** festgelegt ist. Hierbei handelt es sich  _nicht_  um die Standardeinstellung für Debug- und iOS-Simulatorbuilds.

Wenn Probleme mit App-Diensten auftreten, konsultieren Sie den Abschnitt [Problembehandlung](~/ios/deploy-test/provisioning/capabilities/index.md) in der Hauptanleitung.