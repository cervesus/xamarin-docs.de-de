---
title: Verwenden von Team City mit xamarin
description: In diesem Leitfaden werden die Schritte beschrieben, die bei der Verwendung von TeamCity zum Kompilieren mobiler Anwendungen und deren Übermittlung an Xamarin Test Cloud erläutert werden.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 5a16ec338d5929a217ee2e4a622bdce4da617e86
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029800"
---
# <a name="using-team-city-with-xamarin"></a>Verwenden von Team City mit xamarin

_In diesem Leitfaden werden die Schritte beschrieben, die bei der Verwendung von TeamCity zum Kompilieren mobiler Anwendungen und deren Übermittlung an Xamarin Test Cloud erläutert werden._

Wie im Leitfaden [Einführung in Continuous Integration](~/tools/ci/intro-to-ci.md) erläutert, ist Continuous Integration (CI) eine nützliche Vorgehensweise bei der Entwicklung mobiler Anwendungen. Es gibt viele geeignete Optionen für Continuous Integration Server-Software. Dieser Leitfaden konzentriert sich auf [TeamCity](https://www.jetbrains.com/teamcity/) von JetBrains.

Es gibt mehrere verschiedene Permutationen einer TeamCity-Installation. Im folgenden finden Sie eine Liste der folgenden:

- **Windows-Dienst** – in diesem Szenario wird TeamCity gestartet, wenn Windows als Windows-Dienst gestartet wird. Er muss mit einem Mac-buildhost gekoppelt werden, um IOS-Anwendungen kompilieren zu können.

- **Der Launch-Daemon unter OS X** – konzeptionell ist dies mit dem Ausführen von als Windows-Dienst vergleichbar, der im vorherigen Schritt beschrieben wurde. Die Builds werden standardmäßig unter dem Root-Konto ausgeführt.

- **Benutzerkonto unter OS X** – es ist möglich, TeamCity unter einem Benutzerkonto auszuführen, das bei jeder Anmeldung des Benutzers gestartet wird.

In den vorherigen Szenarios ist das Ausführen von TeamCity unter einem Benutzerkonto unter OS X das einfachste und einfachste Setup.

Die Einrichtung von TeamCity umfasst mehrere Schritte:

- **Installieren von TeamCity** – die Installation von TeamCity wird in diesem Handbuch nicht behandelt. In dieser Anleitung wird davon ausgegangen, dass TeamCity installiert ist und unter einem Benutzerkonto ausgeführt wird. Anweisungen zum [Installieren von TeamCity](https://confluence.jetbrains.com/display/TCD8/Installation) finden Sie in der [Dokumentation zu TeamCity 8](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) von JetBrains.

- **Vorbereiten des Buildservers** – dieser Schritt umfasst die Installation der erforderlichen Software, Tools und Zertifikate, die zum Erstellen mobiler Anwendungen und zur Vorbereitung auf die Verteilung erforderlich sind.

- **Erstellen eines Buildskripts** – dieser Schritt ist nicht unbedingt erforderlich, aber ein Buildskript ist eine hilfreiche Hilfe zum unbeaufsichtigten Erstellen von Anwendungen. Die Verwendung eines Buildskripts hilft bei der Problembehandlung von Buildproblemen, die auftreten können, und bietet eine konsistente, wiederholbare Möglichkeit zum Erstellen der Binärdateien für die Verteilung, auch wenn Continuous Integration nicht ausgeführt wird.

- Erstellen **eines TeamCity-Projekts** – nachdem die vorangegangenen drei Schritte abgeschlossen sind, müssen wir ein TeamCity-Projekt erstellen, das alle Metadaten enthält, die zum Abrufen des Quellcodes, zum Kompilieren der Projekte und zum übermitteln der Tests an Xamarin Test Cloud erforderlich sind.

## <a name="requirements"></a>Anforderungen

Die Verwendung von [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) ist erforderlich.

Vertrautheit mit TeamCity 8,1 ist erforderlich. Die Installation von TeamCity geht über den Rahmen dieses Dokuments hinaus. Es wird davon ausgegangen, dass TeamCity auf OS X Mavericks installiert ist und unter einem regulären Benutzerkonto und nicht dem Stammkonto ausgeführt wird.

Der Buildserver sollte ein eigenständiger Computer mit OS X sein, der für Continuous Integration reserviert ist. Im Idealfall ist der Buildserver nicht für andere Rollen verantwortlich, wie z. b. einen Datenbankserver, einen Webserver oder eine Entwickler Arbeitsstation.

> [!IMPORTANT]
> In dieser Anleitung wird nicht die "Monitor lose" Installation von xamarin behandelt.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Vorbereiten des Buildservers

Ein wichtiger Schritt beim Konfigurieren eines Buildservers ist die Installation aller erforderlichen Tools, Software und Zertifikate, um die mobilen Anwendungen zu erstellen. Es ist wichtig, dass der Buildserver in der Lage ist, die mobile Lösung zu kompilieren und alle Tests auszuführen. Um Konfigurationsprobleme zu minimieren, sollten die Software und die Tools in demselben Benutzerkonto installiert werden, das TeamCity gehostet. Im folgenden finden Sie eine Liste der erforderlichen Komponenten:

1. **Visual Studio für Mac** – einschließlich xamarin. IOS und xamarin. Android.
2. **Melden Sie sich beim xamarin-Komponenten Speicher an** – Dies ist ein optionaler Schritt, der nur erforderlich ist, wenn Ihre Anwendung Komponenten aus dem xamarin-Komponenten Speicher verwendet. Wenn Sie sich zu diesem Zeitpunkt proaktiv beim Komponenten Speicher anmelden, werden Probleme vermieden, wenn ein TeamCity-Build versucht, die Anwendung zu kompilieren.
3. **Xcode** – Xcode ist erforderlich, um IOS-Anwendungen zu kompilieren und zu signieren.
4. **Xcode-Befehlszeilen Tools** – Dies wird in Schritt 1 des Abschnitts "Installation" des Handbuchs " [Aktualisieren von Ruby mit rbenv](https://github.com/calabash/calabash-ios/wiki) " beschrieben.
5. **Signieren von Identitäts & Bereitstellungs Profilen** – importieren Sie die Zertifikate und das Bereitstellungs Profil über Xcode. Weitere Informationen finden Sie im Apple-Handbuch zum [Exportieren von Signierungs Identitäten und Bereitstellungs Profilen](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) .
6. **Android-Keystores** – kopieren Sie die erforderlichen Android-Keystores in ein Verzeichnis, auf das der TeamCity-Benutzer Zugriff hat, d. h. `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – Dies ist ein optionaler Schritt, wenn in Ihrer Anwendung Tests mit Calabash geschrieben wurden. Weitere Informationen finden Sie im Handbuch [Installieren von Calabash on OS X Mavericks](https://github.com/calabash/calabash-ios/wiki) und im Leitfaden zum [Aktualisieren von Ruby mit rbenv](https://github.com/calabash/calabash-ios/wiki) .

Im folgenden Diagramm werden alle folgenden Komponenten veranschaulicht:

![](teamcity-images/image1.png "This diagram illustrates all of these components")

Nachdem die Software installiert wurde, melden Sie sich beim Benutzerkonto an, und vergewissern Sie sich, dass die gesamte Software ordnungsgemäß installiert ist und funktioniert. Dies sollte das Kompilieren der Lösung und das übermitteln der Anwendung an Test Cloud einschließen. Dies kann durch Ausführen des Buildskripts erheblich vereinfacht werden, wie im nächsten Abschnitt beschrieben.

## <a name="create-a-build-script"></a>Erstellen eines Buildskripts

Es ist zwar durchaus möglich, dass TeamCity alle Aspekte der Kompilierung und Übermittlung der mobilen Anwendungen an die Test Cloud selbst verarbeitet, es wird jedoch dringend empfohlen, ein Buildskript zu erstellen. Ein Buildskript bietet die folgenden Vorteile:

1. **Dokumentation** – ein Buildskript dient als Dokumentation zur Art der Software Erstellung. Dies entfernt einige der "Magic", die mit der Bereitstellung der Anwendung verknüpft ist, und ermöglicht es Entwicklern, sich auf die Funktionalität zu konzentrieren.
1. **Wiederholbarkeit** – ein Buildskript stellt sicher, dass jedes Mal, wenn die Anwendung kompiliert und bereitgestellt wird, auf genau dieselbe Weise erfolgt, unabhängig davon, wer oder was die Arbeit bewirkt. Diese wiederholbare Konsistenz entfernt alle Probleme oder Fehler, die aufgrund eines falsch ausgeführten Builds oder menschlichen Fehlers auftreten können.
1. **Versionierung** – ein Buildskript kann im Quell Code Verwaltungssystem enthalten sein. Dies bedeutet, dass Änderungen am Buildskript nachverfolgt, überwacht und korrigiert werden können, wenn Fehler oder Ungenauigkeiten gefunden werden.
1. **Vorbereiten der Umgebung** – ein Buildskript kann Logik zum Installieren erforderlicher Drittanbieter-Abhängigkeiten enthalten. Dadurch wird sichergestellt, dass die Anwendungen mit den entsprechenden Komponenten erstellt werden.

Das Buildskript kann so einfach wie eine PowerShell-Datei (unter Windows) oder ein Bash-Skript (unter OS X) sein. Beim Erstellen des Buildskripts stehen verschiedene Auswahlmöglichkeiten für Skriptsprachen zur Verfügung:

- [**Rake**](https://github.com/jimweirich/rake) – Dies ist eine domänenspezifische Sprache (DSL) zum Entwickeln von Projekten, die auf Ruby basiert. "Rake" hat den Vorteil von Beliebtheit und einem umfangreichen Ökosystem von Bibliotheken.

- [**psake**](https://github.com/psake/psake) – Dies ist eine Windows PowerShell-Bibliothek zum Entwickeln von Software.

- [Fake](https://fsharp.github.io/FAKE/) – Dies ist eine DSL-basierte F# , die es ermöglicht, bei Bedarf vorhandene .NET-Bibliotheken zu verwenden.

Welche Skriptsprache verwendet wird, hängt von Ihren Vorlieben und Anforderungen ab. Das [TaskyPro-Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash).Beispiel enthält ein Beispiel für die Verwendung von „Rake“ als [Buildskript](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> Es ist möglich, ein auf XML basierendes Buildsystem wie z. b. MSBuild oder nant zu verwenden, aber diese sind nicht für die Ausdrucksfähigkeit und Wartbarkeit einer DSL vorgesehen, die für das Erstellen von Software reserviert ist

### <a name="parameterizing-the-build-script"></a>Parametrialisieren des Buildskripts

Der Prozess zum entwickeln und Testen von Software erfordert Informationen, die geheim gehalten werden sollten. Insbesondere das Erstellen eines APK erfordert möglicherweise ein Kennwort für den Keystore und/oder den schlüsselalias im keystore. Ebenso erfordert Test Cloud einen API-Schlüssel, der für einen Entwickler eindeutig ist. Diese Wertetypen sollten im Buildskript nicht hart codiert sein. Stattdessen sollten Sie als Variablen an das Buildskript übermittelt werden.

Weniger sensibel sind Werte wie z. b. die IOS-Geräte-ID oder die Android-Geräte-ID, die identifizieren, welche Geräte Test Cloud für Testläufe verwenden soll. Dabei handelt es sich nicht um Werte, die geschützt werden müssen, aber Sie können sich von Build zu Build ändern.

Das Speichern dieser Typen von Variablen außerhalb des Buildskripts erleichtert auch das Freigeben des Buildskripts in einer Organisation, z.b. für Entwickler. Entwickler können genau dasselbe Skript wie der Buildserver verwenden, können jedoch eigene Keystores und API-Schlüssel verwenden.

Es gibt zwei mögliche Optionen zum Speichern dieser sensiblen Werte:

- **Eine Konfigurationsdatei** – um den Test Cloud-API-Schlüssel zu schützen, sollte dieser Wert nicht in die Versionskontrolle eingecheckt werden. Die Datei kann für jeden Computer erstellt werden. Wie Werte aus dieser Datei gelesen werden, hängt von der verwendeten Skriptsprache ab.

- **Umgebungsvariablen** – diese können problemlos auf Computer Basis und unabhängig von der zugrunde liegenden Skriptsprache festgelegt werden.

Jede dieser Optionen hat vor-und Nachteile. TeamCity funktioniert problemlos mit Umgebungsvariablen, sodass diese Vorgehensweise beim Erstellen von Buildskripts empfohlen wird.

### <a name="build-steps"></a>Buildschritte

Das Buildskript muss in der Lage sein, die folgenden Schritte auszuführen:

- **Kompilieren Sie die Anwendung** – Dies umfasst das Signieren der Anwendung mit dem richtigen Bereitstellungs Profil.

- **Übermitteln Sie die Anwendung an Xamarin Test Cloud** – Dies schließt das Signieren und ZIP-Ausrichten des APK mit dem entsprechenden Keystore ein.

Diese beiden Schritte werden im folgenden ausführlicher erläutert.

#### <a name="compiling-a-xamarinios-application"></a>Kompilieren einer xamarin. IOS-Anwendung

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Kompilieren einer xamarin. Android-Anwendung

Verwenden Sie zum Kompilieren einer Android-Anwendung **xbuild** (oder **MSBuild** unter Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Beachten Sie, dass zum Kompilieren der xamarin Android-Anwendung **xbuild** das Projekt verwendet und dass zum Erstellen der IOS-Anwendung **xbuild** die Projekt Mappe erfordert.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Übermitteln von xamarin. uitests an Test Cloud

Uitests werden mithilfe der `test-cloud.exe`-Anwendung übermittelt, wie im folgenden Code Ausschnitt gezeigt:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Beim Ausführen des Tests werden die Testergebnisse in Form einer XML-Datei im nunit-Stil " **Report. XML**" zurückgegeben. TeamCity zeigt die Informationen im Buildprotokoll an.

Weitere Informationen zum Übermitteln von uitests an Test Cloud finden Sie unter [Vorbereiten von xamarin. Android-Apps](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) oder [Vorbereiten von xamarin. IOS-apps](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Übermitteln von Calabash-Tests an Test Cloud

Calabash-Tests werden mit dem `test-cloud`-gem-Wert übermittelt, wie im folgenden Code Ausschnitt gezeigt:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```

Um eine Android-Anwendung an Test Cloud zu übermitteln, müssen Sie zunächst den APK-Test Server mit Calabash-Android neu erstellen:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```

Weitere Informationen zum Übermitteln von Calabash-Tests finden Sie im xamarin-Handbuch zum Übermitteln von [Calabash-Tests an Test Cloud](https://github.com/calabash/calabash-ios/wiki).

## <a name="creating-a-teamcity-project"></a>Erstellen eines TeamCity-Projekts

Sobald TeamCity installiert ist und Visual Studio für Mac das Projekt erstellen können, ist es an der Zeit, ein Projekt in TeamCity zu erstellen, um das Projekt zu erstellen und es an Test Cloud zu senden.

1. Gestartet durch Anmelden bei TeamCity über den Webbrowser. Navigieren Sie zum Stamm Projekt:

    ![Navigieren Sie zum Stamm Projekt](teamcity-images/image2.png "Navigieren Sie zum Stamm Projekt.") . Erstellen Sie unterhalb des Stamm Projekts ein neues Unterprojekt:

    ![Navigieren Sie zum Stamm Projekt unterhalb des Stamm Projekts, und erstellen Sie ein neues Unterprojekt.](teamcity-images/image3.png "Navigieren Sie zum Stamm Projekt unterhalb des Stamm Projekts, und erstellen Sie ein neues Unterprojekt.")
2. Nachdem das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu:

    ![Nachdem das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu.](teamcity-images/image5.png "Nachdem das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu.")
3. Fügen Sie ein VCS-Projekt an die Buildkonfiguration an. Dies erfolgt über den Bildschirm zum Festlegen der Versionskontrolle:

    ![Dies erfolgt über den Bildschirm zum Festlegen der Versionskontrolle.](teamcity-images/image6.png "Dies erfolgt über den Bildschirm zum Festlegen der Versionskontrolle.")

    Wenn kein VCS-Projekt erstellt wurde, haben Sie die Möglichkeit, eine Datei auf der neuen VCS-Stamm Seite zu erstellen, wie unten gezeigt:

    ![Wenn kein VCS-Projekt erstellt wurde, haben Sie die Möglichkeit, eine Datei auf der neuen VCS-Stamm Seite zu erstellen.](teamcity-images/image7.png "Wenn kein VCS-Projekt erstellt wurde, haben Sie die Möglichkeit, eine Datei auf der neuen VCS-Stamm Seite zu erstellen.")

    Nachdem der VCS-Stamm angefügt wurde, wird das Projekt von TeamCity ausgecheckt, und es wird versucht, die Buildschritte automatisch zu erkennen. Wenn Sie mit TeamCity vertraut sind, können Sie einen der erkannten Buildschritte auswählen. Die erkannten Buildschritte können momentan ignoriert werden.

4. Konfigurieren Sie als nächstes einen buildauslösers. Dadurch wird ein Build in die Warteschlange eingereiht, wenn bestimmte Bedingungen erfüllt sind, z. b. Wenn ein Benutzer einen Commit für das Repository ausführt. Der folgende Screenshot zeigt das Hinzufügen eines buildauslösers:

    ![Dieser Screenshot zeigt das Hinzufügen eines buildauslösers](teamcity-images/image8.png "Dieser Screenshot zeigt das Hinzufügen eines buildauslösers.") . Ein Beispiel für das Konfigurieren eines buildauslösers finden Sie im folgenden Screenshot:

    ![Ein Beispiel für das Konfigurieren eines buildauslösers finden Sie in diesem Screenshot.](teamcity-images/image9.png "Ein Beispiel für das Konfigurieren eines buildauslösers finden Sie in diesem Screenshot.")

5. Im vorherigen Abschnitt parameteriup the Build Script wurde empfohlen, einige Werte als Umgebungsvariablen zu speichern. Diese Variablen können der Buildkonfiguration über den Parameter-Bildschirm hinzugefügt werden. Fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die IOS-Geräte-ID und die Android-Geräte-ID hinzu, wie im folgenden Screenshot zu sehen:

    ![Fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die IOS-Geräte-ID und die Android-Geräte-ID hinzu.](teamcity-images/image11.png "Fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die IOS-Geräte-ID und die Android-Geräte-ID hinzu.")

6. Der letzte Schritt besteht darin, einen Buildschritt hinzuzufügen, der das Buildskript aufruft, um die Anwendung zu kompilieren und die Anwendung in die Warteschlange Test Cloud. Der folgende Screenshot zeigt ein Beispiel für einen Buildschritt, der zum Erstellen einer Anwendung eine "rakefile" verwendet:

    ![Dieser Screenshot ist ein Beispiel für einen Buildschritt, der zum Erstellen einer Anwendung eine rakefile verwendet.](teamcity-images/image12.png "Dieser Screenshot ist ein Beispiel für einen Buildschritt, der zum Erstellen einer Anwendung eine rakefile verwendet.")

7. An diesem Punkt ist die Buildkonfiguration fertiggestellt. Es empfiehlt sich, einen Build zu initiieren, um zu bestätigen, dass das Projekt ordnungsgemäß konfiguriert ist. Eine gute Möglichkeit hierfür ist das Ausführen eines Commit für eine kleine, unbedeutende Änderung am Repository. TeamCity sollte den Commit erkennen und einen Build starten.

8. Nachdem der Build abgeschlossen wurde, überprüfen Sie das Buildprotokoll, und prüfen Sie, ob Probleme oder Warnungen mit dem Build vorliegen, die Aufmerksamkeit erfordern.

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung wurde beschrieben, wie Sie TeamCity zum Erstellen von mobilen xamarin-Anwendungen und zum anschließenden einreichen an Test Cloud verwenden. Wir haben das Erstellen eines Buildskripts zum Automatisieren des Buildprozesses erläutert. Das Buildskript kümmert sich um die Kompilierung der Anwendung, die Übermittlung an Test Cloud und das warten auf die Ergebnisse.

Dann haben wir erläutert, wie Sie ein Projekt in TeamCity erstellen, das jedes Mal einen Build in die Warteschlange einreiht, wenn ein Entwickler Code committet, und das Buildskript

## <a name="related-links"></a>Verwandte Links

- [Vorbereiten von xamarin. Android-Apps](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)
- [Vorbereiten von xamarin. IOS-apps](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [Installieren und Konfigurieren von TeamCity](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
