---
title: "IPA-Unterstützung"
description: "In diesem Artikel erfahren Sie, wie Sie eine IPA-Datei erstellen können, die zum Bereitstellen einer App mit der Ad-hoc-Verteilung verwendet werden kann oder zur internen Verteilung internen Anwendungen."
ms.topic: article
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: bee3480fc90c2eac5629e336c57daa90adf9c346
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ipa-support"></a>IPA-Unterstützung

_In diesem Artikel erfahren Sie, wie Sie eine IPA-Datei erstellen können, die zum Bereitstellen einer App mit der Ad-hoc-Verteilung verwendet werden kann oder zur internen Verteilung interner Anwendungen._

Sie können eine Anwendung im iTunes App Store nicht nur für den Verkauf veröffentlichen, sondern auch für folgende Aktionen bereitstellen:

- **Ad-hoc-Tests**: Eine iOS-Anwendung kann bis zu 100 Benutzern für Alpha- oder Betatests bereitgestellt werden. Diese Benutzer werden anhand spezifischer UUIDs der iOS-Geräte identifiziert. Sehen Sie sich die Dokumentation zur [Bereitstellung von iOS-Geräten für die Entwicklung](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) an, um ausführlichere Informationen zum Hinzufügen von iOS-Testgeräten zu Ihrem Apple-Entwicklerkonto zu erhalten. Zudem finden Sie im [Ad-hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)-Leitfaden weitere Informationen zu derartigen Verteilungen.
- **Interne /Unternehmensbereitstellung**: Eine iOS-Anwendung kann intern innerhalb eines Unternehmens bereitgestellt werden, wozu eine Mitgliedschaft im Developer Enterprise-Programm von App erforderlich ist. Weitere Informationen zur internen Verteilung finden Sie im Leitfaden zur [internen Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md).

In beiden Fällen muss ein IPA-Paket erstellt und digital mit dem richtigen Verteilungsbereitstellungsprofil signiert werden. In diesem Artikel werden die Schritte erläutert, die zum Erstellen des IPA-Pakets und dessen Installation auf einem iOS-Gerät mit iTunes auf einem Mac oder Windows-PC erforderlich sind.

<a name="iTunesMetadata" />

## <a name="the-itunesmetadataplist-file"></a>Die „iTunesMetadata.plist“-Datei

Beim Erstellen einer iOS-Anwendung in iTunes Connect (entweder zum Verkauf oder für die kostenlose Veröffentlichung im iTunes App Store) kann der Entwickler die folgenden Angaben machen: Genre, Subgenre, Urheberrechtshinweis, unterstützte iOS-Geräte und Geräteanforderungen.

iOS-Anwendungen, die über die Ad-hoc- oder interne Verteilung bereitgestellt werden, müssen diese Angabe unterstützen, sodass sie in iTunes und auf dem Gerät des Benutzers angezeigt werden können. Standardmäßig wird eine kleine „iTunesMetadata.plist“-Datei immer dann erstellt, wenn Sie ein Projekt erstellen. Sie wird im Projektverzeichnis gespeichert.

Sie können auch eine benutzerdefinierte **iTunesMetadata.plist**-Datei erstellen, um die Zusatzinformationen für eine Verteilung bereitzustellen. Weitere Informationen zum Inhalt dieser Datei und wie Sie diese erstellen finden Sie in der Dokumentation zum [Inhalt der „iTunesMetadata.plist“-Datei](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) und [Creating an iTunesMetadata.plist File (Erstellen einer „iTunesMetadata.plist “-Datei)](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating).

<a name="iTunesArtwork" />

## <a name="itunes-artwork"></a>iTunes-Grafik

Wenn Sie Ihre App nicht über den App Store bereitstellen, müssen Sie zudem ein 512x512 und ein 1024x1024 großes Bild bereitstellen, das das Symbol für Ihre App in iTunes wird.

Um die iTunes-Grafik festzulegen, führen Sie Folgendes aus:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Scrollen Sie zum Bereich **iTunes Artwork** (iTunes-Grafik) im Editor.
3. Falls ein Bild fehlt, klicken Sie auf die Miniaturansicht im Editor, und wählen Sie aus dem Dialogfeld **Datei öffnen** die Bilddatei für die gewünschte iTunes-Grafik aus, und klicken Sie anschließend auf **OK** oder **Öffnen**.
4. Wiederholen Sie diesen Schritt, bis alle benötigten Bilder für Ihre Anwendung angegebene wurden.

Sehen Sie sich die Dokumentation zu [iTunes-Grafik](~/ios/app-fundamentals/images-icons/app-icons.md) an, um weitere Details zu erhalten.

