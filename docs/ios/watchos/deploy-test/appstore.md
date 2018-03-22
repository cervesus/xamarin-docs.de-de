---
title: Auf den App Store bereitstellen
description: "Bereitstellen von Watch-Apps für den App Store"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c5b89570fdd3df80d39c6621fcd12a23babed9ee
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="deploying-to-the-app-store"></a>Auf den App Store bereitstellen

> [!IMPORTANT]
> Überprüfen Sie [Apple Watch Kit Übermittlung Handbuch](https://developer.apple.com/app-store/watch/), und finden Sie unter der [Problembehandlung](#Troubleshooting) Abschnitt für alle Probleme, die Sie möglicherweise.

- Stellen Sie sicher, dass Sie haben:
  - [**Profile des Verteilungs-Bereitstellung** ](#provisioning) für Ihre Projekte erstellt.
  - Die **Bereitstellungsziel** (`MinimumOSVersion`) für den iOS app Satz an übergeordnete **8.2** oder früher (8.3 wird nicht unterstützt).

- In [ **iTunes Connect**](#iTunes_Connect):

  - Erstellen Ihrer iOS-app-Eintrag (oder fügen Sie eine **neue Version** einer vorhandenen Anwendung).
  - Symbol "überwachen" und Screenshots hinzufügen.

- Klicken Sie dann im [Visual Studio für Mac](#xamarin_studio) (Visual Studio wird derzeit nicht unterstützt):

  - Mit der rechten Maustaste in der iOS-app, und wählen Sie **als Startprojekt festlegen**.
  - Ändern Sie in der **App Store** Konfiguration.
  - Verwenden der **Archiv** Funktion erstellen Sie ein Archiv der Beispielwebanwendung.

- Wechseln Sie zum Schluss zur [Xcode 6.2 +](#xcode)

  - Wechseln Sie zu der **Fenster > Planer** , und wählen Sie **Archive**.
  - Wählen Sie die Anwendung und Archivierung aus der Liste aus.
  - (Optional) **Überprüfen...**  das Archiv.
  - **Senden...**  das Archiv und führen Sie die Schritte zum Hochladen in iTunes, für Überprüfung und Genehmigung Connect.

Lesen Sie bestimmte Tipps, die im Zusammenhang mit diesen Elementen weiter unten. Finden Sie unter der [Problembehandlung](#Troubleshooting) Abschnitt, wenn Sie Probleme haben.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Verteilung Bereitstellungsprofile

Für die Bereitstellung der App Store erstellen, Sie erstellen müssen, eine **Verteilung Bereitstellungsprofil** für jede App-ID in der Projektmappe.

Ist einen Platzhalter-Anwendungs-ID, *werden nur ein Bereitstellungsprofil*; aber wenn Sie über eine separate App-ID für jedes Projekt benötigen Sie ein Bereitstellungsprofil für jede App-ID:

![](appstore-images/provisioningprofile-distribution-sml.png "Das Profil für die App Store-Verteilung")

Nachdem Sie alle drei Profile erstellt haben, werden sie in der Liste angezeigt. Denken Sie daran, herunterladen und installieren jeweils (durch Doppelklicken):

![](appstore-images/provisioningprofiles-sml.png "Die Liste der verfügbaren Profile")

Sie können überprüfen, ob das Bereitstellungsprofil in der **Projektoptionen** durch Auswahl der **erstellen > iOS Bundle Signing** Bildschirm- und Auswählen der **AppStore | iPhone** die Konfiguration.

Die **Bereitstellungsprofil** Liste zeigt alle übereinstimmenden Profile – sehen Sie die entsprechenden Profilen, die Sie in der Dropdown-Liste erstellt haben.

![](appstore-images/options-selectprofile-sml.png "Das Dialogfeld für die iOS-Paket zu signieren")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Führen Sie die [Übersicht über app-Verteilung](~/ios/deploy-test/app-distribution/index.md), insbesondere:

- [Eine app konfigurieren im iTunes Connect.](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Wenn Sie die app in iTunes Connect konfigurieren zu können, vergessen Sie nicht auf das Symbol "überwachen" und Screenshots hinzufügen:

![](appstore-images/itunesconnect-watch-sml.png "Das Symbol "überwachen" und Screenshots in iTunes Connect")

Die Symboldatei sollten 1024 x 1024 Pixel, und haben eine zirkuläre Maske, die darauf angewendet werden, wenn er angezeigt wird. Das Symbol "sollte einen alpha-Kanal nicht aufweisen.

Mindestens ein Screenshot erforderlich ist, bis zu fünf übermittelt werden kann.
Sie sollten werden 312 x 390 Pixel und zeigen Ihre Watch-App in Aktion.
Simulator Überwachungsfenster 42 mm können Screenshots in dieser Größe durchführen zu können.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Stellen Sie sicher, dass die iOS-app das Startprojekt ist. Mit der rechten Maustaste, um festzulegen, wenn dies nicht der Fall ist:

  ![](appstore-images/xs-startup.png "Festlegen des Startprojekts")

2. Wählen Sie die **AppStore** Buildkonfiguration:

  ![](appstore-images/xs-appstore.png "Die Buildkonfiguration der App Store")

3. Wählen Sie die **erstellen > Archiv** Menüelement zum Starten des Archivierungsprozesses:

  ![](appstore-images/xs-archive.png "Klicken Sie im Menü erstellen")

Sie könne auch die **Ansicht > Archive...**  Menüelement Archive angezeigt, die zuvor erstellt wurden.

  ![](appstore-images/xs-archives-sml.png "Das Archive-Ansicht")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode wird Archive für Mac in Visual Studio erstellten automatisch angezeigt werden.

1. Starten Sie Xcode, und wählen Sie **Fenster > Planer**:

  ![](appstore-images/xc-organizer.png "Klicken Sie im Menü Fenster")

2. Wechseln Sie zu der **Archive** Registerkarte, und wählen Sie das Archiv, das mit Visual Studio für Mac erstellt wurde:

  ![](appstore-images/xc-archives.png "Die Registerkarte "Archive"")

3. Optional **überprüfen...**  Archivs, wählen Sie dann **senden...**  um der upload der app in iTunes Connect.

4. Wählen Sie das Entwicklungsteam (Wenn Sie mehrere angehören), und bestätigen Sie die Übermittlung:

  ![](appstore-images/xc-submit1.png "Die Development-Bereich "Team"")

5. Finden Sie in iTunes Connect erneut aus, um die hochgeladene Binärdatei finden Sie unter. Wechseln Sie zur Konfigurationsseite für Ihre app, und wählen Sie **Vorabversion** in der oberen Menüleiste, finden Sie unter der **Builds** Liste:

  [![](appstore-images/itc-prerelease-sml.png "Die Konfigurationsseite apps in iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Dann können Sie die app zur Genehmigung übermitteln, auf die **Versionen** Seite. Finden Sie in der [Übersicht über die iOS-app-Verteilung](~/ios/deploy-test/app-distribution/index.md) für Weitere Informationen.


## <a name="troubleshooting"></a>Problembehandlung

Hier sind einige Fehler, die beim Übermitteln der App-Store und die auszuführenden Schritte, die Sie ergreifen können, um das Problem beheben, auftreten können.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Archiv-Menüoption ist nicht sichtbar, in Visual Studio für Mac

Führen Sie die [oben beschriebenen Schritten](#xamarin_studio) so konfigurieren Sie die Projektmappe für die Archivierung. Wenn das Startup-Projekt ordnungsgemäß festgelegt werden kann, stellen Sie sicher, dass die Buildkonfiguration zuerst festgelegt ist, zu debuggen oder freigeben, bevor Sie versuchen, das Startup-Projekt zu ändern. Legen Sie die Buildkonfiguration an **AppStore**.

### <a name="invalid-icon"></a>Symbol "ungültig"

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Führen Sie die [Anleitungen zum Entfernen der alpha-Kanals](~/ios/watchos/troubleshooting.md) aus Ihrer Symbolen.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion-Konflikt

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Alle Projekte in der Projektmappe ein iOS-App, der Watch-Erweiterung und die Watch-App - sollten die gleiche Versionsnummer verwenden. Bearbeiten Sie jede **"Info.plist"** Datei, sodass die Versionsnummer genau übereinstimmt.

### <a name="missing-icons"></a>Fehlende Symbole

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Befolgen Sie die Anweisungen in der [arbeiten mit Symbolen](~/ios/watchos/app-fundamentals/icons.md) Watch-App-Projekt alle erforderlichen Bilder hinzu.

### <a name="missing-icon"></a>Symbol für fehlende

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Sicherzustellen, dass Sie über die neueste Version von Visual Studio für Mac, und dass Ihre **AppIcons.appiconset** enthält einen vollständigen Satz von Bildern. Wenn dieser Fehler weiterhin angezeigt werden, zeigen Sie die Quelle des der **Contents.json** , er enthält einen Eintrag für alle erforderlichen Bilder zu bestätigen. Auch nachdem Sie sichergestellt haben, verwenden Sie die neueste Version von Xamarin, löschen und erneutes Erstellen der **AppIcons.appiconset**.

> [!IMPORTANT]
> Es ist ein bekanntes Problem in Visual Studio für Mac Computer überwachen Symbol Unterstützung: er davon ausgeht, ein Bild 88 x 88 Pixel für die  **29x29@3x**  Image (die 87 x 87 Pixel sein sollte).


Sie können nicht in Visual Studio für Mac - entweder bearbeiten-Standardimage-Medienobjekt in Xcode beheben oder manuell bearbeiten, die **Contents.json** Datei (entsprechend [in diesem Beispiel](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Ungültige WatchKit-Unterstützung

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Diese Meldung kann während der Überprüfung und senden oder in eine automatisierte e-Mail iTunes Connect nach einem anscheinend erfolgreichen Upload angezeigt.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Sie müssen **Archiv** Ihre app in Visual Studio für Mac, und wechseln Sie dann zur Xcode 6.2 +, um zu überprüfen und Hochladen in iTunes Connect.


Verwenden Sie die Channel stabile Xamarin und Xcode 6.2 +.



### <a name="invalid-provisioning-profile"></a>Ungültige Provisioning-Profil

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Profile des Verteilungs-Bereitstellung** muss angegeben werden, für alle drei Projekte in einer Watch-app-Projektmappe: iOS-App, die Watch-Erweiterung und die Watch-App - entweder explizit (drei Profile) oder über eine einzelne Platzhalter-Profil. Überprüfen Sie, dass die Bereitstellung Profile in der iOS Developer Center vorhanden sind und dass Sie heruntergeladen und dem Mac hinzugefügt haben

### <a name="invalid-code-signing-entitlements"></a>Ungültige Berechtigungen für die Codesignatur

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Müssen sicherstellen Sie, Ihre Bereitstellung Profile Einrichtung ordnungsgemäß auf der Apple Developer Center, dass Sie heruntergeladen und installiert haben. Überprüfen Sie auch, dass sie in Visual Studio für Mac Eigenschaftenfenster für jedes Projekt festgelegt wurden.

### <a name="invalid-architecture"></a>Ungültige Architektur

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Überwachen von Apps können nur hinzugefügt [einheitliche API (64-Bit)](~/cross-platform/macios/unified/index.md) Xamarin.iOS apps.
Mit der rechten Maustaste auf das iOS-app-Projekt, und fahren Sie mit **Optionen > Erstellen > iOS-Build > Registerkarte "Erweitert"** und sicherstellen, dass die **unterstützt Architekturen** für den App Store-iPhone-Konfiguration umfasst **ARM64** (z. b. **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Dieses Paket ist ungültig.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Die übergeordnete iOS-Anwendung muss die MinimumOSVersion "8.2" oder ein zuvor festgelegt haben.

### <a name="non-public-api-usage"></a>Nicht öffentliche API-Nutzung

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Stellen Sie sicher, dass Sie die neueste Version von Xcode und Xamarin Tools verwenden.
Der Code sollte nicht öffentlichen APIs nicht zugegriffen werden.

### <a name="build-error-mt5309"></a>Fehler-MT5309 erstellen

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Dieser Fehler ist vermutlich das Ergebnis Ihrer müssen umbenannt die Xcode-Installation von **Xcode.app**. Dieser Fehler wird z. B. auftreten, wenn Sie die Installation auf Umbenennen **XCode 6.2.app**.



## <a name="related-links"></a>Verwandte Links

- [Apple WatchKit Übermittlung-Handbuch](https://developer.apple.com/app-store/watch/)
