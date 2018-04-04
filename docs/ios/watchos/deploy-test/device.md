---
title: Testen auf Geräten überwachen
description: Bereitstellen von Apps auf Ihrer Apple Watch testen
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: d1d00a4d561551435e7d2333520dc614a79dcad3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="testing-on-watch-devices"></a>Testen auf Geräten überwachen

Nachdem Sie ausgeführt haben die [Bereitstellungsschritte](~/ios/watchos/deploy-test/index.md) zum Erstellen von App-IDs und App-Gruppen (falls erforderlich), verwenden Sie die Anweisungen auf dieser Seite können Sie:

- [Einrichten Ihrer Geräte](#devices) im Apple Developer Center, und
- [Erstellen von Bereitstellungsprofilen](#profiles), klicken Sie dann
- [Bereitstellen und testen](#testing) auf einem Apple Watch.

<a name="devices" />

## <a name="devices"></a>Geräte

Testen von iOS-apps auf einem echten iPhone oder iPad ist immer erforderlich, das Gerät im Developer Center registriert werden. Die Geräteliste sieht wie folgt (klicken Sie auf das Pluszeichen **+** zum Hinzufügen eines neuen Geräts):

![](device-images/devices-sml.png "Die Geräteliste sieht wie folgt")

Überwachungen werden nicht von anderen – müssen Sie nun Ihr Apple Watch-Gerät hinzufügen, vor dem Bereitstellen von apps für sie. Suchen der Watch UDID mit **Xcode** (**Windows > Geräte** Liste). Wenn paarweise zugeordneten Phone verbunden ist werden auch die Überwachung Informationen angezeigt:

[![](device-images/xcode-devices-sml.png "Gepaarte Watch-Informationen")](device-images/xcode-devices.png#lightbox)

Wenn Sie wissen, der Überwachung UDID, Hinzufügen der Geräteliste im Developer Center:

![](device-images/devices-watch-sml.png "Der Überwachung in der Geräteliste UDID")

Sobald das Gerät überwachen hinzugefügt wurde, stellen Sie sicher, dass diese Option ausgewählt ist, in dem alle neuen oder vorhandenen Entwicklung oder Ad-hoc-provisioning Profile, die Sie erstellen:

![](device-images/devices-provisioning.png "Liste der verfügbaren Geräte")

Vergessen Sie nicht das Bearbeiten ein vorhandenes provisioning-Profil zum Herunterladen und installieren es erneut.

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Bereitstellungsprofile Entwicklung

Erstellen Sie zum Testen auf Ihrem Gerät Sie erstellen müssen eine **Entwicklung Bereitstellungsprofil** für jede App-ID in der Projektmappe.

Ist einen Platzhalter-Anwendungs-ID, *werden nur ein Bereitstellungsprofil*; aber wenn Sie über eine separate App-ID für jedes Projekt benötigen Sie ein Bereitstellungsprofil für jede App-ID:

![](device-images/provisioningprofile-development.png "Die Entwicklung Provisioning-Profil")

Nachdem Sie alle drei Profile erstellt haben, werden sie in der Liste angezeigt. Denken Sie daran, herunterladen und installieren jeweils:

![](device-images/provisioningprofiles.png "Die verfügbaren Entwicklung Provisioning Profile")

Sie können überprüfen, ob das Bereitstellungsprofil in der **Projektoptionen** durch Auswahl der **erstellen > iOS Bundle Signing** Bildschirm- und Auswählen der **Version** oder **Debuggen iPhone** Konfiguration.

Die **Bereitstellungsprofil** Liste zeigt alle übereinstimmenden Profile – sehen Sie die entsprechenden Profilen, die Sie in der Dropdown-Liste erstellt haben:

![](device-images/options-selectprofile.png "Die Bereitstellung Profilliste")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Testen auf einem Gerät überwachen

Nachdem Sie Ihr Gerät, App-IDs und Provisioning Profile konfiguriert haben, können Sie jetzt testen.

1. Stellen Sie sicher, dem iPhone angeschlossen ist, und die Überwachung wurde bereits mit dem iPhone gekoppelt.

2. Stellen Sie sicher, das so konfiguriert ist **Release** oder **Debuggen**.

3. Stellen Sie sicher, dass das Gerät verbundenen iPhone in der Zielliste ausgewählt ist.

4. Mit der rechten Maustaste auf das iOS-App-Projekt (nicht überwachen oder Erweiterung), und wählen Sie **als Startprojekt festlegen**.

5. Klicken Sie auf die **ausführen** Schaltfläche (oder wählen Sie eine **starten** option die **ausführen** im Menü).

6. Die Projektmappe zu erstellen und die iOS-app wird für das iPhone bereitgestellt werden.
  Wenn die iOS-app oder Watch-Erweiterung Bereitstellung nicht ordnungsgemäß festgelegt ist, schlägt Bereitstellung auf dem iPhone fehl.

7. Wenn Bereitstellung erfolgreich abgeschlossen wurde, versucht das iPhone automatisch der gepaarten Watch Watch-app an. Ihre app-Symbol wird angezeigt, auf dem Bildschirm überwachen mit einer kreisförmigen *installieren* Statusanzeige.

8. Wenn die Watch-app erfolgreich installiert wurde, bleibt das Symbol auf dem Bildschirm überwachen - Tippen sie zum Testen Ihrer app!


## <a name="troubleshooting"></a>Problembehandlung

Bei einem während der Bereitstellung verwenden Fehler die **Ansicht > füllt > Geräteprotokoll** um weitere Informationen zu diesem Fehler anzuzeigen. Im folgenden sind einige Fehler und deren Ursachen aufgeführt:

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Fehler MT3001: Konnte keine AOT der assembly

Dies kann auftreten, wenn im DEBUGMODUS befinden, zur Bereitstellung auf einem Apple Watch-Gerät zu erstellen.

Um *vorübergehend* dieses Problem zu umgehen, deaktivieren Sie **inkrementelle Builds** in Watch-Erweiterung **Projektoptionen > Erstellen > WatchOS Build** Fenster:

[![](device-images/disable-incremental-sml.png "Das Kontrollkästchen inkrementelle Builds")](device-images/disable-incremental.png#lightbox)

Dies wird in einer zukünftigen Version behoben werden, nach dem inkrementelle Builds erneut aktiviert werden können, schnellere Buildzeiten nutzen.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Watch-App kann nicht gestartet werden, während des Debuggens auf Gerät

Wenn der Versuch, eine Watch-app auf einem physischen Gerät, das nur das Symbol "& laden" Spinner "Debuggen angezeigt werden (und schließlich Timeout). Dies wird in einer zukünftigen Version behoben werden; eine problemumgehung besteht darin, einen Releasebuild ausführen (die keine Debuggen zulassen).


### <a name="invalid-application-executable-or-application-verification-failed"></a>Ungültiger ausführbare Datei oder Fehler bei der Anwendung-Überprüfung.

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Ungültige ausführbare Anwendungsdatei-Warnung")

Wenn diese Nachrichten werden *auf dem Bildschirm ansehen* nach dem installieren die app versucht hat, gibt es möglicherweise einige Probleme:

- Das Überwachungsfenster-Gerät selbst wurde nicht als ein Gerät auf der Apple Developer Center hinzugefügt. Führen Sie die Anweisungen, um [Geräte ordnungsgemäß konfigurieren](#devices).

- Das Überwachungsfenster Gerät enthalten keine Entwicklung provisioning Profile für Testzwecke verwendet wird; oder nach der Apple Watch provisioning Profile hinzugefügte datenträgerressourceneinstellungen wurden nicht erneut heruntergeladen und neu installiert. Führen Sie die Anweisungen, um [konfigurieren Sie die Bereitstellung Profile ordnungsgemäß](#profiles).

- Wenn die **iOS-Geräteprotokoll** enthält `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` klicken Sie dann die Watch-App **"Info.plist"** hat die falsche **MinimumOSVersion** Wert.
  Dies dürfte **8.2** – Wenn Sie Xcode 6.3 installiert haben, müssen Sie möglicherweise manuell bearbeiten, die Quelle des, einfügen, legen Sie es auf 8.2.

- Die Watch-App **Entitlements.plist** fälschlicherweise eine Berechtigung aktiviert hat (z. B. die App-Gruppen) darin, dass darf nicht.

- Die Watch-App **App-ID** wurde falsch auf der MSDN-Website, die darf keine Berechtigung (z. B. die App-Gruppen) aktiviert.



### <a name="install-never-finished"></a>Installieren Sie nie abgeschlossen

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Dieser Fehler deutet nicht erforderlich (und ungültig) Schlüssel in die Watch-App **"Info.plist"** Datei. Schlüssel für die iOS-app oder überwachen die Erweiterung in die Watch-App sollte sollte nicht eingeschlossen werden.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"Warte Debugger eine Verbindung herstellen"

Wenn die **Anwendungsausgabe** Fenster bleibt hängen anzeigen

```csharp
waiting for debugger to connect
```

Überprüfen Sie, ob keines der NuGets der in Ihrem Projekt enthaltene Abhängigkeiten bestehen **Microsoft.Bcl.Build**. Dies wird automatisch hinzugefügt, und einige Veröffentlichung durch Microsoft Bibliotheken, darunter die gängigen [Microsoft Http Client Libraries](http://www.nuget.org/packages/Microsoft.Net.Http/).

Die **Microsoft.Bcl.Build.targets** -Datei, die hinzugefügt wird die **csproj** können verhindern, dass das Paket während der Bereitstellung von iOS-Erweiterungen. Sie können verfolgen, die [Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
Eine mögliche Lösung besteht darin die CSPROJ-Datei bearbeiten und verschieben Sie manuell die **Microsoft.Bcl.Build.targets** auf das letzte Element sein.