<a name="createipa" />

## <a name="creating-an-ipa"></a>Erstellen einer IPA-Datei

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Das Erstellen einer IPA-Datei ist nun in den neuen Veröffentlichungsworkflow integriert. Folgen Sie dazu den unten stehenden Anweisungen, um Ihre App zu archivieren, sie zu signieren und Ihre IPA-Datei zu speichern.

Bevor Sie mit dem Erstellen einer IPA-Datei für eine plattformübergreifende Lösung beginnen, stellen Sie sicher, das Sie das iOS-Projekt als Startprojekt ausgewählt haben:

![](ipa-support-images/setasstartup.png "Festlegen des iOS-Projekts als Startprojekt")

### <a name="build-your-archive"></a>Erstellen des Archivs

Um eine IPA-Datei erstellen zu können, müssen Sie ein _Archiv_ eines Releasebuilds der Anwendung erstellen. Dieses Archiv enthält die App und deren identifizierenden Informationen.


1. Wählen Sie in Visual Studio für Mac die Konfiguration **Release | Gerät** aus:  !

    ![](ipa-support-images/buildxs01new.png "Wählen Sie „Release | Gerätekonfiguration“")

1. Wählen Sie aus dem **Build**-Menü die Option **Zur Veröffentlichung aktivieren**:

    ![](ipa-support-images/buildxs02new.png "Wählen Sie „Für Veröffentlichung aktivieren“ aus")

1. Sobald das Archiv erstellt wurde, wird die **Archivansicht** angezeigt:

    ![](ipa-support-images/buildxs03new.png "Die Ansicht „Archive“ wird angezeigt")


### <a name="sign-and-distribute-your-app"></a>Signieren und Verteilen Ihrer App

Beim Erstellen Ihrer Anwendung für das Archiv wird automatisch die **Archivansicht** geöffnet. Darin werden alle archivierten Projekte nach Projektmappe gruppiert angezeigt. Standardmäßig wird in dieser Ansicht nur die aktuelle geöffnete Projektmappe angezeigt. Klicken Sie auf **Alle Archive anzeigen**, um alle Projektmappen mit Archiven anzuzeigen.

Es wird empfohlen, Archive beizubehalten, die den Kunden bereitgestellt wurden (App Store- oder Unternehmensbereitstellungen). Dadurch können alle generierten Debuginformationen zu einem späteren Zeitpunkt symbolisiert werden.

Beachten Sie, dass für Builds, die nicht über den App Store laufen, die **iTunesMetadata.plist**-Datei und die iTunes-Grafiken automatisch in Ihre IPA-Datei einbezogen werden, wenn Sie im Archiv vorhanden sind.

Gehen Sie folgendermaßen vor, um Ihre App für die Verteilung zu signieren und vorzubereiten:


1. Klicken Sie wie unten dargestellt auf die Schaltfläche **Signieren und verteilen...** :

    ![](ipa-support-images/buildxs04new.png "Wählen Sie „Signieren und verteilen...“ aus")

1. Dadurch wird der Veröffentlichungs-Assistent geöffnet. Entscheiden Sie sich zur Erstellung eines Pakets zwischen den Verteilungskanälen **Ad-hoc** und **Unternehmen** (Intern).

    ![](ipa-support-images/distribute01.png "Wählen Sie die Verteilung „Ad-hoc“ oder „Intern“")

1. Wählen Sie auf der Seite „Bereitstellungsprofil“ Ihre Signierungsidentität sowie das entsprechende Bereitstellungsprofil aus, oder signieren Sie mit einer anderen Identität erneut:

    ![](ipa-support-images/distribute02.png "Wählen Sie die Signieridentität und das entsprechende Bereitstellungsprofil aus")

1. Überprüfen Sie die Details Ihres Pakets, und klicken Sie auf **Veröffentlichen**:

    ![](ipa-support-images/distribute03.png "Überprüfen Sie die Paketdetails")

1. Speichern Sie als Letztes die IPA-Datei auf Ihrem Computer:

    ![](ipa-support-images/distribute04.png "Speichern Sie IPA-Datei auf dem Computer")


### <a name="building-via-the-command-line-on-mac"></a>Erstellen über die Befehlszeile (auf einem Mac)

In einigen Fällen, wie z.B. in einer CI-Umgebung, ist es möglicherweise notwendig, die IPA-Datei über die Befehlszeile zu erstellen. Führenden Sie dazu die folgenden Schritte aus:


