---
title: Testen auf Apple Watch Geräten
description: In diesem Dokument wird beschrieben, wie Sie eine watchos-App bereitstellen, die mit xamarin erstellt wurde, um eine tatsächliche Apple Watch zu testen Es werden Geräte, Bereitstellungs Profile und Tests erläutert, und es werden einige Tipps zur Problembehandlung bereitgestellt.
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 6d3756f4215174e17ec45518f430dc38270e3289
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768702"
---
# <a name="testing-on-apple-watch-devices"></a>Testen auf Apple Watch Geräten

Nachdem Sie die [Bereitstellungs Schritte](~/ios/watchos/deploy-test/index.md) zum Erstellen von App-IDs und App-Gruppen ausgeführt haben (falls erforderlich), befolgen Sie die Anweisungen auf dieser Seite für Folgendes:

- [Richten Sie Ihre Geräte](#devices) im Apple dev Center ein, und
- [Erstellen Sie Entwicklungs Bereitstellungs profile](#profiles), und
- Stellen Sie eine Apple Watch bereit [und testen](#testing) Sie Sie.

<a name="devices" />

## <a name="devices"></a>Geräte

Das Testen von IOS-apps auf einem echten iPhone oder iPad erforderte immer das Registrieren des Geräts im dev Center. Die Geräteliste sieht wie folgt aus (Klicken Sie auf **+** das Pluszeichen, um ein neues Gerät hinzuzufügen):

![](device-images/devices-sml.png "Die Geräteliste sieht wie folgt aus.")

Uhren sind nicht anders. Sie müssen nun Ihr Apple Watch Gerät hinzufügen, bevor Sie Apps für das Gerät bereitstellen. Suchen Sie die Benutzerkontensteuerung mithilfe von **Xcode** (**Windows >-Geräte** Liste). Wenn das gekoppelte Telefon verbunden ist, werden die Überwachungsinformationen ebenfalls angezeigt:

[![](device-images/xcode-devices-sml.png "Gekoppelte Überwachungsinformationen")](device-images/xcode-devices.png#lightbox)

Wenn Sie das UDID der Überwachung kennen, fügen Sie es der Geräteliste im dev Center hinzu:

![](device-images/devices-watch-sml.png "UDID der Überwachung in der Geräteliste")

Nachdem das Überwachungsgerät hinzugefügt wurde, stellen Sie sicher, dass es in neuen oder vorhandenen Entwicklungs-oder Ad-hoc-Bereitstellungs Profilen ausgewählt ist, die Sie erstellen:

![](device-images/devices-provisioning.png "Liste der verfügbaren Geräte")

Vergessen Sie nicht, wenn Sie ein vorhandenes Bereitstellungs Profil bearbeiten, um es herunterzuladen und erneut zu installieren.

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Entwicklungs Bereitstellungs profile

Zum Erstellen von Tests auf Ihrem Gerät müssen Sie ein **Entwicklungs Bereitstellungs Profil** für jede APP-ID in ihrer Projekt Mappe erstellen.

Wenn Sie über eine Platzhalter-APP-ID verfügen, *wird nur ein Bereitstellungs Profil benötigt*. Wenn Sie jedoch über eine separate App-ID für jedes Projekt verfügen, benötigen Sie ein Bereitstellungs Profil für jede APP-ID:

![](device-images/provisioningprofile-development.png "Das Entwicklungs Bereitstellungs Profil")

Nachdem Sie alle drei Profile erstellt haben, werden Sie in der Liste angezeigt. Denken Sie daran, die einzelnen herunterzuladen und zu installieren:

![](device-images/provisioningprofiles.png "Die verfügbaren Entwicklungs Bereitstellungs profile")

Sie können das Bereitstellungs Profil in den **Projektoptionen** überprüfen, indem Sie den Bildschirm zum **Erstellen > IOS-Bundle-Signierung** auswählen und die iPhone-Konfiguration **Release** oder **Debug** auswählen.

In der Liste der **Bereitstellungs profile** werden alle übereinstimmenden Profile angezeigt. Sie sollten die entsprechenden Profile sehen, die Sie in dieser Dropdown Liste erstellt haben:

![](device-images/options-selectprofile.png "Die Liste der Bereitstellungs profile")

<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Testen auf einem Überwachungsgerät

Nachdem Sie das Gerät, App-IDs und Bereitstellungs Profile konfiguriert haben, können Sie es testen.

1. Stellen Sie sicher, dass Ihr iPhone angeschlossen ist und die Überwachung bereits mit dem iPhone gekoppelt ist.

2. Stellen Sie sicher, dass die Konfiguration auf **Release** oder **Debug**festgelegt ist.

3. Stellen Sie sicher, dass das verbundene iPhone-Gerät in der Zielliste ausgewählt ist.

4. Klicken Sie mit der rechten Maustaste auf das IOS-App-Projekt (nicht die Überwachung oder Erweiterung), und wählen Sie **als Startprojekt festlegen**aus.

5. Klicken Sie auf die Schaltfläche **Ausführen** , oder wählen Sie im Menü **Ausführen** die Option **Start** aus.

6. Die Lösung wird erstellt, und die IOS-APP wird auf dem iPhone bereitgestellt.
  Wenn die Bereitstellung der IOS-APP oder der Überwachungs Erweiterung nicht ordnungsgemäß festgelegt ist, schlägt die Bereitstellung auf dem iPhone fehl.

7. Wenn die Bereitstellung erfolgreich abgeschlossen wird, versucht das iPhone automatisch, die Watch-APP an die gekoppelte Überwachung zu senden. Das App-Symbol wird auf dem Bildschirm überwachen mit einer zirkulären *Installations* Fortschrittsanzeige angezeigt.

8. Wenn die Watch-App erfolgreich installiert wurde, verbleibt das Symbol auf dem Bildschirm überwachen, um die APP zu testen.

## <a name="troubleshooting"></a>Problembehandlung

Wenn während der Bereitstellung ein Fehler auftritt, verwenden Sie die **Ansicht > Pads > Geräte Protokoll** , um weitere Informationen zum Fehler anzuzeigen. Einige Fehler und deren Gründe sind unten aufgeführt:

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Fehler MT3001: Die Assembly konnte nicht gefunden werden.

Dies kann bei der Erstellung im Debugmodus zur Bereitstellung auf einem Apple Watch Gerät auftreten.

Um dieses Problem *vorübergehend* zu umgehen, deaktivieren Sie **inkrementelle Builds** in den Optionen für das Überwachungs Erweiterungs **Projekt > Build > watchos Build** -Fensters:

[![](device-images/disable-incremental-sml.png "Kontrollkästchen inkrementelle Builds")](device-images/disable-incremental.png#lightbox)

Dies wird in einer zukünftigen Version korrigiert, nach der inkrementelle Builds erneut aktiviert werden können, um schnellere Buildzeiten zu nutzen.

### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Watch-App kann beim Debuggen auf dem Gerät nicht gestartet werden

Wenn Sie versuchen, eine Watch-App auf einem physischen Gerät zu debuggen, wird nur das Symbol & Lade Spinner angezeigt (und schließlich ein Timeout). Dies wird in einer zukünftigen Version behoben werden. eine Problem Umgehung besteht darin, einen Releasebuild auszuführen (was das Debugging nicht zulässt).

### <a name="invalid-application-executable-or-application-verification-failed"></a>Ungültige ausführbare Datei der Anwendung oder Anwendungs Überprüfung

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Ungültige anwendungsausführ Bare Warnung")

Wenn diese Nachrichten *auf dem Bildschirm überwachen* angezeigt werden, nachdem die APP versucht hat, zu installieren, können einige Probleme auftreten:

- Das Überwachungsgerät selbst wurde im Apple dev Center nicht als Gerät hinzugefügt. Befolgen Sie die Anweisungen, um [Geräte ordnungsgemäß zu konfigurieren](#devices).

- Für die für den Test verwendeten Entwicklungs Bereitstellungs profile wurde das Überwachungsgerät nicht eingeschlossen. oder nachdem die Überwachung den Bereitstellungs Profilen hinzugefügt wurde, wurden Sie nicht erneut heruntergeladen und erneut installiert. Befolgen Sie die Anweisungen, um [die Bereitstellungs profile ordnungsgemäß zu konfigurieren](#profiles).

- Wenn das **IOS-Geräte Protokoll** enthält `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` , weist die Datei " **Info. plist** " der Watch-App den falschen **minimumosversion** -Wert auf.
  Dieser Wert sollte **8,2** lauten. Wenn Sie Xcode 6,3 installiert haben, müssen Sie möglicherweise die Quelle manuell bearbeiten, um Sie auf 8,2 festzulegen.

- Die Berechtigung " **Berechtigungen. plist** " der Watch-APP ist fälschlicherweise aktiviert (z. b. app-Gruppen).

- Die **App-ID** der Watch-App weist fälschlicherweise eine Berechtigung (z. b. app-Gruppen) im dev Center auf, die Sie nicht haben sollte.

### <a name="install-never-finished"></a>Installation nie abgeschlossen

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Dieser Fehler kann auf unnötige (und ungültige) Schlüssel in der **Info. plist** -Datei der Watch-App hindeuten. Sie sollten keine Schlüssel für die IOS-APP oder die Watch-Erweiterung in der Watch-App einschließen.

<!--eg. NSLocationAlwaysUsageDescription -->

### <a name="waiting-for-debugger-to-connect"></a>"warten auf Verbindungs Debugger"

Wenn das **Anwendungs Ausgabe** Fenster nicht mehr angezeigt wird

```csharp
waiting for debugger to connect
```

Überprüfen Sie, ob eines der nugets, die in Ihrem Projekt enthalten sind, von **Microsoft. BCL. Build**abhängig ist. Diese wird automatisch mit einigen von Microsoft veröffentlichten Bibliotheken hinzugefügt, einschließlich der gängigen [Microsoft HTTP-Client Bibliotheken](https://www.nuget.org/packages/Microsoft.Net.Http/).

Die Datei " **Microsoft. BCL. Build. targets** ", die der **csproj** -Datei hinzugefügt wird, kann die Paket Erstellung von IOS-Erweiterungen während der Bereitstellung beeinträchtigen. Sie können den [Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)nachverfolgen.
Eine mögliche Problem Umgehung ist das Bearbeiten der CSPROJ-Datei und das manuelle Verschieben von **Microsoft. BCL. Build. targets** als letztes Element.
