---
title: Verwenden Team City mit Xamarin
description: Diese Anleitung wird erläutert, die Schritte, die mit der Verwendung von TeamCity zum Kompilieren von mobiler Anwendungen und sie mit Xamarin Test Cloud zu senden.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: 32338bc89df2ef7ee4426482b1967861f0c0e058
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="using-team-city-with-xamarin"></a>Verwenden Team City mit Xamarin

_Diese Anleitung wird erläutert, die Schritte, die mit der Verwendung von TeamCity zum Kompilieren von mobiler Anwendungen und sie mit Xamarin Test Cloud zu senden._

Entsprechend der Anleitung unter dem [Einführung in die fortlaufende Integration](~/tools/ci/intro-to-ci.md) Guide continuous Integration (CI) ist nützlich, bei der Entwicklung von mobilen Anwendungen. Es gibt viele geeignete Optionen für die fortlaufende Integration-Serversoftware. Dieses Handbuch konzentriert sich auf [TeamCity](http://www.jetbrains.com/teamcity/) aus JetBrains.

Es gibt mehrere andere Permutationen einer TeamCity-Installation. Im folgenden finden eine Liste mit einigen der folgenden:

- **Windows-Dienst** – In diesem Szenario TeamCity gestartet wird, sich bei Windows als Windows-Dienst gestartet wird. Es muss mit einem Mac-Buildhost zum Kompilieren von iOS-Anwendungen gekoppelt werden.

- **Starten der Daemon unter OS X** – konzeptionell, dies ist sehr ähnlich, für die Ausführung als Windows-Dienst im vorherigen Schritt beschrieben. Standardmäßig werden die Builds mit dem Stammkonto ausgeführt werden.

- **Benutzerkonto unter OS X** – es ist möglich, TeamCity unter einem Benutzerkonto ausgeführt wird, die bei jedem Start der Benutzer anmeldet.

Die einfachste und am einfachsten, Setup von den vorherigen Szenarien ist unter OS X TeamCity unter einem Benutzerkonto.

Es sind mehrere Schritte bei der Platzierung TeamCity einrichten:

- **Installieren von TeamCity** – die Installation von TeamCity wird in diesem Handbuch nicht behandelt. Dieses Handbuch setzt voraus, dass TeamCity installiert und unter einem Benutzerkonto ausgeführt wird. Anweisungen zum [installieren TeamCity](http://confluence.jetbrains.com/display/TCD8/Installation) finden Sie in der [TeamCity 8-Dokumentation](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) von JetBrains.

- **Vorbereiten des Servers erstellen** – dieser Schritt umfasst das Installieren der erforderlichen Software, Tools und Zertifikate erforderlich, um mobile Anwendungen erstellen und für die Verteilung vorzubereiten.

- **Creating A erstellen Script** – dieser Schritt ist nicht unbedingt erforderlich, aber ein Buildskript ist eine nützliche Hilfe zum Erstellen von Anwendungen, die für die unbeaufsichtigte. Mithilfe eines Skripts Build unterstützt bei der Problembehandlung von Builds, die auftreten sowie eine konsistente und wiederholbare Möglichkeit zum Erstellen der Binärdateien für die Verteilung, selbst wenn die fortlaufende Integration nicht üben, ist.

- **Erstellen eines TeamCity Projekte** – sobald die vorangegangenen drei Schritte abgeschlossen sind, müssen wir ein TeamCity-Projekt, das alle-Metadaten enthalten erstellen zum Abrufen von Quellcode, Kompilieren die Projekte und übermitteln die Tests mit Xamarin Test Cloud.

## <a name="requirements"></a>Anforderungen

Erfahrung mit [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) ist erforderlich.

Vertrautheit mit TeamCity 8.1 ist erforderlich. Die Installation von TeamCity ist nicht Gegenstand dieses Dokuments. Es wird vorausgesetzt, dass TeamCity auf OS X Mavericks installiert ist und über ein normales Benutzerkonto und nicht mit dem Stammkonto ausgeführt wird.

Der Build-Server muss auf einem eigenständigen Computer, die unter OS X, das für die fortlaufende Integration vorgesehen ist. Im Idealfall wird der Build-Server nicht für andere Rollen, z. B. einen Datenbankserver, Webserver oder Entwicklerarbeitsstation verantwortlich sein.

> [!IMPORTANT]
> Eine "ohne Kopfinformationen" Installation von Xamarin behandelt diesem Handbuch nicht.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Vorbereiten der Build-Server

Ein entscheidender Schritt bei der Konfiguration von einem Build-Server werden alle erforderlichen Tools, Software und Zertifikate, die zum Erstellen von mobilen Anwendungen zu installieren. Es ist wichtig, dass der Build-Server werden mobile Lösung kompilieren und alle Tests ausführen können. Um Probleme mit der Konfiguration zu minimieren, sollte die Software und Tools im gleichen Benutzerkonto installiert werden, die TeamCity gehostet wird. Im folgenden finden eine Übersicht darüber, was erforderlich ist:

1. **Visual Studio für Mac** – Dies schließt Xamarin.iOS und Xamarin.Android.
2. **Anmeldung bei der Komponentenspeicher Xamarin** – Dies ist ein optionaler Schritt, und ist nur erforderlich, wenn Ihre Anwendung Komponenten aus dem Store Xamarin-Komponente verwendet. Protokollierung proaktiv in den Speicher für die Komponente zu diesem Zeitpunkt verhindert Probleme, wenn ein Build TeamCity zum Kompilieren der Anwendung versucht.
3. **Xcode** – Xcode ist erforderlich, um die Kompilierung und melden Sie iOS-Anwendungen.
4. **Xcode-Befehlszeilentools** – hierzu finden Sie in Schritt 1 im Abschnitt Installation von der [aktualisieren Ruby mit Rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) Handbuch.
5. **Signieren von Identitäts- & Provisioning Profile** – importieren Sie die Zertifikate und ein Bereitstellungsprofil über XCode. Finden Sie auf der Apple-Handbuch [Signieren Identitäten exportieren und Provisioning Profile](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) Weitere Details.
6. **Android Schlüsselspeichern** – kopieren Sie den erforderlichen Android Schlüsselspeichern in ein Verzeichnis an, dass der Benutzer TeamCity, d. h. zugreifen können `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – Dies ist ein optionaler Schritt, wenn Ihre Anwendung Tests mit Calabash geschrieben wurde. Finden Sie unter der [installieren Calabash für OS X Mavericks](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) Handbuch und die [aktualisieren Ruby mit Rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) Weitere Informationen.

Das folgende Diagramm zeigt alle diese Komponenten:

![](teamcity-images/image1.png "Dieses Diagramm zeigt alle diese Komponenten")

Sobald die Software installiert wurde, melden Sie sich auf das Benutzerkonto, und vergewissern Sie sich, dass die Software ordnungsgemäß installiert wurde und funktioniert. Dabei sollte getestet werden, Kompilieren die Projektmappe, und senden die Anwendung in Test-Cloud. Dies kann erheblich vereinfacht werden durch Ausführen des Skripts erstellen, wie im nächsten Abschnitt beschrieben.

## <a name="create-a-build-script"></a>Erstellen Sie ein Skript erstellen

Obwohl TeamCity, behandeln alle Aspekte der Kompilierung und übermitteln die mobilen Anwendungen mit Test-Cloud durchaus möglich ist, wird empfohlen, erstellen Sie ein Skript erstellen. Ein Buildskript bietet folgende Vorteile:

1. **Dokumentation** – ein Buildskript dient als eine Form der Dokumentation auf wie die Software erstellt wird. Dies entfernt einige "Magic", die Bereitstellung der Anwendung zugeordnet ist, und ermöglicht es Entwicklern, auf die Funktionen konzentrieren.
1. **Wiederholbarkeit** – ein Buildskript wird sichergestellt, dass jedes Mal die Anwendung kompiliert und bereitgestellt wird, erfolgt auf genau die gleiche Weise, unabhängig davon, wer oder was die Arbeit ausführt. Diese repeatable Konsistenz entfernt ggf. Probleme oder Fehler, die im aufgrund einer fehlerhaft ausgeführten Build verzeichnen möglicherweise oder Benutzerfehler.
1. **Versionsverwaltung** – ein Buildskript in das Quellcodeverwaltungssystem eingeschlossen werden kann. Dies bedeutet, dass Änderungen an das Buildskript können nachverfolgt, überwacht und korrigiert werden, wenn Fehler oder Ungenauigkeiten gefunden werden.
1. **Vorbereiten der Umgebung** – ein Buildskript zählen Logik, um die erforderlichen 3rd Party Abhängigkeiten zu installieren. Dadurch wird sichergestellt, dass die Anwendungen mit den richtigen Komponenten erstellt werden.

Buildskript kann so einfach wie eine Powershell-Datei (unter Windows) oder eines Bash-Skripts (unter OS X) sein. Wenn Sie das Buildskript zu erstellen, stehen mehrere Möglichkeiten für Skriptsprachen:

- [**Neigungswinkel** ](https://github.com/jimweirich/rake) – Dies ist eine bestimmte Domäne Sprache (DSL) zum Erstellen von Projekten, die basierend auf Ruby. Neigungswinkel hat den Vorteil, dass Beliebtheit und Ökosystems Bibliotheken.

- [**Psake** ](https://github.com/psake/psake) – Dies ist eine Windows Powershell-Bibliothek für die Entwicklung von Software

- [**GEFÄLSCHTE** ](http://fsharp.github.io/FAKE/) – Hierbei handelt es sich um eine DSL basierend in F# erläutert werden, wodurch es möglich, vorhandene Bibliotheken für .NET bei Bedarf nutzen.

Welche Skriptsprache verwendet wird, hängt von Ihren Präferenzen und Anforderungen. Die [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) Beispiel enthält ein Beispiel zur Verwendung der Neigungswinkel als eine [Buildskript](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> Es ist möglich, eine XML-basierte Buildsystem wie MSBuild oder NAnt, aber diese fehlende verwenden die Ausdruckskraft und verwaltbarkeit DSL, die für die Entwicklung von Software vorgesehen ist.

### <a name="parameterizing-the-build-script"></a>Parametrisieren Buildskript

Der Prozess des Erstellens und Testen von Software erfordert Informationen, die geheim gehalten werden soll. Insbesondere erstellen ein APK möglicherweise ein Kennwort für den Schlüsselspeicher und/oder den Schlüssel im Schlüsselspeicher-Alias. Andererseits erfordert Test-Cloud einen API-Schlüssel, der dem Entwickler eindeutig ist. Diese Typen von Werten müssen nicht schwierig sein im Buildskript codiert. Stattdessen sollten sie als Variablen an das Buildskript übergeben werden.

Serverroundtrips sind Werte, wie z. B. ausgeführt, wird der iOS-Geräte-ID oder die Android-Gerät-ID, die identifizieren, welche Testen von Geräten, für die Cloud Testen verwendet werden soll. Hierbei handelt es sich nicht um Werte, die geschützt werden müssen, aber sie können von Build zu Build ändern.

Speichern diese Arten von Variablen außerhalb Buildskript vereinfacht auch Buildskript innerhalb einer Organisation, z. B. mit Entwicklern gemeinsam nutzen. Entwickler verwenden Sie dasselbe Skript möglicherweise als dem Build-Server, aber Sie können ihre eigenen Schlüsselspeichern und API-Schlüssel verwenden.

Es gibt zwei Möglichkeiten zum Speichern von diese vertrauliche Werte ein:

- **Eine Konfigurationsdatei** – richtet sich an den Test-Cloud-API-Schlüssel zu schützen, dieser Wert nicht in die Versionskontrolle aktiviert sein sollte. Die Datei kann für jeden Computer erstellt werden. Wie die Werte aus dieser Datei gelesen werden, hängt von der verwendeten Skriptsprache ab.

- **Umgebungsvariablen** – Dies können problemlos auf einer computerspezifischen Basis und werden unabhängig von der zugrunde liegenden Skriptsprache festgelegt werden.

Es gibt vor- und Nachteile auf jede dieser Optionen. TeamCity funktioniert gut mit Umgebungsvariablen, damit dieser Anleitung diese Technik wird, beim Buildskripts zu erstellen empfohlen wird.

### <a name="build-steps"></a>Buildschritte

Buildskript muss die folgenden Schritte ausführen können:

- **Kompilieren Sie die Anwendung** – dazu gehört die Anwendung mit der richtigen Bereitstellungsprofil signieren.

- **Senden Sie die Anwendung in Xamarin Test Cloud** – Dies schließt signieren und Ausrichten von der APK mit den entsprechenden Schlüsselspeicher Zip.

Diese beiden Schritte werden im folgenden ausführlicher erläutert.

#### <a name="compiling-a-xamarinios-application"></a>Beim Kompilieren einer Anwendung Xamarin.iOS

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Beim Kompilieren einer Anwendung Xamarin.Android

Verwenden Sie zum Kompilieren einer Android-Anwendung **Xbuild** (oder **Msbuild** unter Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Beachten Sie, dass die Xamarin Android-Anwendung zu kompilieren **Xbuild** verwendet das Projekt, und dass zum Erstellen der iOS-Anwendung **Xbuild** erfordert die Lösung.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Übermitteln Sie Xamarin.UITests Test Cloud

UITests übermittelt werden, mithilfe der `test-cloud.exe` Anwendung, wie im folgenden Codeausschnitt gezeigt:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Wenn der Test ausgeführt wird, werden die Testergebnisse in Form einer NUnit Stil XML-Datei namens zurückgegeben **report.xml**. TeamCity zeigt die Informationen in das Protokoll zu erstellen.

Weitere Informationen zum Übermitteln von UITests Test Cloud finden Sie in die Xamarin Handbuch auf [übermitteln Tests mit Test Cloud](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Übermitteln von Calabash Tests zum Testen der Cloud

Calabash Tests übermittelt werden, mithilfe der `test-cloud` gem, wie im folgenden Codeausschnitt gezeigt:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Um eine Android-Anwendung zum Testen der Cloud zu senden, ist es zunächst die APK-Testserver mit Calabash Android neu erstellt:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Weitere Informationen zum Senden von Calabash Tests finden Sie in die Xamarin Handbuch auf [übermitteln Calabash Tests mit Test Cloud](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>Erstellen eines Projekts TeamCity

Nachdem TeamCity installiert ist, und Visual Studio für Mac können das Projekt erstellen, ist es Zeit zum Erstellen eines Projekts in TeamCity zum Erstellen des Projekts und Test-Cloud zu übermitteln.

1. Durch Anmeldung TeamCity über den Webbrowser gestartet. Navigieren Sie zu der Stammprojekt:

    ![Navigieren Sie zum Stammverzeichnis Projekt](teamcity-images/image2.png "navigieren Sie zum Stammverzeichnis Projekt") unterhalb der Stammprojekt, erstellen Sie ein neues untergeordnete Projekt:

    ![Navigieren Sie zu der Stamm Underneath the Root Projekten, erstellen Sie ein neues Projekt für die untergeordnete](teamcity-images/image3.png "navigieren Sie zum Stamm Projekt Underneath the Root Projekt, erstellen Sie ein neues untergeordnete Projekt")
2. Sobald das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Build-Konfiguration hinzu:

    ![Sobald das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Buildkonfiguration](teamcity-images/image5.png "nachdem das untergeordnete Projekt erstellt wurde, Hinzufügen einer neuen Build-Konfiguration")
3. Fügen Sie ein Projekt VCS an der Konfiguration zu erstellen. Dies erfolgt über den Bildschirm für die Version des Steuerelements die Einstellung:

    ![Dies erfolgt über die Einstellung der Version des Steuerelements Bildschirm](teamcity-images/image6.png "Dies erfolgt über die Einstellung der Version des Steuerelements-Bildschirm")

    Ist kein VCS-Projekt erstellt haben, müssen Sie die Option zum Erstellen über die neue VCS Stammseite unten:

    ![Ist kein VCS-Projekt erstellt haben, haben Sie die Option zum Erstellen einer auf der Seite neue VCS Stamm](teamcity-images/image7.png "ist kein VCS-Projekt erstellt haben, haben Sie die Möglichkeit, auf der Seite neue VCS Stamm erstellen")

    Nachdem die VCS Stamm angehängt wurden, erkennen TeamCity wird Auschecken des Projekts und versuchen Sie, automatisch Buildschritte. Wenn Sie mit TeamCity vertraut sind, können Sie eine der erkannten Buildschritte auswählen. Sie können ruhig, erkannte Buildschritte fürs ignoriert werden sollen.

4. Als Nächstes konfigurieren Sie einen Trigger erstellen. Dies wird von einem Build zur Warteschlange, wenn bestimmte Bedingungen erfüllt sind, z. B. wenn ein Benutzer Code im Repository ein Commit ausgeführt wird. Der folgende Screenshot zeigt, wie einen Buildtrigger hinzugefügt wird:

    ![Diese bildschirmabbildung zeigt, wie einen Buildtrigger hinzugefügt](teamcity-images/image8.png "diesem Screenshot zeigt, wie ein Buildtrigger hinzugefügt") ein Beispiel zum Konfigurieren eines Triggers Build kann im folgenden Screenshot angezeigt werden:

    ![Ein Beispiel zum Konfigurieren eines Triggers Build sehen Sie in diesem Screenshot](teamcity-images/image9.png "ein Beispiel zum Konfigurieren eines Build-Triggers kann in diesem Screenshot angezeigt werden")

5. Im vorherige Abschnitt parametrisieren das Skript erstellen vorgeschlagen, einige Werte als Umgebungsvariablen gespeichert. Diese Variablen können die Buildkonfiguration über den Bildschirm Parameter hinzugefügt werden. Fügen Sie die Variablen für den Test-Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android-Gerät-ID wie im folgenden Screenshot gezeigt:

    ![Fügen Sie die Variablen für den Test-Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android-Geräte-ID](teamcity-images/image11.png "fügen die Variablen für den Test-Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android-Geräte-ID")

6. Der letzte Schritt besteht darin einen Buildschritt hinzuzufügen, mit der das Buildskript zum Kompilieren der Anwendung und in die Warteschlange einreihen der Anwendung in Test Cloud aufgerufen wird. Der folgende Screenshot ist ein Beispiel für einen Schritt zur Erstellung, der eine Rakefile zum Erstellen einer Anwendung verwendet:

    ![Diese Abbildung ist ein Beispiel für einen Schritt der Erstellung, die eine Rakefile zum Erstellen einer Anwendung verwendet](teamcity-images/image12.png "diesem Screenshot ist ein Beispiel für einen Schritt zur Erstellung, die eine Rakefile zum Erstellen einer Anwendung verwendet")

7. An diesem Punkt ist die Build-Konfiguration abgeschlossen. Es ist ratsam, das Auslösen eines Builds überprüfen Sie, dass das Projekt ordnungsgemäß konfiguriert ist. Eine gute Möglichkeit hierzu ist, eine kleine, der nicht signifikante Änderung für das Repository zu übernehmen. TeamCity sollte das Commit zu erkennen und Starten eines Builds.

8. Nachdem der Build abgeschlossen wurde, überprüfen Sie das Buildprotokoll und feststellen Sie, ob Probleme oder Warnungen für den Build, die Ihre Aufmerksamkeit erfordern.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt wie TeamCity mit Xamarin Mobile Anwendungen erstellen und sie in Test-Cloud zu senden. Erstellen eines Builds Skripts zum Automatisieren des Buildprozesses besprochen. Buildskript übernimmt die Kompilierung der Anwendung senden mit Test-Cloud und die Ergebnisse wartet

Klicken Sie dann behandelt wird wie ein Projekt in TeamCity zu erstellen, wird einen Build zur Warteschlange jedes Mal ein Entwickler Code ein Commit ausgeführt wird und Buildskript aufgerufen wird.

## <a name="related-links"></a>Verwandte Links

- [Übermitteln von Tests mit Xamarin Test Cloud (UITest)](https://developer.xamarin.com~/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Übermitteln von Tests mit Xamarin Test Cloud (Calabash)](https://developer.xamarin.com~/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Installieren und Konfigurieren von TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
