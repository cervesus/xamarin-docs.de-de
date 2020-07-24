---
title: Verwenden von Team City mit xamarin
description: In diesem Handbuch werden die Schritte beschrieben, die bei der Verwendung von TeamCity zum Kompilieren mobiler Anwendungen und deren Übermittlung an App Center Test ausgeführt werden müssen.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 04/01/2020
ms.openlocfilehash: 480ee0526fa707f46827fe764811dc33f5b5b243
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930025"
---
# <a name="using-team-city-with-xamarin"></a>Verwenden von Team City mit xamarin

_In diesem Handbuch werden die Schritte beschrieben, die bei der Verwendung von TeamCity zum Kompilieren mobiler Anwendungen und deren Übermittlung an App Center Test ausgeführt werden müssen._

Wie im Leitfaden [Einführung in Continuous Integration](~/tools/ci/intro-to-ci.md) erläutert, ist Continuous Integration (CI) eine nützliche Vorgehensweise bei der Entwicklung mobiler Anwendungen. Es gibt viele geeignete Optionen für Continuous Integration Server-Software. Dieser Leitfaden konzentriert sich auf [TeamCity](https://www.jetbrains.com/teamcity/) von JetBrains.

Es gibt mehrere verschiedene Permutationen einer TeamCity-Installation. In der folgenden Liste werden einige dieser Permutationen beschrieben:

- **Windows-Dienst** – in diesem Szenario wird TeamCity gestartet, wenn Windows als Windows-Dienst gestartet wird. Er muss mit einem Mac-buildhost gekoppelt werden, um IOS-Anwendungen kompilieren zu können.

- **Starten Sie den Daemon unter OS X** – konzeptionell ist dies vergleichbar mit dem Ausführen von als Windows-Dienst, der im vorherigen Schritt beschrieben wurde. Die Builds werden standardmäßig unter dem Root-Konto ausgeführt.

- **Benutzerkonto unter OS X** – es ist möglich, TeamCity unter einem Benutzerkonto auszuführen, das bei jeder Anmeldung des Benutzers gestartet wird.

In den vorherigen Szenarios ist das Ausführen von TeamCity unter einem Benutzerkonto unter OS X die einfachste und einfachste Einrichtung.

Die Einrichtung von TeamCity umfasst mehrere Schritte:

- **Installieren von TeamCity** – die Installation von TeamCity wird in diesem Handbuch nicht behandelt. In dieser Anleitung wird davon ausgegangen, dass TeamCity installiert ist und unter einem Benutzerkonto ausgeführt wird. Anweisungen zum [Installieren von TeamCity](https://confluence.jetbrains.com/display/TCD8/Installation) finden Sie in der [Dokumentation zu TeamCity 8](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) von JetBrains.

- **Vorbereiten des Buildservers** – dieser Schritt umfasst die Installation der erforderlichen Software, Tools und Zertifikate, die zum Erstellen mobiler Anwendungen und zur Vorbereitung auf die Verteilung erforderlich sind.

- **Erstellen eines Buildskripts** – dieser Schritt ist nicht unbedingt erforderlich, aber ein Buildskript ist eine hilfreiche Hilfe beim unbeaufsichtigten Erstellen von Anwendungen. Die Verwendung eines Buildskripts hilft bei der Problembehandlung von Buildproblemen, die auftreten können, und bietet eine konsistente, wiederholbare Möglichkeit zum Erstellen der Binärdateien für die Verteilung, auch wenn Continuous Integration nicht ausgeführt wird.

- Erstellen **eines TeamCity-Projekts** – nach Abschluss der vorherigen drei Schritte müssen Sie ein TeamCity-Projekt erstellen, das alle Metadaten enthält, die zum Abrufen des Quellcodes, zum Kompilieren der Projekte und zum übermitteln der Tests an App Center Test erforderlich sind.

## <a name="requirements"></a>Anforderungen

Die Verwendung von [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) ist erforderlich.

Vertrautheit mit TeamCity 8,1 ist erforderlich. Die Installation von TeamCity geht über den Rahmen dieses Dokuments hinaus. Es wird davon ausgegangen, dass TeamCity auf OS X Mavericks installiert ist und unter einem regulären Benutzerkonto und nicht dem Stammkonto ausgeführt wird.

Der Buildserver sollte ein eigenständiger Computer mit OS X sein, der für Continuous Integration reserviert ist. Im Idealfall ist der Buildserver nicht für andere Rollen verantwortlich, wie z. b. einen Datenbankserver, einen Webserver oder eine Entwickler Arbeitsstation.

> [!IMPORTANT]
> In dieser Anleitung wird nicht die "Monitor lose" Installation von xamarin behandelt.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Vorbereiten des Buildservers

Ein wichtiger Schritt beim Konfigurieren eines Buildservers ist die Installation aller erforderlichen Tools, Software und Zertifikate, um die mobilen Anwendungen zu erstellen. Es ist wichtig, dass der Buildserver die mobile Lösung kompilieren und alle Tests ausführen kann. Um Konfigurationsprobleme zu minimieren, sollten die Software und die Tools in demselben Benutzerkonto installiert werden, das TeamCity gehostet. In der folgenden Liste sind die erforderlichen Informationen aufgeführt:

1. **Visual Studio für Mac** – einschließlich xamarin. IOS und xamarin. Android.
2. **Melden Sie sich beim xamarin-Komponenten Speicher an** – dieser Schritt ist optional und nur erforderlich, wenn Ihre Anwendung Komponenten aus dem xamarin-Komponenten Speicher verwendet. Wenn Sie sich zu diesem Zeitpunkt proaktiv beim Komponenten Speicher anmelden, werden Probleme vermieden, wenn ein TeamCity-Build versucht, die Anwendung zu kompilieren.
3. **Xcode** – Xcode ist erforderlich, um IOS-Anwendungen zu kompilieren und zu signieren.
4. **Xcode-Befehlszeilen Tools** – Dies wird in Schritt 1 des Abschnitts "Installation" des Handbuchs " [Aktualisieren von Ruby mit rbenv](https://github.com/calabash/calabash-ios/wiki) " beschrieben.
5. **Signieren von Identitäts & Bereitstellungs Profilen** – importieren Sie die Zertifikate und das Bereitstellungs Profil über Xcode. Weitere Informationen finden Sie im Apple-Handbuch zum [Exportieren von Signierungs Identitäten und Bereitstellungs Profilen](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) .
6. **Android-Keystores** – kopieren Sie die erforderlichen Android-Keystores in ein Verzeichnis, auf das der TeamCity-Benutzer Zugriff hat, d. h. `~/Documents/keystores/MyAndroidApp1` .
7. **Calabash** – Dies ist ein optionaler Schritt, wenn in Ihrer Anwendung Tests mit Calabash geschrieben wurden. Weitere Informationen finden Sie im Handbuch [Installieren von Calabash on OS X Mavericks](https://github.com/calabash/calabash-ios/wiki) und im Leitfaden zum [Aktualisieren von Ruby mit rbenv](https://github.com/calabash/calabash-ios/wiki) .

Im folgenden Diagramm werden alle folgenden Komponenten veranschaulicht:

![In diesem Diagramm werden alle diese Komponenten veranschaulicht.](teamcity-images/image1.png)

Sobald die Software installiert ist, melden Sie sich beim Benutzerkonto an, und vergewissern Sie sich, dass die gesamte Software ordnungsgemäß installiert ist und funktioniert. Dies sollte das Kompilieren der Lösung und das übermitteln der Anwendung an App Center Test umfassen. Dies kann durch Ausführen des Buildskripts vereinfacht werden, wie im nächsten Abschnitt beschrieben.

## <a name="create-a-build-script"></a>Erstellen eines Buildskripts

Es ist zwar möglich, dass TeamCity alle Aspekte der Kompilierung und Übermittlung von mobilen Anwendungen an App Center Tests selbst verarbeitet. Es wird empfohlen, ein Buildskript zu erstellen. Ein Buildskript bietet die folgenden Vorteile:

1. **Dokumentation** – ein Buildskript dient als Dokumentation zur Art der Software Erstellung. Dies entfernt einige der "Magic", die mit der Bereitstellung der Anwendung verknüpft ist, und ermöglicht es Entwicklern, sich auf die Funktionalität zu konzentrieren.
1. **Wiederholbarkeit** – ein Buildskript stellt sicher, dass jedes Mal, wenn die Anwendung kompiliert und bereitgestellt wird, auf dieselbe Weise erfolgt, unabhängig davon, wer oder was die Arbeit bewirkt. Diese wiederholbare Konsistenz entfernt alle Probleme oder Fehler, die möglicherweise aufgrund eines falsch ausgeführten Builds oder menschlichen Fehlers auftreten.
1. **Versionierung** – ein Buildskript kann im Quell Code Verwaltungssystem enthalten sein. Dies bedeutet, dass Änderungen am Buildskript nachverfolgt, überwacht und korrigiert werden können, wenn Fehler oder Ungenauigkeiten gefunden werden.
1. **Vorbereiten der Umgebung** – ein Buildskript kann Logik zum Installieren erforderlicher Drittanbieter-Abhängigkeiten enthalten. Dadurch wird sichergestellt, dass die Anwendungen mit den entsprechenden Komponenten erstellt werden.

Das Buildskript kann so einfach wie eine PowerShell-Datei (unter Windows) oder ein Bash-Skript (unter OS X) sein. Beim Erstellen des Buildskripts stehen verschiedene Auswahlmöglichkeiten für Skriptsprachen zur Verfügung:

- [**Rake**](https://github.com/jimweirich/rake) – hierbei handelt es sich um eine domänenspezifische Sprache (DSL) zum Entwickeln von Projekten, die auf Ruby basiert. "Rake" hat den Vorteil von Beliebtheit und einem umfangreichen Ökosystem von Bibliotheken.

- [**psake**](https://github.com/psake/psake) – Dies ist eine Windows PowerShell-Bibliothek zum Entwickeln von Software.

- [**Fake**](https://fsharp.github.io/FAKE/) – Dies ist eine auf F # basierende DSL, die es ermöglicht, bei Bedarf vorhandene .NET-Bibliotheken zu verwenden.

Welche Skriptsprache verwendet wird, hängt von Ihren Vorlieben und Anforderungen ab.

> [!NOTE]
> Es ist möglich, ein auf XML basierendes Buildsystem wie z. b. MSBuild oder nant zu verwenden, aber diese sind nicht für die Ausdrucksfähigkeit und Wartbarkeit einer DSL vorgesehen, die für das Erstellen von Software reserviert ist

### <a name="parameterizing-the-build-script"></a>Parametrialisieren des Buildskripts

Der Prozess zum entwickeln und Testen von Software erfordert Informationen, die geheim gehalten werden sollten. Das Erstellen eines APK erfordert möglicherweise ein Kennwort für den Keystore und/oder den schlüsselalias im keystore. Ebenso erfordert App Center Test einen [API-Schlüssel](/appcenter/api-docs/) , der für einen Entwickler eindeutig ist. Diese Wertetypen sollten im Buildskript nicht hart codiert sein. Stattdessen sollten Sie als Variablen an das Buildskript übermittelt werden.

Weniger sensibel sind Werte wie z. b. die IOS-Geräte-ID oder die Android-Geräte-ID, die identifizieren, welche Geräte App Center für Testläufe verwenden soll. Dabei handelt es sich nicht um Werte, die geschützt werden müssen, aber Sie können sich von Build zu Build ändern.

Das Speichern dieser Typen von Variablen außerhalb des Buildskripts erleichtert auch das Freigeben des Buildskripts in einer Organisation, z.b. für Entwickler. Entwickler können genau dasselbe Skript wie der Buildserver verwenden, können jedoch eigene Keystores und API- [Schlüssel](/appcenter/api-docs/)verwenden.

Es gibt zwei mögliche Optionen zum Speichern dieser sensiblen Werte:

- **Eine Konfigurationsdatei** – um den [API-Schlüssel](/appcenter/api-docs/) zu schützen, sollte dieser Wert nicht in die Versionskontrolle eingecheckt werden. Die Datei kann für jeden Computer erstellt werden. Wie Werte aus dieser Datei gelesen werden, hängt von der verwendeten Skriptsprache ab.

- **Umgebungsvariablen** – diese können problemlos auf Computer Basis festgelegt werden und sind unabhängig von der zugrunde liegenden Skriptsprache.

Jede dieser Optionen hat vor-und Nachteile. TeamCity funktioniert problemlos mit Umgebungsvariablen, sodass diese Vorgehensweise beim Erstellen von Buildskripts empfohlen wird.

### <a name="build-steps"></a>Buildschritte

Das Buildskript muss die folgenden Schritte ausführen:

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
Beim Kompilieren der Android-Anwendung **xbuild** wird das Projekt verwendet, während die **xbuild** der IOS-Anwendung die Lösung verwendet.

#### <a name="submitting-xamarinuitests-to-app-center"></a>Übermitteln von xamarin. uitests an App Center

Uitests werden mithilfe der [App Center CLI](https://github.com/microsoft/appcenter-cli)übermittelt, wie im folgenden Code Ausschnitt gezeigt:

```bash
appcenter test run uitest --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --merge-nunit-xml report.xml --build-dir pathToUITestBuildDir
```

Beim Ausführen des Tests werden die Testergebnisse in Form einer XML-Datei im nunit-Stil mit dem Namen **report.xml**zurückgegeben. TeamCity zeigt die Informationen im Buildprotokoll an.

Weitere Informationen zum Übermitteln von uitests an App Center finden Sie unter [Vorbereiten von xamarin. Android-Apps](/appcenter/test-cloud/uitest/preparing-for-upload-android) oder [Vorbereiten von xamarin. IOS-apps](/appcenter/test-cloud/uitest/preparing-for-upload-ios).

#### <a name="submitting-calabash-tests-to-app-center"></a>Übermitteln von Calabash-Tests an App Center

Calabash-Tests werden mithilfe der [App Center CLI](https://github.com/microsoft/appcenter-cli)übermittelt, wie im folgenden Code Ausschnitt gezeigt:

```bash
appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --project-dir pathToProjectDir
```

Um eine Android-Anwendung an App Center Test zu übermitteln, muss zunächst der APK-Testserver mit Calabash-Android neu erstellt werden:

```bash
$ calabash-android build </path/to/signed/APK>
$ appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK> --project-dir pathToProjectDir
```

Weitere Informationen zum Übermitteln von Calabash-Tests finden Sie im xamarin-Handbuch zum Übermitteln von [Calabash-Tests an Test Cloud](https://github.com/calabash/calabash-ios/wiki).

## <a name="creating-a-teamcity-project"></a>Erstellen eines TeamCity-Projekts

Sobald TeamCity installiert ist und Visual Studio für Mac das Projekt erstellen können, ist es an der Zeit, ein Projekt in TeamCity zu erstellen, um das Projekt zu erstellen und es an App Center zu senden.

1. Gestartet durch Anmelden bei TeamCity über den Webbrowser. Navigieren Sie zum Stamm Projekt:

    ![Navigieren Sie zum Stamm Projekt](teamcity-images/image2.png "Navigieren Sie zum Stamm Projekt.") . Erstellen Sie unterhalb des Stamm Projekts ein neues Unterprojekt:

    ![Navigieren Sie zum Stamm Projekt unterhalb des Stamm Projekts, und erstellen Sie ein neues Unterprojekt.](teamcity-images/image3.png "Navigieren Sie zum Stamm Projekt unterhalb des Stamm Projekts, und erstellen Sie ein neues Unterprojekt.")
2. Fügen Sie eine neue Buildkonfiguration hinzu, sobald das Unterprojekt erstellt wurde:

    ![Fügen Sie eine neue Buildkonfiguration hinzu, sobald das Unterprojekt erstellt wurde.](teamcity-images/image5.png "Nachdem das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Buildkonfiguration hinzu.")
3. Fügen Sie ein VCS-Projekt an die Buildkonfiguration an. Dies erfolgt über den Bildschirm zum Festlegen der Versionskontrolle:

    ![Dies erfolgt über den Bildschirm zum Festlegen der Versionskontrolle.](teamcity-images/image6.png "Dies erfolgt über den Bildschirm zum Festlegen der Versionskontrolle.")

    Wenn kein VCS-Projekt erstellt wurde, können Sie es auf der neuen VCS-Stamm Seite erstellen, wie unten gezeigt:

    ![Wenn kein VCS-Projekt erstellt wurde, können Sie es auf der neuen VCS-Stamm Seite erstellen.](teamcity-images/image7.png "Wenn kein VCS-Projekt erstellt wurde, haben Sie die Möglichkeit, eine Datei auf der neuen VCS-Stamm Seite zu erstellen.")

    Nachdem der VCS-Stamm angefügt wurde, checkt TeamCity das Projekt aus und versucht, die Buildschritte automatisch zu erkennen. Wenn Sie mit TeamCity vertraut sind, können Sie einen der erkannten Buildschritte auswählen. Es ist sicher, die erkannten Buildschritte vorerst zu ignorieren.

4. Konfigurieren Sie als nächstes einen buildauslösers. Dadurch wird ein Build in die Warteschlange eingereiht, wenn bestimmte Bedingungen erfüllt sind, z. b. Wenn ein Benutzer einen Commit für das Repository ausführt. Der folgende Screenshot zeigt das Hinzufügen eines buildauslösers:

    ![Dieser Screenshot zeigt das Hinzufügen eines buildauslösers](teamcity-images/image8.png "Dieser Screenshot zeigt das Hinzufügen eines buildauslösers.") . Ein Beispiel für das Konfigurieren eines buildauslösers finden Sie im folgenden Screenshot:

    ![Ein Beispiel für das Konfigurieren eines buildauslösers finden Sie in diesem Screenshot.](teamcity-images/image9.png "Ein Beispiel für das Konfigurieren eines buildauslösers finden Sie in diesem Screenshot.")

5. Im vorherigen Abschnitt parameteriup the Build Script wurde empfohlen, einige Werte als Umgebungsvariablen zu speichern. Diese Variablen können der Buildkonfiguration über den Parameter-Bildschirm hinzugefügt werden. Fügen Sie die Variablen für den App Center- [API-Schlüssel](/appcenter/api-docs/), die IOS-Geräte-ID und die Android-Geräte-ID hinzu, wie im folgenden Screenshot zu sehen:

    ![Fügen Sie die Variablen für den App Center Test-API-Schlüssel, die IOS-Geräte-ID und die Android-Geräte-ID hinzu.](teamcity-images/image11.png "Fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die IOS-Geräte-ID und die Android-Geräte-ID hinzu.")

6. Der letzte Schritt besteht darin, einen Buildschritt hinzuzufügen, der das Buildskript aufruft, um die Anwendung zu kompilieren und die Anwendung in die Warteschlange App Center Test einzuschließen. Der folgende Screenshot zeigt ein Beispiel für einen Buildschritt, der zum Erstellen einer Anwendung eine "rakefile" verwendet:

    ![Dieser Screenshot ist ein Beispiel für einen Buildschritt, der zum Erstellen einer Anwendung eine "rakefile" verwendet.](teamcity-images/image12.png "Dieser Screenshot ist ein Beispiel für einen Buildschritt, der zum Erstellen einer Anwendung eine "rakefile" verwendet.")

7. An diesem Punkt ist die Buildkonfiguration fertiggestellt. Es empfiehlt sich, einen Build zu initiieren, um zu bestätigen, dass das Projekt ordnungsgemäß konfiguriert ist. Eine gute Möglichkeit hierfür ist das Ausführen eines Commit für eine kleine, unbedeutende Änderung am Repository. TeamCity sollte den Commit erkennen und einen Build starten.

8. Nachdem der Build abgeschlossen wurde, überprüfen Sie das Buildprotokoll, und prüfen Sie, ob Probleme oder Warnungen mit dem Build vorliegen, die Aufmerksamkeit erfordern.

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung wurde beschrieben, wie Sie TeamCity zum Erstellen von mobilen xamarin-Anwendungen und zum anschließenden einreichen an App Center Test verwenden. Wir haben das Erstellen eines Buildskripts zum Automatisieren des Buildprozesses erläutert. Das Buildskript kümmert sich um die Kompilierung der Anwendung, die Übermittlung an App Center Test und das warten auf die Ergebnisse.

Dann haben wir erläutert, wie Sie ein Projekt in TeamCity erstellen, das jedes Mal einen Build in die Warteschlange einreiht, wenn ein Entwickler Code committet, und das Buildskript

## <a name="related-links"></a>Verwandte Links

- [Vorbereiten von xamarin. Android-Apps](/appcenter/test-cloud/uitest/preparing-for-upload-android)
- [Vorbereiten von xamarin. IOS-apps](/appcenter/test-cloud/uitest/preparing-for-upload-ios)
- [Installieren und Konfigurieren von TeamCity](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