1. Achten Sie darauf, dass **Projektoptionen > iOS IPA-Optionen > Include iTunesArtwork images** (iTunesArtwork-Bilder einbeziehen) und **Ad-hoc-/Enterprise-Paket erstellen (IPA)** aktiviert sind:

    ![](ipa-support-images/imagexs04.png "„Umfasst iTunesArtwork-Bilder“ und „Ad-hoc-/Enterprise-Paket erstellen (IPA)“ sind aktiviert")

    Wenn Sie möchten, können Sie auch stattdessen die **CSPROJ**-Datei im Text-Editor bearbeiten und manuell zwei entsprechende Eigenschaften in der `PropertyGroup` für die Konfiguration hinzufügen, die beim Erstellen der App verwendet wird.

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. Wenn Sie eine optionale **iTunesMetadata.plist**-Datei einbeziehen, klicken Sie auf die Schaltfläche **...**, wählen Sie sie aus der Liste aus, und klicken Sie anschließend auf **OK**.

     ![](ipa-support-images/imagexs03.png "Wählen Sie in der Liste „iTunesMetadata.plist“ aus")

1. Rufen Sie **xbuild** (oder für Classic API **mdtool**) direkt auf, oder übergeben Sie diese Eigenschaft über die Befehlszeile:

    ```bash
    /Library/Frameworks/Mono.framework/Commands/xbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sobald das Bereitstellungsprofil erstellt und ausgewählt und die optionale **iTunesMetadata.plist**-Datei erstellt und die iTunes-Grafik in Visual Studio festgelegt wurde, können Sie eine IPA-Datei für die Verteilung erstellen. Als Nächstes müssen Sie Ihr Projekt konfigurieren. Führen Sie folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Xamarin.iOS-Projektnamen. Klicken Sie auf **Eigenschaften**, um diese für die Bearbeitung zu öffnen:

    ![](ipa-support-images/imagevs01.png "Wählen Sie „Eigenschaften“ aus")

2. Klicken Sie in der Dropdownliste **Konfiguration** auf **iOS IPA-Optionen** und auf **Ad-hoc**:

    ![](ipa-support-images/imagevs02.png "Wählen Sie „Ad-Hoc“ in der Dropdownliste „Konfiguration“ aus")

    > [!NOTE]
> Die Ad-hoc-Konfiguration steht möglicherweise nicht für neuere Xamarin.iOS-Projekte zur Verfügung. Wenn Sie nicht verfügbar ist, wählen Sie die Konfiguration **Release** aus.

3. Wenn Sie eine optionale **iTunesMetadata.plist**-Datei einbeziehen, klicken Sie auf die Schaltfläche **...**, wählen Sie sie aus der Liste aus, und klicken Sie anschließend auf **Öffnen**.

    ![](ipa-support-images/imagevs03.png "Wählen Sie in der Liste „iTunesMetadata.plist“ aus")

4. Sie können für die IPA-Datei optional einen **Paketnamen** angeben. Wird kein Paketname angegeben, erhält die Datei den gleichen Namen wie das Xamarin.iOS-Projekt.
5. Speichern Sie die Änderungen an den Projekteigenschaften.
6. Wählen Sie aus der Dropdownliste **Buildkonfiguration** die Option **Ad-hoc** aus: Wählen Sie andernfalls **Release** aus.

    ![](ipa-support-images/imagevs05.png "Wählen Sie „Ad-Hoc“ in der Dropdownliste „Buildkonfiguration“ aus")

7. Erstellen Sie das Projekt, um das IPA-Paket zu erstellen.
8. Die IPA-Datei wird im Ordner **Bin > iOS-Gerät > Ad-hoc (oder Release)** erstellt:

    ![](ipa-support-images/imagevs06.png "Die IPA-Datei im Datei-Explorer")

-----

<a name="Customizing-the-IPA-Location" />

## <a name="customizing-the-ipa-location"></a>Anpassen des IPA-Speicherorts

Eine neue **MSBuild**-Eigenschaft `IpaPackageDir` wurde hinzugefügt, um das Anpassen des Ausgabespeicherorts der **IPA**-Datei zu vereinfachen. Wenn für einen benutzerdefinierten Speicherort `IpaPackageDir` festgelegt ist, wird die **IPA**-Datei an diesem Speicherort anstatt im Standardunterverzeichnis mit Zeitstempel abgelegt. Dies kann beim Erstellen von automatisierten Builds nützlich sein, die für eine korrekte Ausführung einen bestimmten Verzeichnispfad benötigen, wie beispielsweise für fortlaufende Integrationsbuilds (CI, Continuous Integration).

Die neue Eigenschaft kann auf unterschiedliche Weise verwendet werden:

Für eine Ausgabe der **IPA**-Datei in das frühere Standardverzeichnis (wie in Xamarin.iOS 9.6 und früher) können Sie die `IpaPackageDir`-Eigenschaft z.B. mit einer der folgenden Methoden auf `$(OutputPath)` festlegen. Beide Methoden sind mit allen Unified API-Xamarin.iOS-Builds, einschließlich IDE-Builds, sowie Befehlszeilenbuilds, die **xbuild**, **msbuild** oder **mdtool** verwenden, kompatibel:

- Die erste Möglichkeit besteht darin, die `IpaPackageDir`-Eigenschaft in einem `<PropertyGroup>`-Element in einer **MSBuild**-Datei festzulegen. Sie könnten z.B. die folgende `<PropertyGroup>` am Ende der iOS-App-Projektdatei **CSPROJ** hinzufügen (unmittelbar vor dem `</Project>`-Endtag):

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Eine bessere Möglichkeit ist das Hinzufügen eines `<IpaPackageDir>`-Elements am Ende der vorhandenen `<PropertyGroup>`, die der beim Erstellen der **IPA**-Datei verwendeten Konfiguration entspricht. Der Vorteil dieser Möglichkeit ist, dass das Projekt hier für die Kompatibilität mit einer zukünftigen Einstellung auf der Projekteigenschaftenseite der iOS-IPA-Optionen vorbereitet wird. Wenn Sie derzeit die Konfiguration `Release|iPhone` zum Erstellen der **IPA**-Datei verwenden, könnte die vollständige aktualisierte Eigenschaftengruppe etwa folgendermaßen aussehen:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

Eine alternative Methode für **msbuild**- oder **xbuild**-Befehlszeilenbuilds ist das Hinzufügen eines `/p:`-Befehlszeilenarguments, um die `IpaPackageDir`-Eigenschaft festzulegen. Beachten Sie, dass **msbuild** in diesem Fall `$()`-Ausdrücke nicht erweitert, die in der Befehlszeile übergeben werden. Daher kann die `$(OutputPath)`-Syntax nicht verwendet werden. Geben Sie stattdessen einen vollständigen Pfadnamen an. Der Mono-Befehl **xbuild** erweitert zwar `$()`-Ausdrücke, doch sollten Sie trotzdem lieber einen vollständigen Pfadnamen verwenden, da **xbuild** letztendlich in zukünftigen Versionen gegenüber der [plattformübergreifenden **msbuild**](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x)-Version als veraltet gekennzeichnet wird.

Ein vollständiges Beispiel mit dieser Methode könnte unter Windows etwa folgendermaßen aussehen:


```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Unter Mac könnte es folgendermaßen aussehen:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa" />

