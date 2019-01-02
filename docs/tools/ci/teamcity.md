---
title: Verwenden von Team City mit Xamarin
description: Diesem Leitfaden wird erläutert, werden die Schritte zum Verwenden von TeamCity zum Kompilieren von mobiler Anwendungen aus, und diese dann in Xamarin Test Cloud senden.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: a3cb79f33b64d933aa8ab4d3555479cc16238992
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118265"
---
# <a name="using-team-city-with-xamarin"></a>Verwenden von Team City mit Xamarin

_Diesem Leitfaden wird erläutert, werden die Schritte zum Verwenden von TeamCity zum Kompilieren von mobiler Anwendungen aus, und diese dann in Xamarin Test Cloud senden._

Siehe die [Einführung in Continuous Integration](~/tools/ci/intro-to-ci.md) Handbuch, continuous Integration (CI) ist nützlich, bei der Entwicklung mobiler qualitätsanwendungen. Es gibt viele geeignete Optionen für continuous Integration-Serversoftware. Dieser Leitfaden konzentriert sich auf [TeamCity](http://www.jetbrains.com/teamcity/) von JetBrains.

Es gibt mehrere verschiedene Permutationen einer TeamCity-Installation. Im folgenden finden eine Liste mit einigen der folgenden:

- **Windows-Dienst** – In diesem Szenario TeamCity gestartet wird, sich bei Windows als Windows-Dienst gestartet wird. Sie müssen mit einem Mac-Buildhost von iOS-Anwendungen kompilieren kombiniert werden.

- **Starten Sie Daemon unter OS X** – vom Konzept her, dies ist sehr ähnlich, mit einem Windows-Dienst, der im vorherigen Schritt beschrieben. Standardmäßig werden die Builds mit dem Stammkonto ausgeführt werden.

- **Benutzerkonto, unter OS X** – es ist möglich, TeamCity unter einem Benutzerkonto ausgeführt, die jedes Mal startet der Benutzer anmeldet.

Die einfachste und am einfachsten mit der Einrichtung von den vorherigen Szenarien auch ist TeamCity über ein Benutzerkonto unter OS X ausgeführt wird.

Es sind mehrere Schritte zum Einrichten von TeamCity:

- **Installieren von TeamCity** – die Installation von TeamCity wird in diesem Handbuch nicht behandelt. Dieses Handbuch wird davon ausgegangen, dass TeamCity installiert ist und unter einem Benutzerkonto ausgeführt wird. Anweisungen zum [installieren TeamCity](http://confluence.jetbrains.com/display/TCD8/Installation) finden Sie in der [TeamCity 8-Dokumentation](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) von JetBrains.

- **Vorbereiten des Servers erstellen** – dieser Schritt umfasst das Installieren der erforderlichen Software, Tools und erforderlichen Zertifikate zum Erstellen von mobilen Anwendungen und für die Verteilung vorzubereiten.

- **Skript für ein erstellen Erstellen** – dieser Schritt ist nicht unbedingt erforderlich, aber ein Buildskript ist eine nützliche Hilfe zum Erstellen von Anwendungen, die für die unbeaufsichtigte. Mithilfe von ein Buildskript können mit der Problembehandlung von Builds, die entstehen können, und bietet eine konsistente, wiederholbare Möglichkeit, die Binärdateien für die Verteilung zu erstellen, auch wenn die fortlaufende Integration nicht geübt wird.

- **Creating A TeamCity Project** – Wenn die vorangegangenen drei Schritte abgeschlossen sind, müssen wir ein TeamCity-Projekt, das alle Metadaten enthält erstellen erforderlich, den Quellcode, Kompilieren die Projekte und die Tests an Xamarin Test Cloud senden.

## <a name="requirements"></a>Anforderungen

Erfahrung mit [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) ist erforderlich.

Vertrautheit mit TeamCity 8.1 ist erforderlich. Die Installation von TeamCity ist über den Rahmen dieses Dokuments hinaus. Es wird vorausgesetzt, dass TeamCity auf OS X Mavericks installiert ist und ein normales Benutzerkonto und nicht das Root-Konto ausgeführt wird.

Der Build-Server sollte auf einem eigenständigen Computer mit OS X, die ausschließlich für continuous Integration verwendet wird. Im Idealfall werden der Build-Server nicht für alle anderen Rollen, wie z. B. eine Datenbank-Server, Webserver oder Entwicklerarbeitsstation verantwortlich.

> [!IMPORTANT]
> Dieses Handbuch deckt sich nicht auf eine "monitorlose" Installation von Xamarin aus.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Vorbereiten des Buildservers

Ein wichtiger Schritt zum Konfigurieren eines Buildservers ist die erforderlichen Tools, Software und Zertifikate, die zum Erstellen mobiler Anwendungen installieren. Es ist wichtig, dass der Buildserver kompilieren Sie die mobile Projektmappe und alle Tests ausführen können. Um Probleme zu minimieren, sollte die Software und Tools in das gleiche Benutzerkonto installiert werden, die TeamCity gehostet wird. Im folgenden finden eine Übersicht darüber, was erforderlich ist:

1. **Visual Studio für Mac** – dazu gehören, Xamarin.iOS und Xamarin.Android.
2. **Melden Sie sich die Xamarin Component Store** – Dies ist ein optionaler Schritt, und ist nur erforderlich, wenn Ihre Anwendung Komponenten im Xamarin Component Store verwendet werden. Protokollierung proaktiv an diesem Punkt in der Komponentenspeicher verhindert Probleme, wenn ein Build TeamCity versucht wird, um die Anwendung zu kompilieren.
3. **Xcode** – Xcode zum Kompilieren und melden Sie ein iOS-Anwendungen erforderlich ist.
4. **Xcode-Befehlszeilentools** – hierzu finden Sie in Schritt 1 von den Abschnitt "Installation" der [Aktualisieren von Ruby mit Rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) Guide.
5. **Signieren von Identität und Bereitstellungsprofile** : Importieren der Zertifikate und das Bereitstellungsprofil über XCode. Informieren Sie sich die Apple-Handbuch auf [Exportieren von Signierungsidentitäten und Bereitstellungsprofile](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) Weitere Details.
6. **Android Schlüsselspeichern** – kopieren Sie die erforderlichen Android Schlüsselspeichern in ein Verzeichnis an, dass der TeamCity Benutzer zugreifen kann, d. h. `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – Dies ist ein optionaler Schritt, wenn die Anwendung Tests mit Calabash geschrieben wurden. Finden Sie unter den [Calabash installieren, auf OS X Mavericks](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) Handbuch und die [Aktualisieren von Ruby mit Rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) Anleitung finden Sie weitere Informationen.

Das folgende Diagramm zeigt alle diese Komponenten:

![](teamcity-images/image1.png "Dieses Diagramm veranschaulicht die alle diese Komponenten")

Sobald die Software installiert wurde, melden Sie sich bei dem Benutzerkonto, und bestätigen, dass die Software ordnungsgemäß installiert wurde und funktioniert. Dies sollten die Anpassungen die Projektmappe zu kompilieren und übermitteln der Anwendung in Test Cloud. Dies kann erheblich vereinfacht werden durch Ausführen des Skripts erstellen, wie im nächsten Abschnitt beschrieben.

## <a name="create-a-build-script"></a>Erstellen Sie ein Skript erstellen

Obwohl es durchaus möglich, für die TeamCity, alle Aspekte der Kompilierung und übermitteln die mobilen Anwendungen in Test Cloud selbst zu behandeln ist, wird es dringend empfohlen, um einen Buildskript zu erstellen. Ein Buildskript bietet die folgenden Vorteile:

1. **Dokumentation** – ein Buildskript dient als eine Form von Dokumentation wie die Software erstellt wird. Dies entfernt einiger der "magischen", die bei der Bereitstellung der Anwendungs zugeordnet ist, und erlaubt Entwicklern, die auf Funktionalität konzentrieren können.
1. **Wiederholbarkeit** – ein Buildskript wird sichergestellt, dass jedes Mal die Anwendung kompiliert und bereitgestellt, es erfolgt in genau die gleiche Weise, unabhängig davon, wer oder was die Arbeit übernimmt. Diese repeatable Konsistenz entfernt Probleme oder Fehler, die im aufgrund einer fehlerhaft ausgeführten Build einschleichen können oder Benutzerfehler.
1. **Versionsverwaltung** – ein Buildskript kann in der Quellcode-Verwaltungssystems eingeschlossen werden. Dies bedeutet, dass Änderungen an das Buildskript können nachverfolgt und überwacht und korrigiert werden, wenn Fehler oder Ungenauigkeiten gefunden werden.
1. **Vorbereiten der Umgebung** – ein Buildskript zählen Logik, um die erforderlichen 3rd Party-Abhängigkeiten zu installieren. Dadurch wird sichergestellt, dass die Anwendungen mit den richtigen Komponenten erstellt werden.

Das Buildskript kann so einfach wie eine Powershell-Datei (unter Windows) oder ein Bash-Skript (unter OS X) sein. Wenn Sie das Build-Skript zu erstellen, stehen mehrere Optionen für Skriptsprachen:

- [**Rake** ](https://github.com/jimweirich/rake) – Dies ist eine bestimmte Domäne Sprache (DSL) zum Erstellen von Projekten, die basierend auf Ruby. Rake hat den Vorteil, Beliebtheit sowie ein reichhaltiges Ökosystem von Bibliotheken.

- [**Psake** ](https://github.com/psake/psake) – Dies ist eine Windows Powershell-Bibliothek für die Entwicklung von Software

- [**FAKE** ](http://fsharp.github.io/FAKE/) – Hierbei handelt es sich um eine DSL auf der Basis von F#, mit deren Hilfe ggf. vorhandene .NET-Bibliotheken verwendet werden können.

Welche Skriptsprache verwendet wird, hängt davon ab Ihre Einstellungen und Anforderungen. Die [TaskyPro-Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) Beispiel enthält ein Beispiel für mithilfe von Rake als eine [Buildskript](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> Es ist möglich, ein XML-basierte Buildsystem wie MSBuild oder NAnt, aber diese fehlen die Ausdruckskraft und wartbarkeit einer DSL, die speziell für das Erstellen von Software zu verwenden.

### <a name="parameterizing-the-build-script"></a>Parametrisieren das Skript zum Erstellen

Der Prozess der Erstellung und das Testen von Software erfordert Informationen, die geheim gehalten werden soll. Erstellen ein APK insbesondere möglicherweise ein Kennwort für den Schlüsselspeicher und/oder des aliasschlüssels im Schlüsselspeicher. Test Cloud erfordert auch einen API-Schlüssel, der für einen Entwickler eindeutig ist. Diese Typen von Werten sollte nicht schwer sein im Buildskript codiert. Stattdessen sollten sie als Variablen an das Buildskript übergeben werden.

Weniger anfällig sind Werte, wie z. B. der iOS-Geräte-ID oder die Android-Geräte-ID, die identifizieren, welche Geräte, die, denen Test Cloud für die zu verwendende, Testen ausgeführt wird. Dies sind keine Werte, die geschützt werden müssen, aber sie können auch von Build zu Build ändern.

Speichern diese Arten von Variablen außerhalb des Buildskripts erleichtert auch das Buildskript innerhalb einer Organisation, z. B. mit Entwicklern gemeinsam nutzen. Entwickler können Sie dasselbe Skript wie der Buildserver, aber Sie können ihre eigenen Schlüsselspeichern und API-Schlüssel verwenden.

Es gibt zwei Möglichkeiten zum Speichern von diese vertrauliche Werte ein:

- **Eine Konfigurationsdatei** – richtet sich an den Test Cloud-API-Schlüssel zu schützen, dieser Wert nicht in die Versionskontrolle aktiviert sein sollte. Die Datei kann für jeden Computer erstellt werden. Wie die Werte aus dieser Datei gelesen werden, hängt die verwendete Skriptsprache ab.

- **Umgebungsvariablen** – diese können ganz einfach auf einer computerspezifischen Basis und ame Randbereiche unabhängig von der zugrunde liegenden Skriptsprache festgelegt werden.

Es gibt vor- und Nachteile auf jede dieser Optionen. TeamCity funktioniert gut bei Umgebungsvariablen, damit dieser Anleitung diese Technik wird, beim Buildskripts zu erstellen empfohlen wird.

### <a name="build-steps"></a>Buildschritte

Das Buildskript muss die folgenden Schritte ausführen können:

- **Kompilieren Sie die Anwendung** – dazu gehören das Signieren der Anwendung mit dem richtigen Bereitstellungsprofil.

- **Senden Sie die Anwendung in Xamarin Test Cloud** – dazu gehören, Signieren und Ausrichten des APKS mit den entsprechenden Keystore Zip.

Diese beiden Schritte werden im folgenden ausführlicher erläutert.

#### <a name="compiling-a-xamarinios-application"></a>Kompilieren eine Xamarin.iOS-Anwendung

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Kompilieren einer Xamarin.Android-Anwendung

Verwenden Sie zum Kompilieren einer Android-Anwendung **Xbuild** (oder **Msbuild** für Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Beachten Sie, um die Xamarin Android-Anwendung zu kompilieren **Xbuild** verwendet das Projekt, und dass zum Erstellen der iOS-Anwendung **Xbuild** die Lösung erfordert.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Übermitteln von Xamarin.UITests in Testcloud

UITests werden gesendet, mit der `test-cloud.exe` Anwendung, wie im folgenden Codeausschnitt gezeigt:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Wenn der Test ausgeführt wird, werden die Testergebnisse in Form einer NUnit Stil XML-Datei mit dem Namen zurückgegeben werden **report.xml**. TeamCity zeigt die Informationen in das Protokoll zu erstellen.

Weitere Informationen über das Senden von UITests in Test Cloud finden Sie in diesem Handbuch auf [vorbereiten Xamarin.UITests für den Upload](/appcenter/test-cloud/preparing-for-upload/uitest/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Übermitteln von Calabash-Tests zum Testen der Cloud

Tests mit Calabash werden gesendet, mit der `test-cloud` gem, wie im folgenden Codeausschnitt gezeigt:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Um eine Android-Anwendung auf Test Cloud senden zu können, ist es erforderlich, zunächst die APK-Testserver mit Calabash-Android neu erstellen:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Weitere Informationen zum Übermitteln von Tests mit Calabash finden Sie in Xamarin Handbuch auf [Calabash-Tests in Test Cloud senden](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>Erstellen eines Projekts TeamCity

Sobald TeamCity installiert ist, und Visual Studio für Mac können Ihr Projekt erstellen, ist es Zeit zum Erstellen eines Projekts in TeamCity das Projekt zu erstellen und in Test Cloud senden.

1. Durch die Anmeldung bei TeamCity über den Webbrowser gestartet. Navigieren Sie zu das Stammprojekt:

    ![Navigieren Sie zu das Stammprojekt](teamcity-images/image2.png "navigieren Sie zu das Stammprojekt") unter das Stammprojekt erstellen ein neues untergeordneten Projekts:

    ![Navigieren Sie zum Stamm Projekt darunter das Stammprojekt, Erstellen eines neuen untergeordneten Projekts](teamcity-images/image3.png "navigieren Sie auf das Projekt darunter the Root Stammprojekt, erstellen ein neues untergeordneten Projekts")
2. Nachdem das untergeordnete Projekt erstellt wurde, fügen Sie eine neue Build-Konfiguration hinzu:

    ![Nachdem das untergeordnete Projekt erstellt wurde, Hinzufügen einer neuen Buildkonfiguration](teamcity-images/image5.png "nach der das untergeordnete Projekt erstellt wurde, Hinzufügen einer neuen Build-Konfiguration")
3. Fügen Sie ein VCS-Projekt, an die Konfiguration zu erstellen. Dies erfolgt über den Bildschirm für die Einstellung der Version des Steuerelements:

    ![Dies erfolgt über den Bildschirm-Steuerelement Versionseinstellung](teamcity-images/image6.png "Dies erfolgt mithilfe der Einstellung für Steuerelement-Bildschirm")

    Ist kein VCS-Projekt erstellt haben, können Sie die unten gezeigten neuen VCS-Stamm-Seite erstellen:

    ![Ist kein VCS-Projekt erstellt haben, haben Sie die Möglichkeit, ein von der neuen VCS-Stamm-Seite erstellen,](teamcity-images/image7.png "ist kein VCS-Projekt erstellt haben, haben Sie die Möglichkeit, ein von der neuen VCS-Stamm-Seite zu erstellen")

    Nach der Stamm VCS angefügt wurde, erkennen Sie TeamCity wird Auschecken des Projekts und versuchen Sie es zur automatischen Buildschritte. Wenn Sie mit TeamCity vertraut sind, können Sie einen der Buildschritte, erkannten auswählen. Es ist sicher der erkannten Buildschritte ignoriert.

4. Als Nächstes konfigurieren Sie einen Trigger erstellen. Dies wird Einrichten eines Builds zur Warteschlange, wenn bestimmte Bedingungen erfüllt sind, z. B. wenn ein Benutzer Code im Repository committet. Der folgende Screenshot zeigt, wie Sie einen Buildtrigger hinzufügen:

    ![Dieser Screenshot zeigt, wie Sie einen Buildtrigger hinzufügen](teamcity-images/image8.png "dieser Screenshot zeigt, wie Sie einen Buildtrigger hinzufügen") ein Beispiel zum Konfigurieren eines Build-Triggers im folgenden Screenshot sehen:

    ![Ein Beispiel zum Konfigurieren eines Build-Triggers finden Sie in diesem Screenshot](teamcity-images/image9.png "ein Beispiel zum Konfigurieren eines Build-Triggers finden Sie in diesem Screenshot")

5. Im vorherige Abschnitt parametrisieren das Skript zu erstellen, wird empfohlen, einige Werte als Umgebungsvariablen gespeichert. Diese Variablen können für die Buildkonfiguration über den Bildschirm Parameter hinzugefügt werden. Fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android Geräte-ID wie im folgenden Screenshot gezeigt:

    ![Fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android-Geräte-ID](teamcity-images/image11.png "fügen Sie die Variablen für den Test Cloud-API-Schlüssel, die iOS-Geräte-ID und die Android-Geräte-ID")

6. Der letzte Schritt besteht einen Buildschritt hinzufügen, mit der das Buildskript zum Kompilieren der Anwendung und das Einreihen in die Warteschlange der Anwendung in Test Cloud aufgerufen werden. Im folgende Screenshot ist ein Beispiel für einen Buildschritt hinzufügen, der eine Rakefile verwendet, um eine Anwendung zu erstellen:

    ![In diesem Screenshot wird ein Beispiel für einen Buildschritt hinzufügen, die eine Rakefile zum Erstellen einer Anwendung verwendet](teamcity-images/image12.png "in diesem Screenshot wird ein Beispiel für einen Buildschritt hinzufügen, die eine Rakefile verwendet, um eine Anwendung erstellen")

7. An diesem Punkt ist die Build-Konfiguration abgeschlossen. Es ist eine gute Idee, lösen Sie einen Buildvorgang aus, um sicherzustellen, dass das Projekt ordnungsgemäß konfiguriert ist. Eine gute Möglichkeit hierzu ist eine kleine, nicht signifikante Änderung im Repository. TeamCity sollte den Commit zu erkennen, und starten Sie einen Build.

8. Sobald der Build abgeschlossen ist, überprüfen Sie das Buildprotokoll zu und festzustellen Sie, ob Probleme oder Warnungen mit dem Build, die Ihre Aufmerksamkeit erfordern.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt, wie Sie TeamCity mit mobilen Xamarin-Anwendungen erstellen und diese dann in Test Cloud zu senden. Erstellen eines Build-Skripts zum Automatisieren des Buildprozesses erläutert. Das Buildskript kümmert sich die Anwendung kompilieren, in Test Cloud senden und die Ergebnisse wartet

Und wir erläutert, wie Sie ein Projekt in TeamCity zu erstellen, wird einen Build zur Warteschlange jedes Mal ein Entwickler Code committet und ruft das Skript zum Erstellen.

## <a name="related-links"></a>Verwandte Links

- [Vorbereiten der Xamarin.UITests angewendeter Verwaltungsrichtlinie hochladen](/appcenter/test-cloud/preparing-for-upload/uitest/)
- [Installieren und Konfigurieren von TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
