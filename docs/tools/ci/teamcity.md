---
title: Verwenden von Team City mit Xamarin
description: In diesem Handbuch werden die Schritte erläutert, die mit der Verwendung von TeamCity zum Kompilieren mobiler Anwendungen verbunden sind, und diese dann an App Center Test übermitteln.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 04/01/2020
ms.openlocfilehash: 6ecd453180e8c392ba7d7527778617eb40950a9e
ms.sourcegitcommit: 6f3281a32017cfcebadde8a2d6e10651a277828f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587466"
---
# <a name="using-team-city-with-xamarin"></a>Verwenden von Team City mit Xamarin

_In diesem Handbuch werden die Schritte erläutert, die mit der Verwendung von TeamCity zum Kompilieren mobiler Anwendungen verbunden sind, und diese dann an App Center Test übermitteln._

Wie im [Leitfaden "Einführung in die kontinuierliche Integration"](~/tools/ci/intro-to-ci.md) erläutert, ist die kontinuierliche Integration (Continuous Integration, CI) eine nützliche Praxis bei der Entwicklung hochwertiger mobiler Anwendungen. Es gibt viele praktikable Optionen für kontinuierliche Integrationsserversoftware; Dieser Leitfaden wird sich auf [TeamCity](https://www.jetbrains.com/teamcity/) von JetBrains konzentrieren.

Es gibt mehrere verschiedene Permutationen einer TeamCity-Installation. In der folgenden Liste werden einige dieser Permutationen beschrieben:

- **Windows Service** – In diesem Szenario wird TeamCity gestartet, wenn Windows als Windows-Dienst gestartet wird. Es muss mit einem Mac Build-Host gekoppelt werden, um iOS-Anwendungen zu kompilieren.

- **Starten Sie Daemon unter OS X** – Konzeptionell ähnelt dies der Ausführung als Windows-Dienst, der im vorherigen Schritt beschrieben wurde. Standardmäßig werden die Builds unter dem Stammkonto ausgeführt.

- **Benutzerkonto unter OS X** – Es ist möglich, TeamCity unter einem Benutzerkonto auszuführen, das jedes Mal gestartet wird, wenn sich der Benutzer anmeldet.

Von den vorherigen Szenarien ist die Ausführung von TeamCity unter einem Benutzerkonto unter OS X die einfachste und einfachste einzurichten.

Beim Einrichten von TeamCity sind mehrere Schritte erforderlich:

- **Installieren von TeamCity** – Die Installation von TeamCity wird in diesem Handbuch nicht behandelt. In diesem Handbuch wird davon ausgegangen, dass TeamCity unter einem Benutzerkonto installiert und ausgeführt wird. Anweisungen zur [Installation von TeamCity](https://confluence.jetbrains.com/display/TCD8/Installation) finden Sie in der [TeamCity 8-Dokumentation](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) von JetBrains.

- **Vorbereiten des Buildservers** – In diesem Schritt werden die erforderlichen Software, Tools und Zertifikate installiert, die zum Erstellen mobiler Anwendungen und deren Vorbereitung für die Verteilung erforderlich sind.

- **Erstellen eines Buildskripts** – Dieser Schritt ist nicht unbedingt erforderlich, aber ein Buildskript ist eine nützliche Hilfe beim unbeaufsichtigten Erstellen von Anwendungen. Die Verwendung eines Buildskripts hilft bei der Fehlerbehebung bei Buildproblemen, die auftreten können, und bietet eine konsistente, wiederholbare Möglichkeit, die Binärdateien für die Verteilung zu erstellen, auch wenn die kontinuierliche Integration nicht praktiziert wird.

- **Erstellen eines TeamCity-Projekts** – Sobald die vorherigen drei Schritte abgeschlossen sind, müssen wir ein TeamCity-Projekt erstellen, das alle Metadaten enthält, die zum Abrufen des Quellcodes, Kompilieren der Projekte und Senden der Tests an App Center Test erforderlich sind.

## <a name="requirements"></a>Requirements (Anforderungen)

Erfahrung mit [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) ist erforderlich.

Die Vertrautheit mit TeamCity 8.1 ist erforderlich. Die Installation von TeamCity geht über den Rahmen dieses Dokuments hinaus. Es wird davon ausgegangen, dass TeamCity auf OS X Mavericks installiert ist und unter einem regulären Benutzerkonto und nicht unter dem Stammkonto ausgeführt wird.

Der Buildserver sollte ein eigenständiger Computer mit OS X sein, der der kontinuierlichen Integration gewidmet ist. Im Idealfall ist der Buildserver nicht für andere Rollen verantwortlich, z. B. für einen Datenbankserver, einen Webserver oder eine Entwicklerarbeitsstation.

> [!IMPORTANT]
> Dieser Leitfaden deckt keine "kopflose" Installation von Xamarin ab.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Vorbereiten des Buildservers

Ein wichtiger Schritt bei der Konfiguration eines Buildservers besteht darin, alle erforderlichen Tools, Software und Zertifikate zum Erstellen der mobilen Anwendungen zu installieren. Es ist wichtig, dass der Buildserver die mobile Lösung kompilieren und alle Tests ausführen kann. Um Konfigurationsprobleme zu minimieren, sollten die Software und Tools in demselben Benutzerkonto installiert werden, das TeamCity hostet. In der folgenden Liste wird erläutert, was erforderlich ist:

1. **Visual Studio für Mac** – Dazu gehören Xamarin.iOS und Xamarin.Android.
2. **Melden Sie sich im Xamarin Component Store** an – Dieser Schritt ist optional und nur erforderlich, wenn Ihre Anwendung Komponenten aus dem Xamarin Component Store verwendet. Die proaktive Anmeldung im Komponentenspeicher zu diesem Zeitpunkt verhindert Probleme, wenn ein TeamCity-Build versucht, die Anwendung zu kompilieren.
3. **Xcode** – Xcode ist zum Kompilieren und Signieren von iOS-Anwendungen erforderlich.
4. **Xcode Command-Line Tools** – Dies wird in Schritt 1 des Installationsabschnitts des [Handbuchs "Ruby aktualisieren" mit rbenv](https://github.com/calabash/calabash-ios/wiki) beschrieben.
5. **Signieren von Identitäts- & Bereitstellungsprofilen** – Importieren Sie die Zertifikate und das Bereitstellungsprofil über XCode. Weitere Informationen finden Sie in Apples Leitfaden [zum Exportieren von Signaturidentitäten und Bereitstellungsprofilen.](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)
6. **Android Keystores** – Kopieren Sie die erforderlichen Android-Keystores in ein Verzeichnis, auf das der TeamCity-Benutzer Zugriff hat, d. h. `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – Dies ist ein optionaler Schritt, wenn Ihre Anwendung Tests mit Calabash geschrieben hat. Weitere Informationen finden Sie im Handbuch Installieren von [Calabash auf OS X Mavericks](https://github.com/calabash/calabash-ios/wiki) und im Handbuch zum Aktualisieren von [Ruby mit rbenv.](https://github.com/calabash/calabash-ios/wiki)

Das folgende Diagramm veranschaulicht alle diese Komponenten:

![](teamcity-images/image1.png "This diagram illustrates all of these components")

Sobald die gesamte Software installiert ist, melden Sie sich beim Benutzerkonto an und bestätigen Sie, dass die gesamte Software ordnungsgemäß installiert ist und funktioniert. Dies sollte das Kompilieren der Lösung und das Senden der Anwendung an App Center Test umfassen. Dies kann durch Ausführen des Buildskripts vereinfacht werden, wie im nächsten Abschnitt beschrieben.

## <a name="create-a-build-script"></a>Erstellen eines Buildskripts

Obwohl es TeamCity möglich ist, alle Aspekte des Kompilierens und Übermittelns mobiler Anwendungen an App Center Test selbst zu behandeln; Es wird empfohlen, ein Buildskript zu erstellen. Ein Buildskript bietet die folgenden Vorteile:

1. **Dokumentation** – Ein Buildskript dient als Dokumentation darüber, wie die Software erstellt wird. Dadurch werden einige der "Magie" entfernt, die mit der Bereitstellung der Anwendung verbunden ist, und Entwickler können sich auf Funktionalität konzentrieren.
1. **Wiederholbarkeit** – Ein Buildskript stellt sicher, dass jedes Mal, wenn die Anwendung kompiliert und bereitgestellt wird, dies auf die gleiche Weise geschieht, unabhängig davon, wer oder was die Arbeit auslöst. Diese wiederholbare Konsistenz entfernt alle Probleme oder Fehler, die aufgrund eines falsch ausgeführten Builds oder eines menschlichen Fehlers auftreten können.
1. **Versionierung** – Ein Buildskript kann in das Quellcodeverwaltungssystem eingeschlossen werden. Dies bedeutet, dass Änderungen am Buildskript nachverfolgt, überwacht und korrigiert werden können, wenn Fehler oder Ungenauigkeiten gefunden werden.
1. **Vorbereiten der Umgebung** – Ein Buildskript kann Logik enthalten, um alle erforderlichen Abhängigkeiten von Drittanbietern zu installieren. Dadurch wird sichergestellt, dass die Anwendungen mit den richtigen Komponenten erstellt werden.

Das Buildskript kann so einfach sein wie eine PowerShell-Datei (unter Windows) oder ein Bash-Skript (unter OS X). Beim Erstellen des Buildskripts gibt es mehrere Möglichkeiten für Skriptsprachen:

- [**Rake**](https://github.com/jimweirich/rake) – Dies ist eine Domain-Specific Language (DSL) für Bauprojekte, basierend auf Ruby. Rake hat den Vorteil der Popularität und ein reiches Ökosystem von Bibliotheken.

- [**psake**](https://github.com/psake/psake) – Dies ist eine Windows PowerShell-Bibliothek zum Erstellen von Software

- [**FAKE**](https://fsharp.github.io/FAKE/) – Dies ist eine DSL mit Sitz in F, die es ermöglicht, vorhandene .NET-Bibliotheken bei Bedarf zu verwenden.

Welche Skriptsprache verwendet wird, hängt von Ihren Vorlieben und Anforderungen ab.

> [!NOTE]
> Es ist möglich, ein XML-basiertes Buildsystem wie MSBuild oder NAnt zu verwenden, aber diesen fehlt die Ausdruckskraft und Wartbarkeit einer DSL, die sich dem Erstellen von Software widmet.

### <a name="parameterizing-the-build-script"></a>Parametrierung des Buildskripts

Der Prozess des Erstellens und Testens von Software erfordert Informationen, die geheim gehalten werden sollten. Das Erstellen einer APK erfordert möglicherweise ein Kennwort für den Keystore und/oder den Schlüsselalias im Keystore. Ebenso erfordert App Center Test einen [API-Schlüssel,](/appcenter/api-docs/) der für einen Entwickler eindeutig ist. Diese Wertetypen sollten im Buildskript nicht hartcodiert sein. Stattdessen sollten sie als Variablen an das Buildskript übergeben werden.

Weniger sensibel sind Werte wie die iOS-Geräte-ID oder die Android-Geräte-ID, die angeben, welche Geräte App Center für Testläufe verwenden sollte. Dies sind keine Werte, die geschützt werden müssen, aber sie können von Build zu Build wechseln.

Das Speichern dieser Variablentypen außerhalb des Buildskripts erleichtert auch die gemeinsame Nutzung des Buildskripts in einer Organisation, z. B. für Entwickler. Entwickler können genau das gleiche Skript wie der Buildserver verwenden, aber ihre eigenen Schlüsselspeicher und [API-Schlüssel](/appcenter/api-docs/)verwenden.

Es gibt zwei Möglichkeiten zum Speichern dieser sensiblen Werte:

- **Eine Konfigurationsdatei** – Um den [API-Schlüssel](/appcenter/api-docs/) zu schützen, sollte dieser Wert nicht in die Versionskontrolle eingecheckt werden. Die Datei kann für jeden Computer erstellt werden. Wie Werte aus dieser Datei gelesen werden, hängt von der verwendeten Skriptsprache ab.

- **Umgebungsvariablen** – Diese können einfach pro Computer festgelegt werden und sind unabhängig von der zugrunde liegenden Skriptsprache.

Jede dieser Entscheidungen hat Vor- und Nachteile. TeamCity funktioniert gut mit Umgebungsvariablen, daher wird diese Technik in diesem Handbuch beim Erstellen von Buildskripts empfohlen.

### <a name="build-steps"></a>Build-Schritte

Das Buildskript muss die folgenden Schritte ausführen:

- **Kompilieren der Anwendung** – Dazu gehört auch das Signieren der Anwendung mit dem richtigen Bereitstellungsprofil.

- **Einreichen der Anwendung an Xamarin Test Cloud** – Dazu gehört das Signieren und Zip-Ausrichten der APK mit dem entsprechenden Keystore.

Diese beiden Schritte werden im Folgenden näher erläutert.

#### <a name="compiling-a-xamarinios-application"></a>Kompilieren einer Xamarin.iOS-Anwendung

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Kompilieren einer Xamarin.Android-Anwendung

Um eine Android-Anwendung zu kompilieren, verwenden Sie **xbuild** (oder **msbuild** unter Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```
Beim Kompilieren der Android-Anwendung **xbuild** wird das Projekt verwendet, während die iOS-Anwendung **xbuild** die Lösung verwendet.

#### <a name="submitting-xamarinuitests-to-app-center"></a>Übermitteln von Xamarin.UITests an App Center

UITests werden mit der [App Center CLI](https://github.com/microsoft/appcenter-cli)übermittelt, wie im folgenden Ausschnitt gezeigt:

```bash
appcenter test run uitest --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --merge-nunit-xml report.xml --build-dir pathToUITestBuildDir
```

Wenn der Test ausgeführt wird, werden die Testergebnisse in Form einer XML-Datei im NUnit-Stil namens **report.xml**zurückgegeben. TeamCity zeigt die Informationen im Buildprotokoll an.

Weitere Informationen zum Senden von UITests an das App Center finden Sie unter Vorbereiten von [Xamarin.Android-Apps](/appcenter/test-cloud/uitest/preparing-for-upload-android) oder [Vorbereiten von Xamarin.iOS-Apps](/appcenter/test-cloud/uitest/preparing-for-upload-ios).

#### <a name="submitting-calabash-tests-to-app-center"></a>Übermitteln von Calabash-Tests an Das App Center

Calabash-Tests werden mit der [App Center CLI](https://github.com/microsoft/appcenter-cli)übermittelt, wie im folgenden Ausschnitt gezeigt:

```bash
appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --project-dir pathToProjectDir
```

Um eine Android-Anwendung an App Center Test zu senden, müssen Sie den APK-Testserver zuerst mit calabash-android neu erstellen:

```bash
$ calabash-android build </path/to/signed/APK>
$ appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK> --project-dir pathToProjectDir
```

Weitere Informationen zum Einreichen von Calabash-Tests finden Sie im Xamarin-Leitfaden [zum Übermitteln von Calabash-Tests in test Cloud](https://github.com/calabash/calabash-ios/wiki).

## <a name="creating-a-teamcity-project"></a>Erstellen eines TeamCity-Projekts

Sobald TeamCity installiert ist und Visual Studio für Mac Ihr Projekt erstellen kann, ist es an der Zeit, ein Projekt in TeamCity zu erstellen, um das Projekt zu erstellen und es an Das App Center zu übermitteln.

1. Angefangen durch die Anmeldung bei TeamCity über den Webbrowser. Navigieren Sie zum Stammprojekt:

    ![Navigieren zum Root-Projekt](teamcity-images/image2.png "Navigieren zum Root-Projekt") Erstellen Sie unter dem Stammprojekt ein neues Unterprojekt:

    ![Navigieren Sie zum Stammprojekt unterhalb des Stammprojekts, erstellen Sie ein neues Unterprojekt](teamcity-images/image3.png "Navigieren Sie zum Stammprojekt unterhalb des Stammprojekts, erstellen Sie ein neues Unterprojekt")
2. Nachdem das Unterprojekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu:

    ![Nachdem das Teilprojekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu.](teamcity-images/image5.png "Nachdem das Unterprojekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu.")
3. Fügen Sie ein VCS-Projekt an die Buildkonfiguration an. Dies geschieht über den Bildschirm Versionskontrolle Einstellung:

    ![Dies geschieht über den Bildschirm Versionskontrolle](teamcity-images/image6.png "Dies geschieht über den Bildschirm Versionskontrolle")

    Wenn kein VCS-Projekt erstellt wurde, können Sie eines auf der unten gezeigten Seite "Neue VCS-Stammseite" erstellen:

    ![Wenn kein VCS-Projekt erstellt wurde, können Sie eines auf der Seite Neue VCS-Stammseite erstellen.](teamcity-images/image7.png "Wenn kein VCS-Projekt erstellt wurde, haben Sie die Möglichkeit, eines über die Seite Neue VCS-Stammseite zu erstellen.")

    Sobald der VCS-Stamm angefügt wurde, überprüft TeamCity das Projekt und versucht, die Buildschritte automatisch zu erkennen. Wenn Sie mit TeamCity vertraut sind, können Sie einen der erkannten Buildschritte auswählen. Es ist sicher, die erkannten Buildschritte vorerst zu ignorieren.

4. Konfigurieren Sie als Nächstes einen Build-Trigger. Dadurch wird ein Build in die Warteschlange eingereiht, wenn bestimmte Bedingungen erfüllt sind, z. B. wenn ein Benutzer Code in das Repository überträgt. Der folgende Screenshot zeigt, wie Sie einen Build-Trigger hinzufügen:

    ![Dieser Screenshot zeigt, wie Sie einen Build-Trigger hinzufügen](teamcity-images/image8.png "Dieser Screenshot zeigt, wie Sie einen Build-Trigger hinzufügen") Ein Beispiel für die Konfiguration eines Build-Triggers finden Sie im folgenden Screenshot:

    ![Ein Beispiel für die Konfiguration eines Build-Triggers finden Sie in diesem Screenshot](teamcity-images/image9.png "Ein Beispiel für die Konfiguration eines Build-Triggers finden Sie in diesem Screenshot")

5. Im vorherigen Abschnitt, Parametrierung des Buildskripts, wurde vorgeschlagen, einige Werte als Umgebungsvariablen zu speichern. Diese Variablen können der Buildkonfiguration über den Bildschirm Parameter hinzugefügt werden. Fügen Sie die Variablen für den App [Center-API-Schlüssel](/appcenter/api-docs/), die iOS-Geräte-ID und die Android-Geräte-ID hinzu, wie im Screenshot unten gezeigt:

    ![Hinzufügen der Variablen für den App Center-Test-API-Schlüssel, die iOS-Geräte-ID und die Android-Geräte-ID](teamcity-images/image11.png "Hinzufügen der Variablen für den Test Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android-Geräte-ID")

6. Der letzte Schritt besteht darin, einen Buildschritt hinzuzufügen, der das Buildskript aufruft, um die Anwendung zu kompilieren und die Anwendung in App Center Test zu verzahnen. Der folgende Screenshot ist ein Beispiel für einen Buildschritt, der ein Rakefile zum Erstellen einer Anwendung verwendet:

    ![Dieser Screenshot ist ein Beispiel für einen Buildschritt, der ein Rakefile zum Erstellen einer Anwendung verwendet.](teamcity-images/image12.png "Dieser Screenshot ist ein Beispiel für einen Buildschritt, der ein Rakefile zum Erstellen einer Anwendung verwendet.")

7. Zu diesem Zeitpunkt ist die Buildkonfiguration abgeschlossen. Es ist eine gute Idee, einen Build auszulösen, um zu bestätigen, dass das Projekt ordnungsgemäß konfiguriert ist. Eine gute Möglichkeit, dies zu tun, besteht darin, eine kleine, unbedeutende Änderung in das Repository zu übertragen. TeamCity sollte den Commit erkennen und einen Build starten.

8. Nachdem der Build abgeschlossen ist, überprüfen Sie das Buildprotokoll, und überprüfen Sie, ob Probleme oder Warnungen mit dem Build auftreten, die Aufmerksamkeit erfordern.

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde erläutert, wie Sie TeamCity zum Erstellen von Xamarin Mobile-Anwendungen verwenden und diese dann an App Center Test übermitteln. Wir haben das Erstellen eines Buildskripts zur Automatisierung des Buildprozesses erläutert. Das Buildskript kümmert sich um das Kompilieren der Anwendung, das Senden an App Center Test und das Warten auf die Ergebnisse.

Anschließend wurde erläutert, wie sie ein Projekt in TeamCity erstellen, das jedes Mal, wenn ein Entwickler Code feststellt und das Buildskript aufruft, einen Build in die Warteschlange stellt.

## <a name="related-links"></a>Verwandte Links

- [Vorbereiten von Xamarin.Android-Apps](/appcenter/test-cloud/uitest/preparing-for-upload-android)
- [Vorbereiten von Xamarin.iOS-Apps](/appcenter/test-cloud/uitest/preparing-for-upload-ios)
- [Installieren und Konfigurieren von TeamCity](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