## <a name="installing-an-ipa-using-itunes"></a>Installieren einer IPA-Datei mit iTunes

Das entstehende IPA-Paket kann Ihren Testbenutzern zur Installation auf deren iOS-Geräten übergeben oder für die Unternehmensbereitstellung ausgeliefert werden. Unabhängig von der ausgewählten Methode installiert der Benutzer das Paket in seiner iTunes-Anwendung auf seinem Mac oder Windows-PC, indem er auf die IPA-Datei doppelklickt oder sie in das geöffnete iTunes-Fenster zieht.

Die neue iOS-Anwendung wird im Bereich **Meine Apps** angezeigt. Dort können Sie auf sie rechtsklicken und so weitere Informationen enthalten.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 ![](ipa-support-images/installxs01.png "Die neue iOS-Anwendung im Abschnitt Bereich „Meine Apps“")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](ipa-support-images/installvs01.png "Die neue iOS-Anwendung im Abschnitt Bereich „Meine Apps“")

-----

Der Benutzer kann jetzt iTunes mit seinem Gerät synchronisieren, um die neue iOS-Anwendung zu installieren.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden die Vorbereitungen besprochen, die nötig sind, um eine Xamarin.iOS-Anwendung auf einen Build außerhalb des App Stores vorzubereiten. Es wurde gezeigt, wie Sie ein IPA-Paket erstellen, und wie Sie die entstehende iOS-Anwendung auf einem iOS-Gerät eines Benutzers zu Testzwecken und für die interne Verteilung installieren.


## <a name="related-links"></a>Verwandte Links

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interne Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Die Datei „iTunesMetadata.plist“](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [iTunes-Grafik](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [Verteilen von Unternehmens-Apps für iOS-Geräte](http://developer.apple.com/library/ios/#featuredarticles/FA_Wireless_Enterprise_App_Distribution/Introduction/Introduction.html)
