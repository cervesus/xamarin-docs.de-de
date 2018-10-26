---
title: Tests auf der Apple Watch-Geräte
description: Dieses Dokument beschreibt, wie zum Bereitstellen einer WatchOS-app, die für Tests auf einem tatsächlichen Apple Watch mit Xamarin erstellt wurde. Es wird erläutert, Geräte, Profile, Tests, Bereitstellung und bietet einige Tipps zur Problembehandlung.
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f4a21e25f418cc81d5f210098ded648b3d70ae14
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116731"
---
# <a name="testing-on-apple-watch-devices"></a>Tests auf der Apple Watch-Geräte

Nachdem Sie ausgeführt haben die [Bereitstellungsschritte](~/ios/watchos/deploy-test/index.md) zum Erstellen von App-IDs und App-Gruppen (falls erforderlich), verwenden Sie die Anweisungen auf dieser Seite:

- [Einrichten Ihrer Geräte](#devices) im Apple Developer Center, und
- [Erstellen von Entwicklungsbereitstellungsprofilen](#profiles), klicken Sie dann
- [Bereitstellen und testen](#testing) auf einer Apple Watch.

<a name="devices" />

## <a name="devices"></a>Geräte

Testen von iOS-apps auf einem echten iPhone oder iPad arbeiten, ist das Gerät im Dev Center registriert werden immer erforderlich. Die Geräteliste sieht wie folgt aus (klicken Sie auf das Pluszeichen **+** ein neues Gerät hinzufügen):

![](device-images/devices-sml.png "Die Geräteliste sieht wie folgt aus.")

Überwachungen unterscheiden sich nicht – nun müssen Sie Ihrer Apple Watch-Geräte hinzufügen, vor dem Bereitstellen von apps für sie. Suchen Sie der Überwachung des UDID mithilfe **Xcode** (**Windows > Geräte** Liste). Wenn das Telefon gekoppelte verbunden ist wird auch die Überwachung der Informationen angezeigt:

[![](device-images/xcode-devices-sml.png "Gekoppelte Watch-Informationen")](device-images/xcode-devices.png#lightbox)

Wenn Sie wissen, dass der Überwachung des UDID, fügen Sie es der Geräteliste im Developer Center hinzu:

![](device-images/devices-watch-sml.png "Der Überwachung des UDID in der Geräteliste")

Nachdem das Gerät für die Überwachung hinzugefügt wurde, stellen Sie sicher, dass diese Option ausgewählt ist, die in neuen oder vorhandenen Entwicklung oder Ad-hoc-bereitstellungsprofile, die Sie erstellen:

![](device-images/devices-provisioning.png "Liste der verfügbaren Geräte")

Vergessen Sie nicht: Wenn Sie ein vorhandenes Bereitstellungsprofil herunterladen und installieren Sie es erneut bearbeiten

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Entwicklungsbereitstellungsprofilen

Erstellen Sie zum Testen auf Ihrem Gerät, das Sie erstellen müssen eine **Entwicklungsbereitstellungsprofil** für jede App-ID in der Projektmappe.

Wenn Sie einen Platzhalter-App-ID, haben *nur ein Bereitstellungsprofil wird erforderlich sein,*; aber wenn Sie eine separate App-ID für jedes Projekt benötigen Sie ein Bereitstellungsprofil für jede App-ID:

![](device-images/provisioningprofile-development.png "Das Entwicklungsbereitstellungsprofil")

Nachdem Sie alle drei Profile erstellt haben, werden sie in der Liste angezeigt. Denken Sie daran, herunterladen und installieren jeweils:

![](device-images/provisioningprofiles.png "Die verfügbaren Development-Bereitstellungsprofile")

Sie können überprüfen, ob das Bereitstellungsprofil in die **Projektoptionen** durch Auswählen der **erstellen > iOS Bundle-Signierung** Bildschirm- und Auswählen der **Version** oder **Debuggen iPhone** Konfiguration.

Die **Bereitstellungsprofil** Liste werden alle übereinstimmenden Profile angezeigt – sollte die entsprechenden Profilen, die Sie in der Dropdown-Liste erstellt haben:

![](device-images/options-selectprofile.png "Die Liste Bereitstellungsprofil")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Testen auf einem Gerät ansehen

Nachdem Sie Ihre Geräte, App-IDs und Bereitstellungsprofile konfiguriert haben, können Sie testen.

1. Stellen Sie sicher, dass auf Ihrem iPhone angeschlossen ist, und die Überwachung wurde bereits gekoppelt mit dem iPhone.

2. Stellen Sie sicher, die Konfiguration wird festgelegt, um **Version** oder **Debuggen**.

3. Stellen Sie sicher, dass die verbundenen iPhone-Geräte in der Liste ausgewählt ist.

4. Mit der rechten Maustaste auf das iOS-App-Projekt (nicht überwachen oder Erweiterung), und wählen Sie **als Startprojekt festlegen**.

5. Klicken Sie auf der **ausführen** Schaltfläche (oder wählen Sie eine **starten** option die **ausführen** Menü).

6. Die Projektmappe zu erstellen, und die iOS-app wird für das iPhone bereitgestellt werden.
  Wenn die iOS-app oder eine Watch-Erweiterung Bereitstellung nicht ordnungsgemäß festgelegt ist misslingt Bereitstellung für das iPhone.

7. Wenn die Bereitstellung erfolgreich abgeschlossen wurde, versucht das iPhone automatisch der gekoppelte Apple Watch der Watch-app an. Ihr app-Symbol wird angezeigt, auf dem Bildschirm sehen Sie sich eine zirkuläre *installieren* Statusanzeige.

8. Wenn die Watch-app erfolgreich installiert wurde, kann das Symbol verbleibt auf dem Bildschirm sehen Sie sich - touch, um das Testen Ihrer app!


## <a name="troubleshooting"></a>Problembehandlung

Bei einem bei der Verwendung der Bereitstellung Fehler der **Ansicht > Bereiche > Geräteprotokoll** um weitere Informationen zum Fehler anzuzeigen. Im folgenden sind einige Fehler und deren Ursachen aufgeführt:

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Fehler-MT3001: Konnte nicht AOT der assembly

Dies kann auftreten, wenn für die Erstellung im DEBUGMODUS befinden, um auf einem Apple Watch-Gerät bereitstellen.

Um *vorübergehend* dieses Problem umgehen, deaktivieren Sie **inkrementelle Builds** in der Watch-Erweiterung **Projektoptionen > Erstellen > WatchOS-Build** Fenster:

[![](device-images/disable-incremental-sml.png "Das Kontrollkästchen inkrementelle Builds")](device-images/disable-incremental.png#lightbox)

Dies wird in einer zukünftigen Version behoben werden, nach dem inkrementelle Builds erneut aktiviert werden können, um schnellere Builds zu nutzen.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Watch-App kann nicht gestartet werden, während des Debuggens auf Gerät

Wenn der Versuch, eine Watch-app auf einem physischen Gerät, das nur auf das Symbol & wartekreisel für Ladevorgang Debuggen angezeigt werden (und schließlich Timeout). Dies wird in einer zukünftigen Version behoben werden; Dieses Problem zu umgehen, ist einen Releasebuild ausgeführt wird (sodass kein Debuggen).


### <a name="invalid-application-executable-or-application-verification-failed"></a>Ungültiger ausführbare Datei "oder" Fehler beim Überprüfen der Anwendung

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Warnung für ungültigen ausführbare Datei der Anwendung")

Wenn diese Nachrichten werden *auf dem Bildschirm sehen Sie sich* nachdem die app installieren versucht hat, gibt es möglicherweise einige Probleme:

- Das Watch-Gerät selbst wurde nicht als ein Gerät im Apple Dev Center hinzugefügt. Führen Sie die Anweisungen, um [Konfigurieren von Geräten ordnungsgemäß](#devices).

- Das Watch-Gerät enthalten keine Development-bereitstellungsprofile, die für Tests verwendet wird; oder nachdem sie erneut heruntergeladen und erneut installiert waren nicht den bereitstellungsprofilen die Apple Watch hinzugefügt wurde. Führen Sie die Anweisungen, um [konfigurieren Sie die bereitstellungsprofile richtig](#profiles).

- Wenn die **iOS Geräteprotokoll** enthält `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` klicken Sie dann die Watch-App die **"Info.plist"** hat die falsche **MinimumOSVersion** Wert.
  Dies dürfte **8.2** – Wenn Sie Xcode 6.3 installiert haben, möglicherweise manuell bearbeiten, die Quelle müssen, Insert, legen Sie ihn auf 8.2.

- Die Watch-App **"Entitlements.plist"** falsch verfügt über eine Berechtigung aktiviert (z. B. App-Gruppen), sollte nicht zu verwenden.

- Die Watch-App **App-ID** falsch verfügt über eine Berechtigung aktiviert (z. B. App-Gruppen) im Developer Center, die darf nicht.



### <a name="install-never-finished"></a>Installieren Sie nie abgeschlossen

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Dieser Fehler deutet nicht erforderlich (und ungültig) Schlüssel in der Watch-App **"Info.plist"** Datei. Sie darf keine Schlüssel vorgesehen, die für die iOS-app oder sehen Sie sich die Erweiterung in der Watch-App enthalten.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"Warten Debugger eine Verbindung herstellen"

Wenn die **Anwendungsausgabe** Fenster bleibt anzeigen

```csharp
waiting for debugger to connect
```

Überprüfen Sie, ob eines der NuGet-Pakete, die in Ihrem Projekt hinzugefügt wurden Abhängigkeiten bestehen **Microsoft.Bcl.Build**. Dies wird automatisch hinzugefügt, und einige Microsoft veröffentlichten Bibliotheken, darunter die gängigen [Microsoft Http Client Libraries](http://www.nuget.org/packages/Microsoft.Net.Http/).

Die **Microsoft.Bcl.Build.targets** -Datei, die hinzugefügt wird die **csproj** kann mit der paketerstellung während der Bereitstellung von iOS-Erweiterungen beeinträchtigen. Sie können verfolgen, die [Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
Eine mögliche problemumgehung besteht darin, auf die CSPROJ-Datei bearbeiten und verschieben Sie manuell die **Microsoft.Bcl.Build.targets** auf das letzte Element sein.

